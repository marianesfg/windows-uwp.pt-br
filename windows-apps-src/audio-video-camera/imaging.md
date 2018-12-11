---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: Este artigo explica como carregar e salvar arquivos de imagem usando BitmapDecoder e BitmapEncoder e como usar o objeto SoftwareBitmap para representar imagens de bitmap.
title: Criar, editar e salvar imagens de bitmap
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 287079bf7195ebcadc3543d9369a0567f197b10c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8923615"
---
# <a name="create-edit-and-save-bitmap-images"></a>Criar, editar e salvar imagens de bitmap



Este artigo explica como carregar e salvar arquivos de imagem usando [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) e [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) e como usar o objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) para representar imagens de bitmap.

A classe **SoftwareBitmap** é uma API versátil que pode ser criada a partir de várias origens incluindo arquivos de imagem, objetos [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/br243259), superfícies do Direct3D e código. **SoftwareBitmap** permite que você converta facilmente entre modos alfa e formatos de pixel diferentes e permite acesso de baixo nível a dados de pixel. Além disso, o **SoftwareBitmap** é uma interface comum usada por vários recursos do Windows, incluindo:

-   [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/dn278725) permite que você obtenha os quadros capturados pela câmera como um **SoftwareBitmap**.

-   [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) permite que você obtenha uma representação **SoftwareBitmap** de um **VideoFrame**.

-   [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) permite que você detecte rostos em um **SoftwareBitmap**.

O código de exemplo neste artigo usa APIs dos namespaces a seguir.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Criar um SoftwareBitmap de um arquivo de imagem com BitmapDecoder

