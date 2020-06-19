---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Este artigo descreve como usar a classe [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) para capturar fotos ou vídeos usando a interface do usuário da câmera interna do Windows.
title: Capture fotos e vídeos com a interface do usuário da câmera interna do Windows
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: a512f72c01f2082dd067fc867f7434c92d2aa0c8
ms.sourcegitcommit: 79e4b3a9c53060b64513e2e240f0a4f073cc5dab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978931"
---
# <a name="capture-photos-and-video-with-the-windows-built-in-camera-ui"></a>Capture fotos e vídeos com a interface do usuário da câmera interna do Windows

Este artigo descreve como usar a classe [**CameraCaptureUI**](/uwp/api/windows.media.capture.cameracaptureui) para capturar fotos ou vídeos usando a interface do usuário da câmera interna do Windows. Esse recurso é fácil de usar. Ele permite que seu aplicativo obtenha uma foto ou vídeo capturado pelo usuário com apenas algumas linhas de código.

Se você quiser fornecer sua própria interface do usuário da câmera ou se o seu cenário exigir um controle mais robusto e de baixo nível da operação de captura, você deverá usar a classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) e implementar sua própria experiência de captura. Para obter mais informações, consulte [Captura básica de foto, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Você não deve especificar os recursos de **webcam** e de **microfone** no arquivo de manifesto do aplicativo se seu aplicativo usar apenas **CameraCaptureUI**. Se você fizer isso, seu aplicativo será exibido nas configurações de privacidade da câmera do dispositivo, mas mesmo que o usuário negue acesso à câmera para seu aplicativo, isso não impedirá que o **CameraCaptureUI** Capture a mídia. <p>Isso ocorre porque o aplicativo de câmera interno do Windows é um aplicativo de terceiros confiável que exige que o usuário inicie a captura de foto, áudio e vídeo pressionando um botão. Seu aplicativo pode falhar na certificação do kit de certificação de aplicativos do Windows quando enviado para Microsoft Store se você especificar os recursos de webcam ou de microfone ao usar o **CameraCaptureUI** como seu único mecanismo de captura de foto.<p>
Você deve especificar os recursos de **webcam** ou de **microfone** no arquivo de manifesto do aplicativo se estiver usando o **MediaCapture** para capturar áudio, fotos ou vídeo de forma programática.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar uma foto com CameraCaptureUI

Para usar a interface do usuário de captura com câmera, inclua o namespace [**Windows.Media.Capture**](/uwp/api/Windows.Media.Capture) em seu projeto. Para as operações de arquivo com o arquivo de imagem retornado, inclua [**Windows.Storage**](/uwp/api/Windows.Storage).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingCaptureUI":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingCaptureUI":::

Para capturar uma foto, crie um novo objeto [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI). Usando a propriedade [**fotosettings**](/uwp/api/windows.media.capture.cameracaptureui.photosettings) do objeto, você pode especificar propriedades para a foto retornada, como o formato de imagem da foto. Por padrão, a interface do usuário de captura de câmera dá suporte a cortar a foto antes de ser retornada. Isso pode ser desabilitado com a propriedade [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) . Este exemplo define [**CroppedSizeInPixels**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.croppedsizeinpixels) para solicitar que a imagem retornada seja de 200 x 200 pixels.

> [!NOTE]
> Não há suporte para o corte de imagem no **CameraCaptureUI** para dispositivos na família de dispositivos móveis. O valor da propriedade [**AllowCropping**](/uwp/api/windows.media.capture.cameracaptureuiphotocapturesettings.allowcropping) é ignorado quando seu aplicativo é executado nesses dispositivos.

Chame [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) e especifique [**CameraCaptureUIMode.Photo**](/uwp/api/Windows.Media.Capture.CameraCaptureUIMode) para especificar que uma foto deve ser capturada. O método retorna uma instância [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que conterá a imagem se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCapturePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCapturePhoto":::

É fornecido um nome gerado dinamicamente ao **StorageFile** que contém a foto capturada e salvo na pasta local do aplicativo. Para organizar melhor suas fotos capturadas, você pode mover o arquivo para uma pasta diferente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCopyAndDeletePhoto":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCopyAndDeletePhoto":::

Para usar sua foto em seu aplicativo, talvez você queira criar um objeto [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que pode ser usado com vários recursos diferentes de aplicativo Universal do Windows.

Primeiro, inclua o namespace [**Windows. Graphics. Imaging**](/uwp/api/Windows.Graphics.Imaging) em seu projeto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmap":::

Chame [**OpenAsync**](/uwp/api/windows.storage.istoragefile.openasync) para obter um fluxo do arquivo de imagem. Chame [**BitmapDecoder.CreateAsync**](/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obter um decodificador de bitmap para o fluxo. Em seguida, chame [**GetSoftwareBitmap**](/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obter uma representação **SoftwareBitmap** da imagem.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSoftwareBitmap":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSoftwareBitmap":::

Para exibir a imagem na interface do usuário, declare um controle [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) na página XAML.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetImageControl":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetImageControl":::

Para usar o bitmap de software na página XAML, inclua o namespace [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) em uso em seu projeto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetUsingSoftwareBitmapSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.h" id="SnippetUsingSoftwareBitmapSource":::

O controle de **imagem** requer que a origem da imagem esteja no formato BGRA8 com alfa ou sem alfa. Chame o método estático [**SoftwareBitmap. Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para criar um novo bitmap de software com o formato desejado. Em seguida, crie um novo objeto [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) e chame-o de [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para atribuir o bitmap de software à origem. Por fim, defina a propriedade **Image** do controle [**Source**](/uwp/api/windows.ui.xaml.controls.image.source) para exibir a foto capturada na interface do usuário.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetSetImageSource":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetSetImageSource":::

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar um vídeo com CameraCaptureUI

Para capturar um vídeo, crie um novo objeto [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI). Usando a propriedade [**VideoSettings**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) do objeto, você pode especificar propriedades para o vídeo retornado, como o formato do vídeo.

Chame [**CaptureFileAsync**](/uwp/api/windows.media.capture.cameracaptureui.capturefileasync) e especifique o [**vídeo**](/uwp/api/windows.media.capture.cameracaptureui.videosettings) para capturar um vídeo. O método retorna a uma instância [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que conterá o vídeo se a captura for bem-sucedida. Se você cancelar a captura, o objeto retornado será nulo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetCaptureVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetCaptureVideo":::

O que fazer com o arquivo de vídeo capturado depende do cenário para o seu aplicativo. O restante deste artigo mostra como criar rapidamente uma composição de mídia de um ou mais vídeos capturados e mostrá-la em sua interface do usuário.

Primeiro, adicione um controle [**mediaplayerelement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) no qual a composição de vídeo será exibida na sua página XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml" id="SnippetMediaElement":::

Quando o arquivo de vídeo retorna da interface do usuário da captura de câmera, crie um novo [**MediaName**](/uwp/api/windows.media.core.mediasource) chamando **[CreateFromStorageFile](/uwp/api/windows.media.core.mediasource.createfromstoragefile)**. Chame o método **[Reproduzir](/uwp/api/windows.media.playback.mediaplayer.Play)** do **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** padrão associado a **MediaPlayerElement** para reproduzir o vídeo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cs/MainPage.xaml.cs" id="SnippetPlayVideo":::
:::code language="cppwinrt" source="~/../snippets-windows/windows-uwp/audio-video-camera/CameraCaptureUIWin10/cppwinrt/MainPage.cpp" id="SnippetPlayVideo":::

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI)
