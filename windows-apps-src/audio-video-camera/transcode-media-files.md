---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Você pode usar as APIs Windows.Media.Transcoding para transcodificar arquivos de vídeo de um formato para outro.
title: Transcodificar arquivos de mídia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a6eb19ca5954b3ce71ecbaefe3339bee78f8717
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933514"
---
# <a name="transcode-media-files"></a>Transcodificar arquivos de mídia



Você pode usar as APIs [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar arquivos de vídeo de um formato para outro.

*Transcodificação* é a conversão de um arquivo de mídia digital como, por exemplo, um arquivo de vídeo ou de áudio, de um formato para outro. Isso costuma ser realizado por meio da decodificação e recodificação do arquivo. Por exemplo, você pode converter um arquivo do Windows Media em MP4 para que ele possa ser reproduzido em um dispositivo portátil compatível com o formato MP4. Ou você pode converter um arquivo de vídeo de alta definição em uma definição inferior. Nesse caso, o arquivo recodificado pode usar o mesmo codec que o arquivo original, mas ele teria um perfil de codificação diferente.

## <a name="set-up-your-project-for-transcoding"></a>Configurar seu projeto para transcodificação

Além dos namespaces referenciados pelo modelo de projeto padrão, você precisará fazer referência a esses namespaces para transcodificar arquivos de mídia usando o código deste artigo.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Selecionar os arquivos de origem e destino

A maneira como o aplicativo determina os arquivos de origem e destino para transcodificação depende de sua implementação. Este exemplo usa um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e um [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que o usuário escolha um arquivo de origem e um de destino.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Criar um perfil de codificação de mídia

O perfil de codificação contém as configurações que determinam como o arquivo de destino será codificado. É aí que você tem o maior número de opções ao transcodificar um arquivo.

A classe [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) fornece métodos estáticos para criar perfis de codificação predefinidos:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Métodos de criação de perfis de codificação apenas áudio

Método  |Perfil  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Áudio Apple Lossless Audio Codec (ALAC)         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Áudio Free Lossless Audio Codec (FLAC).         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |Áudio AAC (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |Áudio MP3         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |Áudio WAV         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Áudio do Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Métodos de criação de perfis de codificação áudio/vídeo

Método  |Perfil  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |Vídeo High Efficiency Video Coding (HEVC), também conhecido como vídeo H.265 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |Vídeo MP4 (Vídeo H.264 mais áudio AAC) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Vídeo do Windows Media (WMV) |


O código a seguir cria um perfil para vídeo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

O método estático [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) cria um perfil de codificação MP4. O parâmetro para esse método fornece a resolução de destino do vídeo. Nesse caso, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) significa 1280 x 720 pixels a 30 quadros por segundo. ("720p" significa 720 linhas de varredura progressiva por quadro). Todos os outros métodos para a criação de perfis predefinidos seguem esse padrão.

Como alternativa, você pode criar um perfil que corresponda a um arquivo de mídia usando o método [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). Ou, se você sabe exatamente quais configurações de codificação deseja, crie um novo objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) e preencha os detalhes do perfil.

## <a name="transcode-the-file"></a>Transcodificar o arquivo

Para transcodificar o arquivo, crie um novo objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) e chame o método [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Passe o arquivo de origem, o arquivo de destino e o perfil de codificação. Em seguida, chame o método [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) no objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que foi retornado da operação de transcodificação assíncrona.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Responder ao andamento da transcodificação

Você pode registrar eventos para responder quando o andamento da [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) assíncrona mudar. Esses eventos fazem parte da estrutura de programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP) e não são específicos da API de transcodificação.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>Codificar um fluxo de metadados
A partir do Windows 10, versão 1803, você pode incluir metadados programados quando os arquivos de mídia de transcodificação. Ao contrário de exemplos de transcodificação de vídeo acima, que use a mídia integrada codificação métodos de criação de perfil, como [**MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), você deve criar manualmente o perfil de codificação de metadados para suportar o tipo de metadados que está sendo codificado .

Essa primeira etapa na criação de um perfil de incoding de metadados é criar um objeto [**TimedMetadataEncodingProperties**] que descreve a codificação dos metadados a ser transcodificado. A propriedade de subtipo é um GUID que especifica o tipo de metadados. Os detalhes de codificação para cada tipo de metadados é proprietários e não for fornecido pelo Windows. Neste exemplo, o GUID de metadados da GoPro (gprs) é usado. Em seguida, [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) é chamado para definir um blob binário de dados que descrevem o formato de fluxo é específico para o formato de metadados. Em seguida, um **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) é criado a partir das propriedades de codificação, e são um rótulo de faixa e um nome permitir que um aplicativo de leitura do fluxo de endcoded para identificar o fluxo de metadados e, opcionalmente, exibir o nome do fluxo da interface do usuário. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

Depois de criar o **TimedMetadataStreamDescriptor**, você pode criar um **MediaEncodingProfile** que descreve o vídeo, áudio e metadados para ser codificados no arquivo. **TimedMetadataStreamDescriptor** criado no último exemplo é passado para essa função auxiliar de exemplo e é adicionado ao **MediaEncodingProfile** chamando [**SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks).

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




