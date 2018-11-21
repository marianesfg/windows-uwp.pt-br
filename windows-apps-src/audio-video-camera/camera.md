---
author: drewbatgit
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: Este artigo indica os recursos da câmera que estão disponíveis para aplicativos UWP e links para os artigos de instruções que mostram como usá-los.
title: Câmera
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d330a762844e4889d0da9ae5457acc8d9f0c2b91
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423737"
---
# <a name="camera"></a>Câmera

Esta seção fornece orientação para a criação de aplicativos da Plataforma Universal do Windows (UWP) que usam a câmera ou o microfone para capturar fotos, vídeos ou áudio.

## <a name="use-the-windows-built-in-camera-ui"></a>Usar a interface do usuário da câmera interna do Windows

| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows](capture-photos-and-video-with-cameracaptureui.md) | Mostra como usar a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows. Se você só deseja permitir que o usuário capture uma foto ou um vídeo e retornar o resultado para seu aplicativo, essa é a maneira mais rápida e fácil de fazer isso.  |

## <a name="basic-mediacapture-tasks"></a>Tarefas básicas de MediaCapture

| Tópico | Descrição |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Exibir a visualização da câmera](simple-camera-preview-access.md) | Mostra como exibir rapidamente o fluxo de visualização da câmera em uma página XAML em um aplicativo UWP. |
| [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Mostra a maneira mais simples de capturar fotos e vídeo usando a classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). A classe **MediaCapture** expõe um conjunto robusto de APIs que fornecem controle aprofundado sobre o pipeline de captura e permite a captura de cenários avançados, mas este artigo se destina a ajudá-lo a adicionar captura de mídia básica ao seu aplicativo com rapidez e facilidade. |
| [Recursos da interface do usuário da câmera para dispositivos móveis](camera-ui-features-for-mobile-devices.md) | Mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Tarefas avançadas do MediaCapture   
                                                                                                               
| Tópico                                                                                             | Descrição                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Tratar a orientação do dispositivo e da tela com o MediaCapture](handle-device-orientation-with-mediacapture.md) | Mostra como tratar a orientação do dispositivo ao capturar fotos e vídeos usando uma classe auxiliar. | 
| [Descobrir e selecionar as funcionalidades da câmera com perfis de câmera](camera-profiles.md) | Mostra como usar perfis de câmera para descobrir e gerenciar as funcionalidades de diferentes dispositivos de captura de vídeo. Isso inclui tarefas como selecionar perfis com suporte a resoluções ou taxas de quadro específicos, perfis que dão suporte ao acesso simultâneo a várias câmeras e perfis compatíveis com HDR. |
| [Definir o formato, a resolução e a taxa de quadros para o MediaCapture](set-media-encoding-properties.md) | Mostra como usar a interface [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) para definir a resolução e a taxa de quadro do fluxo de visualização da câmera e de fotos e vídeo capturados. Ele também mostra como garantir que a taxa de proporção do fluxo de visualização corresponda ao da mídia capturada. |
| [Captura de fotos HDR e com pouca luz](high-dynamic-range-hdr-photo-capture.md) | Mostra como usar a classe [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) para capturar intervalo HDR (High Dynamic) e fotos de pouca luz. |
| [Controles manuais da câmera para a captura de fotos e vídeos](capture-device-controls-for-photo-and-video-capture.md) | Mostra como usar controles de dispositivo manuais para habilitar cenários de captura de fotos e vídeos avançados, incluindo estabilização de imagem óptica e zoom suave. |
| [Controles manuais da câmera para captura de vídeo](capture-device-controls-for-video-capture.md) | Mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição.  |
| [Efeito de estabilização de vídeo para captura de vídeo](effects-for-video-capture.md) | Mostra como usar o efeito de estabilização de vídeo.  |
| [Análise de cena para o MediaCapture](scene-analysis-for-media-capture.md) | Mostra como usar [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) e [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) para analisar o conteúdo do fluxo de visualização de captura de mídia.  |
| [Capturar uma sequência de fotos com VariablePhotoSequence](variable-photo-sequence.md) | Mostra como capturar uma sequência de fotos variável que permite capturar vários quadros de imagem em sucessão rápida e configurar cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição.  |
| [Processar quadros de mídia com o MediaFrameReader](process-media-frames-with-mediaframereader.md) | Mostra como usar um [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) com [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para obter quadros de mídia de uma ou mais fontes disponíveis, incluindo cores, profundidade e câmeras infravermelho, dispositivos de áudio ou até mesmo fontes personalizadas de quadros, como as que produzem quadros de rastreamento de esqueleto. Esse recurso foi criado para ser usado por aplicativos que executam processamento em tempo real de quadros de mídia, como aplicativos de câmera com reconhecimento de profundidade e realidade aumentada.  |
| [Obter um quadro de visualização](get-a-preview-frame.md) | Mostra como obter um único quadro de visualização do fluxo de visualização de captura de mídia.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>Exemplos de aplicativos UWP para câmera

* [Exemplo de detecção de rosto com a câmera](http://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [Exemplo de quadro de visualização da câmera](http://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [Exemplo de HDR de câmera](http://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [Exemplo de controles manuais da câmera](http://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [Exemplo de perfil da câmera](http://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [Exemplo de resolução da câmera](http://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [Kit inicial de câmera](http://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [Exemplo de estabilização de vídeo da câmera](http://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>Tópicos relacionados

* [Áudio, vídeo e câmera](index.md)
 

 




