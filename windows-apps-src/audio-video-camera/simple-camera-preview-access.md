---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: "Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera em uma página XAML em um aplicativo UWP (Plataforma Universal do Windows)."
title: "Exibir a visualização da câmera"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d65d09349850f580d8bcee2d3875b38b8ed189f1
ms.lasthandoff: 02/07/2017

---

# <a name="display-the-camera-preview"></a>Exibir a visualização da câmera

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera em uma página XAML em um aplicativo UWP (Plataforma Universal do Windows). Criar um aplicativo que captura fotos e vídeos usando a câmera exige que você realize tarefas como manipular a orientação do dispositivo e da câmera ou definir opções de codificação para o arquivo capturado. Em alguns cenários de aplicativo, talvez você queira simplesmente mostrar o fluxo de visualização da câmera sem se preocupar com essas outras considerações. Este artigo mostra como fazer isso com um mínimo de código. Observe que você deve sempre desligar o fluxo de visualização corretamente quando o tiver concluído, seguindo as etapas abaixo.

Para saber mais sobre como desenvolver um aplicativo de câmera que capture fotos ou vídeos, consulte [Captura básica de fotos, vídeo e áudio com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Adicionar declarações de funcionalidade ao manifesto do aplicativo

Para que seu aplicativo acesse a câmera do dispositivo, você deve declarar que seu aplicativo usa as funcionalidades de *webcam* e *microphone* do dispositivo. 

**Adicionar funcionalidades ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.

## <a name="add-a-captureelement-to-your-page"></a>Adicionar um CaptureElement à sua página

Use um [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) para exibir o fluxo de visualização na página XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Usar o MediaCapture para iniciar o fluxo de visualização

O objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) é a interface do seu aplicativo para a câmera do dispositivo. Esta classe é um membro do namespace Windows.Media.Capture. O exemplo deste artigo também usa APIs dos namespaces [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) e [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx), além daqueles incluídos pelo modelo de projeto padrão.

Adicione usando diretivas para incluir os seguintes namespaces no arquivo. cs da sua página.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Declare uma variável de membro de classe para o objeto **MediaCapture** e um valor booliano para controlar se a câmera está visualizando no momento. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Declare uma variável do tipo [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest) que será usada para garantir que a exibição não seja desativada enquanto a visualização estiver em execução.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Crie uma nova instância da classe **MediaCapture** e chame [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar o dispositivo de captura. Esse método pode falhar em dispositivos que não têm uma câmera, por exemplo. Por isso, você deve chamá-lo de dentro de um bloco **try**. Uma **UnauthorizedAccessException** será lançada quando você tentar inicializar a câmera, se o usuário tiver desabilitado o acesso à câmera nas configurações de privacidade do dispositivo. Você também verá essa exceção durante o desenvolvimento, se não tiver adicionado os recursos adequados ao manifesto do seu aplicativo.

**Importante** Em algumas famílias de dispositivos, uma solicitação de consentimento do usuário é exibida para o usuário antes de seu aplicativo receber acesso à câmera do dispositivo. Por esse motivo, você só deve chamar [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) do thread de interface do usuário principal. Tentar iniciar a câmera de outro thread pode resultar em falha de inicialização.

Conecte a **MediaCapture** ao **CaptureElement** definindo a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280). Inicie a visualização chamando [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613). Chame [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive) para garantir que o dispositivo não entre em suspensão enquanto a visualização estiver em execução. Por fim, defina a propriedade [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) como [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) para impedir que a interface do usuário e o **CaptureElement** girem quando o usuário mudar a orientação do dispositivo. Para obter mais informações sobre como lidar com alterações de orientação do dispositivo, consulte [**Manipular a orientação do dispositivo com o MediaCapture**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## <a name="shut-down-the-preview-stream"></a>Desligar o fluxo de visualização

Ao terminar de usar o fluxo de visualização, você sempre deve desligar o fluxo e dispor corretamente dos recursos associados para garantir que a câmera esteja disponível para outros aplicativos no dispositivo. As etapas necessárias para desligar o fluxo de visualização são:

-   Se a câmera estiver atualmente em visualização, chame [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) para interromper o fluxo de visualização. Uma exceção será gerada se você chamar **StopPreviewAsync** enquanto a visualização não estiver em execução.
-   Defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) do **CaptureElement** como nulo. Use [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) para garantir que essa chamada seja executada no thread de interface do usuário.
-   Chame o método [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) do objeto **MediaCapture** para liberar o objeto. Novamente, use [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) para garantir que essa chamada seja executada no thread de interface do usuário.
-   Defina a variável do membro do **MediaCapture** como nulo.
-   Chame [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease) para permitir que a tela seja desativada quando inativa.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Você deve desligar o fluxo de visualização quando o usuário navegar para fora de sua página, substituindo o método [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Você também deve desligar o fluxo de visualização corretamente quando seu aplicativo estiver suspenso. Para fazer isso, registre um manipulador para o evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) no construtor da página.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

No manipulador de eventos **Suspending**, verifique primeiro se a página está exibindo o [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) do aplicativo, comparando o tipo de página com a propriedade [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390). Se a página não estiver sendo exibida no momento, o evento **OnNavigatedFrom** já deve tiver sido gerado e o fluxo de visualização encerrado. Se a página estiver sendo exibida no momento, obtenha um objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) dos argumentos de evento passados para o manipulador para garantir que o sistema não suspenda seu aplicativo até que o fluxo de visualização seja encerrado. Depois de desligar o fluxo, chame o método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) de adiamento para permitir que o sistema continue a suspender seu aplicativo.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Obter um quadro de visualização](get-a-preview-frame.md)

