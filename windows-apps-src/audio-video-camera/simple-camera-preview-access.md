---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera em uma página XAML em um aplicativo UWP (Plataforma Universal do Windows).
title: Exibir a visualização da câmera
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7fba78a619f18d7da2e190758d73ac7a56b12fb9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360662"
---
# <a name="display-the-camera-preview"></a>Exibir a visualização da câmera


Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera em uma página XAML em um aplicativo UWP (Plataforma Universal do Windows). Criar um aplicativo que captura fotos e vídeos usando a câmera exige que você realize tarefas como manipular a orientação do dispositivo e da câmera ou definir opções de codificação para o arquivo capturado. Em alguns cenários de aplicativo, talvez você queira simplesmente mostrar o fluxo de visualização da câmera sem se preocupar com essas outras considerações. Este artigo mostra como fazer isso com um mínimo de código. Observe que você deve sempre desligar o fluxo de visualização corretamente quando o tiver concluído, seguindo as etapas abaixo.

Para saber mais sobre como desenvolver um aplicativo de câmera que capture fotos ou vídeos, consulte [Captura básica de fotos, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Adicionar declarações de funcionalidades ao manifesto do aplicativo

Para que seu app acesse a câmera do dispositivo, você deve declarar que o app usa as funcionalidades de *webcam* e *microphone* do dispositivo. 

**Adicionar recursos ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.

## <a name="add-a-captureelement-to-your-page"></a>Adicionar um CaptureElement à sua página

Use um [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) para exibir o fluxo de visualização na página XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Usar o MediaCapture para iniciar o fluxo de visualização

O objeto [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) é a interface do seu aplicativo para a câmera do dispositivo. Esta classe é um membro do namespace Windows.Media.Capture. O exemplo deste artigo também usa APIs dos namespaces [**Windows.ApplicationModel**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel) e [System.Threading.Tasks](https://docs.microsoft.com/dotnet/api/system.threading.tasks?redirectedfrom=MSDN), além daqueles incluídos pelo modelo de projeto padrão.

Adicione usando diretivas para incluir os seguintes namespaces no arquivo. cs da sua página.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Declare uma variável de membro de classe para o objeto **MediaCapture** e um valor booliano para controlar se a câmera está visualizando no momento. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Declare uma variável do tipo [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) que será usada para garantir que a exibição não seja desativada enquanto a visualização estiver em execução.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Crie um método auxiliar para iniciar a visualização de câmera, chamada **StartPreviewAsync** neste exemplo. Dependendo do cenário do aplicativo, você pode querer chamá-lo a partir do manipulador de eventos **OnNavigatedTo** que é chamado quando a página é carregada ou esperar e iniciar a visualização em resposta a eventos da interface do usuário.

Crie uma nova instância da classe **MediaCapture** e chame [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) para inicializar o dispositivo de captura. Esse método pode falhar em dispositivos que não têm uma câmera, por exemplo. Por isso, você deve chamá-lo de dentro de um bloco **try**. Uma **UnauthorizedAccessException** será lançada quando você tentar inicializar a câmera, se o usuário tiver desabilitado o acesso à câmera nas configurações de privacidade do dispositivo. Você também verá essa exceção durante o desenvolvimento, se não tiver adicionado os recursos adequados ao manifesto do seu aplicativo.

**Importante** Em algumas famílias de dispositivos, uma solicitação de consentimento do usuário é exibida para o usuário antes de seu aplicativo receber acesso à câmera do dispositivo. Por esse motivo, você só deve chamar [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) do thread de interface do usuário principal. Tentar iniciar a câmera de outro thread pode resultar em falha de inicialização.

Conecte a **MediaCapture** ao **CaptureElement** definindo a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source). Inicie a visualização chamando [**StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Esse método gerará uma **FileLoadException** se outro app tiver controle exclusivo do dispositivo de captura. Veja a próxima seção para obter informações sobre as alterações no controle exclusivo.

Chame [**RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive) para garantir que o dispositivo não entre em suspensão enquanto a visualização estiver em execução. Por fim, defina a propriedade [**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) como [**Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations) para impedir que a interface do usuário e o **CaptureElement** girem quando o usuário mudar a orientação do dispositivo. Para obter mais informações sobre como lidar com alterações de orientação do dispositivo, consulte [**Manipular a orientação do dispositivo com o MediaCapture**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>Manipular as alterações no controle exclusivo
Conforme mencionado na seção anterior, **StartPreviewAsync** gerará uma **FileLoadException** se outro aplicativo tiver o controle exclusivo do dispositivo de captura. A partir do Windows 10, versão 1703, você pode registrar um manipulador para o evento [MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged), que é acionado sempre que o status de controle exclusivo do dispositivo é alterado. No manipulador para este evento, verifique a propriedade [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) para ver qual é o status atual. Se o novo status for **SharedReadOnlyAvailable**, então você saberá que no momento não é possível iniciar a visualização e talvez convenha atualizar sua interface do usuário para alertar o usuário. Se o novo status for **ExclusiveControlAvailable**, então você poderá tentar iniciar a visualização de câmera novamente.

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>Desligar o fluxo de visualização

Ao terminar de usar o fluxo de visualização, você sempre deve desligar o fluxo e dispor corretamente dos recursos associados para garantir que a câmera esteja disponível para outros aplicativos no dispositivo. As etapas necessárias para desligar o fluxo de visualização são:

-   Se a câmera estiver atualmente em visualização, chame [**StopPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) para interromper o fluxo de visualização. Uma exceção será gerada se você chamar **StopPreviewAsync** enquanto a visualização não estiver em execução.
-   Defina a propriedade [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.captureelement.source) do **CaptureElement** como nulo. Use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para garantir que essa chamada seja executada no thread de interface do usuário.
-   Chame o método [**Dispose**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.dispose) do objeto **MediaCapture** para liberar o objeto. Novamente, use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para garantir que essa chamada seja executada no thread de interface do usuário.
-   Defina a variável do membro do **MediaCapture** como nulo.
-   Chame [**RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease) para permitir que a tela seja desativada quando inativa.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Você deve desligar o fluxo de visualização quando o usuário navegar para fora de sua página, substituindo o método [**OnNavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Você também deve desligar o fluxo de visualização corretamente quando seu aplicativo estiver suspenso. Para fazer isso, registre um manipulador para o evento [**Application.Suspending**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.suspending) no construtor da página.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

No manipulador de eventos **Suspending**, verifique primeiro se a página está exibindo o [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) do aplicativo, comparando o tipo de página com a propriedade [**CurrentSourcePageType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype). Se a página não estiver sendo exibida no momento, o evento **OnNavigatedFrom** já deve tiver sido gerado e o fluxo de visualização encerrado. Se a página estiver sendo exibida no momento, obtenha um objeto [**SuspendingDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.SuspendingDeferral) dos argumentos de evento passados para o manipulador para garantir que o sistema não suspenda seu aplicativo até que o fluxo de visualização seja encerrado. Depois de desligar o fluxo, chame o método [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.suspendingdeferral.complete) de adiamento para permitir que o sistema continue a suspender seu aplicativo.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Obtenha um quadro de visualização](get-a-preview-frame.md)
