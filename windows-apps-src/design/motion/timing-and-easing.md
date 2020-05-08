---
Description: Saiba como o Motion fluente usa funções de tempo e de atenuação.
title: Tempo e suavização
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 098a75da573a977aa393197a61a62b0337f0dc06
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970501"
---
# <a name="timing-and-easing"></a>Tempo e suavização

Enquanto o movimento é baseado no mundo real, também estamos em um meio digital, que traz a expectativa de velocidade e desempenho.

## <a name="examples"></a>Exemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o aplicativo da <strong style="font-weight: semi-bold">Galeria de controles XAML</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/EasingFunction">abrir o aplicativo e ver as funções de atenuação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Como o movimento fluente usa o tempo

O tempo é um elemento importante para fazer o movimento ser natural para objetos que entram, saem ou se movem na interface do usuário.

1. Objetos ou cenas que entram no campo de visualização são rápidas, mas comemoradas. Essas animações normalmente duram mais tempo do que as saídas para permitir a criação hierárquica de uma cena.
1. Objetos ou cenas que saem do campo de visualização são muito rápidas. O usuário deve ser capaz de entender para onde a interface foi. No entanto, depois que a interface do usuário é ignorada, ela deve sair do caminho.
1. Objetos traduzindo em uma cena devem ter uma duração adequada para a distância percorrida.

## <a name="timing-in-fluent-motion"></a>Tempo no movimento fluente

O tempo de movimento em fluente usa 500 ms (ou meio segundo) como uma linha de base, pois é a quantidade máxima de tempo que um usuário percebe como instante.

![imagem Hero](images/time.gif)

### <a name="150ms-exit"></a>**150 ms** (saída)

:::row:::
    :::column:::
Use para objetos ou páginas que estão saindo da cena ou do fechamento.
Permite um feedback direcional muito rápido de saída da interface do usuário, onde o tempo não impede a taxa de quadros de atingir uma animação suave.
    :::column-end:::
    :::column:::
        ![150ms Motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ms** (entrada)

:::row:::
    :::column:::
Use para objetos ou páginas que estão entrando na cena ou abrindo.
Permite que uma quantidade razoável de tempo para comemorar o conteúdo quando entra em cena.
    :::column-end:::
    :::column:::
        ![Movimento de 300 ms](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 ms** (movimento)

:::row:::
    :::column:::
Use para objetos que estão sendo convertidos em uma única cena ou em várias cenas. 
    :::column-end:::
    :::column:::
        ![movimento de 500 MS](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Suavização no movimento fluente

A suavização é uma maneira de manipular a velocidade de um objeto à medida que ele transita. Os movimentos fluentes são a cola que une todas as experiências de movimento. Embora seja extremo, a suavização é usada no sistema para ajudar a unificar a sensação física dos objetos se movendo pelo sistema. Esta é uma forma de imitar o mundo real e fazer objetos em movimento parecerem estar no seu ambiente.

![imagem Hero](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Aplicar uma suavização de movimento

Esses suavizações ajudam você a atingir uma aparência mais natural e são a linha de base que usamos para o movimento fluente.

Os exemplos de código mostram como aplicar valores de suavização recomendados para animações de Storyboard (XAML) ou animações de Composição (C#).

### <a name="accelerate-exit"></a>**Acelerar** (saída)

:::row:::
    :::column:::
Use para interface do usuário ou objetos que estão saindo da cena.

Os objetos se tornam alimentados e recebem impulso até atingirem a velocidade de escape.
A sensação resultante é que o objeto está experimentando seu mais difícil de sair do caminho do usuário e de liberar espaço para o novo conteúdo.
    :::column-end:::
    :::column:::
        ![Acelere a atenuação](images/accelEase.gif)
    :::column-end:::
:::row-end:::

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

:::row:::
    :::column:::
Use para objetos ou interface do usuário entrando na cena, navegando ou gerando.

Uma vez em cena, o objeto é atendido com um resfricdor extremo, o que torna o restante do objeto lento.
A sensação resultante é que o objeto percorreu de uma longa distância e entrou em uma velocidade extrema, ou retorna rapidamente a um estado Rest.

Mesmo que seja precedido por um momento de falta de resposta, a velocidade do objeto de entrada tem o efeito de se sentir rápido e responsivo.
    :::column-end:::
    :::column:::
        ![atenuação desacelerada](images/decelEase.gif)
    :::column-end:::
:::row-end:::

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

:::row:::
    :::column:::
Essa é a atenuação de linha de base para qualquer alteração de parâmetro animada dentro do sistema.
Use a suavização padrão para objetos que mudam de um estado para outro na tela, como uma simples mudança de posição. Além disso, use-o para objetos que se transformam na cena, como um objeto que cresce.

A sensação resultante é que os objetos que alteram o estado de a para B estão chegando e assumidos pelo, forças naturais.
    :::column-end:::
    :::column:::
        ![atenuação padrão](images/standardEase.gif)
    :::column-end:::
:::row-end:::

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
- [Direcionalidade e gravidade](directionality-and-gravity.md)
