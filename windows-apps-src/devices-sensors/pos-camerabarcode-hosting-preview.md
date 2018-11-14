---
author: TerryWarwick
title: Hospedagem da visualização do scanner de código de barras da câmera
description: Saiba como hospedar uma visualização de scanner de código de barras de câmera em seu aplicativo
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 9684db2495e974c23d81b21e9a4a2e764d390255
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6191579"
---
# <a name="hosting-a-camera-barcode-scanner-preview-in-your-application"></a>Hospedagem de uma visualização de scanner de código de barras de câmera em seu aplicativo
## <a name="step-1-setup-your-camera-preview"></a>Etapa 1: Configurar a visualização da câmera
A primeira etapa ao adicionar uma visualização ao seu aplicativo para o scanner de código de barras de câmera pode ser realizada por meio das instruções no tópico [Exibir a visualização da câmera](../audio-video-camera/simple-camera-preview-access.md).  Após a conclusão dessa etapa, retorne a este tópico para modificações específicas no scanner de código de barras de câmera.

## <a name="step-2-update-capability-declarations"></a>Etapa 2: Atualizar declarações de funcionalidade
Para evitar que os usuários recebam a solicitação de consentimento para microfone, você pode excluí-la das funcionalidades listadas no manifesto do aplicativo.

1. No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2. Selecione a guia **Recursos**.
3. Desmarque a caixa de **Microfone**

 ## <a name="step-3-add-additional-using-directive-for-media-capture"></a>Etapa 3: Adicionar mais diretivas de uso para captura de mídia

```Csharp
using Windows.Media.Capture;
```

## <a name="step-4-set-up-your-mediacapture-initialization-settings"></a>Etapa 4: Configurar as configurações de inicialização MediaCapture
O método de exemplo a seguir inicializa o [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings). 

```Csharp
 private void InitCaptureSettings()
{
    _captureInitSettings = new MediaCaptureInitializationSettings();
    _captureInitSettings.VideoDeviceId = BarcodeScanner.VideoDeviceId;
    _captureInitSettings.StreamingCaptureMode = StreamingCaptureMode.Video;
    _captureInitSettings.PhotoCaptureSource = PhotoCaptureSource.VideoPreview;
}
```
## <a name="step-5-associate-your-mediacapture-object-with-the-camera-barcode-scanner"></a>Etapa 5: Associar o objeto MediaCapture ao scanner de código de barras de câmera
Substitua o mediaCapture.InitializeAsync() existente em *StartPreviewAsync()* pelo seguinte:

```Csharp
try
    {

        mediaCapture = new MediaCapture();
        await mediaCapture.InitializeAsync(InitCaptureSettings());

        displayRequest.RequestActive();
        DisplayInformation.AutoRotationPreferences = DisplayOrientations.Landscape;
    }
```

> [!TIP]
> Consulte [Exibir a visualização da câmera](https://docs.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access#add-capability-declarations-to-the-app-manifest) para mais tópicos avançados sobre hospedagem de uma visualização de câmera em seu aplicativo.