Para criar um [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) de um arquivo, obtenha uma instância de [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que contenha os dados da imagem. Este exemplo usa um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para permitir que o usuário selecione um arquivo de imagem.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Chame o método [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) do objeto **StorageFile** para obter um fluxo de acesso aleatório que contém os dados da imagem. Chame o método estático [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) para obter uma instância da classe [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) para o fluxo especificado. Chame [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332) para obter um objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que contém a imagem.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Salvar um SoftwareBitmap em um arquivo com BitmapEncoder

Para salvar um **SoftwareBitmap** em um arquivo, obtenha uma instância de **StorageFile** na qual a imagem será salva. Este exemplo usa um [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que o usuário selecione um arquivo de saída.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

Chame o método [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) do objeto **StorageFile** para obter um fluxo de acesso aleatório no qual a imagem será gravada. Chame o método estático [**BitmapEncoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226211) para obter uma instância da classe [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) para o fluxo especificado. O primeiro parâmetro para **CreateAsync** é um GUID que representa o codec que deve ser usado para codificar a imagem. A classe **BitmapEncoder** expõe uma propriedade que contém a ID de cada codec compatível com o codificador, como [**JpegEncoderId**](https://msdn.microsoft.com/library/windows/apps/br226226).

Use o método [**SetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887337) para definir a imagem que será codificada. Você pode definir valores da propriedade [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) para aplicar transformações básicas à imagem enquanto ela está sendo codificada. A propriedade [**IsThumbnailGenerated**](https://msdn.microsoft.com/library/windows/apps/br226225) determina se uma miniatura é gerada pelo codificador. Observe que nem todos os formatos de arquivo suportam miniaturas. Portanto, se você usar esse recurso, deverá capturar o erro de operação não suportada que será gerado se não houver suporte para miniaturas.

Chame [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216) para fazer com que o codificador grave os dados de imagem no arquivo especificado.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Você pode especificar opções de codificação adicionais ao criar o **BitmapEncoder**, criando um novo objeto [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) e populando-o com um ou mais objetos [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) que representam as configurações de codificador. Para obter uma lista de opções de codificador com suporte, consulte [Referência de opções de BitmapEncoder](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Usar SoftwareBitmap com um controle de imagem XAML

Para exibir uma imagem em uma página XAML usando o controle [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752), defina primeiro um controle **Image** em sua página XAML.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Atualmente, o controle **Image** só dá suporte a imagens que usam a codificação BGRA8 e canal alfa pré-multiplicado ou nenhum. Antes de tentar exibir uma imagem, teste para verificar se ele tem o formato correto e, se não tiver, use o método [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) estático de **SoftwareBitmap** para converter a imagem no formato com suporte.

Crie um novo objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854). Defina o conteúdo dos objetos de origem chamando [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) e passando um **SoftwareBitmap**. Em seguida, você pode definir a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) do controle **Image** para o **SoftwareBitmapSource** recém-criado.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

Você também pode usar **SoftwareBitmapSource** para definir um **SoftwareBitmap** como o [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210105) para um [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101).

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Criar um SoftwareBitmap de um WriteableBitmap

Você pode criar um **SoftwareBitmap** de um **WriteableBitmap** existente chamando [**SoftwareBitmap.CreateCopyFromBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887370) e fornecendo a propriedade **PixelBuffer** do **WriteableBitmap** para definir os dados de pixel. O segundo argumento permite que você solicite um formato de pixel para o **WriteableBitmap** recém-criado. Você pode usar as propriedades [**PixelWidth**](https://msdn.microsoft.com/library/windows/apps/br243253) e [**PixelHeight**](https://msdn.microsoft.com/library/windows/apps/br243251) do **WriteableBitmap** para especificar as dimensões da nova imagem.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Criar ou editar um SoftwareBitmap programaticamente

Até agora este tópico mostrou como trabalhar com arquivos de imagem. Você também pode criar um novo **SoftwareBitmap** programaticamente no código e usar a mesma técnica para acessar e modificar os dados de pixel do **SoftwareBitmap**.

**SoftwareBitmap** usa interoperabilidade COM para expor o buffer bruto que contém os dados de pixel.

Para usar interoperabilidade COM, você deve incluir uma referência ao namespace **System.Runtime.InteropServices** em seu projeto.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Inicialize a interface COM [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) adicionando o seguinte código em seu namespace.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Crie um novo **SoftwareBitmap** com o formato de pixel e o tamanho desejados. Ou use um **SoftwareBitmap** existente do qual você deseja editar os dados de pixel. Chame [**SoftwareBitmap.LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887380) para obter uma instância da classe [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) que representa o buffer de dados de pixel. Converta **BitmapBuffer** em **IMemoryBufferByteAccess** na interface COM e chame [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506) para popular uma matriz de bytes com os dados. Use o método [**BitmapBuffer.GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330) para obter um objeto [**BitmapPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887342) que ajudará você a calcular o deslocamento para o buffer de cada pixel.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Como esse método acessa o buffer bruto subjacente aos tipos do Windows Runtime, ele deve ser declarado usando a palavra-chave **unsafe**. Você também deve configurar seu projeto no Microsoft Visual Studio para permitir a compilação de código não seguro. Para fazer isso, abra a página **Propriedades** do projeto, clique na página de propriedade **Build** e selecione a caixa de seleção **Permitir Código Não Seguro**.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Criar um SoftwareBitmap de uma superfície de Direct3D

Para criar um objeto **SoftwareBitmap** de uma superfície de Direct3D, você deve incluir o namespace [**Windows.Graphics.DirectX.Direct3D11**](https://msdn.microsoft.com/library/windows/apps/dn895104) em seu projeto.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Chame [**CreateCopyFromSurfaceAsync**](https://msdn.microsoft.com/library/windows/apps/dn887373) para criar um novo **SoftwareBitmap** da superfície. Como o nome indica, o novo **SoftwareBitmap** tem uma cópia separada dos dados de imagem. As modificações no **SoftwareBitmap** não terão nenhum efeito na superfície do Direct3D.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Converter um SoftwareBitmap em um formato de pixel diferente

A classe **SoftwareBitmap** fornece o método estático [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) que permite criar facilmente um novo **SoftwareBitmap** que usa o formato de pixel e o modo alfa especificados de um **SoftwareBitmap** existente. Observe que o bitmap recém-criado tem uma cópia separada dos dados de imagem. As modificações no novo bitmap não afetarão o bitmap de origem.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcodificar um arquivo de imagem

Você pode transcodificar um arquivo de imagem diretamente de um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) para um [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Crie um [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) do arquivo a ser transcodificado. Crie um novo **BitmapDecoder** do fluxo de entrada. Crie um novo [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) para o codificador a ser gravado e chame [**BitmapEncoder.CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214) passando o fluxo de memória e o objeto de decodificador. As opções de codificação não são compatíveis na transcodificação; em vez disso, você deve usar [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Todas as propriedades de arquivo de imagem de entrada que não são definidas especificamente no codificador são gravadas no arquivo de saída inalteradas. Chame [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216) para fazer com que o codificador codifique o fluxo de memória. Por fim, procure o fluxo do arquivo e o fluxo de memória no início e chame [**CopyAsync**](https://msdn.microsoft.com/library/windows/apps/hh701827) para gravar o fluxo de memória no fluxo de arquivo.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Tópicos relacionados

* [Referência de opções de BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadados de imagem](image-metadata.md)
 

 




