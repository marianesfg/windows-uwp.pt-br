---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows.
title: capturar fotos e vídeos com CameraCaptureUI
---

# Capturar fotos e vídeos com CameraCaptureUI

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artigo descreve como usar a classe CameraCaptureUI para capturar fotos ou vídeos usando a interface do usuário da câmera integrada ao Windows Esse recurso é fácil de usar e permite que seu aplicativo obtenha uma foto capturada pelo usuário ou um vídeo com apenas algumas linhas de código.

Se seu cenário exigir um controle aprofundado e mais robusto da operação de captura, você deve usar o objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) e implementar sua própria experiência de captura. Para obter mais informações, consulte [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Capturar uma foto com CameraCaptureUI

Para usar a interface do usuário de captura com câmera, inclua o namespace [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) em seu projeto. Para as operações de arquivo com o arquivo de imagem retornado, inclua [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346).

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

Para capturar uma foto, crie um novo objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). Usando a propriedade [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) do objeto, você pode especificar propriedades para a foto retornada, como o formato de imagem da foto. Por padrão, a interface do usuário de captura com câmera permite ao usuário recortar a foto antes de ela ser retornada, embora isso possa ser desabilitado com a propriedade [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042). Este exemplo define [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) para solicitar que a imagem retornada seja de 200 x 200 pixels.

**Observação**  Não há suporte para o corte de imagem no CameraCaptureUI para dispositivos da família de dispositivos móveis. O valor da propriedade [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) é ignorado quando seu aplicativo é executado nesses dispositivos.

Chame [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) e especifique [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040) para especificar que uma foto deve ser capturada. O método retorna uma instância [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que conterá a imagem se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

Assim que você tiver o **StorageFile** contendo a foto capturada, poderá criar um objeto [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) que possa ser usado com vários recursos diferentes de aplicativo Universal do Windows.

Primeiro você deve incluir o namespace [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) em seu projeto.

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

Chame [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) para obter um fluxo do arquivo de imagem. Chame [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) para obter um decodificador de bitmap para o fluxo. Em seguida, chame [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) para obter uma representação **SoftwareBitmap** da imagem.

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

Para exibir a imagem na interface do usuário, declare um controle [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) na página XAML.

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

Para usar o bitmap de software na página XAML, inclua o namespace [**Windows**](https://msdn.microsoft.com/library/windows/apps/br243258) em uso em seu projeto.

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

O controle **Image** requer que o controle de imagem esteja no formato de BGRA8 com alfa pré-multiplicado ou nenhum alfa, portanto chame o método estático [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) para criar um novo bitmap de software com o formato desejado. Em seguida, crie um novo objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) e chame [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) para atribuir o bitmap de software à fonte. Por fim, defina a propriedade **Image** do controle [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) para exibir a foto capturada na interface do usuário.

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## Capturar um vídeo com CameraCaptureUI

Para capturar um vídeo, crie um novo objeto [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030). Usando a propriedade [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) do objeto, você pode especificar propriedades para o vídeo retornado, como o formato de imagem de vídeo.

Chame [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) e especifique [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059) para especificar que um vídeo deve ser capturado. O método retorna a uma instância [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que conterá o vídeo se a captura for bem-sucedida. Se o usuário cancelar a captura, o objeto retornado será nulo.

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

O que fazer com o arquivo de vídeo capturado depende do cenário para o seu aplicativo. O restante deste artigo mostra como criar rapidamente uma composição de mídia de um ou mais vídeos capturados e mostrá-la em sua interface do usuário.

Primeiro, adicione um controle [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) no qual a composição de vídeo será exibida na página XAML.

[!code-xml[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

Adicione os namespaces [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) e [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) ao seu projeto.


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

Declare as variáveis para um objeto [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) e uma [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716) que você deseja manter no escopo para o tempo de vida da página.

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

Antes de capturar vídeos, você deve criar uma nova instância da classe **MediaComposition**.

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

Com o arquivo de vídeo retornado da interface do usuário de captura com câmera, crie um novo [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) chamando [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). Adicione o clip de mídia à coleção de [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) da composição.

Chame [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) para criar o objeto **MediaStreamSource** da composição.

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

Por fim, defina a fonte de fluxo usando o método [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) do elemento de mídia para mostrar a composição da interface do usuário.

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

Você pode continuar a capturar videoclipes e adicioná-los à composição. Para saber mais sobre composições de mídia, consulte [Composições de mídia e edição](media-compositions-and-editing.md).

**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)
 

 






<!--HONumber=Mar16_HO1-->


