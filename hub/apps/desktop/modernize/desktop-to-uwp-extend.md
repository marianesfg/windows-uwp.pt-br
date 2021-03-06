---
Description: Estender o aplicativo da área de trabalho com interfaces do usuário e componentes do Windows
title: Estender o aplicativo com interfaces do usuário e componentes do Windows
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: a38f5fa7f3ef99f5970ec5d476fb65761aa39db4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75302580"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Estender o aplicativo da área de trabalho com componentes UWP modernos

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de aplicativo moderno. Se você quiser adicionar essas experiências, estenda seu aplicativo da área de trabalho com projetos UWP e componentes do Windows Runtime.

Em muitos casos, você pode chamar APIs do Windows Runtime do seu aplicativo da área de trabalho. Portanto, antes de examinar este guia, confira [Aprimorar para o Windows 10](desktop-to-uwp-enhance.md).

> [!NOTE]
> Os recursos descritos neste artigo exigem que seu aplicativo da área de trabalho tenha um [identificador de pacote](modernize-packaged-apps.md), seja [empacotando o aplicativo da área de trabalho em um pacote MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) ou [concedendo a identidade do aplicativo usando um pacote esparso](grant-identity-to-nonpackaged-apps.md).

Se você estiver pronto, vamos começar.

<a id="setup" />

## <a name="first-setup-your-solution"></a>Primeiro, configure sua solução

Adicione um ou mais projetos UWP e componentes de runtime à sua solução.

Comece com uma solução que contenha um **Projeto de Empacotamento de Aplicativo do Windows** com uma referência ao seu aplicativo da área de trabalho.

Esta imagem mostra um exemplo de solução.

![Estender projeto inicial](images/desktop-to-uwp/extend-start-project.png)

