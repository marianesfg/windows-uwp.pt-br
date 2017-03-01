---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "Você pode usar as APIs Windows.Media.Transcoding para transcodificar arquivos de vídeo de um formato para outro."
title: "Transcodificar arquivos de mídia"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: bcf9532f65b9f0574942d1fb4dd23f5a63613ec9
ms.lasthandoff: 02/07/2017

---

# <a name="transcode-media-files"></a>Transcodificar arquivos de mídia

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Você pode usar as APIs [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar arquivos de vídeo de um formato para outro.

*Transcodificação* é a conversão de um arquivo de mídia digital como, por exemplo, um arquivo de vídeo ou de áudio, de um formato para outro. Isso costuma ser realizado por meio da decodificação e recodificação do arquivo. Por exemplo, você pode converter um arquivo do Windows Media em MP4 para que ele possa ser reproduzido em um dispositivo portátil compatível com o formato MP4. Ou você pode converter um arquivo de vídeo de alta definição em uma definição inferior. Nesse caso, o arquivo recodificado pode usar o mesmo codec que o arquivo original, mas ele teria um perfil de codificação diferente.

## <a name="set-up-your-project-for-transcoding"></a>Configurar seu projeto para transcodificação

Além dos namespaces referenciados pelo modelo de projeto padrão, você precisará fazer referência a esses namespaces para transcodificar arquivos de mídia usando o código deste artigo.

[!code-cs[Usando](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Selecionar os arquivos de origem e destino

A maneira como o aplicativo determina os arquivos de origem e destino para transcodificação depende de sua implementação. Este exemplo usa um [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) e um [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que o usuário escolha um arquivo de origem e um de destino.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Criar um perfil de codificação de mídia

O perfil de codificação contém as configurações que determinam como o arquivo de destino será codificado. É aí que você tem o maior número de opções ao transcodificar um arquivo.

A classe [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) fornece os métodos estáticos para criar perfis de codificação predefinidos:

-   Wav
-   Áudio AAC (M4A)
-   Áudio MP3
-   Áudio do Windows Media (WMA)
-   Avi
-   Vídeo MP4 (Vídeo H.264 mais áudio AAC)
-   Vídeo do Windows Media (WMV)

Os primeiros quatro perfis dessa lista contêm apenas áudio. Os outros três contêm vídeo e áudio.

O código a seguir cria um perfil para vídeo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

O método estático [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) cria um perfil de codificação MP4. O parâmetro para esse método fornece a resolução de destino do vídeo. Nesse caso, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) significa 1280 x 720 pixels a 30 quadros por segundo. ("720p" significa 720 linhas de varredura progressiva por quadro). Todos os outros métodos para a criação de perfis predefinidos seguem esse padrão.

Como alternativa, você pode criar um perfil que corresponda a um arquivo de mídia usando o método [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). Ou, se você sabe exatamente quais configurações de codificação deseja, crie um novo objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) e preencha os detalhes do perfil.

## <a name="transcode-the-file"></a>Transcodificar o arquivo

Para transcodificar o arquivo, crie um novo objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) e chame o método [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Passe o arquivo de origem, o arquivo de destino e o perfil de codificação. Em seguida, chame o método [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) no objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que foi retornado da operação de transcodificação assíncrona.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Responder ao andamento da transcodificação

Você pode registrar eventos para responder quando o andamento da [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) assíncrona mudar. Esses eventos fazem parte da estrutura de programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP) e não são específicos da API de transcodificação.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 





