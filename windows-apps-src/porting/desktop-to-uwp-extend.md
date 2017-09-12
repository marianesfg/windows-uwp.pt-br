---
author: normesta
Description: "Estender seu aplicativo da área de trabalho com interfaces do usuário e componentes do Windows"
Search.Product: eADQiWindows 10XVcnh
title: "Estender seu aplicativo da área de trabalho com interfaces do usuário e componentes do Windows"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Estender seu aplicativo da área de trabalho com componentes UWP modernos

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Se você quiser adicionar essas experiências, estenda seu aplicativo da área de trabalho com um componente UWP.

Em muitos casos, você pode chamar APIs UWP diretamente do seu aplicativo da área de trabalho, então antes de examinar este guia, consulte [Aprimorar para o Windows 10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Este guia pressupõe que você tenha criado um pacote de aplicativo do Windows para seu aplicativo da área de trabalho usando a Ponte de Desktop. Se você ainda não tenha feito isso, consulte [Ponte de Desktop](desktop-to-uwp-root.md).

Se você estiver pronto, vamos começar.

## <a name="show-a-modern-xaml-ui"></a>Mostrar uma UI XAML moderna

Como parte do fluxo de seu aplicativo, você pode incorporar interfaces do usuário modernas baseadas em XAML em seu aplicativo da área de trabalho. Essas interfaces do usuário são naturalmente adaptáveis para diferentes tamanhos de tela e resoluções e dão suporte a modelos interativos modernos, como toque e tinta.

Por exemplo, com uma pequena quantidade de marcação XAML, você pode oferecer aos usuários recursos de visualização de mapa eficientes.

Esta imagem mostra um aplicativo VB6 que abre uma interface do usuário moderna baseada em XAML que contém um controle de mapa.

![adaptive-design](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Assistir a um vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [Obter o app](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>O padrão de design

Para mostrar uma interface do usuário baseada em XAML, faça o seguinte:

:one: [Adicione um projeto UWP à sua solução](#project)

:two: [Adicione uma extensão de protocolo ao projeto](#protocol)

:three: [Inicie o aplicativo UWP do seu aplicativo da área de trabalho](#start)

:four: [No projeto UWP, mostre a página desejada](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>Adicionar um projeto UWP

Adicione um projeto **App em Branco (Universal do Windows)** à sua solução.

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>Adicionar uma extensão de protocolo

No **Solution Explorer**, abra o arquivo **package.appxmanifest** do projeto e adicione a extensão.

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
      </uap:Extension>
    </Extensions>     
```

Dê um nome ao protocolo, forneça o nome do executável produzido pelo projeto UWP e o nome da classe de ponto de entrada.

Você também pode abrir o **package.appxmanifest** no designer, escolher a guia **Declarações** e então adicione a extensão ali.

![declarations-tab](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> Os controles de mapa baixam dados da Internet e, se você usar um, também terá de adicionar o recurso "cliente da internet" ao seu manifesto.

<span id="start" />
### <a name="start-the-uwp-app"></a>Iniciar o aplicativo UWP

Primeiro, crie um [Uri](https://msdn.microsoft.com/library/system.uri.aspx) que inclua o nome de protocolo e os parâmetros que você deseja transmitir para o aplicativo UWP. Em seguida, chame o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_).

Veja um exemplo básico em C#.

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
Em nosso exemplo, estamos fazendo algo um pouco mais indireto. Integramos a chamada a uma função de interoperabilidade chamável pelo VB6 denominada ``LaunchMap``. Essa função é escrita usando C++.

Veja o bloco do VB:

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

Esta é a função do C++:

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<span id="parse" />
### <a name="parse-parameters-and-show-a-page"></a>Analisar parâmetros e mostrar uma página

Na classe **App** do seu projeto UWP, substitua o manipulador de eventos **OnActivated**. Se o app for ativado pelo seu protocolo, analise os parâmetros e então abra a página desejada.

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>Exemplos semelhantes

[Exemplo da Northwind: exemplo completo de código herdado Win32 e IU UWA](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Exemplo da Northwind: aplicativo UWP conectando-se ao SQL Server](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>Fornecer serviços a outros apps

Você adiciona um serviço que outros apps podem consumir. Por exemplo, você pode adicionar um serviço que ofereça aos outros apps acesso controlado ao banco de dados por trás de seu aplicativo. Ao implementar uma tarefa em segundo plano, os apps podem acessar o serviço mesmo se seu aplicativo da área de trabalho não estiver em execução.

Aqui está um exemplo que faz isso.

![adaptive-design](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Assistir a um vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [Obter o app](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>O padrão de design

Para fornecer um serviço, faça o seguinte:

:one: [Adicione um componente do Windows Runtime](#component)

:two: [Adicione uma extensão de serviço de aplicativo](#extension)

:three: [Implemente o serviço de aplicativo](#appservice)

:four: [Teste o serviço de aplicativo](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>Adicionar um componente do Windows Runtime

Adicione um projeto **Componente do Tempo de Execução do Windows (Universal do Windows)** à sua solução.

Em seguida, faça referência ao projeto do componente do tempo de execução desde o projeto de empacotamento de UWP.

<span id="extension" />
### <a name="add-an-app-service-extension"></a>Adicionar uma extensão de serviço de aplicativo

No **Solution Explorer**, abra o arquivo **package.appxmanifest** do seu projeto de empacotamento e adicione uma extensão de serviço de aplicativo.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

Dê ao serviço de aplicativo um nome e forneça o nome da classe de ponto de entrada. Essa é a classe que você usará para implementar o serviço de aplicativo.

<span id="appservice" />
### <a name="implement-the-app-service"></a>Implementar o serviço de aplicativo

Veja onde você validará e lidará com solicitações de outros apps.

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<span id="test" />
### <a name="test-the-app-service"></a>Testar o serviço de aplicativo

Teste seu serviço chamando-o de outro app.

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
        string result = "";
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

Saiba mais sobre os serviços de aplicativo aqui: [Criar e consumir um serviço de aplicativo ](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

### <a name="similar-samples"></a>Exemplos semelhantes

[Exemplo de ponte de serviço de aplicativo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[Exemplo de ponte de serviço de aplicativo com um app win32 em C++](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[Aplicativo MFC que recebe notificações por push](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>Tornando seu aplicativo da área de trabalho um destino de compartilhamento

Você pode tornar seu aplicativo da área de trabalho um destino de compartilhamento para que os usuários possam compartilhar com facilidade dados como imagens de outros aplicativos que dão suporte a compartilhamento.

Por exemplo, os usuários podem escolher seu app para compartilhar fotos do Microsoft Edge, o app Fotos. Veja um aplicativo WPF de exemplo que tem esse recurso.

![compartilhar destino](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Assistir a um vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [Obter o app](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>O padrão de design

Para tornar seu aplicativo um destino de compartilhamento, faça o seguinte:

:one: [Adicione um projeto UWP à sua solução](#project2)

:two: [Adicione uma extensão de destino de compartilhamento](#share-extension)

:three: [Substitua o manipulador de eventos OnNavigatedTo](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>Adicionar um projeto UWP à sua solução

Adicione um projeto **App em Branco (Universal do Windows)** à sua solução.

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>Adicionar uma extensão de destino de compartilhamento

No **Solution Explorer**, abra o arquivo **package.appxmanifest** do projeto e adicione a extensão.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Forneça o nome do executável produzido pelo projeto UWP e o nome da classe de ponto de entrada. Você também precisará especificar os tipos de arquivos que poderão ser compartilhados com seu app.

<span id="override" />
### <a name="override-the-onnavigatedto-event-handler"></a>Substituir o manipulador de eventos OnNavigatedTo

Substitua o manipulador de eventos **OnNavigatedTo** na classe **App** do seu projeto UWP.

Esse manipulador de eventos é chamado quando os usuários escolhem seu app para compartilhar seus arquivos.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
