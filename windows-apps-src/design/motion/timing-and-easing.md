---
author: jwmsft
Description: Learn how Fluent motion uses timing and easing functions.
title: Tempo e suavização - animação em aplicativos UWP
label: Timing and easing
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 412ba7e36c2bb36562ceee13bb1e204ff402a882
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843713"
---
# <a name="timing-and-easing"></a>Tempo e suavização

Enquanto o movimento é baseado no mundo real, também estamos em um meio digital, que traz a expectativa de velocidade e desempenho. 

## <a name="how-fluent-motion-uses-time"></a>Como o movimento fluente usa o tempo

O tempo é um elemento importante para fazer o movimento ser natural para objetos que entram, saem ou se movem na interface do usuário.

1. Objetos ou cenas que entram no campo de visualização são rápidas, mas comemoradas. Essas animações normalmente duram mais tempo do que as saídas para permitir a criação hierárquica de uma cena.
1. Objetos ou cenas que saem do campo de visualização são muito rápidas. O usuário deve ser capaz de entender para onde a interface foi. No entanto, depois que a interface do usuário é ignorada, ela deve sair do caminho.
1. Objetos traduzindo em uma cena devem ter uma duração adequada para a distância percorrida.

## <a name="timing-in-fluent-motion"></a>Tempo no movimento fluente

O tempo de movimento em fluente usa 500 ms (ou meio segundo) como uma linha de base, pois é a quantidade máxima de tempo que um usuário percebe como instante.

![imagem hero](images/time.gif)

### <a name="150ms-exit"></a>**150 ms** (saída)

:::linha::: :::coluna::: Use para objetos ou páginas que estão saindo da cena ou fechando.
Permite um feedback direcional muito rápido de saída da interface do usuário, onde o tempo não impede a taxa de quadros de atingir uma animação suave.
:::final da coluna::: :::coluna::: ![movimento de 150 ms](images/150msAlt.gif) :::fim da coluna::: :::fim da linha:::

### <a name="300ms-enter"></a>**300 ms** (entrada)

:::linha::: :::coluna::: Use para objetos ou páginas que estão entrando na cena ou abrindo.
Permite que uma quantidade razoável de tempo para comemorar o conteúdo quando entra em cena.
:::final da coluna::: :::coluna::: ![movimento de 300 ms](images/300ms.gif) :::fim da coluna::: :::fim da linha:::

### <a name="500ms-move"></a>**≤500 ms** (movimento)

::: linha:::::: coluna::: Use para objetos em uma única cena ou várias cenas. :::final da coluna::: :::coluna::: ![movimento de 500 ms](images/500ms.gif) :::fim da coluna::: :::fim da linha:::

## <a name="easing-in-fluent-motion"></a>Suavização no movimento fluente

A suavização é uma maneira de manipular a velocidade de um objeto à medida que ele transita. Os movimentos fluentes são a cola que une todas as experiências de movimento. Embora seja extremo, a suavização é usada no sistema para ajudar a unificar a sensação física dos objetos se movendo pelo sistema. Esta é uma forma de imitar o mundo real e fazer objetos em movimento parecerem estar no seu ambiente.

![imagem hero](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Aplicar uma suavização de movimento

Esses suavizações ajudam você a atingir uma aparência mais natural e são a linha de base que usamos para o movimento fluente.

Os exemplos de código mostram como aplicar valores de suavização recomendados para animações de Storyboard (XAML) ou animações de Composição (C#).

### <a name="accelerate-exit"></a>**Acelerar** (saída)

:::linha::: :::coluna::: Use para objetos ou interfaces do usuário que estão saindo da cena.

        Objects become powered and gain momentum until they reach escape velocity.
        The resulting feel is that the object is trying its hardest to get out of the user's way and make room for new content to come in.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::fim da linha:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**Desacelerar** (entrar)

:::linha::: :::coluna::: Use para objetos ou interfaces do usuários que estão entrando na cena, por navegação ou reprodução.

        Once on-scene, the object is met with extreme friction, which slows the object to rest.
        The resulting feel is that the object traveled from a long distance away and entered at an extreme velocity, or is quickly returning to a rest state.

        Even if it's preceded by a moment of unresponsiveness, the velocity of the incoming object has the effect of feeling fast and responsive.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::fim da linha:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**Suavização padrão** (mover)

:::linha::: :::coluna::: Essa é a suavização de linha de base para qualquer mudança de parâmetro animado no sistema.
Use a suavização padrão para objetos que mudam de um estado para outro na tela, como uma simples mudança de posição. Além disso, use-o para objetos que se transformam na cena, como um objeto que cresce.

        The resulting feel is that objects changing state from A to B are overcoming, and taken over by, natural forces.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::fim da linha:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral do movimento](index.md)
- [Direção e gravidade](directionality-and-gravity.md)