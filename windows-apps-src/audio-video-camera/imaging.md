---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: Este artigo explica como carregar e salvar arquivos de imagem usando BitmapDecoder e BitmapEncoder e como usar o objeto SoftwareBitmap para representar imagens de bitmap.
title: Criar, editar e salvar imagens de bitmap
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c25fc09d606c0f143f357dd7f89026fa94b80922
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318353"
---
# <a name="create-edit-and-save-bitmap-images"></a>Criar, editar e salvar imagens de bitmap



Este artigo explica como carregar e salvar arquivos de imagem usando [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) e [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) e como usar o objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) para representar imagens de bitmap.

A classe **SoftwareBitmap** é uma API versátil que pode ser criada a partir de várias origens incluindo arquivos de imagem, objetos [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), superfícies do Direct3D e código. **O SoftwareBitmap** permite que você converta facilmente entre modos alfa e formatos de pixel diferentes e permite acesso de baixo nível a dados de pixel. Além disso, o **SoftwareBitmap** é uma interface comum usada por vários recursos do Windows, incluindo:

-   [**CapturedFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrame) permite que você obtenha os quadros capturados pela câmera como uma **SoftwareBitmap**.

-   [**VideoFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) permite que você obtenha um **SoftwareBitmap** representação de um **VideoFrame**.

-   [**FaceDetector** ](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) permite que você detecte rostos em uma **SoftwareBitmap**.

O código de exemplo neste artigo usa APIs dos namespaces a seguir.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Criar um SoftwareBitmap de um arquivo de imagem com BitmapDecoder

