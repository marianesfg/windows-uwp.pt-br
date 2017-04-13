---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "Consulta de áudio e vídeos codificadores e decodificadores instalados em um dispositivo."
title: Consulta de codecs instalados
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, codec, codificador, decodificador, consulta
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>Consulta de codecs instalados
Este artigo mostra como consultar para obter áudio e vídeo codificadores e decodificadores instalados em um dispositivo. O artigo [Codecs compatíveis](supported-codecs.md) lista os codecs que estão incluídos no sistema operacional para cada família de dispositivos. Em um dispositivo individual, o usuário ou um app pode instalar codecs adicionais. A classe [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) pode ser usada para listar o conjunto atual de codificadores e decodificadores que estão instalados no dispositivo no qual seu app é executado.

A classe **CodecQuery** está incluída no [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) namespace e fornece um construtor que não tem argumentos.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

O [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) método da classe **CodecQuery** retorna todos os codecs instalados que atendem aos parâmetros especificados. Esses parâmetros incluem um valor da enumeração [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) que especifica se deve ser retornados codecs de áudio ou vídeos e um valor [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) que especifica se os codecs de áudio ou vídeo devem ser retornados.

O parâmetro final é uma cadeia de caracteres que representa um subtipo de mídia, como vídeo H264 ou áudio FLAC, que deve ser compatível com qualquer codec retornado de **FindAllAsync**. Você pode especificar a cadeia de caracteres nula ou vazia para este parâmetro retornar codecs para todos os subtipos de mídia. O exemplo a seguir lista todos os codificadores instalados no dispositivo atual.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

Se você especificar um subtipo de mídia, você deve usar a representação de cadeia de caracteres de um subtipo GUIDs listados em [subtipo de áudio GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) ou [subtipo de vídeo GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx). A classe [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) fornece propriedades para a maioria dos subtipos de mídia compatíveis que retornam a representação de cadeia de caracteres do subtipo GUID. Você também pode especificar um código FOURCC para o parâmetro de subtipo de mídia. Para obter mais informações, consulte [Códigos FOURCC](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx). 

O exemplo a seguir usa o código FOURCC para consultar decodificadores de vídeo que dão suporte ao vídeo H.264. Um cenário de exemplo para essa operação é verificar se um formato de vídeo é compatível antes de tentar reproduzir um arquivo de vídeo.

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

As consultas de exemplo a seguir para obter codificadores de áudio compatíveis com o subtipo de mídia FLAC. Este exemplo cria um objeto AudioEncodinAudioEncodingProperties e um MediaEncodingProfile para o subtipo de mídia. Isso pode ser usado para gravar áudio em um formato específico ou áudio de transcodificação de outro formato. Para obter mais informações, consulte [Captura básica de foto, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) e [Arquivos de transcodificação de mídia](transcode-media-files.md).

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Tópicos relacionados

* [Codecs compatíveis](supported-codecs.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodificar arquivos de mídia](transcode-media-files.md)
 

 




