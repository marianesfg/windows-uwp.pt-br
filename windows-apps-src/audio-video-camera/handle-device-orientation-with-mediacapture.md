---
author: drewbatgit
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: Este artigo mostra como tratar a orientação do dispositivo ao capturar fotos e vídeos usando uma classe auxiliar.
title: Tratar a orientação do dispositivo com MediaCapture
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1367c880bd6dde573ab4fc30733ed9d1fefa6b0b
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7104832"
---
# <a name="handle-device-orientation-with-mediacapture"></a>Tratar a orientação do dispositivo com MediaCapture
Quando seu aplicativo captura uma foto ou vídeo que deve ser exibido fora do aplicativo, como salvar um arquivo no dispositivo do usuário ou compartilhar online, é importante que você codifique a imagem com os metadados de orientação adequados para que quando outro aplicativo ou dispositivo exibir a imagem, ela fique na posição correta. Determinar os dados de orientação corretos para incluir em um arquivo de mídia pode ser uma tarefa complexa porque há várias variáveis a serem consideradas, inclusive a orientação do chassi do dispositivo, a orientação da tela e o posicionamento da câmera no chassi (se é uma câmera frontal ou traseira). 

Para simplificar o processo de manipulação da orientação, recomendamos usar uma classe auxiliar, **CameraRotationHelper**, cuja definição completa está no final deste artigo. Você pode adicionar essa classe ao seu projeto e seguir as etapas neste artigo para adicionar suporte para orientação ao seu aplicativo de câmera. A classe auxiliar também torna mais fácil para você girar os controles na interface do usuário da câmera para que eles sejam renderizados corretamente do ponto de vista do usuário.

> [!NOTE] 
> Este artigo se baseia no código e nos conceitos abordados no artigo [**Captura básica de fotos, áudio e vídeo com o MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). Recomendamos que você se familiarize com os conceitos básicos do uso da classe **MediaCapture** antes de adicionar suporte para orientação ao seu aplicativo.

## <a name="namespaces-used-in-this-article"></a>Namespaces usados neste artigo
O exemplo de código neste artigo usa APIs dos namespaces a seguir que você deve incluir em seu código. 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

A primeira etapa da adição de suporte para orientação ao seu aplicativo é bloquear a tela para que ela não gire automaticamente quando o dispositivo for girado. A rotação automática da interface do usuário funciona muito bem para a maioria dos tipos de aplicativos, mas ele não é intuitiva para os usuários quando a visualização da câmera gira. Bloqueie a orientação da tela definindo a propriedade [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) como [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations). 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## <a name="tracking-the-camera-device-location"></a>Rastreando a localização do dispositivo de câmera
Para calcular a orientação correta para a mídia capturada, seu aplicativo deve determinar a localização do dispositivo de câmera no chassi. Adicione uma variável de membro booleano para controlar se a câmera é externa para o dispositivo, como uma webcam USB. Adicione outra variável booleana para controlar se a visualização deve ser espelhada, que é o caso quando uma câmera frontal é usada. Além disso, adicione uma variável para armazenar um objeto **DeviceInformation** que representa a câmera selecionada.

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>Selecionar um dispositivo de câmera e inicializar o objeto MediaCapture
O artigo [**Captura básica de fotos, áudio e vídeo com o MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) mostra como inicializar o objeto **MediaCapture** com apenas algumas linhas de código. Para dar suporte à orientação da câmera, adicionaremos mais algumas etapas ao processo de inicialização.

Primeiro, chame [**Findallasync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) passando o seletor de dispositivo [**DeviceClass.VideoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceClass) para obter uma lista de todos os dispositivos de captura de vídeo disponíveis. Em seguida, selecione o primeiro dispositivo na lista onde a localização do painel da câmera é conhecida e onde corresponde ao valor fornecido, que, neste exemplo, é uma câmera frontal. Se nenhuma câmera for encontrada no painel desejado, a primeira ou a câmera disponível padrão será usada.

Se um dispositivo de câmera for encontrado, um novo objeto [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings) é criado e a propriedade [**VideoDeviceId**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.VideoDeviceId) é definida como o dispositivo selecionado. Em seguida, crie o objeto MediaCapture e chame [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync), passando o objeto de configurações para dizer para o sistema usar a câmera selecionada.

Por fim, verifique se o painel de dispositivo selecionado é nulo ou desconhecido. Se for, a câmera é externa, o que significa que sua rotação não está relacionada à rotação do dispositivo. Se o painel for conhecido e estiver na parte frontal do chassi do dispositivo, sabemos que a visualização deve ser espelhada, então, a variável que controla isso é definida.

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## <a name="initialize-the-camerarotationhelper-class"></a>Inicializar a classe CameraRotationHelper

