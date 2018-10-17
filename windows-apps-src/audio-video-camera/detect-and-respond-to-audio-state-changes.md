---
author: drewbatgit
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: Este artigo explica como os aplicativos UWP podem detectar e responder a alterações iniciadas pelo sistema em níveis de fluxo de áudio
title: Detectar e responder a alterações de estado de áudio
ms.author: drewbat
ms.date: 04/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c60fcd705acf2d0d1e3162e80bc1d85095aa0fb4
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4694248"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Detectar e responder a alterações de estado de áudio
A partir do Windows 10, versão 1803, seu aplicativo pode detectar quando o sistema reduz o nível ou silencia o nível de áudio de um stream de áudio usado pelo aplicativo. Você pode receber notificações de captura e renderizar fluxos para um dispositivo de áudio e categoria de áudio específico ou para um objeto [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) que o aplicativo está usando para reprodução de mídia. Por exemplo, o sistema pode reduzir ou "ignorar", o nível de reprodução de áudio quando um alarme está tocando. O sistema ativa o mudo do aplicativo quando entra em segundo plano caso não tenha declarado a funcionalidade *backgroundMediaPlayback* no manifesto. 

O padrão para processar alterações de estado de áudio é o mesmo para todos os fluxos de áudio com suporte. Primeiro, crie uma instância da classe [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor). No exemplo a seguir, o aplicativo está usando a classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para capturar o áudio para chat de jogo. Um método de fábrica é chamado para obter um monitoramento de estado do áudio associado ao fluxo de captura de áudio de chat do jogo do dispositivo de comunicação padrão.  Em seguida, um manipulador é registrado para o evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged), que pode ser acionado quando o nível de áudio do stream associado é alterado pelo sistema.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

No manipulador de eventos **SoundLevelChanged** , verifique a propriedade [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) do remetente **AudioStateMonitor** passado para o manipulador para determinar qual é o novo nível de fluxo de áudio. Neste exemplo, o aplicativo para de capturar áudio quando o nível de som é desligado e retoma a captura quando o nível de áudio retorna ao volume completo.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Para obter mais informações sobre a captura de áudio com **MediaCapture**, consulte [Captura de foto, vídeo e áudio básica com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Cada instância da classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) tem um **AudioStateMonitor** associado para uso na detecção de alterações de nível de volume do conteúdo reproduzido no momento pelo sistema. Você pode optar por manipular alterações de estado de áudio de forma diferente dependendo do tipo de conteúdo reproduzido. Por exemplo, você pode decidir pausar a reprodução de um podcast quando o áudio é abaixado, mas continuar a reprodução se o conteúdo for música. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Para obter mais informações sobre como usar o **MediaPlayer**, consulte [Reproduzir áudio e vídeo com MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Tópicos relacionados