---
author: drewbatgit
ms.assetid: 
description: "Este artigo mostra a maneira mais simples para capturar fotos e vídeos usando a classe MediaCapture."
title: "Captura básica de fotos, áudio e vídeo com MediaCapture"
translationtype: Human Translation
ms.sourcegitcommit: 9cbe7948767ba45e8ef495a9349621969957ab04
ms.openlocfilehash: 98f71104b5a95f9327a0b3f879e4dbb91b74b581

---

# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Captura básica de fotos, áudio e vídeo com MediaCapture

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo mostra a maneira mais simples para capturar fotos e vídeos usando a classe [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). A classe **MediaCapture** expõe um conjunto robusto de APIs que fornecem controle aprofundado sobre o pipeline de captura e permite a captura de cenários avançados, mas este artigo se destina a ajudar você a adicionar capturas de mídia básicas ao seu aplicativo com rapidez e facilidade. Para saber mais sobre os recursos que a classe **MediaCapture** fornece, consulte [**Câmera**](camera.md).

Se você simplesmente deseja tirar uma foto ou fazer um vídeo e não pretende adicionar recursos de captura de mídias adicionais, ou se não quer criar sua própria interface do usuário da câmera, talvez você prefira usar a classe [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI), que permite simplesmente iniciar o aplicativo integrado de câmera do Windows e receber o arquivo de foto ou vídeo que foi capturado. Para saber mais, consulte [**Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows**](capture-photos-and-video-with-cameracaptureui.md)

O código neste artigo foi adaptado do exemplo [**Kit inicial de câmera**](https://go.microsoft.com/fwlink/?linkid=619479). Você pode baixar o exemplo para ver o código usado no contexto ou utilizá-lo como ponto de partida para seu próprio app.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Adicionar declarações de funcionalidades ao manifesto do aplicativo

Para que seu app acesse a câmera do dispositivo, você deve declarar que o app usa as funcionalidades de *webcam* e *microphone* do dispositivo. Se quiser salvar as fotos e os vídeos capturados na biblioteca de imagens ou de vídeos do usuário, você deverá declarar também as funcionalidades *picturesLibrary* e *videosLibrary*.

**Para adicionar funcionalidades ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar as bibliotecas de imagens e vídeos, marque as caixas para **Biblioteca de Imagens** e para **Biblioteca de Vídeos**.


## <a name="initialize-the-mediacapture-object"></a>Inicializar o objeto MediaCapture
Todos os métodos de captura descritos neste artigo requerem a primeira etapa de inicialização do objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) chamando o construtor e, em seguida, chamando [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync). Uma vez que o objeto **MediaCapture** será acessado de vários lugares no seu aplicativo, declare uma variável de classe para armazenar o objeto.  Implemente um manipulador para o evento [**Failed** ](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed) do objeto **MediaCapture** para ser notificado se houver falha em uma operação de captura.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Configurar a visualização da câmera
É possível capturar fotos, vídeos e áudio usando **MediaCapture** sem mostrar a visualização de câmera, mas normalmente você quer mostrar o fluxo de visualização para que o usuário possa ver o que está sendo capturado. Além disso, alguns recursos do **MediaCapture** requerem que o fluxo de visualização esteja em execução antes que eles possam ser habilitados, incluindo o foco automático, a exposição automática e a proporção de branco automática. Para saber como configurar a visualização de câmera, consulte [**Exibir a visualização da câmera**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturar uma foto para um SoftwareBitmap
A classe [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) foi introduzida no Windows 10 para fornecer uma representação comum de imagens em vários recursos. Se deseja capturar uma foto e usar imediatamente a imagem capturada em seu aplicativo, como exibi-la em XAML, em vez de capturar para um arquivo, você deve capturar para um **SoftwareBitmap**. Você ainda tem a opção de salvar a imagem em disco posteriormente.

Após inicializar o objeto **MediaCapture**, você pode capturar uma foto para um **SoftwareBitmap** usando a classe [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture). Obtenha uma instância dessa classe chamando [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync), passando um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) e especificando o formato de imagem desejado. [**CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) cria uma codificação não compactada com o formato de pixel especificado. Capture uma foto chamando [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync), que retorna um objeto [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto). Obtenha um **SoftwareBitmap** acessando a propriedade [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame) e, em seguida, a propriedade [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap).

Se quiser, você pode capturar várias fotos chamando **CaptureAsync** repetidamente. Quando terminar a captura, chame [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) para encerrar a sessão **LowLagPhotoCapture** e liberar os recursos associados. Depois de chamar **FinishAsync**, para iniciar a captura de fotos novamente, você precisará chamar [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) novamente para reinicializar a sessão de captura, antes de chamar [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync).

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

Para saber mais sobre como trabalhar com o objeto **SoftwareBitmap**, incluindo como exibir um deles em uma página XAML, consulte [**Criar, editar e salvar imagens de bitmap**](imaging.md).

## <a name="capture-a-photo-to-a-file"></a>Capturar uma foto para um arquivo
Um aplicativo típico de fotografia salvará uma foto capturada em disco ou em um armazenamento em nuvem e será necessário adicionar os metadados, como a orientação da foto, ao arquivo. O exemplo a seguir mostra como capturar uma foto para um arquivo. Você ainda tem a opção de criar um **SoftwareBitmap** do arquivo de imagem posteriormente. 

A técnica mostrada neste exemplo captura a foto para um fluxo de memória e transcodifica a foto do fluxo para um arquivo no disco. Este exemplo usa [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) para obter a biblioteca de imagens do usuário e a propriedade [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder) para obter uma pasta padrão de salvamento de referência. Lembre-se de adicionar a funcionalidade **Pictures Library** ao manifesto do aplicativo para acessar essa pasta. [**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) cria um novo [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) no qual a mídia capturada será salva.