Para criar um [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de um arquivo, obtenha uma instância de [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contenha os dados da imagem. Este exemplo usa um [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir que o usuário selecione um arquivo de imagem.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Chame o método [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) do objeto **StorageFile** para obter um fluxo de acesso aleatório que contém os dados da imagem. Chame o método estático [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obter uma instância da classe [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para o fluxo especificado. Chame [**GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obter um objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que contém a imagem.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Salvar um SoftwareBitmap em um arquivo com BitmapEncoder

Para salvar um **SoftwareBitmap** em um arquivo, obtenha uma instância de **StorageFile** na qual a imagem será salva. Este exemplo usa um [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir que o usuário selecione um arquivo de saída.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

Chame o método [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) do objeto **StorageFile** para obter um fluxo de acesso aleatório no qual a imagem será gravada. Chame o método estático [**BitmapEncoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) para obter uma instância da classe [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) para o fluxo especificado. O primeiro parâmetro para **CreateAsync** é um GUID que representa o codec que deve ser usado para codificar a imagem. A classe **BitmapEncoder** expõe uma propriedade que contém a ID de cada codec compatível com o codificador, como [**JpegEncoderId**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid).

Use o método [**SetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) para definir a imagem que será codificada. Você pode definir valores da propriedade [**BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform) para aplicar transformações básicas à imagem enquanto ela está sendo codificada. A propriedade [**IsThumbnailGenerated**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) determina se uma miniatura é gerada pelo codificador. Observe que nem todos os formatos de arquivo suportam miniaturas. Portanto, se você usar esse recurso, deverá capturar o erro de operação não suportada que será gerado se não houver suporte para miniaturas.

Chame [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) para fazer com que o codificador grave os dados de imagem no arquivo especificado.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Você pode especificar opções de codificação adicionais ao criar o **BitmapEncoder**, criando um novo objeto [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) e populando-o com um ou mais objetos [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) que representam as configurações de codificador. Para obter uma lista de opções de codificador com suporte, consulte [Referência de opções de BitmapEncoder](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Usar SoftwareBitmap com um controle de imagem XAML

Para exibir uma imagem em uma página XAML usando o controle [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), defina primeiro um controle **Image** em sua página XAML.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Atualmente, o controle **Image** só dá suporte a imagens que usam a codificação BGRA8 e canal alfa pré-multiplicado ou nenhum. Antes de tentar exibir uma imagem, teste para verificar se ele tem o formato correto e, se não tiver, use o método [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert) estático de **SoftwareBitmap** para converter a imagem no formato com suporte.

Crie um novo objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource). Defina o conteúdo dos objetos de origem chamando [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) e passando um **SoftwareBitmap**. Em seguida, você pode definir a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) do controle **Image** para o **SoftwareBitmapSource** recém-criado.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

Você também pode usar **SoftwareBitmapSource** para definir um **SoftwareBitmap** como o [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) para um [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush).

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Criar um SoftwareBitmap de um WriteableBitmap

Você pode criar um **SoftwareBitmap** de um **WriteableBitmap** existente chamando [**SoftwareBitmap.CreateCopyFromBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) e fornecendo a propriedade **PixelBuffer** do **WriteableBitmap** para definir os dados de pixel. O segundo argumento permite que você solicite um formato de pixel para o **WriteableBitmap** recém-criado. Você pode usar as propriedades [**PixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) e [**PixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) do **WriteableBitmap** para especificar as dimensões da nova imagem.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Criar ou editar um SoftwareBitmap programaticamente

Até agora este tópico mostrou como trabalhar com arquivos de imagem. Você também pode criar um novo **SoftwareBitmap** programaticamente no código e usar a mesma técnica para acessar e modificar os dados de pixel do **SoftwareBitmap**.

O **SoftwareBitmap** usa interoperabilidade COM para expor o buffer bruto que contém os dados de pixel.

Para usar interoperabilidade COM, você deve incluir uma referência ao namespace **System.Runtime.InteropServices** em seu projeto.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Inicialize a interface COM [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions/mt297505(v=vs.85)) adicionando o seguinte código em seu namespace.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Crie um novo **SoftwareBitmap** com o formato de pixel e o tamanho desejados. Ou use um **SoftwareBitmap** existente do qual você deseja editar os dados de pixel. Chame [**SoftwareBitmap.LockBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) para obter uma instância da classe [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) que representa o buffer de dados de pixel. Converta **BitmapBuffer** em **IMemoryBufferByteAccess** na interface COM e chame [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) para popular uma matriz de bytes com os dados. Use o método [**BitmapBuffer.GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) para obter um objeto [**BitmapPlaneDescription**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) que ajudará você a calcular o deslocamento para o buffer de cada pixel.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Como esse método acessa o buffer bruto subjacente aos tipos do Windows Runtime, ele deve ser declarado usando a palavra-chave **unsafe**. Você também deve configurar seu projeto no Microsoft Visual Studio para permitir a compilação de código não seguro. Para fazer isso, abra a página **Propriedades** do projeto, clique na página de propriedade **Build** e selecione a caixa de seleção **Permitir Código Não Seguro**.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Criar um SoftwareBitmap de uma superfície de Direct3D

Para criar um objeto **SoftwareBitmap** de uma superfície de Direct3D, você deve incluir o namespace [**Windows.Graphics.DirectX.Direct3D11**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11) em seu projeto.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Chame [**CreateCopyFromSurfaceAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) para criar um novo **SoftwareBitmap** da superfície. Como o nome indica, o novo **SoftwareBitmap** tem uma cópia separada dos dados de imagem. As modificações no **SoftwareBitmap** não terão nenhum efeito na superfície do Direct3D.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Converter um SoftwareBitmap em um formato de pixel diferente

A classe **SoftwareBitmap** fornece o método estático [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.convert) que permite criar facilmente um novo **SoftwareBitmap** que usa o formato de pixel e o modo alfa especificados de um **SoftwareBitmap** existente. Observe que o bitmap recém-criado tem uma cópia separada dos dados de imagem. As modificações no novo bitmap não afetarão o bitmap de origem.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcodificar um arquivo de imagem

Você pode transcodificar um arquivo de imagem diretamente de um [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para um [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Crie um [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) do arquivo a ser transcodificado. Crie um novo **BitmapDecoder** do fluxo de entrada. Crie um novo [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) para o codificador a ser gravado e chame [**BitmapEncoder.CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) passando o fluxo de memória e o objeto de decodificador. As opções de codificação não são compatíveis na transcodificação; em vez disso, você deve usar [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Todas as propriedades de arquivo de imagem de entrada que não são definidas especificamente no codificador são gravadas no arquivo de saída inalteradas. Chame [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) para fazer com que o codificador codifique o fluxo de memória. Por fim, procure o fluxo do arquivo e o fluxo de memória no início e chame [**CopyAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.randomaccessstream.copyasync) para gravar o fluxo de memória no fluxo de arquivo.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Tópicos relacionados

* [Referência de opções BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadados de imagem](image-metadata.md)
 

 




