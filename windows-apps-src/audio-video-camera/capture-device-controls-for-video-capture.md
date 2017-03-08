---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "Este artigo mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição."
title: "Controles manuais da câmera para captura de vídeo"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: cd6326ebad94c33fd03bf39f2dfd11f1c27e9b37
ms.lasthandoff: 02/07/2017

---

# <a name="manual-camera-controls-for-video-capture"></a>Controles manuais da câmera para captura de vídeo

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição.

Os controles de dispositivo de vídeo discutidos neste artigo são todos adicionados ao seu app usando o mesmo padrão. Primeiro, verifique se o controle tem suporte no dispositivo atual em que seu app está sendo executado. Se o controle tiver suporte, defina o modo desejado para o controle. Normalmente, se um determinado controle não tiver suporte no dispositivo atual, você deverá desabilitar ou ocultar o elemento da interface do usuário que permite ao usuário habilitar o recurso.

Todas as APIs de controle de dispositivo discutidas neste artigo são membros do namespace [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código neste artigo presume que seu app já tenha uma instância de MediaCapture inicializada corretamente.

## <a name="hdr-video"></a>Vídeo HDR

O recurso de vídeo HDR (High Dynamic Range) aplica processamento HDR ao fluxo de vídeo do dispositivo de captura. Determinar se o vídeo HDR é suportado, selecionando a propriedade [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

O controle de vídeo HDR oferece suporte a três modos: ativado, desativado e automático, o que significa que o dispositivo determina dinamicamente se o processamento de vídeo HDR melhora a captura de mídia e, nesse caso, habilita o vídeo HDR. Para determinar se um modo específico tem suporte no dispositivo atual, verifique se a coleção [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contém o modo desejado.

Habilite ou desabilite o processamento de vídeo HDR. Basta definir [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) como o modo desejado.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Prioridade de exposição

O [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644), quando habilitado, avalia os quadros de vídeo do dispositivo de captura para determinar se o vídeo está capturando uma cena com pouca luz. Em caso afirmativo, o controle diminui a taxa de quadros do vídeo capturado para aumentar o tempo de exposição de cada quadro e melhorar a qualidade visual do vídeo capturado.

Determine se o controle de prioridade de exposição tem suporte no dispositivo atual. Basta verificar a propriedade [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Habilite ou desabilite o controle de prioridade de exposição. Defina [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) como o modo desejado.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 





