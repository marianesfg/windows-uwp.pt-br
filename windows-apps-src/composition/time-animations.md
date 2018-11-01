---
author: jwmsft
title: Animações baseadas em tempo
description: Use as classes KeyFrameAnimation para alterar a interface do usuário ao longo do tempo.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: bf6d3f16c7b240ca370c01a787fef09862f35863
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871313"
---
# <a name="time-based-animations"></a>Animações baseadas em tempo

Quando um componente ou toda uma experiência de usuário muda, os usuários finais geralmente observam isso de duas maneiras: ao longo do tempo ou instantaneamente. Na plataforma Windows, a primeira alternativa é preferível à última; - experiências de usuário que mudam instantaneamente geralmente confundem e surpreendem os usuários finais, pois eles não são capazes de acompanhar o que aconteceu. O usuário final considera essa experiência algo irritante e forçado.

Em vez disso, você pode alterar a interface do usuário ao longo do tempo para orientar o usuário final ou informá-los sobre as alterações feitas na experiência. Na plataforma Windows, isso é feito através de animações baseadas em tempo, também conhecidas como KeyFrameAnimations. Os KeyFrameAnimations permitem alterar uma interface do usuário ao longo do tempo e controlar cada aspecto da animação, incluindo como e quando ela será iniciada e como ela atingirá seu estado final. Por exemplo, animar um objeto para uma nova posição em 300 milissegundos é mais agradável do "teletransportá-lo" instantaneamente para lá. Quando são usadas animações, em vez de alterações instantâneas, o resultado é uma experiência mais agradável e atrativa.

## <a name="types-of-time-based-animations"></a>Tipos de animações baseadas em tempo

Há duas categorias de animações baseadas em tempo que você pode usar para criar excelentes experiências de usuário no Windows:

**Animações explícitas** – como o nome já diz, você inicia explicitamente a animação para fazer atualizações.
**Animações implícitas** – estas animações são inicializadas pelo sistema em seu nome quando uma condição é atendida.

Neste artigo, abordaremos como criar e usar animações _explícitas_ baseadas em tempo com os KeyFrameAnimations.

Há diferentes tipos de animações explícitas e implícitas baseadas em tempo, que correspondem aos diferentes tipos de propriedades dos CompositionObjects que você pode animar.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Criar animações baseadas em tempo com os KeyFrameAnimations

Antes de explicar como criar e usar animações explícitas baseadas em tempo com os KeyFrameAnimations, vamos falar sobre alguns conceitos.

- KeyFrames – São "instantâneos" individuais que uma animação colocará em movimento.
  - Definidos como pares de chave/valor. A chave representa o progresso entre 0 e 1, também conhecido como o momento do tempo de vida útil da animação em que este "instantâneo" ocorre. O outro parâmetro representa o valor de propriedade neste momento.
- Propriedades de KeyFrameAnimation – opções de personalização que você pode aplicar para atender às necessidades da interface do usuário.
  - DelayTime – tempo que precede o início de uma animação depois que o StartAnimation é chamado.
  - Duration – duração da animação.
  - IterationBehavior – comportamento de contagem ou de repetição infinita de uma animação.
  - IterationCount – número de vezes finitas que uma animação de quadro-chave será repetida.
  - KeyFrame Count – leitura do número de quadros-chave em determinada animação de quadro-chave.
  - StopBehavior – especifica o comportamento de um valor de propriedade de animação quando StopAnimation é chamado.
  - Direction – especifica a direção da animação para reprodução.
- Animation Group – inicia várias animações ao mesmo tempo.
  - Usado geralmente quando é preciso animar várias propriedades ao mesmo tempo.

Para obter mais informações, consulte [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup).

Com esses conceitos em mente, vamos falar sobre a fórmula geral da criação de um KeyFrameAnimation:

1. Identifique o CompositionObject e a respectiva propriedade que você precisa animar.
1. Crie um modelo de tipo de KeyFrameAnimation do compositor que corresponda ao tipo de propriedade que você deseja animar.
1. Usando o modelo de animação, comece adicionando KeyFrames e definindo as propriedades da animação.
    - Pelo menos um KeyFrame é necessário (quadro chave 100% ou 1f).
    - É recomendável definir uma duração também.
1. Quando você estiver pronto para executar esta animação, chame StartAnimation(...) no CompositionObject, indicando a propriedade que você deseja animar. Especificamente:
    - `Visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `Visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Se você tiver uma animação em execução e quiser interromper a animação ou o grupo de animações, use estas APIs:
    - `Visual.StopAnimation("targetProperty");`
    - `Visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Vamos analisar um exemplo para ver essa fórmula em ação.

## <a name="example"></a>Exemplo

Neste exemplo, você deseja animar o deslocamento de um elemento visual de <0,0,0> a <20,20,20> em mais de um segundo. Além disso, você deseja ver a animação do elemento visual entre essas posições 10 vezes.

![Animação do quadro chave](images/animation/animated-rectangle.gif)

Você começa identificando o CompositionObject e a propriedade que deseja animar. Nesse caso, o quadrado vermelho é representado por um elemento visual de composição chamado `redSquare`. Inicie a animação a partir desse objeto.

Como você quer animar a propriedade Offset, precisará criar uma Vector3KeyFrameAnimation (deslocamento do tipo Vector3). Você também definirá os KeyFrames correspondentes do KeyFrameAnimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

Em seguida, definiremos as propriedades do KeyFrameAnimation para descrever sua duração, juntamente com o comportamento, para realizar a animação entre as duas posições (atual e <200,0,0>) 10 vezes.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Por fim, para executar uma animação, você precisará iniciá-la na propriedade de um CompositionObject.

```csharp
redVisual.StartAnimation("Offset.X", animation);
```

Aqui está o código completo.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redSquare)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    visual.StartAnimation("Offset.X", animation);
} 
```