Agora podemos começar a usar a classe **CameraRotationHelper**. Declare uma variável de membro de classe para armazenar o objeto. Chame o construtor, passando a localização do compartimento da câmera selecionada. A classe auxiliar usa essas informações para calcular a orientação correta para a mídia capturada, o fluxo de visualização e a interface do usuário. Registre um manipulador para o evento **OrientationChanged** da classe auxiliar, que será gerado quando precisarmos atualizar a orientação da interface do usuário ou o fluxo de visualização.

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>Adicionar dados de orientação ao fluxo de visualização da câmera
A adição da orientação correta aos metadados do fluxo de visualização não afeta como a visualização é exibida ao usuário, mas ajuda o sistema a codificar os quadros capturados do fluxo de visualização corretamente.

Você inicia a visualização da câmera chamando [**MediaCapture.StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartPreviewAsync). Antes de fazer isso, verifique na variável de membro se a visualização deve ser espelhada (para uma câmera frontal). Nesse caso, defina a propriedade [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.FlowDirection) do [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.CaptureElement), denominado *PreviewControl* neste exemplo, como [**RightToLeft**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FlowDirection). Depois de iniciar a visualização, chame o método auxiliar **SetPreviewRotationAsync** para definir a rotação da visualização. Depois vem a implementação desse método.

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

Definimos a rotação da visualização em um método separado para que ele possa ser atualizado quando a orientação do telefone muda sem reinicializar o fluxo de visualização. Se a câmera for externa para o dispositivo, nenhuma ação será executada. Caso contrário, o método **CameraRotationHelper** **GetCameraPreviewOrientation** é chamado e retorna a orientação adequada para o fluxo de visualização. 

Para definir os metadados, as propriedades do fluxo de visualização são recuperadas chamando [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.VideoDeviceController.GetMediaStreamProperties). Em seguida, crie o GUID que representa o atributo MFT (Media Foundation Transform) para a rotação do fluxo de vídeo. Em C++, você pode usar a constante [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/library/windows/desktop/hh162880.aspx), mas em C#, você deve especificar manualmente o valor de GUID. 

Adicione um valor de propriedade ao objeto de propriedades do fluxo, especificando o GUID como a chave e a rotação da visualização como o valor. Essa propriedade espera valores em unidades de graus no sentido anti-horário, então, o método **CameraRotationHelper** **ConvertSimpleOrientationToClockwiseDegrees** é usado para converter o valor de orientação simples. Por fim, chame [**SetEncodingPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.SetEncodingPropertiesAsync(Windows.Media.Capture.MediaStreamType,Windows.Media.MediaProperties.IMediaEncodingProperties,Windows.Media.MediaProperties.MediaPropertySet)) para aplicar a nova propriedade de rotação ao fluxo.

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

Em seguida, adicione o manipulador ao evento **CameraRotationHelper.OrientationChanged**. Esse evento passa um argumento que permite que você saiba se o fluxo de visualização precisa ser girado. Se a orientação do dispositivo for alterada para virado para cima ou virado para baixo, esse valor será false. Se a visualização precisar ser girada, chame o **SetPreviewRotationAsync** que foi definido anteriormente.

Em seguida, no manipulador de eventos **OrientationChanged**, atualize sua interface do usuário, se necessário. Obtenha a atual orientação da interface do usuário recomendada da classe auxiliar chamando **GetUIOrientation** e converta o valor em graus no sentido horário, que é usado para transformações de XAML. Crie um [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.RotateTransform) do valor de orientação e defina a propriedade [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.RenderTransform) dos controles XAML. Dependendo do layout da interface do usuário, talvez seja necessário fazer ajustes adicionais aqui além de simplesmente girar os controles. Além disso, lembre-se de que todas as atualizações da interface do usuário devem ser feitas no thread da interface do usuário, então, você deve colocar esse código dentro de uma chamada para [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority,Windows.UI.Core.DispatchedHandler)).  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## <a name="capture-a-photo-with-orientation-data"></a>Capturar uma foto com dados de orientação
O artigo [**Captura básica de fotos, áudio e vídeo com o MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) mostra como capturar uma foto em um arquivo capturando primeiro em fluxo de memória e, em seguida, usando um decodificador para ler os dados da imagem do fluxo e um codificador para transcodificar os dados de imagem para um arquivo. Os dados de orientação, obtidos da classe **CameraRotationHelper**, podem ser adicionados ao arquivo de imagem durante a operação de transcodificação.

