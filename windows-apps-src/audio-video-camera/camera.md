---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: Este artigo indica os recursos da câmera que estão disponíveis para aplicativos UWP e links para os artigos de instruções que mostram como usá-los.
title: Camera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7d7fcdeb3740ac4c6851170796243392676d1d1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254297"
---
# <a name="camera"></a>Camera

Esta seção fornece orientação para a criação de aplicativos da Plataforma Universal do Windows (UWP) que usam a câmera ou o microfone para capturar fotos, vídeos ou áudio.

## <a name="use-the-windows-built-in-camera-ui"></a>Usar a interface do usuário da câmera interna do Windows

| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capture photos and video with Windows built-in camera UI](capture-photos-and-video-with-cameracaptureui.md) | Mostra como usar a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows. Se você só deseja permitir que o usuário capture uma foto ou um vídeo e retornar o resultado para seu aplicativo, essa é a maneira mais rápida e fácil de fazer isso.  |

## <a name="basic-mediacapture-tasks"></a>Tarefas básicas de MediaCapture

| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Display the camera preview](simple-camera-preview-access.md) | Mostra como exibir rapidamente o fluxo de visualização da câmera em uma página XAML em um aplicativo UWP. |
| [Basic photo, video, and audio capture with MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Mostra a maneira mais simples de capturar fotos e vídeo usando a classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture). A classe **MediaCapture** expõe um conjunto robusto de APIs que fornecem controle aprofundado sobre o pipeline de captura e permite a captura de cenários avançados, mas este artigo se destina a ajudá-lo a adicionar captura de mídia básica ao seu aplicativo com rapidez e facilidade. |
| [Camera UI features for mobile devices](camera-ui-features-for-mobile-devices.md) | Mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Tarefas avançadas do MediaCapture   
                                                                                                               
| Tópico                                                                                             | Descrição                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Handle device and screen orientation with MediaCapture](handle-device-orientation-with-mediacapture.md) | Mostra como tratar a orientação do dispositivo ao capturar fotos e vídeos usando uma classe auxiliar. | 
| [Discover and select camera capabilities with camera profiles](camera-profiles.md) | Mostra como usar perfis de câmera para descobrir e gerenciar as funcionalidades de diferentes dispositivos de captura de vídeo. Isso inclui tarefas como selecionar perfis com suporte a resoluções ou taxas de quadro específicos, perfis que dão suporte ao acesso simultâneo a várias câmeras e perfis compatíveis com HDR. |
| [Set format, resolution, and frame rate for MediaCapture](set-media-encoding-properties.md) | Mostra como usar a interface [**IMediaEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.IMediaEncodingProperties) para definir a resolução e a taxa de quadro do fluxo de visualização da câmera e de fotos e vídeo capturados. Ele também mostra como garantir que a taxa de proporção do fluxo de visualização corresponda ao da mídia capturada. |
| [HDR and low-light photo capture](high-dynamic-range-hdr-photo-capture.md) | Mostra como usar a classe [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) para capturar intervalo HDR (High Dynamic) e fotos de pouca luz. |
| [Manual camera controls for photo and video capture](capture-device-controls-for-photo-and-video-capture.md) | Mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave. |
| [Manual camera controls for video capture](capture-device-controls-for-video-capture.md) | Mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição.  |
| [Video stabilization effect for video capture](effects-for-video-capture.md) | Mostra como usar o efeito de estabilização de vídeo.  |
| [Scene anlysis for MediaCapture](scene-analysis-for-media-capture.md) | Mostra como usar [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) e [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) para analisar o conteúdo do fluxo de visualização de captura de mídia.  |
| [Capture a photo sequence with VariablePhotoSequence](variable-photo-sequence.md) | Mostra como capturar uma sequência de fotos variável que permite capturar vários quadros de imagem em sucessão rápida e configurar cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição.  |
| [Process media frames with MediaFrameReader](process-media-frames-with-mediaframereader.md) | Mostra como usar um [**MediaFrameReader**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) com [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para obter quadros de mídia de uma ou mais fontes disponíveis, incluindo cores, profundidade e câmeras infravermelho, dispositivos de áudio ou até mesmo fontes personalizadas de quadros, como as que produzem quadros de rastreamento de esqueleto. Esse recurso foi criado para ser usado por aplicativos que executam processamento em tempo real de quadros de mídia, como aplicativos de câmera com reconhecimento de profundidade e realidade aumentada.  |
| [Get a preview frame](get-a-preview-frame.md) | Mostra como obter um único quadro de visualização do fluxo de visualização de captura de mídia.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>Exemplos de aplicativos UWP para câmera

* [Camera face detection sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection)
* [Camera preview frame sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraGetPreviewFrame)
* [Camera HDR sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraAdvancedCapture)
* [Camera manual controls sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls)
* [Camera profile sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraProfile)
* [Camera resolution sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraResolution)
* [Camera starter kit](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit)
* [Camera video stabilization sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraVideoStabilization)

## <a name="related-topics"></a>Tópicos relacionados

* [Áudio, vídeo e câmera](index.md)
 

 




