---
author: jwmsft
title: Animações de mola
description: Saiba como usar animações de mola de movimento natural.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: 2b28653fc7746075c57f862b0c885beac6d4934f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701065"
---
# <a name="spring-animations"></a>Animações de mola

O artigo mostra como usar os NaturalMotionAnimations de mola.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações de movimento natural](natural-animations.md)

## <a name="why-springs"></a>Por que molas?

As molas helicoidais são uma experiência de movimento comum que todos nós já vivenciamos em algum momento de nossas vidas; desde brinquedos helicoidais a experiências de física em sala de aula com um bloco vinculado à mola. O movimento de oscilação de uma mola geralmente incita uma resposta alegre e divertida nas pessoas que o observam. Consequentemente, o movimento de uma mola se adequa perfeitamente à interface do usuário do aplicativo para aqueles que pretendem criar uma experiência de movimento mais dinâmica que seja mais "evidente" para o usuário final do que um tradicional Bézier Cúbico. Nesses casos, o movimento da mola não apenas cria uma experiência de movimento mais dinâmica, mas também pode tornar mais atrativo um conteúdo novo ou atualmente animado. Dependendo do idioma de identidade visual ou movimento do aplicativo, a oscilação fica mais visível; no entanto, em outros casos, é mais sutil.

![Movimento com animação de mola](images/animation/offset-spring.gif)
![Movimento com animação de Bézier cúbico](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Usando molas na interface do usuário

Como mencionado anteriormente, as molas podem ser um movimento útil a ser integrado ao aplicativo para criar uma experiência de interface do usuário muito familiar e divertida. O uso comum das molas na interface do usuário são:

| Descrição do uso da mola | Exemplo visual |
| ------------------------ | -------------- |
| Fazendo uma experiência de movimento "surgir" e tornar-se mais dinâmica. (Escala de animação) | ![Movimento de escala com animação de mola](images/animation/scale-spring.gif) |
| Fazendo uma experiência de movimento sutilmente mais dinâmica (deslocamento animado) | ![Movimento de deslocamento com animação de mola](images/animation/offset-spring.gif) |

Em cada um desses casos, o movimento de mola pode ser acionado "saltando para" e oscilando em torno de um novo valor ou oscilando em torno de seu valor atual com alguma velocidade inicial.

![Oscilação da animação de mola](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Definindo o movimento da mola

Crie uma experiência de mola usando as APIs de NaturalMotionAnimation. Especificamente, você cria um SpringNaturalMotionAnimation usando os métodos Create * a partir do Compositor. Em seguida, você será capaz de definir as seguintes propriedades do movimento:

- DampingRatio – expressa o nível de amortecimento do movimento da mola usado na animação.

| Valor da proporção de amortecimento | Descrição |
| ------------------- | ----------- |
| DampingRatio = 0 | Undamped – a mola oscilará por muito tempo |
| 0 < DampingRatio < 1 | Underdamped – a mola oscilará com pouca a muita intensidade. |
| DampingRatio = 1 | Criticallydamped – a mola não apresentará oscilação. |
| DampingRation > 1 | Overdamped – a mola atingirá rapidamente seu destino com uma desaceleração abrupta e nenhuma oscilação |

- Period – o tempo que a mola leva para executar uma única oscilação.
- Final/Starting Value – define as posições inicial e final do movimento da mola (se não for definido, o valor inicial e/ou final será o valor atual).
- Initial Velocity – velocidade inicial programática do movimento.

Você também pode definir um conjunto de propriedades do movimento igual ao de KeyFrameAnimations:

- DelayTime/Delay Behavior
- StopBehavior

Nos casos comuns de deslocamento de animação e escala/tamanho, os seguintes valores são recomendados pela equipe de design do Windows para DampingRatio e Period em diferentes tipos de molas:

| Propriedade | Normal Spring | Dampened Spring | Less-Dampened Spring |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Damping Ratio = 0,8 <br/> Period = 50 ms | Damping Ratio = 0,85 <br/> Period = 50 ms | Damping Ratio = 0,65 <br/> Period = 60 ms |
| Scale/Size | Damping Ratio = 0,7 <br/> Period = 50 ms | Damping Ratio = 0,8 <br/> Period = 50 ms | Damping Ratio = 0,6 <br/> Period = 60 ms |

Depois que você tiver definido as propriedades, poderá transmitir o NaturalMotionAnimation da mola para o método StartAnimation de um CompositionObject ou a propriedade Motion de um InteractionTracker InertiaModifier.

## <a name="example"></a>Exemplo

Neste exemplo, você cria uma experiência de interface do usuário de navegação e de tela na qual o usuário clica em um botão de expansão e um painel de navegação é animado com um movimento de oscilação helicoidal.

![Animação de mola ao clicar](images/animation/spring-animation-on-click.gif)

Comece definindo a animação em mola no evento clicado para o momento em que o painel de navegação aparecer. Depois, defina as propriedades da animação, usando o recurso InitialValueExpression para usar uma expressão que definirá o FinalValue. Você também controla se o painel será aberto ou não e, quando estiver pronto, iniciará a animação.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

Mas, e se você quiser vincular esse movimento à entrada? Então, se o usuário passar o dedo para fora, os painéis se destacarão com um movimento de mola? O mais importante é que, se o usuário passar o dedo com mais firmeza ou mais rapidez, o movimento se adaptará de acordo com a velocidade do usuário final.

![Animação de mola ao passar o dedo](images/animation/spring-animation-on-swipe.gif)

Para fazer isso, você pode usar a mesma animação de mola e passá-la para um InertiaModifier com InteractionTracker. Para obter mais informações sobre os InputAnimations e o InteractionTracker, consulte [Experiências personalizadas de manipulação com o InteractionTracker](interaction-tracker-manipulations.md). Suponhamos que, neste exemplo de código, você já tenha configurado o InteractionTracker e o VisualInteractionSource. Vamos nos concentrar na criação dos InertiaModifiers que usarão um NaturalMotionAnimation, neste caso, uma mola.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

Agora você tem dois uma animação de mola programática e controlada por entrada na sua interface do usuário!

Em resumo, estas são as etapas para usar uma animação de mola no app:

1. Criar o SpringAnimation a partir do Compositor.
1. Definir as propriedades do SpringAnimation se você quiser valores não padrão:
    - DampingRatio
    - Period
    - Final Value
    - Initial Value
    - Initial Velocity
1. Atribuir ao destino.
    - Se você estiver animando a propriedade CompositionObject, passe o SpringAnimation como parâmetro para StartAnimation.
    - Se você quiser usar com entrada, defina a propriedade NaturalMotion de um InertiaModifier para SpringAnimation.