No exemplo a seguir, uma foto é capturada em um [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream) com uma chamada para [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) e um [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) é criado a partir do fluxo. Depois, um [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) é criado e aberto para recuperar um [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.IRandomAccessStream) para gravar no arquivo. 

Antes de transcodificar o arquivo, a orientação da foto é recuperada do método de classe auxiliar **GetCameraCaptureOrientation**. Esse método retorna um objeto [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientation) que é convertido em um objeto [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.FileProperties.PhotoOrientation) com o método auxiliar **ConvertSimpleOrientationToPhotoOrientation**. Em seguida, um novo objeto **BitmapPropertySet** é criado e uma propriedade é adicionada em que a chave é "System.Photo.Orientation" e o valor é a orientação da foto, expressa como **BitmapTypedValue**. "System.Photo.Orientation" é uma das muitas propriedades do Windows que podem ser adicionadas como metadados a um arquivo de imagem. Para obter uma lista de todas as propriedades relacionadas a fotos, consulte [**Propriedades do Windows – Foto**](https://msdn.microsoft.com/library/windows/desktop/ff516600). Para saber mais sobre como trabalhar com metadados de imagens, consulte [**Metadados de imagem**](image-metadata.md).

Por fim, o conjunto de propriedades que inclui os dados de orientação é definido para o codificador com uma chamada para [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/) e a imagem é transcodificada com uma chamada para [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync).

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## <a name="capture-a-video-with-orientation-data"></a>Capturar um vídeo com dados de orientação
A captura básica de vídeo é descrita no artigo [**Captura básica de fotos, áudio e vídeo com o MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). A adição de dados de orientação à codificação do vídeo capturado é feita usando a mesma técnica descrita anteriormente na seção sobre adição de dados de orientação ao fluxo de visualização.

No exemplo a seguir, cria-se um arquivo no qual o vídeo capturado será gravado. Um perfil de codificação MP4 é criado usando o método estático [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4). A orientação adequada para o vídeo é obtida da classe **CameraRotationHelper** com uma chamada para **GetCameraCaptureOrientation** porque a propriedade de rotação requer que a orientação seja expressa em graus no sentido anti-horário, o método auxiliar **ConvertSimpleOrientationToClockwiseDegrees** é chamado para converter o valor da orientação. Em seguida, crie o GUID que representa o atributo MFT (Media Foundation Transform) para a rotação do fluxo de vídeo. Em C++, você pode usar a constante [**MF_MT_VIDEO_ROTATION**](https://msdn.microsoft.com/library/windows/desktop/hh162880.aspx), mas em C#, você deve especificar manualmente o valor de GUID. Adicione um valor de propriedade ao objeto de propriedades do fluxo, especificando o GUID como a chave e a rotação como o valor. Por fim, chame [**StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.StartRecordToStorageFileAsync(Windows.Media.MediaProperties.MediaEncodingProfile,Windows.Storage.IStorageFile)) para começar a gravar um vídeo codificado com os dados de orientação.

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## <a name="camerarotationhelper-full-code-listing"></a>Listagem do código completo CameraRotationHelper
O trecho de código a seguir lista o código completo para a classe **CameraRotationHelper** que gerencia os sensores de orientação de hardware, calcula os valores de orientação adequados para fotos e vídeos e fornece métodos auxiliares para converter entre as diferentes representações de orientação que são usadas pelos diferentes recursos do Windows. Se você seguir as diretrizes mostradas no artigo acima, poderá adicionar essa classe ao seu projeto como está, sem precisar fazer alterações. Obviamente, fique à vontade para personalizar o código a seguir de acordo com as necessidades de seu cenário específico.

Essa classe auxiliar usa o [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Sensors.SimpleOrientationSensor) do dispositivo para determinar a orientação atual do chassi do dispositivo e a classe [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation) para determinar a orientação atual da tela. Cada uma dessas classes fornece eventos que são gerados quando a orientação atual muda. O painel no qual o dispositivo de captura está montado – frontal, traseiro ou externo – é usado para determinar se o fluxo de visualização deve ser espelhado. Além disso, a propriedade [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.EnclosureLocation.RotationAngleInDegreesClockwise), compatível com alguns dispositivos, é usada para determinar a orientação em que a câmera está montada no chassi.

Os métodos a seguir podem ser usados para obter valores de orientação recomendados para as tarefas especificadas do aplicativo de câmera:

* **GetUIOrientation** – Retorna a orientação sugerida para os elementos da interface do usuário da câmera.
* **GetCameraCaptureOrientation** – Retorna a orientação sugerida para codificar os metadados de imagem.
* **GetCameraPreviewOrientation** – Retorna a orientação sugerida para o fluxo de visualização para oferecer uma experiência natural ao usuário.

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




