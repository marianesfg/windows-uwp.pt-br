---
author: drewbatgit
ms.assetid: 25553c4d-fa4f-4130-af9b-97f993fefd43
description: "Esta seção fornece informações sobre como criar aplicativos universais do Windows que reproduzam ou áudio e vídeo."
title: "Reprodução de mídia"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 58b41c9d0d8bd7cd07b5a1f4aecf5936422a4ee8
ms.lasthandoff: 02/08/2017

---

# <a name="media-playback"></a>Reprodução de mídia

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção fornece informações sobre como criar aplicativos universais do Windows que reproduzam ou áudio e vídeo. 

## <a name="media-playback-developer-features"></a>Recursos de desenvolvedor de reprodução de mídia

A tabela a seguir lista os artigos de instruções que fornecem orientações detalhadas sobre como adicionar recursos de reprodução de mídia ao seu app.
 
| Tópico                                                                                             | Descrição                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md) | Este artigo mostra como tirar proveito dos novos recursos e aprimoramentos para o sistema de reprodução de mídia para apps UWP. A partir do Windows 10, versão 1607, a prática recomendada para reprodução de mídia é usar a classe [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) em vez de [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) para reprodução de mídia. O [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), um controle XAML leve, foi introduzido para permitir que você renderize conteúdo de mídia em uma página XAML. **MediaPlayer** fornece várias vantagens, inclusive integração automática com os controles de transporte de mídia do sistema e um modelo de um processo mais simples para áudio em segundo plano. Este artigo também mostra como renderizar vídeo para uma superfície Windows.UI.Composition e como usar um [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) para sincronizar vários players de mídia.                                                                                                          |
| [Itens de mídia, playlists e faixas](media-playback-with-mediasource.md)                         | Este artigo mostra como usar a classe [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), que fornece uma maneira comum de referenciar e reproduzir mídia de diferentes fontes, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato de mídia subjacente. A classe [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) estende a funcionalidade do **MediaSource**, permitindo que você gerencie e selecione várias faixas de áudio, vídeo e metadados contidas em um item de mídia. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite que você crie playlists a partir de um ou mais itens de reprodução de mídia.                                                                                                               |
| [Integrar aos controles de transporte de mídia do sistema](integrate-with-systemmediatransportcontrols.md)                               | Este artigo mostra como integrar seu app com os controles de transporte de mídia do sistema (SMTC). A partir do Windows 10, versão 1607, cada instância do **MediaPlayer** que você criar para reproduzir mídia será exibida automaticamente pelo SMTC. Este artigo mostra como fornecer ao SMTC os metadados sobre o conteúdo que você está reproduzindo e como ampliar ou substituir completamente o comportamento padrão dos controles SMTC.                                   |
| [Criar, programar e gerenciar pausas de mídia](create-schedule-and-manage-media-breaks.md)                                                                             | Este artigo mostra como criar, programar e gerenciar pausas de mídia para seu app de reprodução de mídia. A partir do Windows 10, versão 1607, você pode usar a classe [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) de forma rápida e fácil para adicionar quebras de mídia a qualquer [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) que você usar com um [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Quebras de mídia geralmente são usadas para inserir anúncios de áudio ou vídeos no conteúdo de mídia. Depois de programar uma ou mais interrupções de mídia, o sistema reproduzirá automaticamente seu conteúdo de mídia no horário especificado durante a reprodução. O **MediaBreakManager** fornece eventos para que seu app possa reagir quando as pausas de mídia começarem, terminarem ou quando forem ignoradas pelo usuário. Você também pode acessar uma [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) para que suas pausas de mídia monitorem eventos, como download e atualizações do andamento de buffer.                                                                                                                     |
| [Reproduzir mídia em segundo plano](background-audio.md)                                                                             | Este artigo mostra como configurar seu app para que a mídia continue a ser reproduzida quando o app for movido do primeiro para o segundo plano. Isso significa que, mesmo depois que o usuário minimizar o app, retornar à tela inicial ou navegar para fora do app de alguma outra forma, seu app poderá continuar a reproduzir o áudio. Com o Windows 10, versão 1607, um novo modelo de processo único para reprodução de mídia em segundo plano foi introduzido que é muito mais rápido e mais fácil de implementar do que o modelo herdado de dois processos. Este artigo inclui informações sobre como lidar com os novos eventos de ciclo de vida do app [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) para gerenciar o uso de memória de seu app durante a execução em segundo plano.                                                                                                                    |
| [Streaming adaptável](adaptive-streaming.md)                                                       | Este artigo descreve como adicionar a reprodução de conteúdo multimídia de streaming adaptável a apps da Plataforma Universal do Windows (UWP). Atualmente, esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP).                                          |
| [Transmissão de mídia](media-casting.md)                                                                 | Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo universal do Windows.                                                                                                                                                                                                       |
| [DRM do PlayReady](playready-client-sdk.md)                                                          | Este tópico descreve como adicionar conteúdo de mídia protegido do PlayReady ao seu app da Plataforma Universal do Windows (UWP).                                                                                                                                                                                |
| [Extensão de mídia criptografada do PlayReady](playready-encrypted-media-extension.md)                     | Esta seção descreve como modificar seu app Web PlayReady para oferecer suporte às alterações feitas na versão do Windows 8.1 anterior para a versão do Windows 10.                                                                                                                                       |

## <a name="media-playback-sdk-samples"></a>Amostras do SDK de reprodução de mídia

As amostras a seguir do SDK demonstram os recursos de reprodução de mídia disponíveis para apps UWP no Windows 10. Use essas amostras para ver as APIs de reprodução de mídia usadas no contexto ou como um ponto de partida para seu próprio app.

* [Amostra de streaming adaptável](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)
* [Amostra de áudio em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)
* [Exemplo de transporte de mídia do sistema](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)                                                                                               
 





