---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Este artigo mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição.
title: Controles manuais da câmera para captura de vídeo
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 91f9d21f6df09bf8f3cad3e5a43f3ed94049338e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7417437"
---
# <a name="manual-camera-controls-for-video-capture"></a>Controles manuais da câmera para captura de vídeo



Este artigo mostra como usar controles de dispositivo manuais para permitir cenários de captura de vídeo aprimorados, incluindo vídeo HDR e prioridade de exposição.

Os controles de dispositivo de vídeo discutidos neste artigo são todos adicionados ao seu aplicativo usando o mesmo padrão. Primeiro, verifique se o controle tem suporte no dispositivo atual em que seu aplicativo está sendo executado. Se o controle tiver suporte, defina o modo desejado para o controle. Normalmente, se um determinado controle não tiver suporte no dispositivo atual, você deverá desabilitar ou ocultar o elemento da interface do usuário que permite ao usuário habilitar o recurso.

Todas as APIs de controle de dispositivo discutidas neste artigo são membros do namespace [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. Recomendamos que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código neste artigo presume que seu aplicativo já tenha uma instância de MediaCapture inicializada corretamente.

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

## <a name="temporal-denoising"></a>Denoising temporal
A partir do Windows 10, versão 1803, você pode habilitar o denoising temporal para vídeo em dispositivos compatíveis. Esse recurso funde os dados da imagem de vários quadros adjacentes em tempo real para produzir quadros de vídeo com menos ruído visual.

O [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) permite que o aplicativo determine se o denoising temporal é compatível com o dispositivo atual e, caso afirmativo, qual modo de denoising é suportado. Os modos de denoising são [**Desligado**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**Ligado**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode) e [**Automático**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode). Um dispositivo podem ser suportar todos os modos, mas todos os dispositivos devem suporte **Automático** ou **Ligado** e **Desligado**.

O exemplo a seguir usa uma interface do usuário simples para fornecer botões de opção, permitindo que o usuário alterne entre os modos de denoising.

[!code-cs[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

No método a seguir, a propriedade [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) é verificada para confirmar se o denoising temporal é suportado no dispositivo atual. Caso afirmativo, em seguida, verificamos para certificar-se de que **Desligado** e **Automático** ou **Ligado** são suportados. Nesse caso, podemos tornar os botões de opção visíveis. Em seguida, os botões **Automático** e **Ligado** ficam visíveis se há suporte para esses métodos.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

No manipulador de eventos **Checked** dos botões de opção, o nome do botão é verificado e o modo correspondente é definido ao configurar a propriedade [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode).

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Desabilitar o denoising temporal durante o processamento de quadros
Os vídeos processados usando denoising temporal podem ser mais agradáveis visualmente. No entanto, como o denoising temporal pode afetar a consistência de imagem e diminuir a quantidade de detalhes no quadro, os aplicativos que executam o processamento de imagens em quadros, por exemplo, o reconhecimento de registro ou caracteres ópticos, talvez optem por desabilitar de forma programática o denoising quando o processamento de imagens está habilitado.

O exemplo a seguir determina quais modos de denoising são compatíveis e armazena essas informações em algumas variáveis de classe.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

Quando o aplicativo permite o processamento de quadros, ele define o modo de denoising como **Desligado** se esse modo for compatível, para que o processamento de quadro possa usar quadros brutos sem denoising.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

Quando o aplicativo desabilita o processamento de quadro, ele define o modo de denoising como **Ligado** ou **Automático**, dependendo de qual modo é compatível.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

Para obter mais informações sobre como obter quadros de vídeo para processamento de imagem, consulte [Processar quadros de mídia com MediaFrameReader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Processar quadros de mídia com o MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 




