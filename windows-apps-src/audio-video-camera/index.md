---
author: drewbatgit
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: "Esta seção fornece informações sobre como criar um aplicativo Universal do Windows que capture, reproduza ou edite fotos, vídeos ou áudio."
title: "Áudio, vídeo e câmera"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8480eb181b6af26eae5950a1f1bf040d0cb5db99

---

# Áudio, vídeo e câmera

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção fornece informações sobre como criar um aplicativo Universal do Windows que capture, reproduza ou edite fotos, vídeos ou áudio.
 
| Tópico                                                                                             | Descrição                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturar fotos e vídeos com CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) | Este artigo descreve como usar a classe [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows                                                                                                            |
| [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)       | Este artigo descreve as etapas para capturar fotos e vídeo usando a API do [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124) incluindo como inicializar e desligar o MediaCapture e como lidar com as mudanças na orientação do dispositivo.                                  |
| [Detectar rostos em imagens ou vídeos](detect-and-track-faces-in-an-image.md)                         | Este tópico mostra como usar o [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150) que é otimizado para acompanhamento facial ao longo do tempo em uma sequência de quadros de vídeo.                                                                                                               |
| [Composições e edição de mídia](media-compositions-and-editing.md)                               | Este artigo mostra como usar as APIs no namespace [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) para desenvolver rapidamente aplicativos que permitem que os usuários criem composições de mídia de arquivos de origem de áudio e vídeo.                                    |
                                                                                                                                        | [Efeitos de vídeo personalizados](custom-video-effects.md)                               | Este artigo descreve como criar um Componente do Tempo de Execução do Windows que implemente a interface IBasicVideoEffect para permitir que você crie efeitos personalizados para fluxos de vídeo.                                                                                                                                |
| [Imagens](imaging.md)                                                                             | Este artigo explica como carregar e salvar arquivos de imagem usando o objeto [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358) para representar imagens de bitmap.                                                                                                                     |
| [Propriedades de informações do dispositivo de áudio](audio-device-information-properties.md)                                                                             | Este artigo lista as propriedades de informações de dispositivo relacionadas a dispositivos de áudio.                                                                                                                      |
| [Transcodificar arquivos de mídia](transcode-media-files.md)                                                 | Você pode usar as APIs [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar arquivos de vídeo de um formato para outro.                                                                                                                                |
| [Processar arquivos de mídia em segundo plano](process-media-files-in-the-background.md)                 | Este artigo mostra como usar o [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) e uma tarefa em segundo plano para processar arquivos de mídia em segundo plano.                                                                                             |
| [Reprodução de mídia com o MediaSource](media-playback-with-mediasource.md)                             | A classe [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905) fornece uma maneira comum de fazer referência e reproduzir mídia de diferentes origens, como arquivos locais ou remotos, e expõe um modelo comum para acessar dados de mídia, independentemente do formato da mídia subjacente.  |
| [Streaming adaptável](adaptive-streaming.md)                                                       | Este artigo descreve como adicionar reprodução de conteúdo de multimídia de streaming adaptativo a aplicativos UWP (Plataforma Universal do Windows). Atualmente, esse recurso oferece suporte para reprodução de conteúdo Http Live Streaming (HLS) e Dynamic Streaming over HTTP (DASH).                                          |
| [Áudio em segundo plano](background-audio.md)                                                           | Este artigo descreve como criar aplicativos UWP que reproduzem áudio em segundo plano.                                                                                                                                                                                                               |
| [Controles de transporte de mídia do sistema](system-media-transport-controls.md)                             | A classe [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677) permite que seu aplicativo use os controles de transporte de mídia do sistema que estão integrados ao Windows e atualize os metadados que os controles exibem sobre a mídia que seu aplicativo está reproduzindo atualmente. |
| [Transmissão de mídia](media-casting.md)                                                                 | Este artigo mostra como converter mídia em dispositivos remotos de um aplicativo Universal do Windows.                                                                                                                                                                                                       |
| [Gráficos de áudio](audio-graphs.md)                                                                   | Este artigo mostra como usar as APIs no namespace [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341) para criar gráficos de áudio para cenários de roteamento, mixagem e processamento de áudio.                                                                            |
| [MIDI](midi.md)                                                                                   | Este artigo mostra como enumerar dispositivos MIDI (Interface Digital de Instrumento Musical) e enviar e receber mensagens MIDI de um aplicativo Universal do Windows.                                                                                                                                   |
| [Lanterna independente da câmera](camera-independent-flashlight.md)                                 | Este artigo mostra como acessar e usar a lâmpada do dispositivo, se houver. A funcionalidade da lâmpada é gerenciada separadamente da funcionalidade da câmera e do flash da câmera do dispositivo.                                                                                                                 |
| [Codecs compatíveis](supported-codecs.md)                                                           | Este artigo lista os codecs de áudio e de vídeo e os formatos compatíveis com os aplicativos UWP.                                                                                                                                                                                                                  |
| [DRM do PlayReady](playready-client-sdk.md)                                                          | Este tópico descreve como adicionar conteúdo de mídia protegido pelo PlayReady a seu aplicativo UWP (Plataforma Universal do Windows).                                                                                                                                                                                |
| [Extensão de mídia criptografada do PlayReady](playready-encrypted-media-extension.md)                     | Esta seção descreve como modificar seu aplicativo Web PlayReady para oferecer suporte às alterações feitas na versão anterior do Windows 8.1 para a versão do Windows 10.                                                                                                                                       |

 

 

 







<!--HONumber=Jun16_HO4-->


