---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Estender seu aplicativo da área de trabalho com interfaces do usuário e componentes do Windows
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f3354dad1702d275fb7b2af53516689d2c5d5014
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4204732"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Estender seu aplicativo da área de trabalho com componentes UWP modernos

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Se você quiser adicionar essas experiências, estenda seu aplicativo da área de trabalho com projetos UWP e componentes do Tempo de Execução do Windows.

Em muitos casos, você pode chamar APIs UWP diretamente do seu aplicativo da área de trabalho, então antes de examinar este guia, consulte [Aprimorar para o Windows 10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Este guia pressupõe que você tenha criado um pacote de aplicativo do Windows para seu aplicativo da área de trabalho usando a Ponte de Desktop. Se você ainda não tenha feito isso, consulte [Ponte de Desktop](desktop-to-uwp-root.md).

Se você estiver pronto, vamos começar.

<a id="setup" />

## <a name="first-setup-your-solution"></a>Primeiro, configure sua solução

Adicione um ou mais projetos UWP e componentes de tempo de execução à sua solução.

Comece com uma solução que contenha um **projeto de empacotamento de aplicativo do Windows** com uma referência ao seu aplicativo da área de trabalho.

Esta imagem mostra um exemplo de solução.

![Estender projeto inicial](images/desktop-to-uwp/extend-start-project.png)

Se sua solução não contiver um projeto de empacotamento, consulte [Empacotar seu aplicativo usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md).

### <a name="add-a-uwp-project"></a>Adicionar um projeto UWP

Adicione um **App em Branco (Universal do Windows)** à sua solução.

Aqui você criará uma interface XAML moderna ou usará APIs que são executadas somente dentro de um processo UWP.

![Projeto UWP](images/desktop-to-uwp/add-uwp-project-to-solution.png)

No seu projeto de empacotamento, clique com botão direito no nó **Aplicativos** e clique em **Adicionar referência**.

![Fazer referência a projeto UWP](images/desktop-to-uwp/add-uwp-project-reference.png)

Depois, adicione uma referência ao projeto UWP.

![Fazer referência a projeto UWP](images/desktop-to-uwp/choose-uwp-project.png)

A solução terá a aparência a seguir:

![Solução com projeto UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Opcional) Adicionar um componente do Tempo de Execução do Windows

Para realizar alguns cenários, você precisará adicionar código a um componente do Tempo de Execução do Windows.

![serviço de aplicativo de componente do tempo de execução](images/desktop-to-uwp/add-runtime-component.png)

Em seguida, no projeto UWP, adicione uma referência ao componente de tempo de execução. A solução terá a aparência a seguir:

![Referência do componente de tempo de execução](images/desktop-to-uwp/runtime-component-reference.png)

Vamos dar uma olhada em algumas coisas que você pode fazer com seus projetos UWP e os componentes de tempo de execução.

## <a name="show-a-modern-xaml-ui"></a>Mostrar uma UI XAML moderna

Como parte do fluxo de seu aplicativo, você pode incorporar interfaces do usuário modernas baseadas em XAML em seu aplicativo da área de trabalho. Essas interfaces do usuário são naturalmente adaptáveis para diferentes tamanhos de tela e resoluções e dão suporte a modelos interativos modernos, como toque e tinta.

Por exemplo, com uma pequena quantidade de marcação XAML, você pode oferecer aos usuários recursos de visualização de mapa eficientes.

Esta imagem mostra um aplicativo Windows Forms que abre uma interface do usuário moderna baseada em XAML, a qual contém um controle de mapa.

![adaptive-design](images/desktop-to-uwp/extend-xaml-ui.png)

### <a name="the-design-pattern"></a>O padrão de design

Para mostrar uma interface do usuário baseada em XAML, faça o seguinte:

:um: [Configurar a solução](#solution-setup)

:dois: [Criar uma interface do usuário XAML](#xaml-UI)

:três: [Adicione uma extensão de protocolo ao projeto UWP](#protocol)

:quatro: [Inicie o aplicativo UWP do seu aplicativo da área de trabalho](#start)

:cinco: [No projeto UWP, mostre a página desejada](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>Configurar a solução

Para obter diretrizes gerais sobre como configurar a solução, consulte a seção [Primeiro, configure sua solução](#setup) no início deste guia.

A solução terá a aparência a seguir:

![Solução da interface do usuário XAML](images/desktop-to-uwp/xaml-ui-solution.png)

Neste exemplo, o projeto Windows Forms é denominado **Landmarks** e o projeto UWP que contém a interface do usuário XAML é denominado **MapUI**.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>Criar uma interface do usuário XAML

Adicione uma interface do usuário XAML ao seu projeto UWP. Veja o XAML do mapa básico.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"    
                     HorizontalAlignment="Left"               
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>Adicionar uma extensão de protocolo

No **Gerenciador de soluções**, abra o arquivo **Package. appxmanifest** do projeto empacotamento em sua solução e adicione essa extensão.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>    
```

Dê um nome ao protocolo, forneça o nome do executável produzido pelo projeto UWP e o nome da classe de ponto de entrada.

Você também pode abrir o **package.appxmanifest** no designer, escolher a guia **Declarações** e então adicione a extensão ali.

![declarations-tab](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Os controles de mapa baixam dados da Internet e, se você usar um, também terá de adicionar o recurso "cliente da internet" ao seu manifesto.

<a id="start" />

### <a name="start-the-uwp-app"></a>Iniciar o aplicativo UWP

Primeiro, em seu aplicativo da área de trabalho, crie um [Uri](https://msdn.microsoft.com/library/system.uri.aspx) que inclua o nome de protocolo e os parâmetros que você deseja transmitir para o aplicativo UWP. Em seguida, chame o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync).

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>Analisar parâmetros e mostrar uma página

Na classe **App** do seu projeto UWP, substitua o manipulador de eventos **OnActivated**. Se o app for ativado pelo seu protocolo, analise os parâmetros e então abra a página desejada.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

Substitua o método ``OnNavigatedTo`` para usar os parâmetros passados para a página. Nesse caso, usaremos a latitude e longitude passado para essa página a fim de mostrar uma localização em um mapa.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

### <a name="similar-samples"></a>Exemplos semelhantes

[Adicionar uma experiência do usuário XAML da UWP ao aplicativo VB6](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

[Exemplo da Northwind: exemplo completo de código herdado Win32 e IU UWA](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Exemplo da Northwind: aplicativo UWP conectando-se ao SQL Server](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>Fornecer serviços a outros apps

Você adiciona um serviço que outros apps podem consumir. Por exemplo, você pode adicionar um serviço que ofereça aos outros apps acesso controlado ao banco de dados por trás de seu aplicativo. Ao implementar uma tarefa em segundo plano, os apps podem acessar o serviço mesmo se seu aplicativo da área de trabalho não estiver em execução.

Aqui está um exemplo que faz isso.

![adaptive-design](images/desktop-to-uwp/winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Obter o app](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>O padrão de design

Para fornecer um serviço, faça o seguinte:

:one: [Implemente o serviço de aplicativo](#appservice)

:two: [Adicione uma extensão de serviço de aplicativo](#extension)

:three: [Teste o serviço de aplicativo](#test)

<a id="appservice" />

### <a name="implement-the-app-service"></a>Implementar o serviço de aplicativo

Veja onde você validará e lidará com solicitações de outros apps. Adicione esse código a um Componente do Tempo de Execução do Windows em sua solução.

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

<a id="extension" />

### <a name="add-an-app-service-extension-to-the-packaging-project"></a>Adicione uma extensão de serviço de aplicativo para o projeto de empacotamento

Abra o arquivo **Package. appxmanifest** do projeto de empacotamento e adicione uma extensão de serviço de aplicativo para o ``<Application>`` elemento.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="AppServiceComponent.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```
Dê ao serviço de aplicativo um nome e forneça o nome da classe de ponto de entrada. Esta é a classe em que você implementou o serviço.

<a id="test" />

### <a name="test-the-app-service"></a>Testar o serviço de aplicativo

Teste seu serviço chamando-o de outro app. Esse código pode ser um aplicativo da área de trabalho como um aplicativo de formulários do Windows ou outro aplicativo UWP.

> [!NOTE]
> Este código funciona apenas se você definir corretamente a propriedade ``PackageFamilyName`` da classe ``AppServiceConnection``. Você pode obter esse nome chamando ``Windows.ApplicationModel.Package.Current.Id.FamilyName`` no contexto do projeto UWP. Consulte [Criar e consumir um serviço de app](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

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

Saiba mais sobre os serviços de aplicativo aqui: [Criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

### <a name="similar-samples"></a>Exemplos semelhantes

[Exemplo de ponte de serviço de aplicativo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[Exemplo de ponte de serviço de aplicativo com um app win32 em C++](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[Aplicativo MFC que recebe notificações por push](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>Tornando seu aplicativo da área de trabalho um destino de compartilhamento

Você pode tornar seu aplicativo da área de trabalho um destino de compartilhamento para que os usuários possam compartilhar com facilidade dados como imagens de outros aplicativos que dão suporte a compartilhamento.

Por exemplo, os usuários podem escolher seu app para compartilhar fotos do Microsoft Edge, o app Fotos. Veja um aplicativo WPF de exemplo que tem esse recurso.

![compartilhar destino](images/desktop-to-uwp/share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Obter o app](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>O padrão de design

Para tornar seu aplicativo um destino de compartilhamento, faça o seguinte:

:one: [Adicione uma extensão de destino de compartilhamento](#share-extension)

:two: [Substituir o manipulador de eventos OnNavigatedTo](#override)

<a id="share-extension" />

### <a name="add-a-share-target-extension"></a>Adicionar uma extensão de destino de compartilhamento

No **Gerenciador de soluções**, abra o arquivo **Package. appxmanifest** do projeto empacotamento em sua solução e adicione a extensão.

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

<a id="override" />

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

## <a name="create-a-background-task"></a>Criar uma tarefa em segundo plano

Adicione uma tarefa em segundo plano para executar o código, mesmo quando o aplicativo está suspenso. Tarefas em segundo plano são ótimas para tarefas pequenas que não exigem a interação do usuário. Por exemplo, sua tarefa pode baixar emails, mostrar uma notificação do sistema sobre uma mensagem de bate-papo recebida ou reagir a uma alteração em uma condição de sistema.

Este é um aplicativo de exemplo do WPF que registra uma tarefa em segundo plano.

![tarefa em segundo plano](images/desktop-to-uwp/sample-background-task.png)

A tarefa faz uma solicitação http e mede o tempo que leva para a solicitação retornar uma resposta. As tarefas provavelmente serão muito mais interessantes, mas este exemplo é ótimo para aprender a mecânica básica de uma tarefa em segundo plano.

### <a name="have-a-closer-look-at-this-app"></a>Examine este app mais de perto

:heavy_check_mark: [Procurar o código](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)

### <a name="the-design-pattern"></a>O padrão de design

Para criar um serviço em segundo plano, faça o seguinte:

:one: [Implementar a tarefa em segundo plano](#implement-task)

:two: [Configurar a tarefa em segundo plano](#configure-background-task)

:three: [Registrar a tarefa em segundo plano](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>Implementar a tarefa em segundo plano

Implemente a tarefa em segundo plano adicionando código para um projeto de componente do tempo de execução do Windows.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>Configurar a tarefa em segundo plano

No designer de manifesto, abra o arquivo **Package. appxmanifest** do projeto empacotamento em sua solução.

Na guia **Declarações**, adicione uma declaração **Tarefas em segundo plano**.

![Opção de tarefa em segundo plano](images/desktop-to-uwp/background-task-option.png)

Em seguida, escolha as propriedades desejadas. Nosso exemplo usa a propriedade **Timer**.

![Propriedade Timer](images/desktop-to-uwp/timer-property.png)

Forneça o nome totalmente qualificado da classe no seu componente do tempo de execução do Windows que implementa a tarefa em segundo plano.

![Propriedade Timer](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano

Adicione código ao seu projeto de aplicativo da área de trabalho que registra a tarefa em segundo plano.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```
## <a name="support-and-feedback"></a>Suporte e comentários

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
