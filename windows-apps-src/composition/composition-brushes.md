---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pincéis de composição
description: Um pincel pinta a área de um Visual com sua saída. Pincéis diferentes têm diferentes tipos de saída.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eb0d48cee4fe6698ec371c882c913affa5af7729
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644881"
---
# <a name="composition-brushes"></a>Pincéis de composição
Todos os itens na tela de um aplicativo UWP são visíveis devido à pintura com um Pincel. Pincéis permitem pintar objetos de interface de usuário com conteúdo que varia de cores simples e sólidas para imagens ou desenhos até uma cadeia de efeitos complexos. Este tópico apresenta os conceitos de pintura com CompositionBrush.

Observe que ao trabalhar com aplicativos UWP XAML, você pode optar por pintar um UIElement com um [Pincel XAML](/windows/uwp/design/style/brushes) ou um [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). Normalmente, é aconselhável e mais fácil escolher um pincel XAML se o cenário for compatível com um pincel XAML. Por exemplo, animar a cor de um botão, alterar o preenchimento de um texto ou uma forma com uma imagem. Por outro lado, se você estiver tentando fazer algo que não é compatível com um pincel XAML como pintar com uma máscara animada ou uma ampliação de grade de nove animada ou uma cadeia de efeito, você pode usar um CompositionBrush para pintar um UIElement com o uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Ao trabalhar com a amada Visual, é necessário usar um CompositionBrush para pintar a área de um [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Pré-requisitos](./composition-brushes.md#prerequisites)
-   [Pintar com CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar com uma cor sólida](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar com um gradiente linear](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Pintar com uma imagem](./composition-brushes.md#paint-with-an-image)
    -   [Pintar com um desenho personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar com um vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar com um efeito de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar com um CompositionBrush com uma máscara de opacidade](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar com um CompositionBrush usando NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar usando Pixels da tela de fundo](./composition-brushes.md#paint-using-background-pixels)
-   [Combinando CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Usando um pincel de XAML vs. CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Tópicos relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Pré-requisitos
Esta visão geral parte do princípio de que você já está familiarizado com a estrutura de um aplicativo básico de Composição, como descrito na [Visão geral da camada visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Pintar com um CompositionBrush

Um [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" uma área com sua saída. Pincéis diferentes têm diferentes tipos de saída. Alguns pincéis pintam uma área com uma cor sólida, outros com um gradiente, uma imagem, um desenho personalizado ou um efeito. Também há pincéis especializados que modificam o comportamento de outros pincéis. Por exemplo, a máscara de opacidade pode ser usada para controlar qual área é pintada por um CompositionBrush ou é possível usar nove grades para controlar a ampliação aplicada a um CompositionBrush ao pintar uma área. O CompositionBrush pode ser de um dos seguintes tipos:

|Classe                                   |Detalhes                                         |Introduzido em|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta uma área com uma cor sólida                        |Atualização do Windows 10 de novembro do Windows (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta uma área com o conteúdo de um [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Atualização do Windows 10 de novembro do Windows (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta uma área com o conteúdo de um efeito de composição |Atualização do Windows 10 de novembro do Windows (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Pinta um visual usando um CompositionBrush com uma máscara de opacidade |Atualização de aniversário do Windows 10 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta uma área com uma CompositionBrush usando uma ampliação NineGrid |Atualização de aniversário do Windows 10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta uma área com um gradiente linear                    |Atualização para Windows 10 Fall Creators Update (SDK do Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta uma área pela amostragem dos pixels de fundo de um aplicativo ou dos pixels diretamente por trás da janela do aplicativo na área de trabalho. Usado como uma entrada para outro CompositionBrush como um CompositionEffectBrush | Atualização de Aniversário do Windows 10 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Pintar com uma cor sólida

Um [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta uma área com uma cor sólida. Existem diversas maneiras de especificar a cor de um SolidColorBrush. Por exemplo, você pode especificar os canais de alfa, vermelho, azul e verde (ARGB) ou usar uma das cores predefinidas fornecidas pela classe [Cores](https://docs.microsoft.com/uwp/api/windows.ui.colors).

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

A ilustração e o código a seguir mostram um SpriteVisual pintado com um LinearGradientBrush e duas paradas, usando uma cor de vermelha e amarela.

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

Um [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta uma área com pixels renderizados em um ICompositionSurface. Por exemplo, um CompositionSurfaceBrush pode ser usado para pintar uma área com uma imagem renderizada em uma superfície ICompositionSurface usando a API [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface).

A ilustração e o código a seguir mostram um SpriteVisual pintado com um bitmap de um licorice renderizado em um ICompositionSurface usando LoadedImageSurface. As propriedades do CompositionSurfaceBrush podem ser usadas para ampliar e alinhar o bitmap nos limites do elemento visual.

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
A [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) também pode ser usado para pintar uma área com pixels de um ICompositionSurface renderizado usando [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (ou D2D).

O código a seguir mostra um SpriteVisual pintado com uma execução de texto renderizada em um ICompositionSurface usando Win2D. Observe que para usar o Win2D, você deve incluir o pacote [NuGet Win2D](https://www.nuget.org/packages/Win2D.uwp) ao projeto.

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

Da mesma forma, o CompositionSurfaceBrush também pode ser usado para pintar um SpriteVisual com uma Cadeia de permuta usando a interoperabilidade do Win2D. [Este exemplo](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) mostra como usar o Win2D para pintar um SpriteVisual com uma cadeia de permuta.

### <a name="paint-with-a-video"></a>Pintar com um vídeo
Um [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) também pode ser usado para pintar uma área com pixels de um ICompositionSurface renderizado usando um vídeo carregado pela classe [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer).

O código a seguir mostra um SpriteVisual pintado com um vídeo carregado em um ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

Um [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta uma área com a saída de um CompositionEffect. Efeitos na Camada visual podem ser pensados como efeitos de filtro animados aplicados a uma coleção de conteúdo de origem, como cores, gradientes, imagens, vídeos, cadeias de permuta, regiões da interface do usuário ou árvores de elementos visuais. O conteúdo de origem normalmente é especificado usando outro CompositionBrush.

A ilustração e o código a seguir mostram um SpriteVisual pintado com uma imagem de um gato com o efeito de filtro de dessaturação aplicado.

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

Para obter mais informações sobre como criar um Efeito usando CompositionBrushes, consulte [Efeitos na camada visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar usando um CompositionBrush com uma máscara de opacidade aplicada

O [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta uma área com um CompositionBrush usando uma máscara de opacidade aplicada a ele. A origem da máscara de opacidade pode ser qualquer CompositionBrush do tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush ou CompositionNineGridBrush. A máscara de opacidade deve ser especificada como um CompositionSurfaceBrush.

A ilustração e o código a seguir mostram um SpriteVisual pintado com um CompositionMaskBrush. A origem da máscara é um CompositionLinearGradientBrush mascarado com a aparência de um círculo usando uma imagem do círculo como uma máscara.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar com um CompositionBrush usando a ampliação NineGrid

A [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta uma área com um CompositionBrush, que é ampliado usando a metáfora de nove grades. A metáfora de nove grades permite que você amplie bordas e cantos de um CompositionBrush de modo diferente do seu centro. A origem da ampliação de nove grades pode ser qualquer CompositionBrush do tipo CompositionColorBrush, CompositionSurfaceBrush ou CompositionEffectBrush.

O código a seguir mostram um SpriteVisual pintado com um CompositionNineGridBrush. A origem da máscara é um CompositionSurfaceBrush ampliado usando as Nove grades.

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

### <a name="paint-using-background-pixels"></a>Pintar usando Pixels de fundo

Um [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) pinta uma área com o conteúdo atrás da área. Um CompositionBackdropBrush nunca é usado por conta própria, mas em vez disso, é usado como uma entrada para outro CompositionBrush como um EffectBrush. Por exemplo, ao usar um CompositionBackdropBrush como uma entrada para um efeito de Desfoque, você pode obter um efeito de vidro fosco.

O código a seguir mostra uma pequena árvore visual para criar uma imagem usando o CompositionSurfaceBrush e uma sobreposição de vidro fosco acima da imagem. A sobreposição de vidro fosco é criada ao colocar um SpriteVisual preenchido com um EffectBrush acima da imagem. O EffectBrush usa um CompositionBackdropBrush como uma entrada para o efeito de desfoque.

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

## <a name="combining-compositionbrushes"></a>Combinar CompositionBrushes
Alguns CompositionBrushes usam outros CompositionBrushes como entradas. Por exemplo, ao usar o método SetSourceParameter é possível definir outro CompositionBrush como uma entrada para um CompositionEffectBrush. A tabela abaixo descreve as combinações compatíveis de CompositionBrushes. Observe que o uso de uma combinação sem suporte gera uma exceção.

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
<td>NÃO</td>
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
<td>NÃO</td>
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
<td>NÃO</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Usando um pincel de XAML vs. CompositionBrush

A tabela a seguir fornece uma lista dos cenários e se o uso do pincel XAML ou de Composição é recomendado ao pintar um UIElement ou um SpriteVisual no aplicativo. 

> [!NOTE]
> Se um CompositionBrush for sugerido para um UIElement XAML, considera-se que o CompositionBrush é empacotado usando um XamlCompositionBrushBase.

|Cenário                                                                   | XAML UIElement                                                                                                |SpriteVisual de composição
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar uma área com cores sólidas                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar uma área com cores animadas                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar uma área com um gradiente estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar uma área com paradas de gradiente animado                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar uma área com uma imagem                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar uma área com uma página da Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/D
|Pintar uma área com uma imagem usando a ampliação NineGrid                         |[Controle de imagem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar uma área com uma ampliação NineGrid animada                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar uma área com uma cadeia de permuta                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade de cadeia de permuta
|Pintar uma área com um vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade de mídia
|Pintar uma área com desenho 2D personalizado                                       |[CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) do Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) com interoperabilidade de Win2D
|Pintar uma área com máscara não animada                                       |Usar [formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML para definir uma máscara   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar uma área com uma máscara animada                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar uma área com um efeito de filtro animado                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar uma área com um efeito aplicado aos pixels de tela de fundo        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Tópicos relacionados

[Composição DirectX e Direct2D interoperabilidade nativa com BeginDraw e EndDraw](composition-native-interop.md)

[Interoperabilidade de pincel XAML com XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
