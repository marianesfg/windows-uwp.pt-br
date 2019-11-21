---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: Este artigo descreve como usar SceneAnalysisEffect e FaceDetectionEffect para analisar o conteúdo do fluxo de visualização de captura de mídia.
title: Efeitos para análise de quadros de câmera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 406af54cfaae8710cea2d989278a16f28c8dd619
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256212"
---
# <a name="effects-for-analyzing-camera-frames"></a>Efeitos para análise de quadros de câmera



Este artigo descreve como usar [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) e [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) para analisar o conteúdo do fluxo de visualização de captura de mídia.

## <a name="scene-analysis-effect"></a>Efeito de análise de cena

[  **SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) analisa os quadros de vídeo no fluxo de visualização de captura de mídia e recomenda opções de processamento para melhorar o resultado da captura. Atualmente, o efeito permite detectar se a captura poderia ser melhorada usando o processamento em HDR (High Dynamic Range).

Se o efeito recomendar o uso de HDR, você pode fazer isso das seguintes maneiras:

-   Use a classe [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) para capturar fotos usando o algoritmo de processamento em HDR interno do Windows. Para obter mais informações, consulte [Captura de fotos em HDR (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

-   Use [**HdrVideoControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.HdrVideoControl) para capturar vídeos usando o algoritmo de processamento HDR interno do Windows. Para obter mais informações, consulte [Controles de captura do dispositivo para a captura de vídeo](capture-device-controls-for-video-capture.md).

-   Use [**VariablePhotoSequenceControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) para capturar uma sequência de quadros com os quais você pode fazer uma composição usando uma implementação personalizada em HDR. Para obter mais informações, consulte [Sequência de fotos variável](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Namespaces de análise de cena

Para usar a análise de cena, seu aplicativo deve incluir os namespaces a seguir, além dos namespaces necessários para a captura de mídia básica.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Inicializar o efeito de análise de cena e adicioná-lo ao fluxo de visualização

Os efeitos de vídeo são implementados usando-se duas APIs, uma definição de efeito, que fornece as configurações que o dispositivo de captura precisa para inicializar o efeito, e uma instância de efeito, que pode ser usada para controlar o efeito. Como você pode querer acessar a instância de efeito de vários locais dentro do seu código, normalmente você deve declarar uma variável de membro para manter o objeto.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

No seu aplicativo, depois de você ter inicializado o objeto **MediaCapture**, crie uma nova instância de [**SceneAnalysisEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition).

Registre o efeito com o dispositivo de captura chamando [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) no seu objeto **MediaCapture**, fornecendo **SceneAnalysisEffectDefinition** e especificando [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) para indicar que o efeito deve ser aplicado ao fluxo de visualização de vídeo, em vez do fluxo de captura. **AddVideoEffectAsync** retorna uma instância do efeito adicionado. Como esse método pode ser usado com vários tipos de efeito, você deve converter a instância retornada em um objeto [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect).

Para receber os resultados da análise de cena, você deve registrar um manipulador para o evento [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed).

Atualmente, o efeito de análise de cena inclui somente o analisador de HDR. Habilite a análise de HDR definindo o efeito [**HighDynamicRangeControl.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) como true.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implementar o manipulador de eventos SceneAnalyzed

Os resultados da análise de cena são retornados no manipulador de eventos **SceneAnalyzed**. O objeto [**SceneAnalyzedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) passado para o manipulador tem um objeto [**SceneAnalysisEffectFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) que possui um objeto [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput). A propriedade [**Certainty**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) da saída de HDR fornece um valor entre 0 e 1,0, onde 0 indica que o processamento em HDR não ajudaria a melhorar o resultado da captura e 1,0 indica que o processamento em HDR ajudaria. Você pode decidir o ponto de limite no qual deseja usar HDR ou mostrar os resultados ao usuário e permitir que ele decida.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

O objeto [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) passado para o manipulador também tem uma propriedade [**FrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers), que contém controladores de quadro sugeridos para capturar uma sequência de fotos variável para processamento em HDR. Para obter mais informações, consulte [Sequência de fotos variável](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Limpar o efeito da análise de cena

Quando seu aplicativo terminar a captura, antes de descartar o objeto **MediaCapture**, você deve desabilitar o efeito de análise de cena definindo a propriedade [**HighDynamicRangeAnalyzer.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) do efeito como false e cancelar o registro de seu manipulador de eventos [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed). Chame [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) especificando o fluxo de visualização de vídeo, desde que tenha sido o fluxo ao qual o efeito foi adicionado. Por fim, defina a variável de membro como nula.

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>Efeito de detecção de rosto

O [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) identifica a localização de rostos dentro do fluxo de visualização da captura de mídia. O efeito permite que você receba uma notificação sempre que um rosto for detectado no fluxo de visualização e fornece a caixa delimitadora para cada rosto detectado dentro do quadro de visualização. Em dispositivos com suporte, o efeito de detecção de rosto também fornece exposição avançada e foco no rosto mais importante na cena.

### <a name="face-detection-namespaces"></a>Namespaces de detecção de rosto

Para usar a detecção de rosto, seu aplicativo deve incluir os namespaces a seguir, além dos namespaces necessários para a captura de mídia básica.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Inicializar o efeito de detecção de rosto e adicioná-lo ao fluxo de visualização

Os efeitos de vídeo são implementados usando-se duas APIs, uma definição de efeito, que fornece as configurações que o dispositivo de captura precisa para inicializar o efeito, e uma instância de efeito, que pode ser usada para controlar o efeito. Como você pode querer acessar a instância de efeito de vários locais dentro do seu código, normalmente você deve declarar uma variável de membro para manter o objeto.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

No seu aplicativo, depois de você ter inicializado o objeto **MediaCapture**, crie uma nova instância de [**FaceDetectionEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition). Defina a propriedade [**DetectionMode**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) para priorizar a detecção de rosto mais rápida ou a mais precisa. Defina [**SynchronousDetectionEnabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) para especificar que os quadros recebidos não atrasem aguardando a detecção de rosto ser concluída, já que isso pode resultar em uma experiência de visualização truncada.

Registre o efeito com o dispositivo de captura chamando [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) no seu objeto **MediaCapture**, fornecendo **FaceDetectionEffectDefinition** e especificando [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) para indicar que o efeito deve ser aplicado ao fluxo de visualização de vídeo, em vez do fluxo de captura. **AddVideoEffectAsync** retorna uma instância do efeito adicionado. Como esse método pode ser usado com vários tipos de efeito, você deve converter a instância retornada em um objeto [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect).

Habilite ou desabilite os efeitos definindo a propriedade [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled). Ajuste quantas vezes o efeito analisa os quadros definindo a propriedade [**FaceDetectionEffect.DesiredDetectionInterval**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval). Ambas as propriedades podem ser ajustadas enquanto captura de mídia está em andamento.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>Receber notificações quando os rostos forem detectados

Se você deseja executar algumas ações quando rostos forem detectados, como desenhar uma caixa ao redor dos rostos detectados na visualização de vídeo, você pode registrar o evento [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected).

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

No manipulador do evento, você pode obter uma lista de todos os rostos detectados em um quadro acessando a propriedade [**FaceDetectionEffectFrame.DetectedFaces**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) do [**FaceDetectedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectedEventArgs). A propriedade [**FaceBox**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.detectedface.facebox) é uma estrutura [**BitmapBounds**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBounds) que descreve o retângulo contendo o rosto detectado em unidades relativas às dimensões do fluxo de visualização. Para ver o código de amostra que transforma as coordenadas do fluxo de visualização em coordenadas de tela, consulte o [amostra UWP de detecção de rosto](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection).

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>Limpar o efeito de detecção de rosto

Quando seu aplicativo terminar a captura, antes de descartar o objeto **MediaCapture**, você deve desabilitar o efeito de detecção de rosto com [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled) e cancelar o registro do seu manipulador de eventos [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected) se tiver registrado um anteriormente. Chame [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync) especificando o fluxo de visualização de vídeo, desde que tenha sido o fluxo ao qual o efeito foi adicionado. Por fim, defina a variável de membro como nula.

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Verificar se há suporte para foco e exposição para os rostos detectados

Nem todos os dispositivos têm um mecanismo de captura que possa ajustar o foco e a exposição com base nos rostos detectados. Como a detecção de rosto consome recursos do dispositivo, você pode querer apenas ativar a detecção de rosto em dispositivos que possam usar o recurso para aprimorar a captura. Para ver se a otimização de captura com base no rosto está disponível, obtenha o [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) para sua [MediaCapture](capture-photos-and-video-with-mediacapture.md) inicializada e baixe o [**RegionsOfInterestControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) do controlador de dispositivo de vídeo. Verifique se o [**MaxRegions**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) dá suporte a pelo menos uma região. Em seguida, verifique se [**AutoExposureSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) ou [**AutoFocusSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) estão definidas como true. Se essas condições forem atendidas, o dispositivo poderá aproveitar a detecção de rosto para melhorar a captura.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Foto básica, vídeo e captura de áudio com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




