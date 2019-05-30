---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: Este artigo mostra como capturar uma sequência de fotos variável que permite que você capture vários quadros de imagem em sucessão rápida e configure cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição.
title: Sequência de fotos variável
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 506ea5f7fe199c3df0b6089da73a108d53d98ef3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361381"
---
# <a name="variable-photo-sequence"></a>Sequência de fotos variável



Este artigo mostra como capturar uma sequência de fotos variável que permite que você capture vários quadros de imagem em sucessão rápida e configure cada quadro para usar diferentes configurações de foco, flash, ISO, exposição e compensação de exposição. Esse recurso habilita cenários como criar imagens em HDR (High Dynamic Range).

Se você deseja capturar imagens em HDR, mas não deseja implementar seu próprio algoritmo de processamento, use a API [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) para utilizar os recursos HDR nativos do Windows. Para obter mais informações, consulte [Captura de fotos em HDR (High Dynamic Range)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Este artigo se baseia em conceitos e códigos discutidos em [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), que descreve as etapas para implementar uma captura básica de fotos e vídeos. É recomendável que você se familiarize com o padrão de captura de mídia básica neste artigo antes de passar para cenários de captura mais avançados. O código deste artigo presume que seu aplicativo já tenha uma instância do MediaCapture inicializada corretamente.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Configurar seu aplicativo para usar a captura de sequência de fotos variável

Além dos namespaces necessários para captura de mídia básica, implementar uma captura de sequência de fotos variável exige os namespaces a seguir.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Declare uma variável de membro para armazenar o objeto [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture), que é usado para iniciar a captura de sequência de fotos. Declare uma matriz de objetos [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) para armazenar cada imagem capturada na sequência. Além disso, declare uma matriz para armazenar o objeto [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) para cada quadro. Isso pode ser usado pelo seu algoritmo de processamento de imagem para determinar quais configurações foram usadas para capturar cada quadro. Por fim, declare um índice que será usado para controlar a imagem que está sendo capturada na sequência.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>Preparar a captura de sequência de fotos variável

Depois de iniciar seu [MediaCapture](capture-photos-and-video-with-mediacapture.md), confira se as sequências de fotos variáveis têm suporte no dispositivo atual por meio da obtenção de uma instância do [**VariablePhotoSequenceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) a partir do [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) da captura de mídia e da verificação da propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtenha um objeto [**FrameControlCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) a partir do controlador de sequência de fotos variável. Esse objeto tem uma propriedade para cada configuração que pode ser definida por quadro de uma sequência de fotos. Como por exemplo:

-   [**Exposição**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Flash**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Foco**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

Este exemplo definirá um valor de compensação de exposição diferente para cada quadro. Para verificar se a compensação de exposição tem suporte para sequências de fotos no dispositivo atual, verifique a propriedade [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) do objeto [**FrameExposureCompensationCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) acessado por meio da propriedade **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Crie um novo objeto [**FrameController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameController) para cada quadro que você deseja capturar. Este exemplo captura três quadros. Defina os valores para os controles que você deseja variar para cada quadro. Em seguida, desmarque a coleção [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) do **VariablePhotoSequenceController** e adicione o controlador de cada quadro à coleção.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Crie um objeto [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) para definir a codificação que você deseja usar para as imagens capturadas. Chame o método estático [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync), por meio da transmissão das propriedades codificadas. Este método retorna um objeto [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture). Por fim, registre manipuladores de eventos para os eventos [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) e [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>Iniciar a captura de sequência de fotos variável

Para iniciar a captura de sequência de fotos variável, chame [**VariablePhotoSequenceCapture.StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync). Assegure-se de inicializar as matrizes para armazenar os valores de controle de quadro e as imagens capturadas. Defina o índice atual como 0. Defina a variável de estado de gravação de seu aplicativo e atualize sua interface do usuário para desabilitar a inicialização de outra captura enquanto uma captura está em andamento.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>Receber os quadros capturados

O evento [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) é gerado para cada quadro capturado. Salve os valores de controle de quadro e a imagem capturada do quadro. Em seguida, incremente o índice de quadro atual. Este exemplo mostra como obter uma representação [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de cada quadro. Para obter mais informações sobre o uso de **SoftwareBitmap**, consulte [Geração de imagens](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Manipular a conclusão da captura de sequência de fotos variável

O evento [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) é gerado quando todos os quadros na sequência forem capturados. Atualize o estado de gravação do seu aplicativo e sua interface do usuário para permitir que o usuário inicie novas capturas. Nesse ponto, você pode passar os valores de controle de quadro e as imagens capturadas a seu código de processamento de imagem.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>Atualizar controladores de quadro

Se você deseja executar outra captura de sequência de fotos variável com diferentes configurações por quadro, não é necessário reinicializar a **VariablePhotoSequenceCapture**. Você pode desmarcar a coleção [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) e adicionar novos controladores de quadro ou pode modificar os valores de controlador de quadro existentes. Os exemplos a seguir conferem o objeto [**FrameFlashCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) para verificar se o dispositivo atual suporta energia flash e flash para quadros de sequência de fotos variável. Nesse caso, cada quadro é atualizado para habilitar o flash em 100% de energia. Os valores de compensação de exposição que já foram definidos para cada quadro ainda estão ativos.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Desmarcar a captura de sequência de fotos variável

Quando você terminar de capturar as sequências de fotos variáveis ou seu aplicativo estiver suspenso, desmarque o objeto da sequência de fotos variável. Basta chamar [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync). Cancele o registro de manipuladores de evento do objeto e defina-o como null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