Crie um [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream) e chame [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) para capturar uma foto para o fluxo, passando o fluxo e um objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique o formato de imagem a ser usado. Você pode criar propriedades de codificação personalizadas inicializando o objeto por conta própria, mas a classe fornece métodos estáticos, como [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg) para formatos de codificação comuns. Em seguida, crie um fluxo de arquivo para o arquivo de saída chamando [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync). Crie um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) para decodificar a imagem a partir do fluxo de memória e crie um [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) para codificar a imagem para o arquivo. Basta chamar [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync).

Opcionalmente, você pode criar um objeto [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet) e, em seguida, chamar [**SetPropertiesAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/br226252.aspx) no codificador de imagem para incluir metadados sobre a foto no arquivo de imagem. Para saber mais sobre propriedades de codificação, consulte [**Metadados de imagem**](image-metadata.md). Tratar a orientação do dispositivo corretamente é essencial para a maioria dos aplicativos de fotografia. Para saber mais, consulte [**Tratar a orientação do dispositivo com MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por fim, chame [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) no objeto codificador para transcodificar a foto do fluxo de memória para o arquivo.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Para saber mais sobre como trabalhar com arquivos e pastas, consulte [**Arquivos, pastas e bibliotecas**](https://msdn.microsoft.com/windows/uwp/files/index).

## <a name="capture-a-video"></a>Capturar um vídeo
Adicione capturas de vídeo ao seu aplicativo com rapidez usando a classe [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording). Primeiro, declare uma variável de classe para o objeto.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Em seguida, crie um objeto **StorageFile** no qual o vídeo será salvo. Observe que, para salvar na biblioteca de vídeos do usuário, conforme mostrado neste exemplo, você deve adicionar a funcionalidade **Videos Library** ao manifesto do aplicativo. Chame [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) para inicializar a gravação da mídia, passando o arquivo de armazenamento e um objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) que especifique a codificação do vídeo. A classe fornece os métodos estáticos, como [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4), para criar perfis de codificação de vídeo comuns.

Por fim, chame [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) para iniciar a captura de vídeo.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Para parar a gravação de vídeo, chame [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Você pode continuar a chamar **StartAsync** e **StopAsync** para capturar vídeos adicionais. Quando terminar de capturar vídeos, chame [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) para descartar a sessão de captura e limpar os recursos associados. Após essa chamada, você deve chamar **PrepareLowLagRecordToStorageFileAsync** novamente para reinicializar a sessão de captura, antes de chamar **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Ao capturar vídeo, você deve registrar um manipulador para o evento [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded) do objeto **MediaCapture** que será gerado pelo sistema operacional se você ultrapassar o limite para uma única gravação, que atualmente é de três horas. No manipulador para o evento, você deve finalizar a gravação chamando [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

## <a name="pause-and-resume-video-recording"></a>Pausar e retomar a gravação de vídeos
Você pode pausar uma gravação de vídeo e depois retomá-la sem criar um arquivo separado de saída. Basta chamar [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync) e, em seguida, chamar [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync).

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

A partir do Windows 10, versão 1607, você pode pausar uma gravação de vídeo e receber o último quadro capturado antes da gravação ter sido pausada. Você pode sobrepor esse quadro na visualização de câmera para permitir que o usuário alinhe a câmera com o quadro pausado antes de continuar a gravação. Chamar [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) retorna um objeto [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult). A propriedade [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame) é um objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame) que representa o último quadro. Para exibir o quadro em XAML, obtenha a representação **SoftwareBitmap** do quadro do vídeo. No momento, somente imagens no formato BGRA8 com canal alfa pré-multiplicado ou vazio são compatíveis, portanto, chame [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) para obter o formato correto, se necessário.  Crie um novo objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) e chame [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) para inicializá-lo. Por fim, defina a propriedade **Source** de um controle [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) de XAML para exibir a imagem. Para que esse truque funcione, sua imagem deve estar alinhada ao controle **CaptureElement** e deve ter um valor de opacidade menor do que um. Não se esqueça de que você pode modificar apenas a interface do usuário no thread da interface do usuário, portanto faça essa chamada dentro de [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync).

**PauseWithResultAsync** também retorna a duração do vídeo que foi gravado no segmento precedente, caso seja necessário monitorar o tempo total de gravação.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Quando retomar a gravação, você pode definir a origem da imagem como nula e ocultá-la.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Observe que você também pode obter um quadro de resultado quando interromper o vídeo chamando [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync).


## <a name="capture-audio"></a>Capturar áudio 
Você pode adicionar rapidamente capturas de áudio ao seu aplicativo usando a mesma técnica mostrada acima para a captura de vídeos. O exemplo abaixo cria um **StorageFile** na pasta de dados do aplicativo. Chame [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) para inicializar a sessão de captura, passando o arquivo e um [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile), que é gerado neste exemplo pelo método estático [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3). Para começar a gravar, chame [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync).

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Chame [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync) para parar a gravação de áudio.

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Você pode chamar **StartAsync** e **StopAsync** várias vezes para gravar vários arquivos de áudio. Quando terminar de capturar o áudio, chame [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) para descartar a sessão de captura e limpar os recursos associados. Após essa chamada, você deve chamar **PrepareLowLagRecordToStorageFileAsync** novamente para reinicializar a sessão de captura, antes de chamar **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Tratar a orientação do dispositivo com MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Criar, editar e salvar imagens de bitmap](imaging.md)
* [Arquivos, pastas e bibliotecas](https://msdn.microsoft.com/windows/uwp/files/index)




<!--HONumber=Dec16_HO1-->


