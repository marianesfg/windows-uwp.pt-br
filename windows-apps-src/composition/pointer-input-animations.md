---
title: Animações baseadas em ponteiro
description: Saiba como usar a posição de um ponteiro para criar experiências dinâmicas "controladas pelo cursor".
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: 3512d47c8b3e689b0baadec26c1d8f0f510e03ef
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458142"
---
# <a name="pointer-based-animations"></a>Animações baseadas em ponteiro

Este artigo mostra como usar a posição de um ponteiro para criar experiências dinâmicas "controladas pelo cursor".

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações controladas por entrada](input-driven-animations.md)
- [Animações baseadas em relações](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>Por que criar experiências de ponteiro controladas por posição?

Na linguagem do Fluent Design, o toque não é a única maneira de interagir com a interface do usuário. Como a UWP abrange vários fatores forma de dispositivo, os usuários finais interagem com apps por meio de outras modalidades de entrada, como mouse e caneta. O uso dos dados de posição nessas outras modalidades de entrada é uma oportunidade de fazerem os usuários finais se sentirem ainda mais conectados ao seu app.

As experiências de ponteiro controladas por posição permitem que você aproveite a posição de uma modalidade de entrada de ponteiro na tela para criar movimento adicional e experiências de interface do usuário para seu app. Essas experiências geralmente fornecem contexto adicional e feedback aos usuários finais sobre o comportamento e a estrutura da interface do usuário. A experiência não é mais um fluxo unidirecional, mas começa a se tornar um fluxo bidirecional, no qual o usuário final interage por meio de suas modalidades de entrada e a interface do usuário do app pode responder de volta.

Veja a seguir alguns exemplos:

- Animando a posição de um destaque para seguir o cursor

    ![Exemplo de destaque do ponteiro](images/animation/spotlight-reveal.gif)

- Girando uma imagem com base na posição de um ponteiro

    ![Exemplo de rotação do ponteiro](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>Usando o PointerPositionPropertySet

Você pode criar essas experiências usando o PointerPositionPropertySet. Este PropertySet é criado para um UIElement para manter a posição do ponteiro enquanto o UIElement passa com sucesso pelo teste de acertos. O valor de posição é relativa ao espaço de coordenadas do UIElement (a posição <0,0> é o canto superior esquerdo do UIElement). Em seguida, você pode aproveitar esse conjunto de propriedades em uma animação para promover o movimento de outra propriedade.

Para cada um das diferentes modalidades de entrada de ponteiro, há diversos estados que a entrada pode assumir nos quais a posição é alterada: Focalizar, Pressionado, Pressionado e Movido. O PointerPositionPropertySet só mantém a posição do ponteiro nos estados Focalizar, Pressionado, e Pressionado e Movido para mouse e caneta.

Etapas gerais para começar:

1. Identifique o UIElement no qual a posição do ponteiro deve ser rastreada.
1. Acesse o PointerPositionPropertySet via ElementCompositionPreview.
    - Passe o UIElement para o método ElementCompositionPreview.GetPointerPositionPropertySet.
1. Crie uma ExpressionAnimation que faça referência à propriedade Position no PropertySet.
    - Não se esqueça de definir o parâmetro de referência!
1. Indique uma propriedade de CompositionObject com o ExpressionAnimation.

> [!NOTE]
> É recomendável que você atribua o PropertySet retornado pelo método GetPointerPositionPropertySet a uma variável de classe. Isso garante que o conjunto de propriedades não será eliminado pela coleta de lixo e, portanto, não tem nenhum efeito sobre o ExpressionAnimation no qual ele é referenciado. O ExpressionAnimations não mantém uma referência forte a nenhum dos objetos usados na equação.

## <a name="example"></a>Exemplo

Vamos analisar um exemplo no qual aproveitamos a posição Focalizar de uma modalidade de entrada Mouse e Caneta para girar dinamicamente uma imagem.

![Exemplo de rotação do ponteiro](images/animation/pointer-rotate.gif)

A imagem é um UIElement, portanto, vamos primeiro obter uma referência ao PointerPositionPropertySet

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

Neste exemplo, você tem duas expressões em execução:

- Uma expressão na qual a imagem gira com base na distância do ponteiro em relação ao centro da imagem. Quanto mais distante, maior a rotação.
- Uma expressão na qual o eixo de rotação muda com base na posição do ponteiro. Você deseja que o eixo de rotação seja perpendicular ao vetor da posição.

Vamos definir as duas expressões, um que indica a propriedade RotationAngle e a outro que indica a propriedade RotationAxis. Faça referência ao PointerPositionPropertySet como a qualquer outro PropertySet.

Neste exemplo, você está criando expressões por meio das classes do ExpressionBuilder.

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```