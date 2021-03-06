---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: Este artigo mostra a maneira mais simples para capturar fotos e vídeos usando a classe MediaCapture.
title: Captura básica de fotos, áudio e vídeo com MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d758a19800f52284011b3260f83826f7cde300f3
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683899"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Captura básica de fotos, áudio e vídeo com MediaCapture


Este artigo mostra a maneira mais simples para capturar fotos e vídeos usando a classe [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture). A classe **MediaCapture** expõe um conjunto robusto de APIs que fornecem controle aprofundado sobre o pipeline de captura e permite a captura de cenários avançados, mas este artigo se destina a ajudá-lo a adicionar captura de mídia básica ao seu aplicativo com rapidez e facilidade. Para saber mais sobre os recursos que a classe **MediaCapture** fornece, consulte [**Câmera**](camera.md).

Se você simplesmente deseja tirar uma foto ou fazer um vídeo e não pretende adicionar recursos de captura de mídias adicionais, ou se não quer criar sua própria interface do usuário da câmera, talvez você prefira usar a classe [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI), que permite simplesmente iniciar o aplicativo integrado de câmera do Windows e receber o arquivo de foto ou vídeo que foi capturado. Para saber mais, consulte [**Capturar fotos e vídeos com a interface do usuário da câmera interna do Windows**](capture-photos-and-video-with-cameracaptureui.md)

O código neste artigo foi adaptado do exemplo [**Kit inicial de câmera**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit). Você pode baixar a amostra para ver o código usado no contexto ou usar a amostra como ponto de partida para seu próprio aplicativo.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Adicionar declarações de funcionalidades ao manifesto do aplicativo

Para que seu app acesse a câmera do dispositivo, você deve declarar que o app usa as funcionalidades de *webcam* e *microphone* do dispositivo. Se quiser salvar as fotos e os vídeos capturados na biblioteca de imagens ou de vídeos do usuário, você deverá declarar também as funcionalidades *picturesLibrary* e *videosLibrary*.

**Para adicionar recursos ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar a biblioteca de Imagens e Vídeos, marque as caixas da **Biblioteca de imagens** e da **Biblioteca de vídeos**.


