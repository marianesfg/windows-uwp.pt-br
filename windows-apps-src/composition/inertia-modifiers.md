---
author: jwmsft
title: Usar modificadores de inércia para criar pontos de ajuste
description: Saiba como usar o recurso InertiaModifier do InteractionTracker para criar experiências de movimento que se ajustam a um ponto especificado.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: 20c10b1cc621da834a8a7c411e75eb92b1944b5a
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6849356"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>Criar pontos de ajuste com modificadores de inércia

Neste artigo, analisares detalhadamente como usar o recurso InertiaModifier do InteractionTracker para criar experiências de movimento que se ajustam a um ponto especificado.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações controladas por entrada](input-driven-animations.md)
- [Experiências personalizadas de manipulação com o InteractionTracker](interaction-tracker-manipulations.md)
- [Animações baseadas em relações](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>Quais são os pontos de ajuste e por que eles são úteis?

Ao criar experiências de manipulação personalizadas, às vezes é útil criar _pontos de posição_ especializados na tela de rolagem/zoom na qual o InteractionTracker sempre entrará em repouso. Eles geralmente são chamados de _pontos de ajuste_.

Observe no exemplo a seguir como a rolagem pode deixar a interface do usuário em uma posição estranha entre as diferentes imagens:

![Rolagem sem pontos de ajuste](images/animation/snap-points-none.gif)

Se você adicionar pontos de ajuste, quando parar a rolagem entre as imagens, elas se "ajustarão" em uma posição especificada. Os pontos de ajuste tornam a experiência de rolagem pelas imagens muito mais limpas e sensíveis.

![Rolagem com um ponto de ajuste único](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker e InertiaModifiers

Ao criar experiências de manipulação personalizadas com o InteractionTracker, você poderá gerar experiências de movimento de ponto de ajuste usando InertiaModifiers. Os InertiaModifiers são basicamente uma maneira de definir onde ou como o InteractionTracker atinge seu destino ao entrar no estado de inércia. Você pode aplicar InertiaModifiers para impactar a posição X ou Y ou a propriedade Scale do InteractionTracker.

Existem três tipos de InertiaModifiers:

- InteractionTrackerInertiaRestingValue – uma maneira de modificar a posição de repouso final após uma interação ou uma velocidade programática. Um movimento predefinido levará o InteractionTracker a essa posição.
- InteractionTrackerInertiaMotion – uma maneira de definir um movimento específico que o InteractionTracker executará após uma interação ou uma velocidade programática. A posição final derivará desse movimento.
- InteractionTrackerInertiaNaturalMotion – uma maneira de definir a posição de repouso final após uma interação ou uma velocidade programática, mas com uma animação baseada em física (NaturalMotionAnimation).

Ao entrar em inércia, o InteractionTracker avalia cada um dos InertiaModifiers atribuídos a ele e determina se algum deles será aplicável. Isso significa que você pode criar e atribuir vários InertiaModifiers a um InteractionTracker, mas, ao definir cada um deles, você precisará fazer o seguinte:

1. Definir a condição - uma expressão que define a instrução condicional quando este InertiaModifier específico precisar ser implementado. Isso geralmente requer a análise do NaturalRestingPosition do InteractionTracker (inércia padrão determinada pelo destino).
1. Defina o RestingValue/Motion/NaturalMotion - defina a expressão de valor de repouso, a expressão de movimento ou a animação de movimento natural que ocorre quando a condição é atendida.

> [!NOTE]
> O aspecto da condição dos InertiaModifiers só é avaliado uma vez quando InteractionTracker entra em inércia. No entanto, apenas para InertiaMotion, a expressão de movimento é avaliada a cada quadro para o modificador cuja condição é verdadeira.

## <a name="example"></a>Exemplo

Agora, vamos analisar como você pode usar os InertiaModifiers para criar algumas experiências de pontos de ajuste que recriarão a tela de rolagem de imagens. Neste exemplo, cada manipulação deve mover-se por uma única imagem; isso geralmente é denominado pontos de ajuste obrigatórios únicos.

Vamos começar configurando o InteractionTracker, o VisualInteractionSource e o Expression que aproveitarão a posição do InteractionTracker.

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

Em seguida, como o comportamento do ponto de ajuste obrigatório único moverá o conteúdo para cima ou para baixo, você precisará de dois modificadores de inércia diferentes: um que move o conteúdo rolável para cima e um que o desloca para baixo.

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

Quer o ajuste para cima ou para baixo seja determinado ou não pelo local onde o InteractionTracker naturalmente se posicionaria em relação à distância instantânea, a distância entre os locais de ajuste. Se ponto intermediário for ultrapassado, faça o ajuste para baixo; caso contrário, faça o ajuste para cima. (Neste exemplo, você armazena a distância de ajuste em um PropertySet)

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

Este diagrama fornece uma descrição visual à lógica que está acontecendo:

![Diagrama de modificador de inércia](images/animation/inertia-modifier-diagram.png)

Agora você só precisa definir os valores de repouso de cada InertiaModifier: mova a posição do InteractionTracker para a posição de ajuste anterior ou seguinte.

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

Por fim, adicione os InertiaModifiers ao InteractionTracker. Agora, quando o InteractionTracker entrar no estado de inércia, ele verificará as condições dos InertiaModifiers para saber se sua posição deve ser modificada.

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```