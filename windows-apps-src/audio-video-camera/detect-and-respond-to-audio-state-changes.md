---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: Este artigo explica como os aplicativos UWP podem detectar e responder a alterações iniciadas pelo sistema em níveis de fluxo de áudio
title: Detectar e responder a alterações de estado de áudio
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a680347e9d1a749cc6e1d86ef1f02da280b4b74
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361776"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Detectar e responder a alterações de estado de áudio
A partir do Windows 10, versão 1803, seu aplicativo pode detectar quando o sistema reduz o nível ou silencia o nível de áudio de um stream de áudio usado pelo aplicativo. Você pode receber notificações de captura e renderizar fluxos para um dispositivo de áudio e categoria de áudio específico ou para um objeto [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) que o aplicativo está usando para reprodução de mídia. Por exemplo, o sistema pode reduzir ou "ignorar", o nível de reprodução de áudio quando um alarme está tocando. O sistema ativa o mudo do aplicativo quando entra em segundo plano caso não tenha declarado a funcionalidade *backgroundMediaPlayback* no manifesto. 

O padrão para processar alterações de estado de áudio é o mesmo para todos os fluxos de áudio com suporte. Primeiro, crie uma instância da classe [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor). No exemplo a seguir, o aplicativo está usando a classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para capturar o áudio para chat de jogo. Um método de fábrica é chamado para obter um monitoramento de estado do áudio associado ao fluxo de captura de áudio de chat do jogo do dispositivo de comunicação padrão.  Em seguida, um manipulador é registrado para o evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged), que pode ser acionado quando o nível de áudio do stream associado é alterado pelo sistema.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

No **SoundLevelChanged** manipulador de eventos, verifique os [ **SoundLevel** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) propriedade do **AudioStateMonitor** remetente passado para o manipulador para determinar o que é o novo nível de áudio para o fluxo. Neste exemplo, o aplicativo para de capturar áudio quando o nível de som é desligado e retoma a captura quando o nível de áudio retorna ao volume completo.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Para obter mais informações sobre a captura de áudio com **MediaCapture**, consulte [Captura de foto, vídeo e áudio básica com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Cada instância da classe [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) tem um **AudioStateMonitor** associado para uso na detecção de alterações de nível de volume do conteúdo reproduzido no momento pelo sistema. Você pode optar por manipular alterações de estado de áudio de forma diferente dependendo do tipo de conteúdo reproduzido. Por exemplo, você pode decidir pausar a reprodução de um podcast quando o áudio é abaixado, mas continuar a reprodução se o conteúdo for música. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Para obter mais informações sobre como usar o **MediaPlayer**, consulte [Reproduzir áudio e vídeo com MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Tópicos relacionados