## <a name="initialize-the-mediacapture-object"></a>Inicializar o objeto MediaCapture
Todos os métodos de captura descritos neste artigo requerem a primeira etapa de inicialização do objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) chamando o construtor e, em seguida, chamando [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync). Uma vez que o objeto **MediaCapture** será acessado de vários lugares no seu aplicativo, declare uma variável de classe para armazenar o objeto.  Implemente um manipulador para o evento [**Failed**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.failed) do objeto **MediaCapture** para ser notificado se houver falha em uma operação de captura.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Configurar a visualização da câmera
É possível capturar fotos, vídeos e áudio usando **MediaCapture** sem mostrar a visualização de câmera, mas normalmente você quer mostrar o fluxo de visualização para que o usuário possa ver o que está sendo capturado. Além disso, alguns recursos do **MediaCapture** requerem que o fluxo de visualização esteja em execução antes que eles possam ser habilitados, incluindo o foco automático, a exposição automática e a proporção de branco automática. Para saber como configurar a visualização de câmera, consulte [**Exibir a visualização da câmera**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturar uma foto para um SoftwareBitmap
A classe [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) foi introduzida no Windows 10 para fornecer uma representação comum de imagens em vários recursos. Se deseja capturar uma foto e usar imediatamente a imagem capturada em seu aplicativo, como exibi-la em XAML, em vez de capturar para um arquivo, você deve capturar para um **SoftwareBitmap**. Você ainda tem a opção de salvar a imagem em disco posteriormente.

Após inicializar o objeto **MediaCapture**, você pode capturar uma foto para um **SoftwareBitmap** usando a classe [**LowLagPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.LowLagPhotoCapture). Obtenha uma instância dessa classe chamando [**PrepareLowLagPhotoCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync), passando um objeto [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) e especificando o formato de imagem desejado. [**CreateUncompressed**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed) cria uma codificação descompactada com o formato de pixel especificado. Capture uma foto chamando [**CaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotocapture.captureasync), que retorna um objeto [**CapturedPhoto**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedPhoto). Obtenha um **SoftwareBitmap** acessando a propriedade [**Frame**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedphoto.frame) e, em seguida, a propriedade [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.softwarebitmap).

Se quiser, você pode capturar várias fotos chamando **CaptureAsync** repetidamente. Quando terminar a captura, chame [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.advancedphotocapture.finishasync) para encerrar a sessão **LowLagPhotoCapture** e liberar os recursos associados. Depois de chamar **FinishAsync**, para iniciar a captura de fotos novamente, você precisará chamar [**PrepareLowLagPhotoCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) novamente para reinicializar a sessão de captura, antes de chamar [**CaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotocapture.captureasync).

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

A partir do Windows, versão 1803, você pode acessar a propriedade [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) da classe **CapturedFrame** retornada de **CaptureAsync** para recuperar metadados da foto capturada. Você pode passar esses dados para um **BitmapEncoder** a fim de salvar os metadados em um arquivo. Anteriormente, não havia nenhuma maneira de acessar esses dados para formatos de imagem descompactada. Você também pode acessar a propriedade [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues) a fim de recuperar um objeto [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues) que descreve os valores de controle, como exposição e controle de branco, da estrutura capturada.

Para saber mais sobre como usar o **BitmapEncoder** e trabalhar com o objeto **SoftwareBitmap**, incluindo como exibir um deles em uma página XAML, consulte [**Criar, editar e salvar imagens de bitmap**](imaging.md). 

Para saber mais sobre valores de controle de dispositivo de captura de configuração, consulte [Controles de dispositivo de captura para fotos e vídeos](capture-device-controls-for-photo-and-video-capture.md).

A partir do Windows 10, versão 1803, você pode obter os metadados, como informações de EXIF, para fotos capturadas no formato não compactado ao acessar a propriedade [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de **CapturedFrame** retornado por **MediaCapture**. Em versões anteriores, esses dados estavam acessíveis somente no cabeçalho de fotos capturadas em um formato de arquivo compactado. Você pode fornecer esses dados para um [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder) ao escrever manualmente um arquivo de imagem. Para obter mais informações sobre como codificar bitmaps, consulte [Criar, editar e salvar imagens de bitmap](imaging.md).  Você também pode acessar os valores de controle de quadro, como as configurações de exposição e flash, usados quando a imagem foi capturada ao acessar a propriedade [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues). Para obter mais informações, consulte [Controles do dispositivo para a captura de fotos e vídeo](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Capturar uma foto para um arquivo
Um aplicativo típico de fotografia salvará uma foto capturada em disco ou em um armazenamento em nuvem e será necessário adicionar os metadados, como a orientação da foto, ao arquivo. O exemplo a seguir mostra como capturar uma foto para um arquivo. Você ainda tem a opção de criar um **SoftwareBitmap** do arquivo de imagem posteriormente. 

A técnica mostrada neste exemplo captura a foto para um fluxo de memória e transcodifica a foto do fluxo para um arquivo no disco. Este exemplo usa [**GetLibraryAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.getlibraryasync) para obter a biblioteca de imagens do usuário e a propriedade [**SaveFolder**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.savefolder) para obter uma pasta padrão de salvamento de referência. Lembre-se de adicionar a funcionalidade **Pictures Library** ao manifesto do aplicativo para acessar essa pasta. O [**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync) cria um novo [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) no qual a foto será salva.

Crie um [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) e chame [**CapturePhotoToStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) para capturar uma foto para o fluxo, passando o fluxo e um objeto [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique o formato de imagem a ser usado. Você pode criar propriedades de codificação personalizadas inicializando o objeto por conta própria, mas a classe fornece métodos estáticos, como [**ImageEncodingProperties.CreateJpeg**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg) para formatos de codificação comuns. Em seguida, crie um fluxo de arquivo para o arquivo de saída chamando [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync). Crie um [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para decodificar a imagem a partir do fluxo de memória e crie um [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) para codificar a imagem para o arquivo. Basta chamar [**CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync).

Opcionalmente, você pode criar um objeto [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) e, em seguida, chamar [**SetPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) no codificador de imagem para incluir metadados sobre a foto no arquivo de imagem. Para saber mais sobre propriedades de codificação, consulte [**Metadados de imagem**](image-metadata.md). Tratar a orientação do dispositivo corretamente é essencial para a maioria dos aplicativos de fotografia. Para saber mais, consulte [**Tratar a orientação do dispositivo com MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por fim, chame [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) no objeto codificador para transcodificar a foto do fluxo de memória para o arquivo.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Para saber mais sobre como trabalhar com arquivos e pastas, consulte [**Arquivos, pastas e bibliotecas**](https://docs.microsoft.com/windows/uwp/files/index).

## <a name="capture-a-video"></a>Capturar um vídeo
Adicione capturas de vídeo ao seu aplicativo com rapidez usando a classe [**LowLagMediaRecording**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.LowLagMediaRecording). Primeiro, declare uma variável de classe para o objeto.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

Em seguida, crie um objeto **StorageFile** no qual o vídeo será salvo. Observe que, para salvar na biblioteca de vídeos do usuário, conforme mostrado neste exemplo, você deve adicionar a funcionalidade **Videos Library** ao manifesto do aplicativo. Chame [**PrepareLowLagRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) para inicializar a gravação da mídia, passando o arquivo de armazenamento e um objeto [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) que especifique a codificação do vídeo. A classe fornece os métodos estáticos, como [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), para criar perfis de codificação de vídeo comuns.

Por fim, chame [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.startasync) para iniciar a captura de vídeo.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Para parar a gravação de vídeo, chame [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Você pode continuar a chamar **StartAsync** e **StopAsync** para capturar vídeos adicionais. Quando terminar de capturar vídeos, chame [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) para descartar a sessão de captura e limpar os recursos associados. Após essa chamada, você deve chamar **PrepareLowLagRecordToStorageFileAsync** novamente para reinicializar a sessão de captura, antes de chamar **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

Ao capturar vídeo, você deve registrar um manipulador para o evento [**RecordLimitationExceeded**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded) do objeto **MediaCapture** que será gerado pelo sistema operacional se você ultrapassar o limite para uma única gravação, que atualmente é de três horas. No manipulador para o evento, você deve finalizar a gravação chamando [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Reproduzir e editar arquivos de vídeo capturados
Depois de capturar um vídeo em um arquivo, convém carregar o arquivo e reproduzi-lo na interface do usuário do aplicativo. Você pode fazer isso usando o controle XAML **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** e um **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** associado. Para obter mais informações sobre a reprodução de mídia em uma página XAML, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md).

Você também pode criar um objeto **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)** a partir de um arquivo de vídeo ao chamar **[CreateFromFileAsync](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)** .  Uma **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** fornece funcionalidade de edição de vídeo básica, como organizar a sequência de objetos **MediaClip**, corte da duração do vídeo, criação de camadas, adicionar música de fundo e aplicar efeitos de vídeo. Para saber mais sobre como trabalhar com composições de mídia, consulte [Composições de mídia e edição](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Pausar e retomar a gravação de vídeos
Você pode pausar uma gravação de vídeo e depois retomá-la sem criar um arquivo separado de saída. Basta chamar [**PauseAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync) e, em seguida, chamar [**ResumeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync).

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

A partir do Windows 10, versão 1607, você pode pausar uma gravação de vídeo e receber o último quadro capturado antes da gravação ter sido pausada. Você pode sobrepor esse quadro na visualização de câmera para permitir que o usuário alinhe a câmera com o quadro pausado antes de continuar a gravação. Chamar [**PauseWithResultAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) retorna um objeto [**MediaCapturePauseResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapturePauseResult). A propriedade [**LastFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe) é um objeto [**VideoFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) que representa o último quadro. Para exibir o quadro em XAML, obtenha a representação **SoftwareBitmap** do quadro do vídeo. No momento, somente imagens no formato BGRA8 com canal alfa pré-multiplicado ou vazio são compatíveis, portanto, chame [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) para obter o formato correto, se necessário.  Crie um novo objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) e chame [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para inicializá-lo. Por fim, defina a propriedade **Source** de um controle [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) de XAML para exibir a imagem. Para que esse truque funcione, sua imagem deve estar alinhada ao controle **CaptureElement** e deve ter um valor de opacidade menor do que um. Não se esqueça de que você pode modificar apenas a interface do usuário no thread da interface do usuário, portanto faça essa chamada dentro de [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync).

**PauseWithResultAsync** também retorna a duração do vídeo que foi gravado no segmento precedente, caso seja necessário monitorar o tempo total de gravação.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Quando retomar a gravação, você pode definir a origem da imagem como nula e ocultá-la.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Observe que você também pode obter um quadro de resultado quando interromper o vídeo chamando [**StopWithResultAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync).


## <a name="capture-audio"></a>Capturar áudio 
Você pode adicionar rapidamente capturas de áudio ao seu aplicativo usando a mesma técnica mostrada acima para a captura de vídeos. O exemplo abaixo cria um **StorageFile** na pasta de dados do aplicativo. Chame [**PrepareLowLagRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) para inicializar a sessão de captura, passando o arquivo e um [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile), que é gerado neste exemplo pelo método estático [**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3). Para começar a gravar, chame [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.startasync).

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Chame [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) para parar a gravação de áudio.

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)  
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Você pode chamar **StartAsync** e **StopAsync** várias vezes para gravar vários arquivos de áudio. Quando terminar de capturar o áudio, chame [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) para descartar a sessão de captura e limpar os recursos associados. Após essa chamada, você deve chamar **PrepareLowLagRecordToStorageFileAsync** novamente para reinicializar a sessão de captura, antes de chamar **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar e responder às alterações no nível de áudio pelo sistema
A partir do Windows 10, versão 1803, seu aplicativo pode detectar quando o sistema reduz o nível ou ativa o mudo da captura de áudio do aplicativo e do streaming de renderização de áudio. Por exemplo, o sistema pode ativar o mudo dos fluxos do aplicativo quando ele entra em segundo plano. A classe [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) permite que você registre-se para receber um evento quando o sistema modifica o volume de um fluxo de áudio. Obtenha uma instância de **AudioStateMonitor** para monitorar fluxos de captura de áudio ao chamar [**CreateForCaptureMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring). Obter uma instância para monitoramento de fluxos de renderização de áudio chamando [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring). Registre um manipulador para o evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) de cada monitor para ser notificado quando o áudio da categoria de streaming correspondente é alterado pelo sistema.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

No manipulador de eventos **SoundLevelChanged** do stream de captura, você pode verificar a propriedade [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) do remetente de **AudioStateMonitor** para determinar o novo nível de som. Observe que um fluxo de captura nunca deve ser reduzido ou "ignorado" pelo sistema. Ele deve ser apenas desativado ou retornado ao volume completo. Se o fluxo de áudio for silenciado, você pode parar uma captura em andamento. Se o fluxo de áudio tiver o volume restaurado, você pode começar a capturar novamente. O exemplo a seguir usa algumas variáveis de classe booliana para controlar se o aplicativo está capturando áudio e se a captura foi interrompida devido à alteração de estado de áudio. Essas variáveis são usadas para determinar quando é apropriado parar ou iniciar a captura de áudio de forma programática.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

O exemplo de código a seguir ilustra uma implementação do manipulador **SoundLevelChanged** para renderização de áudio. Dependendo do cenário de aplicativo e do tipo de conteúdo que você está reproduzindo, convém pausar a reprodução de áudio quando o nível de som é ignorado. Para obter mais informações sobre como processar as alterações no nível de sons para reprodução de mídia, consulte [Reproduzir áudio e vídeo com o MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Capture fotos e vídeos com a interface do usuário da câmera interna do Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Manipular a orientação do dispositivo com MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Criar, editar e salvar imagens de bitmap](imaging.md)
* [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/index)

