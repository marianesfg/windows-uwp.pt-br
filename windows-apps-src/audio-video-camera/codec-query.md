---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Consulta de áudio e vídeos codificadores e decodificadores instalados em um dispositivo.
title: Consulta de codecs instalados
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, codec, codificador, decodificador, consulta
ms.localizationpriority: medium
ms.openlocfilehash: 4241aad5a01617d6a002c6f5d6da0a4bb1455616
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8890326"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Consulta de codecs instalados em um dispositivo
A classe **[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)** permite que você consulte os codecs instalados no dispositivo atual. A lista de codecs incluídos no Windows 10 para diferentes famílias de dispositivos é mostrada no artigo [Codecs compatíveis](supported-codecs.md), mas já que os usuários e os aplicativos podem instalar codecs adicionais em um dispositivo, talvez você queira consultar o suporte ao codec em tempo de execução para determinar quais codecs estão disponíveis no dispositivo atual.

A API CodecQuery é um membro do namespace **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** e, portanto, você precisará incluir esse namespace em seu aplicativo.

A API CodecQuery é um membro do namespace **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** e, portanto, você precisará incluir esse namespace em seu aplicativo.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Inicialize uma nova instância da classe **CodecQuery** ao chamar o construtor.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

O método **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)** retorna todos os codecs instalados que correspondem aos parâmetros fornecidos. Esses parâmetros incluem um valor **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)** que especifica se você está consultando codecs de áudio ou de vídeos ou ambos, um valor **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)** que especifica se você está consultando codificadores ou decodificadores e uma cadeia de caracteres que representa o subtipo de codificação de mídia que você está consultando, como vídeo H.264 ou áudio MP3.

Especifique uma cadeia de caracteres vazia ou nula para o valor de subtipo retornar codecs para todos os subtipos. O exemplo a seguir lista todos os codificadores de vídeo instalados no dispositivo.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

A cadeia de caracteres de subtipo passada para **FindAllAsync** pode ser uma representação de cadeia de caracteres do GUID de subtipo, que é definido pelo sistema, ou um código FOURCC para o subtipo. O conjunto de GUIDs de subtitpo de mídia com suporte é mostrado nos artigos [GUIDs de subtipo de áudio](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) e [GUIDs de subtipo de vídeo](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx), mas a classe **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)** fornece propriedades que retornam os valores de GUID para cada tipo compatível. Para saber mais sobre códigos FOURCC, veja [Códigos FOURCC](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx) 

O exemplo a seguir especifica o código FOURCC "H264" para determinar se há um decodificador de vídeo H.264 instalado no dispositivo. Você também pode executar essa consulta antes de tentar reproduzir conteúdo de vídeo H.264. Você também pode manipular codecs sem suporte ao tempo de reprodução. Para saber mais, veja [Manipular erros desconhecidos e de codecs sem suporte ao abrir itens de mídia](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

O exemplo a seguir consulta para determinar se um codificador de áudio FLAC está instalado no dispositivo atual e, nesse caso, um **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** é criado para o subtipo que pode ser usado para capturar áudio de um arquivo ou para transcodificar áudio de outro formato para um arquivo de áudio FLAC.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Tópicos relacionados

* [Reprodução de mídia](media-playback.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodificar arquivos de mídia](transcode-media-files.md)
* [Codecs compatíveis](supported-codecs.md)
 

 




