---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pincéis de composição
description: Um pincel pinta a área de um Visual com sua saída. Pincéis diferentes têm diferentes tipos de saída.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1122ae00f9b84980a7793deef08a30a16711b4be
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5823682"
---
# <a name="composition-brushes"></a>Pincéis de composição
Todos os itens visíveis na tela de um aplicativo UWP é visível porque ele foi pintado por um pincel. Pincéis permitem que você pintar objetos (UI) da interface de usuário com conteúdo que variam de cores simples e sólidas a imagens ou desenhos à cadeia de efeitos complexos. Este tópico apresenta os conceitos de pintar com CompositionBrush.

Observe que, ao trabalhar com o aplicativo UWP XAML, você pode escolher para pintar um UIElement com um [Pincel de XAML](/windows/uwp/design/style/brushes) ou um [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). Normalmente, é aconselhável escolher um pincel XAML, se seu cenário seja compatível com um pincel de XAML e mais fácil. Por exemplo, animando a cor de um botão, alterando o preenchimento de um texto ou uma forma com uma imagem. Por outro lado, se você estiver tentando fazer algo que não é compatível com um pincel XAML como como pintura com uma máscara animada ou um stretch inserções animado ou uma cadeia de efeitos, você pode usar um CompositionBrush para pintar um UIElement com o uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Ao trabalhar com a camada Visual, um CompositionBrush deve ser usado para pintar a área de um [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Pré-requisitos](./composition-brushes.md#prerequisites)
-   [Paint com CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar com uma cor sólida](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar com um gradiente linear](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Pintar com uma imagem](./composition-brushes.md#paint-with-an-image)
    -   [Pintar com um desenho personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar com um vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar com um efeito de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar com um CompositionBrush com uma máscara de opacidade](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar com um CompositionBrush usando NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar usando Pixels em segundo plano](./composition-brushes.md#paint-using-background-pixels)
-   [Combinando CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Usando um pincel versus CompositionBrush XAML](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Tópicos relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Pré-requisitos
Esta visão geral assume que você esteja familiarizado com a estrutura de um aplicativo básico de composição, conforme descrito na [Visão geral da camada Visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Paint com um CompositionBrush

Um [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" uma área com sua saída. Pincéis diferentes têm diferentes tipos de saída. Alguns pincéis pintar uma área com uma cor sólida, outras pessoas com um gradiente, imagem, desenho personalizado ou efeito. Também há pincéis especializados que modificam o comportamento de outros pincéis. Por exemplo, máscara de opacidade pode ser usado para controlar qual área é pintada por um CompositionBrush ou uma inserções podem ser usadas para controlar o stretch aplicado a um CompositionBrush ao pintar uma área. CompositionBrush pode ser de um dos seguintes tipos:

|Classe                                   |Detalhes                                         |Introduzido no|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta uma área com uma cor sólida                        |Atualização de novembro do Windows 10 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta uma área com o conteúdo de um [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Atualização de novembro do Windows 10 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta uma área com o conteúdo de um efeito de composição |Atualização de novembro do Windows 10 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Pinta um elemento visual com um CompositionBrush com uma máscara de opacidade |Atualização de aniversário do Windows 10 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta uma área com um CompositionBrush usando um stretch NineGrid |Atualização de aniversário do Windows 10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta uma área com um gradiente linear                    |Windows 10 Fall Creators Update (SDK do Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta uma área com amostras de pixels de plano de fundo do aplicativo ou pixels diretamente por trás da janela do aplicativo na área de trabalho. Usada como uma entrada para outro CompositionBrush como um CompositionEffectBrush | Atualização de aniversário do Windows 10 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Pintar com uma cor sólida

Um [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta uma área com uma cor sólida. Há diversas maneiras para especificar a cor de um SolidColorBrush. Por exemplo, você pode especificar seus canais de (ARGB) alfa, vermelho, azuis e verdes ou usar uma das cores predefinidas fornecidas pela classe de [cores](https://docs.microsoft.com/uwp/api/windows.ui.colors) .

A ilustração e o código a seguir mostram uma pequena árvore visual para criar um retângulo tracejado com um pincel de cor preta e pintado com um pincel de cor sólida que tem o valor de cor 0x9ACD32.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>Pintar com um gradiente linear

Um [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) pinta uma área com um gradiente linear. Um gradiente linear combina duas ou mais cores em uma linha, o eixo do gradiente. Você pode usar objetos GradientStop para especificar as cores no gradiente e suas posições.

A ilustração e o código a seguir mostra um SpriteVisual pintado com um LinearGradientBrush com 2 paradas usando uma cor vermelha e amarela.

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-an-image"></a>Pintar com uma imagem

Um [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta uma área com pixels renderizados em um ICompositionSurface. Por exemplo, um CompositionSurfaceBrush pode ser usado para pintar uma área com uma imagem renderizada para uma superfície ICompositionSurface usando [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API.

A ilustração e o código a seguir mostra um SpriteVisual pintado com um bitmap de uma imagem de licorice renderizado em um ICompositionSurface usando LoadedImageSurface. As propriedades de CompositionSurfaceBrush podem ser usadas para alongar e alinhar o bitmap dentro dos limites do elemento visual.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>Pintar com um desenho personalizado
Um [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) também pode ser usado para pintar uma área com pixels de um ICompositionSurface renderizados usando [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (ou D2D).

O código a seguir mostra que um SpriteVisual pintado com um fluxo renderizado em um ICompositionSurface de texto usando Win2D. Observe que, para usar Win2D, você precisa incluir o pacote [NuGet Win2D](http://www.nuget.org/packages/Win2D.uwp) ao seu projeto.

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

Da mesma forma, o CompositionSurfaceBrush também pode ser usado para pintar um SpriteVisual com um SwapChain usando o interop Win2D. [Essa amostra](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) fornece um exemplo de como usar Win2D para pintar um SpriteVisual com um swapchain.

### <a name="paint-with-a-video"></a>Pintar com um vídeo
Um [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) também pode ser usado para pintar uma área com pixels de um ICompositionSurface renderizado usando um vídeo carregado por meio da classe [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) .

O código a seguir mostra que um SpriteVisual pintado com um vídeo carregado em um ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>Pintar com um efeito de filtro

Um [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta uma área com a saída de um CompositionEffect. Efeitos na camada Visual podem ser pensados como efeitos de filtro animáveis aplicados a uma coleção de conteúdo de origem, como cores, gradientes, imagens, vídeos, cadeias de troca, regiões de sua interface do usuário ou árvores de elementos visuais. O conteúdo de origem normalmente é especificado usando outro CompositionBrush.

A ilustração e o código a seguir mostra um SpriteVisual pintado com uma imagem de um gato que tem o efeito de filtro de dessaturação aplicado.

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

Para obter mais informações sobre como criar um efeito usando CompositionBrushes consulte [efeitos na camada Visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar com um CompositionBrush com máscara de opacidade aplicada

Um [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta uma área com um CompositionBrush com uma máscara de opacidade aplicada. A origem da máscara de opacidade pode ser qualquer CompositionBrush do tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush ou CompositionNineGridBrush. A máscara de opacidade deve ser especificada como um CompositionSurfaceBrush.

A ilustração e o código a seguir mostra um SpriteVisual pintado com um CompositionMaskBrush. A origem da máscara é um CompositionLinearGradientBrush que máscara com a aparência de um círculo usando uma imagem de círculo como uma máscara.

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar com um CompositionBrush usando NineGrid stretch

Um [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta uma área com uma CompositionBrush que é ampliado usando a metáfora inserções. A metáfora inserções permite que você Alongar bordas e cantos de um CompositionBrush de forma diferente de seu centro. A origem do stretch inserções pode por qualquer CompositionBrush do tipo CompositionColorBrush, CompositionSurfaceBrush ou CompositionEffectBrush.

O código a seguir mostra que um SpriteVisual pintado com um CompositionNineGridBrush. A origem da máscara é um CompositionSurfaceBrush que é ampliado usando uma inserções.

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>Pintar usando Pixels em segundo plano

Um [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) pinta uma área com conteúdo por trás da área. Um CompositionBackdropBrush nunca é usado por conta própria, mas em vez disso, é usado como uma entrada para outro CompositionBrush como um EffectBrush. Por exemplo, usando um CompositionBackdropBrush como uma entrada para um efeito de desfoque, você pode obter um efeito de vidro fosco.

O código a seguir mostra uma pequena árvore visual para criar uma imagem usando CompositionSurfaceBrush e uma sobreposição de vidro fosco acima da imagem. A sobreposição de vidro fosco é criada, colocando um SpriteVisual preenchido com um EffectBrush acima da imagem. O EffectBrush usa um CompositionBackdropBrush como uma entrada para o efeito de desfoque.

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>Combinando CompositionBrushes
Um número de CompositionBrushes usa outros CompositionBrushes como entradas. Por exemplo, usando o método SetSourceParameter pode ser usado para definir outro CompositionBrush como uma entrada para um CompositionEffectBrush. A tabela a seguir descreve as combinações suportadas de CompositionBrushes. Observe que usando uma combinação sem suporte gerará uma exceção.

<table>
<tbody>
<tr>
<th>Pincel</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>NÃO</td>
<td>NÃO</td>
<td>SIM</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>NÃO</td>
<td>NÃO</td>
<td>NÃO</td>
<td>NÃO</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>SIM</td>
<td>SIM</td>
<td>SIM</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>SIM</td>
<td>NÃO</td>
<td>NÃO</td>
<td>NÃO</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Usando um pincel versus CompositionBrush XAML

A tabela a seguir fornece uma lista dos cenários e uso de pincel XAML ou composição prescrito ao pintar um UIElement ou um SpriteVisual em seu aplicativo. 

> [!NOTE]
> Se um CompositionBrush é sugerido para um UIElement XAML, presume-se que o CompositionBrush é empacotado usando um XamlCompositionBrushBase.

|Cenário                                                                   | UIElement XAML                                                                                                |SpriteVisual de composição
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar uma área com cor sólida                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar uma área com cor animada                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar uma área com um gradiente estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar uma área com um gradiente animado                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar uma área com uma imagem                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar uma área com uma página da Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/D
|Pintar uma área com uma imagem usando stretch NineGrid                         |[Controle de imagem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar uma área com animado stretch NineGrid                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar uma área com uma swapchain                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade swapchain
|Pintar uma área com um vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade de mídia
|Pintar uma área com desenho 2D personalizado                                       |[CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) do Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade do Win2D
|Pintar uma área com máscara não animado                                       |Use XAML [formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) para definir uma máscara   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar uma área com uma máscara animada                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar uma área com um efeito de filtro animado                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar uma área com um efeito aplicado aos pixels em segundo plano        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Tópicos relacionados

[Composição nativo Direct2D interoperabilidade entre DirectX e com BeginDraw e EndDraw](composition-native-interop.md)

[Interoperabilidade de pincel XAML com XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
