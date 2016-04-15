---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera dentro de uma página XAML em um aplicativo UWP (Plataforma Universal do Windows).
title: Acesso de visualização de câmera simples
---

# Acesso de visualização de câmera simples

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo descreve como exibir rapidamente o fluxo de visualização de câmera dentro de uma página XAML em um aplicativo UWP (Plataforma Universal do Windows). Criar um aplicativo que captura fotos e vídeos usando a câmera exige que você realize tarefas como manipular a orientação do dispositivo e da câmera ou definir opções de codificação para o arquivo capturado. Em alguns cenários de aplicativo, talvez você queira simplesmente mostrar o fluxo de visualização da câmera sem se preocupar com essas outras considerações. Este artigo mostra como fazer isso com um mínimo de código. Observe que você deve sempre desligar o fluxo de visualização corretamente quando o tiver concluído, seguindo as etapas abaixo.

Para saber mais sobre como escrever um aplicativo de câmera que captura fotos ou vídeos, veja [Capturar fotos e vídeo com MediaCapture](capture-photos-and-video-with-mediacapture.md).

## Adicionar declarações de funcionalidade ao manifesto do aplicativo

Para que seu aplicativo acesse a câmera do dispositivo, você deve declarar que seu aplicativo usa as funcionalidades de *webcam* e *microphone* do dispositivo. Se você quiser salvar as fotos e os vídeos capturados na biblioteca Imagens ou Vídeos do usuário, deverá declarar também as funcionalidades *picturesLibrary* e *videosLibrary*.

**Adicionar funcionalidades ao manifesto do aplicativo**

1.  No Microsoft Visual Studio, no **Gerenciador de Soluções**, abra o designer do manifesto do aplicativo clicando duas vezes no item **package.appxmanifest**.
2.  Selecione a guia **Recursos**.
3.  Marque a caixa da **Webcam** e a caixa do **Microfone**.
4.  Para acessar as bibliotecas Imagens e Vídeos, marque as caixas para **Biblioteca de Imagens** e a caixa para **Biblioteca de Vídeos**.

## Adicionar um CaptureElement à sua página

Use um [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) para exibir o fluxo de visualização na página XAML.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## Usar o MediaCapture para iniciar o fluxo de visualização

O objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) é a interface do seu aplicativo para a câmera do dispositivo. Esta classe é um membro do namespace Windows.Media.Capture. O exemplo deste artigo também usa APIs dos namespaces [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) e [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx), além daqueles incluídos pelo modelo de projeto padrão.

[!code-cs[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

Adicione usando diretivas para incluir os seguintes namespaces no arquivo. cs da sua página.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Declare uma variável de classe para o objeto **MediaCapture**.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Crie uma nova instância da classe **MediaCapture** e chame [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar o dispositivo de captura. Esse método pode falhar em dispositivos que não têm uma câmera, por exemplo. Por isso, você deve chamá-lo de dentro de um bloco **try**. Uma **UnauthorizedAccessException** será lançada quando você tentar inicializar a câmera, se o usuário tiver desabilitado o acesso à câmera nas configurações de privacidade do dispositivo. Você também verá essa exceção durante o desenvolvimento, se não tiver adicionado os recursos adequados ao manifesto do seu aplicativo.

**Importante** Em algumas famílias de dispositivos, uma solicitação de consentimento do usuário é exibida para o usuário antes de seu aplicativo receber acesso à câmera do dispositivo. Por esse motivo, você só deve chamar [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) do thread de interface do usuário principal. Tentar iniciar a câmera de outro thread pode resultar em falha de inicialização.

Conecte a **MediaCapture** ao **CaptureElement** definindo a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280). Por fim, inicie a visualização chamando [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## Encerrar o fluxo de visualização

Ao terminar de usar o fluxo de visualização, você sempre deve encerrar o fluxo e dispor corretamente dos recursos associados para garantir que a câmera esteja disponível para outros aplicativos no dispositivo. As etapas necessárias para encerrar o fluxo de visualização são:

-   Chame [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) para interromper o fluxo de visualização.
-   Defina a propriedade [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) do **CaptureElement** como nulo.
-   Chame o método [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) do objeto **MediaCapture** para liberar o objeto.
-   Defina a variável do membro do **MediaCapture** como nulo.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Você deve encerrar o fluxo de visualização quando o usuário navegar para fora de sua página, substituindo o método [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507).

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Você também deve encerrar o fluxo de visualização corretamente quando seu aplicativo estiver suspenso. Para fazer isso, registre um manipulador para o evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) no construtor da página.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

No manipulador de eventos **Suspending**, verifique primeiro se a página está exibindo o [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) do aplicativo, comparando o tipo de página com a propriedade [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390). Se a página não estiver sendo exibida no momento, o evento **OnNavigatedFrom** já deve tiver sido gerado e o fluxo de visualização encerrado. Se a página estiver sendo exibida no momento, obtenha um objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) dos argumentos de evento passados para o manipulador para garantir que o sistema não suspenda seu aplicativo até que o fluxo de visualização seja encerrado. Após encerrar o fluxo, chame o método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) de adiamento para permitir que o sistema continue a suspender seu aplicativo.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## Capturar uma imagem estática do fluxo de visualização

É simples obter uma imagem estática do fluxo de visualização de captura de mídia na forma de um [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358). Para saber mais, veja [Obter um quadro de visualização](get-a-preview-frame.md).

## Tópicos relacionados

* [Capturar fotos e vídeos com o MediaCapture](capture-photos-and-video-with-mediacapture.md)
* [Obter um quadro de visualização](get-a-preview-frame.md)


<!--HONumber=Mar16_HO5-->


