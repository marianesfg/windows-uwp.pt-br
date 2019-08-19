---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera interna do Windows.
title: Capture fotos e vídeos com a interface do usuário da câmera interna do Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4fb8132e7b382ee801d83986261bf71d8fef490f
ms.sourcegitcommit: 04a6e60c3b24d6efae0f0e2ada1d66a369471fb3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68830422"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Capture fotos e vídeos com a interface do usuário da câmera interna do Windows



Este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera interna do Windows. Esse recurso é fácil de usar. Ele permite que seu aplicativo obtenha uma foto ou vídeo capturado pelo usuário com apenas algumas linhas de código.

Se você quiser fornecer a interface de usuário da sua própria câmera ou se seu cenário exigir um controle aprofundado e mais robusto da operação de captura, você deverá usar o objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) e implementar sua própria experiência de captura. Para obter mais informações, consulte [Captura básica de foto, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Você não deve especificar os recursos de **webcam** ou de **microfone** no arquivo de manifesto do aplicativo se seu aplicativo usar apenas CameraCaptureUI. Se você fizer isso, seu aplicativo será exibido nas configurações de privacidade da câmera do dispositivo, mas mesmo que o usuário negue acesso à câmera para seu aplicativo, isso não impedirá que o CameraCaptureUI Capture a mídia. <p>Isso ocorre porque o aplicativo de câmera interno do Windows é um aplicativo de terceiros confiável que exige que o usuário inicie a captura de foto, áudio e vídeo pressionando um botão. Seu aplicativo pode falhar na certificação do kit de certificação de aplicativos do Windows quando enviado para Microsoft Store se você especificar os recursos de webcam ou de microfone ao usar o CameraCaptureUI como seu único mecanismo de captura de foto.<p>
Você deve especificar os recursos de webcam ou de microfone no arquivo de manifesto do aplicativo se estiver usando o MediaCapture para capturar áudio, fotos ou vídeo de forma programática.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar uma foto com CameraCaptureUI

Para usar a interface do usuário de captura com câmera, inclua o namespace [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) em seu projeto. Para as operações de arquivo com o arquivo de imagem retornado, inclua [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar uma foto, crie um novo objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI). Usando a propriedade [**fotosettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.photosettings) do objeto, você pode especificar propriedades para a foto retornada, como o formato de imagem da foto. Por padrão, a interface do usuário de captura de câmera dá suporte a cortar a foto antes de ser retornada. Isso pode ser desabilitado com a propriedade [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) . Este exemplo define [**CroppedSizeInPixels**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) para solicitar que a imagem retornada seja de 200 x 200 pixels.

> [!NOTE]
> Não há suporte para o corte de imagem no **CameraCaptureUI** para dispositivos na família de dispositivos móveis. O valor da propriedade [**AllowCropping**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) é ignorado quando seu aplicativo é executado nesses dispositivos.

Chame [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) e especifique [**CameraCaptureUIMode.Photo**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) para especificar que uma foto deve ser capturada. O método retorna uma instância [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que conterá a imagem se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

É fornecido um nome gerado dinamicamente ao **StorageFile** que contém a foto capturada e salvo na pasta local do aplicativo. Para organizar melhor suas fotos capturadas, você pode mover o arquivo para uma pasta diferente.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Para usar sua foto em seu aplicativo, talvez você queira criar um objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que pode ser usado com vários recursos diferentes de aplicativo Universal do Windows.

Primeiro, você deve incluir o namespace [**Windows. Graphics. Imaging**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) em seu projeto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Chame [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) para obter um fluxo do arquivo de imagem. Chame [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obter um decodificador de bitmap para o fluxo. Em seguida, chame [**GetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obter uma representação **SoftwareBitmap** da imagem.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para exibir a imagem na interface do usuário, declare um controle [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) na página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar o bitmap de software na página XAML, inclua o namespace [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) em uso em seu projeto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

O controle de **imagem** requer que a origem da imagem esteja no formato BGRA8 com alfa ou sem alfa. Chame o método estático [**SoftwareBitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para criar um novo bitmap de software com o formato desejado. Em seguida, crie um novo objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) e chame-o de [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para atribuir o bitmap de software à origem. Por fim, defina a propriedade **Image** do controle [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) para exibir a foto capturada na interface do usuário.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar um vídeo com CameraCaptureUI

Para capturar um vídeo, crie um novo objeto [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI). Usando a propriedade [**VideoSettings**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) do objeto, você pode especificar propriedades para o vídeo retornado, como o formato do vídeo.

Chame [**CaptureFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) e especifique o [**vídeo**](https://docs.microsoft.com/uwp/api/windows.media.capture.cameracaptureui.videosettings) para capturar um vídeo. O método retorna a uma instância [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que conterá o vídeo se a captura for bem-sucedida. Se você cancelar a captura, o objeto retornado será nulo.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

O que fazer com o arquivo de vídeo capturado depende do cenário para o seu aplicativo. O restante deste artigo mostra como criar rapidamente uma composição de mídia de um ou mais vídeos capturados e mostrá-la em sua interface do usuário.

Primeiro, adicione um controle [**mediaplayerelement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) no qual a composição de vídeo será exibida na sua página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Quando o arquivo de vídeo retorna da interface do usuário da captura de câmera, crie um novo [**MediaName**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) chamando **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)** . Chame o método **[Reproduzir](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** do **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** padrão associado a **MediaPlayerElement** para reproduzir o vídeo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Foto básica, vídeo e captura de áudio com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 
 

 




