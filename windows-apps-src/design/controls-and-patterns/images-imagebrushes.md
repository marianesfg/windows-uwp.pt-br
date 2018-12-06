---
Description: Learn how to integrate images into your app, including how to use the APIs of the two main XAML classes, Image and ImageBrush.
title: Imagens e pincéis de imagem
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: af6dff9c0cf8aad1f9d7df7f94cc2af099a2ca1e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746060"
---
# <a name="images-and-image-brushes"></a>Imagens e pincéis de imagem

Para exibir uma imagem, você pode usar o objeto **Image** ou **ImageBrush**. Um objeto Image renderiza uma imagem e um objeto ImageBrush pinta outro objeto com uma imagem. 

> **APIs importantes**: [classe Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [propriedade Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx), [classe ImageBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx), [propriedade ImageSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)

## <a name="are-these-the-right-elements"></a>Estes são os elementos certos?
Use um elemento **Image** para exibir uma imagem independente em seu aplicativo.

Use um **ImageBrush** para aplicar uma imagem a outro objeto. Os usos de um ImageBrush incluem efeitos de texto decorativos ou planos de fundo lado a lado para contêineres de layout ou controles.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Image">abrir o aplicativo e ver o Image em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>Criar uma imagem

### <a name="image"></a>Imagem
Este exemplo mostra como criar uma imagem ao usar o objeto [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx).


```XAML
<Image Width="200" Source="sunset.jpg" />
```

Este é o objeto Image renderizado.

![Exemplo de um elemento de imagem](images/Image_Licorice.jpg)

Neste exemplo, a propriedade [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) especifica o local da imagem que você deseja exibir. Você pode definir a origem especificando uma URL absoluta (por exemplo, http://contoso.com/myPicture.jpg) ou especificando uma URL que é relativa à sua estrutura de empacotamento do aplicativo. Para nosso exemplo, colocamos o arquivo de imagem "licorice.jpg" na pasta raiz do projeto e declaramos as configurações do projeto que incluem o arquivo de imagem como conteúdo.

### <a name="imagebrush"></a>ImageBrush

Com o objeto [ImageBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx), você pode usar uma imagem para pintar uma área que inclui um objeto [Brush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx). Por exemplo, você pode usar um ImageBrush para o valor da propriedade [Fill](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) de um [Ellipse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.ellipse.aspx) ou a propriedade [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx) de um [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx).

O exemplo a seguir mostra como usar um ImageBrush para pintar um Ellipse.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Este é o Ellipse pintado pelo ImageBrush.

![Um Ellipse pintado por um ImageBrush.](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>Ampliar uma imagem

Se você não definir os valores [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) ou [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) de um **Image**, ele será exibido com as dimensões da imagem especificada por **Source**. A configuração de **Width** e de **Height** cria um área de conteúdo retangular na qual a imagem é exibida. Você pode especificar como a imagem deve preencher essa área de conteúdo usando a propriedade [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.stretch.aspx). A propriedade Stretch aceita estes valores, que a enumeração [Stretch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.stretch.aspx) define:

-   **None**: a imagem não é ampliada para preencher as dimensões de saída. Cuidado com essa configuração de Stretch: se a imagem de origem for maior que a área de conteúdo, sua imagem será recortada e, geralmente, isso não é desejável, pois você não tem controle sobre o visor, como em um [Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) deliberado.
-   **Uniform**: a imagem é redimensionada para se ajustar às dimensões de saída. Mas a proporção do conteúdo é preservada. Esse é o valor padrão.
-   **UniformToFill**: a imagem é redimensionada para que preencha completamente a área de saída, mas preserva a sua proporção original.
-   **Fill**: a imagem é redimensionada para preencher as dimensões de saída. Como a largura e a altura do conteúdo são redimensionadas de maneira independente, a proporção original da imagem pode não ser preservada. Isto é, a imagem pode ser distorcida para preencher completamente a área de saída.

![Um exemplo de configurações de ampliação.](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>Cortar uma imagem

Você pode usar a propriedade [Clip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.clip.aspx) para recortar uma área da saída da imagem. Você define a propriedade Clip como uma [Geometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.geometry.aspx). Atualmente, não há suporte para recorte não retangular.

O próximo exemplo mostra como usar uma [RectangleGeometry](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.aspx) como a região de recorte de uma imagem. Neste exemplo, definimos um objeto **Image** com uma altura de 200. Uma **RectangleGeometry** define um retângulo para a área da imagem que será exibida. A propriedade [Rect](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rectanglegeometry.rect.aspx) é definida como "25,25,100,150", que define o início de um retângulo na posição "25,25" como uma largura de 100 e uma altura de 150. Apenas a parte da imagem que está dentro da área do retângulo será exibida.

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Esta é a imagem recortada em um plano de fundo preto.

![Um objeto Image recortado por um RectangleGeometry.](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>Aplicar opacidade

Você pode aplicar [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) a uma imagem, de forma que a imagem seja renderizada semitranslúcida. Os valores de opacidade estão entre 0,0 e 1,0, em que 1,0 é totalmente opaco e 0,0 é totalmente transparente. Este exemplo mostra como aplicar uma opacidade de 0,5 a um Image.

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

Esta é a imagem renderizada com uma opacidade de 0,5 e um plano de fundo preto que mostra a opacidade parcial.

![Um objeto Image com opacidade de 0,5.](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>Formatos de arquivo de imagem

**Image** e **ImageBrush** podem exibir os seguintes formatos de arquivo de imagem:

-   JPEG
-   PNG
-   BMP
-   GIF
-   TIFF
-   JPEG XR
-   ícones (ICO)

As APIs de [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx), [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) e [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) não incluem métodos dedicados para codificar e decodificar formatos de mídia. Todas as operações de codificação e decodificação são internas e irão, no máximo, trazer à tona aspectos de codificação e decodificação como parte dos dados de eventos de carregamento. Caso deseje realizar trabalhos especiais com codificação ou decodificação de imagens (algo que você provavelmente fará se seu aplicativo realizar conversões ou manipulação de imagens), use as APIs disponíveis no namespace [Windows.Graphics.Imaging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.graphics.imaging.aspx). Essas APIs também têm suporte do componente Windows Imaging Component (WIC) no Windows.

A partir do Windows 10, versão 1607, o elemento **Image** dá suporte a imagens GIF animadas. Quando você usa um **BitmapImage** como a imagem de **Origem**, pode acessar APIs de BitmapImage para controlar a reprodução da imagem GIF animada. Para obter mais informações, consulte os Comentários sobre a página da classe [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx).

> **Observação**&nbsp;&nbsp;O suporte para GIF animado está disponível quando seu aplicativo é compilado para Windows 10, versão 1607 e executado na versão 1607 (ou posterior). Quando seu aplicativo é compilado para ou é executado em versões anteriores, o primeiro quadro do GIF é mostrado, mas não é animado.

Para obter mais informações sobre os recursos do aplicativo e como empacotar as origens de imagem em um aplicativo, consulte [Definindo recursos de aplicativos](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

### <a name="writeablebitmap"></a>WriteableBitmap

Um [WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx) fornece uma [BitmapSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.aspx) que pode ser modificada e que não usa a decodificação básica baseada em arquivo do WIC. Você pode alterar imagens dinamicamente e renderizar novamente a imagem atualizada. Para definir o conteúdo do buffer de um **WriteableBitmap**, use a propriedade [PixelBuffer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer.aspx) para acessar o buffer e use um tipo de buffer de fluxo ou específico à linguagem para preenchê-lo. Para obter um código de exemplo, consulte [WriteableBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.writeablebitmap.aspx).

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

A classe [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx) pode capturar a árvore da interface do usuário XAML de um aplicativo em execução e representar a origem de uma imagem de bitmap. Após a captura, a origem dessa imagem poderá ser aplicada a outras partes do aplicativo, salva como recurso ou dados de aplicativo pelo usuário ou utilizada em outros cenários. Um cenário especificamente útil é a criação de uma miniatura em tempo de execução de uma página XAML para um esquema de navegação, como incluir o link da imagem de um controle [Hub](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hub.aspx). **RenderTargetBitmap** tem algumas limitações sobre o conteúdo que vai aparecer na imagem capturada. Para obter mais informações, consulte o tópico de referência da API de [RenderTargetBitmap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.rendertargetbitmap.aspx).

### <a name="image-sources-and-scaling"></a>Origens de imagens e dimensionamento

Você deve criar origens de imagens em vários tamanhos recomendados para garantir que seu aplicativo tenha uma ótima aparência quando o Windows o dimensionar. Ao especificar **Source** para um **Image**, você pode usar uma convenção de nomenclatura que referencie automaticamente o recurso correto da escala atual. Para ter acesso aos detalhes da convenção de nomenclatura e saber mais, consulte [Guia de início rápido: usando recursos de arquivo ou imagem](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325).

Para obter mais informações sobre como desenvolver para dimensionamento, consulte [Diretrizes da experiência do usuário para layout e dimensionamento](https://msdn.microsoft.com/library/windows/apps/dn611863).

### <a name="image-and-imagebrush-in-code"></a>Image e ImageBrush em código

É comum especificar os elementos Image e ImageBrush usando XAML em vez de código. Isso ocorre porque esses elementos costumam ser a saída das ferramentas de design como parte de uma definição da interface do usuário do XAML.

Se você definir um Image ou ImageBrush usando código, use os construtores padrão e defina a propriedade relevante ([Image.Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.source.aspx) ou [ImageBrush.ImageSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imagesource.aspx)). As propriedades de source exigem um [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapimage.aspx) (não um URI) quando você os define usando código. Se a sua origem for um fluxo, use o método [SetSourceAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync.aspx) para inicializar o valor. Se sua origem for um URI que inclua conteúdo no seu aplicativo que usa o esquema **ms-appx** ou **ms-resource**, use o construtor [BitmapImage](https://msdn.microsoft.com/library/windows/apps/xaml/br243238.aspx) que inclui um URI. Você também pode considerar a manipulação do evento [ImageOpened](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) se houver algum problema de temporização com a recuperação ou decodificação da origem da imagem, onde você pode precisar de conteúdo alternativo para exibir até que a origem da imagem esteja disponível. Consulte um código de amostra em [Amostra de imagens XAML](http://go.microsoft.com/fwlink/p/?linkid=238575).

> [!NOTE]
> Se você definir as imagens usando código, poderá usar a manipulação automática para acessar recursos não qualificados com os qualificadores atuais de escala e cultura ou usar [ResourceManager](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemanager.aspx) e [ResourceMap](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcemap.aspx) com qualificadores de escala e cultura para obter esses recursos diretamente. Para saber mais, veja [Sistema de gerenciamento de recursos](https://msdn.microsoft.com/library/windows/apps/xaml/jj552947.aspx).

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

-   [Áudio, vídeo e câmera](https://msdn.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Classe Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx)
-   [Classe ImageBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.aspx)