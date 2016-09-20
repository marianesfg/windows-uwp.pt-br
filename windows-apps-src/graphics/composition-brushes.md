---
author: scottmill
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: "Pincéis de composição"
description: "Um pincel pinta a área de um Visual com sua saída. Pincéis diferentes têm diferentes tipos de saída."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 11989aafb86d280b93eed7c2e3f016b5914b15ab

---
# Pincéis de composição

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Um pincel pinta a área de um [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) com sua saída. Pincéis diferentes têm diferentes tipos de saída. A API de composição fornece três tipos de pincel:

-   [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) pinta um elemento visual com uma cor sólida
-   [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) pinta um elemento visual com o conteúdo de uma superfície de composição
-   [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) pinta um elemento visual com o conteúdo de um efeito de composição

Todos os pincéis são herdados de [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589398); eles são criados direta ou indiretamente pelo [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789) e são recursos independentes do dispositivo. Embora os pincéis sejam independentes do dispositivo, [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) e [**CompositionEffectBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589406) pintam um [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) com o conteúdo de uma superfície de composição que depende do dispositivo.

-   [Pré-requisitos](./composition-brushes.md#prerequisites)
-   [Noções básicas de cor](./composition-brushes.md#color-basics)
    -   [Modos Alfa](./composition-brushes.md#alpha-modes)
-   [Usando o pincel de cor](./composition-brushes.md#using-color-brush)
-   [Usando o pincel de superfície](./composition-brushes.md#using-surface-brush)
-   [Configurando a ampliação e o alinhamento](./composition-brushes.md#configuring-stretch-and-alignment)

## Pré-requisitos

Esta visão geral parte do princípio de que você já está familiarizado com a estrutura de um aplicativo básico de composição, conforme descrito em [Interface do usuário de composição](visual-layer.md).

## Noções básicas de cor

Antes de pintar com um [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399), você precisa escolher as cores. A API de composição usa a estrutura do Windows Runtime, Color, para representar uma cor. A estrutura Color usa a codificação sRGB. A codificação sRGB divide as cores em quatro canais: alfa, vermelho, verde e azul. Cada componente é representado por um valor de ponto flutuante com um intervalo normal de 0.0 a 1.0. Um valor de 0.0 indica a ausência completa da cor, enquanto um valor de 1.0 indica que a cor está totalmente presente. Para o componente alfa, 0.0 representa uma cor totalmente transparente e 1.0 representa uma cor totalmente opaca.

### Modos Alfa

Os valores de cor no [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) sempre são interpretados como alfa puro.

## Usando o pincel de cor

Para criar um pincel de cor, chame o método Compositor.[**CreateColorBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createcolorbrush.aspx), que retorna um [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399). A cor padrão para **CompositionColorBrush** é \#00000000. A ilustração e o código a seguir mostram uma pequena árvore visual para criar um retângulo tracejado com um pincel de cor preta e pintado com um pincel de cor sólida que tem o valor de cor 0x9ACD32.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)
```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual1, visual2;
CompositionColorBrush _blackBrush, _greenBrush; 

_compositor = new Compositor();
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
visual1 = _compositor.CreateSpriteVisual();
visual1.Brush = _blackBrush;
visual1.Size = new Vector2(156, 156);
visual1.Offset = new Vector3(0, 0, 0);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
Visual2 = _compositor.CreateSpriteVisual();
Visual2.Brush = _greenBrush;
Visual2.Size = new Vector2(150, 150);
Visual2.Offset = new Vector3(3, 3, 0);
```

Ao contrário de outros pincéis, criar um [**CompositionColorBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589399) é uma operação relativamente barata. Você pode criar objetos **CompositionColorBrush** a cada renderização, com pouco ou nenhum impacto no desempenho.

## Usando o pincel de superfície

Um [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) pinta um elemento visual com uma superfície de composição (representada por um objeto [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819)). A ilustração a seguir mostra um elemento visual quadrado pintado com um bitmap da imagem licorice renderizado em um **ICompositionSurface** usando D2D.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png) O primeiro exemplo inicia uma superfície de composição para uso com o pincel. A superfície de composição é criada usando um método auxiliar, LoadImage, que recebe um [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) e uma URL como uma cadeia de caracteres. Ela carrega a imagem da URL, renderiza a imagem em um [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) e define a superfície como conteúdo do **CompositionSurfaceBrush**. Observe que **ICompositionSurface** fica exposto somente no código nativo, portanto, o método LoadImage é implementado em código nativo.

```cs
LoadImage(Brush,
          "ms-appx:///Assets/liqorice.png");
```

Para criar o pincel de superfície, chame o método Compositor.[**CreateSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositor.createsurfacebrush.aspx). O método retorna um objeto [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415). O exemplo a seguir ilustra o código que pode ser usado para pintar um elemento visual com o conteúdo de um **CompositionSurfaceBrush**.

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual visual;
CompositionSurfaceBrush _surfaceBrush;

_surfaceBrush = _compositor.CreateSurfaceBrush();
LoadImage(_surfaceBrush, "ms-appx:///Assets/liqorice.png");
visual.Brush = _surfaceBrush;
```

## Configurando a ampliação e o alinhamento

Às vezes, o conteúdo do [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Dn706819) para um [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) não preenche completamente as áreas do elemento visual que está sendo pintado. Quando isso acontece, a API de composição usa as configurações de modo [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx), [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) e [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) do pincel para determinar como preencher a área restante.

-   [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) e [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio) são do tipo float e podem ser usados para controlar o posicionamento do pincel dentro dos limites do elemento visual.
    -   O valor 0.0 alinha o canto esquerdo/superior do pincel com o canto esquerdo/superior do elemento visual
    -   O valor de 0,5 alinha o centro do pincel com o centro do elemento visual
    -   O valor de 1.0 alinha o canto direito/inferior do pincel com o canto direito/inferior do elemento visual
-   A propriedade [**Stretch**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.stretch) aceita estes valores, que a enumeração [**CompositionStretch**](https://msdn.microsoft.com/library/windows/apps/Dn706786) define:
    -   None: o pincel não se amplia para preencher os limites do elemento visual. Tome cuidado com essa configuração de ampliação: se o pincel for maior do que os limites do elemento visual, o conteúdo do pincel será recortado. A parte do pincel usada para pintar os limites do elemento visual pode ser controlada usando as propriedades [**HorizontalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.horizontalalignmentratio.aspx) e [**VerticalAlignmentRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionsurfacebrush.verticalalignmentratio).
    -   Uniform: o pincel é dimensionado para caber nos limites do elemento visual; a taxa de proporção do pincel é preservada. Esse é o valor padrão.
    -   UniformToFill: o pincel é dimensionado para preencher completamente os limites do elemento visual; a taxa de proporção do pincel é preservada.
    -   Fill: o pincel é dimensionado para caber nos limites do elemento visual. Como a altura e a largura do pincel são escalonadas independentemente, a taxa de proporção original do pincel pode não ser preservada. Isto é, o pincel pode ser distorcida para preencher completamente os limites do elemento visual.

 

 







<!--HONumber=Aug16_HO3-->


