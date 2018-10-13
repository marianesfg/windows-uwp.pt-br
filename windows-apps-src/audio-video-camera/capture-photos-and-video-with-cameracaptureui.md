---
author: drewbatgit
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: Este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows
title: Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ba33a1e79a2447c5dac546ce0f1caeaf16929a3
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4572543"
---
# <a name="capture-photos-and-video-with-windows-built-in-camera-ui"></a>Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows



Este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows Esse recurso é fácil de usar e permite que seu aplicativo obtenha uma foto capturada pelo usuário ou um vídeo com apenas algumas linhas de código.

Se você quiser fornecer a interface de usuário da sua própria câmera ou se seu cenário exigir um controle aprofundado e mais robusto da operação de captura, você deverá usar o objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) e implementar sua própria experiência de captura. Para obter mais informações, consulte [Captura básica de foto, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

> [!NOTE]
> Você não deve especificar as funcionalidades de **webcam** ou **microfone** no arquivo de manifesto do aplicativo se o aplicativo usa apenas CameraCaptureUI. Se você fizer isso, seu aplicativo será exibido nas configurações de privacidade da câmera do dispositivo, mas, mesmo se o usuário negar o acesso da câmera ao seu aplicativo, isso não impedirá CameraCaptureUI de capturar a mídia. Isso ocorre porque o aplicativo de câmera interno do Windows é um aplicativo de terceiros confiável que exige que o usuário inicie a captura de foto, áudio e vídeo pressionando um botão. Seu aplicativo pode falhar na certificação do Kit de certificação de aplicativo do Windows quando enviado para a loja se você especificar as funcionalidades de webcam ou microfone ao usar CameraCaptureUI como o único mecanismo de captura de foto.
> Você deve especificar as funcionalidades de webcam ou microfone no arquivo de manifesto do aplicativo se estiver usando o MediaCapture para capturar áudio, fotos ou vídeo por meio de programação.

## <a name="capture-a-photo-with-cameracaptureui"></a>Capturar uma foto com CameraCaptureUI

Para usar a interface do usuário de captura com câmera, inclua o namespace [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) em seu projeto. Para as operações de arquivo com o arquivo de imagem retornado, inclua [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar uma foto, crie um novo objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). Usando a propriedade [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) do objeto, você pode especificar propriedades para a foto retornada, como o formato de imagem da foto. Por padrão, a interface do usuário de captura com câmera permite ao usuário recortar a foto antes de ela ser retornada, embora isso possa ser desabilitado com a propriedade [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042). Este exemplo define [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) para solicitar que a imagem retornada seja de 200 x 200 pixels.

> [!NOTE]
> Não há suporte para o corte de imagem no **CameraCaptureUI** para dispositivos da família de dispositivos móveis. O valor da propriedade [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) é ignorado quando seu aplicativo é executado nesses dispositivos.

Chame [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) e especifique [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) para especificar que uma foto deve ser capturada. O método retorna uma instância [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que conterá a imagem se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

É fornecido um nome gerado dinamicamente ao **StorageFile** que contém a foto capturada e salvo na pasta local do aplicativo. Para organizar melhor suas fotos capturadas, convém mover o arquivo para uma pasta diferente.

[!code-cs[CopyAndDeletePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCopyAndDeletePhoto)]

Para usar sua foto em seu aplicativo, talvez você queira criar um objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que pode ser usado com vários recursos diferentes de aplicativo Universal do Windows.

Primeiro você deve incluir o namespace [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) em seu projeto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Chame [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) para obter um fluxo do arquivo de imagem. Chame [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) para obter um decodificador de bitmap para o fluxo. Em seguida, chame [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) para obter uma representação **SoftwareBitmap** da imagem.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para exibir a imagem na interface do usuário, declare um controle [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) na página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar o bitmap de software na página XAML, inclua o namespace [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) em uso em seu projeto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

O controle **Image** requer que o controle de imagem esteja no formato de BGRA8 com alfa pré-multiplicado ou nenhum alfa, portanto chame o método estático [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) para criar um novo bitmap de software com o formato desejado. Em seguida, crie um novo objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) e chame [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) para atribuir o bitmap de software à fonte. Por fim, defina a propriedade **Image** do controle [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) para exibir a foto capturada na interface do usuário.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## <a name="capture-a-video-with-cameracaptureui"></a>Capturar um vídeo com CameraCaptureUI

Para capturar um vídeo, crie um novo objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). Usando a propriedade [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) do objeto, você pode especificar propriedades para o vídeo retornado, como o formato de imagem de vídeo.

Chame [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) e especifique [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) para especificar que um vídeo deve ser capturado. O método retorna a uma instância [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que conterá o vídeo se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

O que fazer com o arquivo de vídeo capturado depende do cenário para o seu aplicativo. O restante deste artigo mostra como criar rapidamente uma composição de mídia de um ou mais vídeos capturados e mostrá-la em sua interface do usuário.

Primeiro, adicione um controle [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) no qual a composição de vídeo será exibida na página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]


Com o arquivo de vídeo retornado da interface do usuário de captura com câmera, crie um novo [**MediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource) chamando **[CreateFromStorageFile](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromstoragefile)**. Chame o método **[Reproduzir](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play)** do **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** padrão associado a **MediaPlayerElement** para reproduzir o vídeo.

[!code-cs[PlayVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetPlayVideo)]
 

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 
 

 