Caso a sua solução não contenha um projeto de empacotamento, confira [Empacotar seu aplicativo da área de trabalho usando o Visual Studio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Configurar o aplicativo da área de trabalho

Verifique se o aplicativo da área de trabalho tem referências aos arquivos de que você precisa para chamar as APIs do Windows Runtime.

Para isso, confira a seção [Configurar seu projeto](desktop-to-uwp-enhance.md#set-up-your-project).

### <a name="add-a-uwp-project"></a>Adicionar um projeto UWP

Adicionar um **Aplicativo em branco (Universal do Windows)** à sua solução.

Aqui, você criará uma interface XAML moderna ou usará APIs que são executadas somente dentro de um processo UWP.

![Projeto UWP](images/desktop-to-uwp/add-uwp-project-to-solution.png)

No seu projeto de empacotamento, clique com botão direito no nó **Aplicativos** e clique em **Adicionar Referência**.

![Fazer referência a projeto UWP](images/desktop-to-uwp/add-uwp-project-reference.png)

Depois, adicione uma referência ao projeto UWP.

![Fazer referência a projeto UWP](images/desktop-to-uwp/choose-uwp-project.png)

A solução terá a seguinte aparência:

![Solução com projeto UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Opcional) Adicionar um componente do Windows Runtime

Para realizar alguns cenários, você precisará adicionar código a um componente do Windows Runtime.

![serviço de aplicativo de componente de runtime](images/desktop-to-uwp/add-runtime-component.png)

Em seguida, no projeto UWP, adicione uma referência ao componente de runtime. A solução terá a seguinte aparência:

![Referência do componente de runtime](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Criar sua solução

Crie sua solução para garantir que nenhum erro seja exibido. Se você receber erros, abra **Gerenciador de Configurações** e verifique se os projetos se destinam à mesma plataforma.

![Gerenciador de Configurações](images/desktop-to-uwp/config-manager.png)

Vamos dar uma olhada em algumas coisas que você pode fazer com seus projetos UWP e componentes de runtime.

## <a name="show-a-modern-xaml-ui"></a>Mostrar uma interface do usuário XAML moderna

Como parte do fluxo de seu aplicativo, você pode incorporar interfaces do usuário modernas baseadas em XAML em seu aplicativo da área de trabalho. Essas interfaces do usuário são naturalmente adaptáveis para diferentes tamanhos de tela e resoluções e dão suporte a modelos interativos modernos, como toque e tinta.

Por exemplo, com uma pequena quantidade de marcação XAML, você pode oferecer aos usuários recursos de visualização de mapa eficientes.

Esta imagem mostra um aplicativo Windows Forms que abre uma interface do usuário moderna baseada em XAML, a qual contém um controle de mapa.

![design adaptável](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>Esse exemplo mostra uma interface de usuário XAML adicionando um projeto UWP à solução. Essa é a abordagem estável compatível para mostrar interfaces do usuário XAML em um aplicativo da área de trabalho. A alternativa é adicionar os controles do XAML UWP diretamente ao aplicativo da área de trabalho usando uma Ilha XAML. As ilhas XAML estão disponíveis atualmente como uma versão prévia para desenvolvedores. Embora incentivemos você a testá-los em seu próprio código de protótipo, não recomendamos que você os use no código de produção neste momento. Essas APIs e controles continuarão ser aprimorados e estabilizados em versões futuras do Windows. Para saber mais sobre as Ilhas XAML, confira [Controles UWP em aplicativos da área de trabalho](xaml-islands.md)

### <a name="the-design-pattern"></a>O padrão de design

Para mostrar uma interface do usuário baseada em XAML, é preciso:

:one: [Configurar a solução](#solution-setup)

:two: [Criar uma interface do usuário XAML](#xaml-UI)

:three: [Adicionar uma extensão de protocolo ao projeto UWP](#add-a-protocol-extension)

:four: [Iniciar o aplicativo UWP em seu aplicativo da área de trabalho](#start)

:five: [No projeto UWP, mostrar a página desejada](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>Configurar a solução

Para obter diretrizes gerais sobre como configurar a solução, confira a seção [Primeiro, configure sua solução](#setup) no início deste guia.

A solução terá a seguinte aparência:

![Solução da interface do usuário XAML](images/desktop-to-uwp/xaml-ui-solution.png)

Nesse exemplo, o projeto do Windows Forms é denominado **Landmarks**, e o projeto UWP que contém a interface do usuário XAML é denominado **MapUI**.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>Criar uma interface do usuário XAML

Adicione uma interface do usuário XAML a seu projeto UWP. Confira o XAML do mapa básico.

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

No **Gerenciador de Soluções**, abra o arquivo **package.appxmanifest** do projeto de Empacotamento da sua solução e adicione esta extensão.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

Dê um nome ao protocolo, forneça o nome do executável produzido pelo projeto UWP e o nome da classe de ponto de entrada.

Você também pode abrir o **package.appxmanifest** no designer, escolher a guia **Declarações** e adicionar a extensão ali.

![guia declarações](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Os controles de mapa baixam dados da Internet e, se você usar um, também terá que adicionar o recurso "cliente da internet" ao seu manifesto.

<a id="start" />

### <a name="start-the-uwp-app"></a>Iniciar o aplicativo UWP

Primeiro, em seu aplicativo da área de trabalho, crie um [Uri](https://docs.microsoft.com/dotnet/api/system.uri) que inclua o nome de protocolo e os parâmetros que você deseja transmitir para o aplicativo UWP. Em seguida, chame o método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync).

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

Na classe **App** do projeto UWP, substitua o manipulador de eventos **OnActivated**. Se o aplicativo for ativado pelo seu protocolo, analise os parâmetros e abra a página desejada.

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

No código por trás da página XAML, substitua o método ``OnNavigatedTo`` para usar os parâmetros passados para a página. Nesse caso, usaremos a latitude e longitude passadas para essa página a fim de mostrar uma localização em um mapa.

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

## <a name="making-your-desktop-application-a-share-target"></a>Tornar seu aplicativo da área de trabalho um destino de compartilhamento

Você pode tornar seu aplicativo da área de trabalho um destino de compartilhamento para que os usuários possam compartilhar com facilidade dados como imagens de outros aplicativos que dão suporte a compartilhamento.

Por exemplo, os usuários podem escolher seu aplicativo para compartilhar fotos do Microsoft Edge, o aplicativo Fotos. Confira um aplicativo WPF de exemplo que tem esse recurso.

![destino de compartilhamento](images/desktop-to-uwp/share-target.png).

Confira o exemplo completo [aqui](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>O padrão de design

Para tornar seu aplicativo um destino de compartilhamento, é preciso:

:one: [Adicionar uma extensão de destino de compartilhamento](#share-extension)

:two: [Substituir o manipulador de eventos OnShareTargetActivated](#override)

:three: [Adicionar extensões de área de trabalho ao projeto UWP](#desktop-extensions)

:four: [Adicionar a extensão de processo de confiança total](#full-trust)

:five: [Modificar o aplicativo da área de trabalho para obter o arquivo compartilhado](#modify-desktop)

<a id="share-extension" />

As etapas a seguir  

### <a name="add-a-share-target-extension"></a>Adicionar uma extensão de destino de compartilhamento

No **Gerenciador de Soluções**, abra o arquivo **package.appxmanifest** do projeto de Empacotamento da sua solução e adicione a extensão de destino de compartilhamento.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Forneça o nome do executável produzido pelo projeto UWP e o nome da classe de ponto de entrada. Essa marcação pressupõe que o nome do executável para seu aplicativo UWP é `ShareTarget.exe`.

Você também precisará especificar os tipos de arquivos que poderão ser compartilhados com seu aplicativo. Nesse exemplo, estamos tornando o aplicativo da área de trabalho [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) um destino de compartilhamento para imagens bitmap, portanto, especificamos `Bitmap` como o tipo de arquivo com suporte.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>Substituir o manipulador de eventos OnShareTargetActivated

Substitua o manipulador de eventos **OnActivated** na classe **App** do projeto UWP.

Esse manipulador de eventos é chamado quando os usuários escolhem seu aplicativo para compartilhar arquivos.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

Nesse código, salvamos a imagem que está sendo compartilhada pelo usuário em uma pasta de armazenamento local de aplicativos. Em seguida, modificaremos o aplicativo da área de trabalho para extrair imagens da mesma pasta. O aplicativo da área de trabalho pode fazer isso porque ele está incluído no mesmo pacote que o aplicativo UWP.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Adicionar extensões de área de trabalho ao projeto UWP

Adicione a extensão **Extensões da área de trabalho do Windows para UWP** ao projeto de aplicativo UWP.

![extensão de área de trabalho](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>Adicionar a extensão de processo de confiança total

No **Explorador de soluções**, abra o arquivo **package.appxmanifest** no projeto de Empacotamento na sua solução e adicione a extensão de processo de confiança total ao lado da extensão de destino de compartilhamento que você adicionou a esse arquivo anteriormente.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Essa extensão permitirá que o aplicativo UWP inicie o aplicativo da área de trabalho com o qual você deseja compartilhar um arquivo. No exemplo, fazemos referência ao executável do aplicativo da área de trabalho [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo).

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Modificar o aplicativo da área de trabalho para obter o arquivo compartilhado

Modifique o aplicativo da área de trabalho para encontrar e processar o arquivo compartilhado. Neste exemplo, o aplicativo UWP armazenou o arquivo compartilhado na pasta de dados do aplicativo local. Portanto, modificaremos o aplicativo da área de trabalho [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) para extrair fotos dessa pasta.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Para instâncias do aplicativo da área de trabalho já abertas pelo usuário, também podemos manipular o evento [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher) e passar o caminho para o local do arquivo. Dessa forma, qualquer instância aberta do aplicativo da área de trabalho mostrará a foto compartilhada.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>Criar uma tarefa em segundo plano

Adicione uma tarefa em segundo plano para executar o código, mesmo quando o aplicativo estiver suspenso. É ótimo deixar em segundo plano tarefas pequenas que não exigem a interação do usuário. Por exemplo, sua tarefa pode baixar emails, mostrar uma notificação do sistema sobre uma mensagem de chat recebida ou reagir a uma alteração em uma condição do sistema.

Este é um exemplo de aplicativo WPF que registra uma tarefa em segundo plano.

![tarefa em segundo plano](images/desktop-to-uwp/sample-background-task.png)

A tarefa faz uma solicitação http e mede o tempo que leva para a solicitação retornar uma resposta. As tarefas provavelmente serão muito mais interessantes, mas esse exemplo é ótimo para aprender a mecânica básica de uma tarefa em segundo plano.

Confira o exemplo completo [aqui](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>O padrão de design

Para criar um serviço em segundo plano, é preciso:

:one: [Implementar a tarefa em segundo plano](#implement-task)

:two: [Configurar a tarefa em segundo plano](#configure-background-task)

:three: [Registrar a tarefa em segundo plano](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>Implementar a tarefa em segundo plano

Implemente a tarefa em segundo plano adicionando código a um projeto de componente do Windows Runtime.

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

No designer de manifesto, abra o arquivo **package.appxmanifest** do projeto de Empacotamento em sua solução.

Na guia **Declarações**, adicione uma declaração **Tarefas em segundo plano**.

![Opção de tarefa em segundo plano](images/desktop-to-uwp/background-task-option.png)

Em seguida, escolha as propriedades desejadas. Nosso exemplo usa a propriedade **Timer**.

![Propriedade Timer](images/desktop-to-uwp/timer-property.png)

Forneça o nome totalmente qualificado da classe no seu componente do Windows Runtime que implementa a tarefa em segundo plano.

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

## <a name="find-answers-to-your-questions"></a>Encontrar respostas para suas dúvidas

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
