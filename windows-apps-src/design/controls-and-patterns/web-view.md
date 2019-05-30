---
Description: Um controle de modo de exibição da Web incorpora um modo de exibição em seu aplicativo que renderiza o conteúdo da web usando o mecanismo de renderização do Microsoft Edge. Hiperlinks também podem aparecer e funcionar em um controle de modo de exibição da Web.
title: Modo de exibição da Web
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c996b22395fc8186fb1b6dc786a73fa4a97ecf16
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363990"
---
# <a name="web-view"></a>Modo de exibição da Web
 

Um controle de modo de exibição da Web incorpora um modo de exibição em seu aplicativo que renderiza o conteúdo da web usando o mecanismo de renderização do Microsoft Edge. Hiperlinks também podem aparecer e funcionar em um controle de modo de exibição da Web.

> **APIs importantes**: [Classe WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Utilize um controle de modo de exibição na Web para exibir o conteúdo HTML sofisticadamente formatado a partir de um servidor Web remoto, código gerado dinamicamente ou arquivos de conteúdo no pacote de seu aplicativo. Conteúdo sofisticado também pode conter código de script e comunicar-se entre o script e o código de seu aplicativo.

## <a name="create-a-web-view"></a>Criar um modo de exibição da Web

**Modificar a aparência de uma exibição da web**

[WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) não é uma subclasse [Control](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), portanto, não tem um modelo de controle. No entanto, você pode definir várias propriedades para controlar alguns aspectos visuais do modo de exibição da web.
- Para restringir a área de exibição, defina as propriedades [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) e [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height). 
- Para traduzir, dimensionar, inclinar e girar uma exibição da Web, use a propriedade [RenderTransform](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform).
- Para controlar a opacidade do modo de exibição da Web, defina a propriedade [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity).
- Para especificar uma cor para usar como o plano de fundo da página da Web quando o conteúdo HTML não especificar uma cor, defina a propriedade [DefaultBackgroundColor](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor). 

**Obtém o título da página da web**

Você pode obter o título do documento HTML exibido atualmente no modo de exibição da Web usando a propriedade o [DocumentTitle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.documenttitle). 

**Eventos de entrada e a ordem de tabulação**

Embora o WebView não seja uma subclasse Control, ele irá receber o foco de entrada do teclado e participar na sequência de tabulação. Ele fornece um método [Focus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.focus), além de eventos [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) e [LostFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus), mas não tem nenhuma propriedade relacionada a guia. Sua posição na sequência de tabulação é a mesma que sua posição na ordem do documento de XAML. A sequência de tabulação inclui todos os elementos no conteúdo de modo de exibição da Web que pode receber o foco de entrada. 

Conforme indicado na Tabela de eventos na página da classe [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView), o modo de exibição da web não dá suporte a maioria dos eventos de entrada de usuário herdados do [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement), como [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown), [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) e [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed). Em vez disso, você pode usar o [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) com a função **eval** do JavaScript para usar os manipuladores de eventos HTML e usar o **window.external.notify** do manipulador de eventos HTML para notificar o aplicativo usando [WebView.ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify).

### <a name="navigating-to-content"></a>Navegando para conteúdo

Exibição da Web oferece várias APIs para navegação básica: [GoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goback), [GoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.goforward), [parar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.stop), [atualizar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.refresh), [CanGoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoback), e [CanGoForward](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cangoforward). Você pode usá-los para adicionar recursos típicos de navegação na Web a seu aplicativo. 

Para definir o conteúdo inicial do modo de exibição da Web, defina a propriedade [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.source) no XAML. O analisador XAML converte automaticamente a cadeia de caracteres para um [Uri](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

A propriedade Source pode ser definida no código, mas em vez de fazer isso, você normalmente usa um dos métodos **Navigate** para carregar conteúdo no código. 

Para carregar o conteúdo da web, use o método [Navigate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigate) com um **Uri** que usa o esquema http ou https. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Para navegar para o URI com uma solicitação POST e cabeçalhos HTTP, use o método [NavigateWithHttpRequestMessage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage). Esse método possui suporte apenas ao [HttpMethod.Post](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.post) e [HttpMethod.Get](https://docs.microsoft.com/uwp/api/windows.web.http.httpmethod.get) para o valor da propriedade [HttpRequestMessage.Method](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage.method). 

Para carregar o conteúdo descompactado e não criptografado a partir do armazenamento de dados [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) ou [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) do seu aplicativo, use o método **Navigate** com um **Uri** que usa o [ms-appdata scheme](/windows/uwp/app-resources/uri-schemes). O suporte a exibição da Web para esse esquema requer que você coloque o conteúdo em uma subpasta sob a pasta local ou temporary. Isso permite a navegação para URIs como ms-appdata:///local/*folder*/*file*.html e ms-appdata:///temp/*folder*/*file*.html. (para carregar arquivos compactados ou criptografados, consulte [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri)). 

Cada uma dessas subpastas de primeiro nível é isolada do conteúdo em outras subpastas de primeiro nível. Por exemplo, você pode navegar para ms-appdata:///temp/folder1/file.html, mas você não pode ter um link nesse arquivo para ms-appdata:///temp/folder2/file.html. No entanto, você ainda pode vincular ao conteúdo HTML no pacote do aplicativo usando o **ms-appx-web scheme** e ao conteúdo da Web usando os esquemas de URI **http** e **https**.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Para carregar o conteúdo do pacote do aplicativo, use o método **Navigate** com um **Uri** que usa o [ms-appx-web scheme](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)). 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Você pode carregar o conteúdo local por meio de uma resolução personalizada usando o método [NavigateToLocalStreamUri](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri). Isso possibilita cenários avançados, como download e armazenamento em cache de conteúdo baseado na Web para uso offline ou extração de conteúdo de um arquivo compactado.

### <a name="responding-to-navigation-events"></a>Respondendo a eventos de navegação

O controle de modo de exibição da Web fornece vários eventos que você pode usar para responder a navegação e estados de carregamento de conteúdo. Os eventos ocorrem na seguinte ordem para o conteúdo da exibição da web raiz: [NavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationstarting), [ContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.contentloading), [DOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded), [NavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** - Ocorre antes do modo de exibição da Web navegar para um novo conteúdo. Você pode cancelar a navegação em um manipulador para esse evento, definindo a propriedade WebViewNavigationStartingEventArgs.Cancel como true. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** - ocorre quando a exibição da Web começar a carregar um novo conteúdo. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** - ocorre quando o modo de exibição da Web tiver terminado de analisar o conteúdo HTML atual. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** - Ocorre quando o modo de exibição da Web tiver terminado de carregar o conteúdo atual ou se tiver ocorrido uma falha na navegação. Para determinar se a navegação falhou, verifique as propriedades [IsSuccess](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess) e [WebErrorStatus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus) da classe [WebViewNavigationCompletedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs). 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Eventos semelhantes ocorrem na mesma ordem para cada **iframe** no conteúdo de modo de exibição da Web: 
- [FrameNavigationStarting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting) - ocorre antes que um quadro na exibição da Web navegue para um novo conteúdo. 
- [FrameContentLoading](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framecontentloading) - ocorre quando um quadro na exibição da Web começou a carregar um novo conteúdo. 
- [FrameDOMContentLoaded](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded) - ocorre quando um quadro na Web concluiu a análise de seu conteúdo HTML atual. 
- [FrameNavigationCompleted](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted) - ocorre quando um quadro na exibição da Web tiver terminado de carregar o conteúdo. 

### <a name="responding-to-potential-problems"></a>Respondendo a possíveis problemas

Você pode responder a possíveis problemas com o conteúdo, como execução longa de scripts, conteúdo que não pode carregar a exibição da Web e avisos de conteúdo não seguro. 

Seu aplicativo pode parecer sem resposta durante a execução de scripts. O evento [LongRunningScriptDetected](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected) ocorre periodicamente enquanto o modo de exibição da Web executa o JavaScript e oferece uma oportunidade para interromper o script. Para determinar por quanto tempo o script foi executado, verifique a propriedade [ExecutionTime](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime) do [WebViewLongRunningScriptDetectedEventArgs](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs). Para interromper o script, defina a propriedade [StopPageScriptExecution](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution) dos argumentos do evento como **true**. O script interrompido não será executado novamente, a menos que ele seja recarregado durante uma navegação de modo de exibição da Web subsequente. 

O controle de modo de exibição da Web não pode hospedar tipos de arquivos arbitrários. Quando é feita uma tentativa de carregar o conteúdo que não pode hospedar o modo de exibição da Web, o evento [UnviewableContentIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified) ocorre. Você pode manipular esse evento e notificar o usuário ou usar a classe [Launcher](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) para redirecionar o arquivo para um navegador externo ou de outro aplicativo.

Da mesma forma, o evento [UnsupportedUriSchemeIdentified](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified) ocorre quando um esquema de URI que não é suportado é chamado no conteúdo da Web, como o fbconnect:// ou mailto://. Você pode manipular esse evento para fornecer um comportamento personalizado em vez de permitir o iniciador de sistema padrão iniciar o URI.

O [UnsafeContentWarningDisplayingevent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) ocorre quando o modo de exibição da Web mostra uma página de aviso para o conteúdo que foi relatado como inseguro pelo Filtro SmartScreen. Se o usuário optar por continuar a navegação, a navegação subsequente para a página não irá exibir o aviso nem disparar o evento.

### <a name="handling-special-cases-for-web-view-content"></a>Manipulando casos especiais para exibir o conteúdo da Web

Você pode usar a propriedade [ContainsFullScreenElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement) e o evento [ContainsFullScreenElementChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged) para detectar, responder e habilitar experiências de tela inteira no conteúdo da Web, por exemplo, na reprodução de vídeo em tela inteira. Por exemplo, você pode usar o evento ContainsFullScreenElementChanged para redimensionar o modo de exibição da Web para ocupar todo o modo de exibição do aplicativo ou, como o exemplo a seguir ilustra, colocar um aplicativo de janela no modo de tela inteira quando uma experiência de Internet em tela inteira é desejada.

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Você pode usar o evento [NewWindowRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested) para tratar casos onde o conteúdo da web hospedado solicita uma nova janela para ser exibido, como uma janela pop-up. Você pode usar outro controle WebView para exibir o conteúdo da janela solicitada.

Use o evento [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) para habilitar os recursos da Web que exigem recursos especiais. Atualmente, isso inclui localização geográfica, armazenamento IndexedDB e áudio e vídeo do usuário (por exemplo, de um microfone ou webcam). Se seu aplicativo acessa a localização ou mídia do usuário, você ainda precisa declarar essa funcionalidade no manifesto do aplicativo. Por exemplo, um aplicativo que usa a localização geográfica precisa no mínimo das seguintes declarações de funcionalidade no Package.appxmanifest:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

Além da manipulação do evento [PermissionRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) pelo aplicativo, o usuário terá que aprovar caixas de diálogo do sistema padrão para aplicativos que solicitam recursos de mídia ou local para que esses recursos sejam ativados.

Aqui está um exemplo de como um aplicativo permitiria a localização geográfica em um mapa do Bing:

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Se seu aplicativo requer entrada do usuário ou outras operações assíncronas para responder a uma solicitação de permissão, use o método [Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) de [WebViewPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) para criar um [WebViewDeferredPermissionRequest](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest) que pode ser tratado em um momento posterior. Consulte [WebViewPermissionRequest.Defer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer). 

Se os usuários devem sair com segurança de um site hospedado em um modo de exibição da Web ou em outros casos, quando a segurança é importante, chame o método estático [ClearTemporaryWebDataAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) para apagar o todo o conteúdo armazenado em cache localmente em uma sessão de modo de exibição da Web. Isso impede que usuários mal-intencionados acessem dados confidenciais. 

### <a name="interacting-with-web-view-content"></a>Interagindo com o conteúdo de modo de exibição da Web

Você pode interagir com o conteúdo do modo de exibição da Web usando o método [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) para chamar ou inserir scripts no conteúdo de modo de exibição da Web e o evento [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) para obter informações de volta do conteúdo de modo de exibição da Web.

Para chamar o JavaScript dentro do conteúdo de modo de exibição da Web, use o método [InvokeScriptAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync). O script chamado pode retornar apenas os valores de cadeia de caracteres. 

Por exemplo, se o conteúdo de um modo de exibição da web chamado `webView1` contém uma função denominada `setDate` que aceita 3 parâmetros, você pode chamá-lo assim. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Você pode usar o **InvokeScriptAsync** com a função do JavaScript **eval** para inserir conteúdo na página da Web.

Aqui, o texto de uma caixa de texto XAML (`nameTextBox.Text`) é gravado em um div em uma página HTML hospedada em `webView1`. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Scripts no conteúdo de modo de exibição da Web podem usar **window.external.notify** com um parâmetro de cadeia de caracteres para enviar informações de volta para seu aplicativo. Para receber essas mensagens, manipule o evento [ScriptNotify](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.scriptnotify). 

Para que uma página da Web externa possa acionar o evento **ScriptNotify** ao chamar window.external.notify, inclua o URI da página na seção **ApplicationContentUriRules** do manifesto do aplicativo. (Você pode fazer isso no Microsoft Visual Studio na guia URIs de conteúdo do designer Package. appxmanifest.) Os URIs nessa lista devem usar HTTPS e pode conter caracteres curinga de subdomínio (por exemplo, `https://*.microsoft.com`), mas elas não podem conter caracteres curinga do domínio (por exemplo, `https://*.com` e `https://*.*`). O requisito de manifesto não se aplica ao conteúdo que se origina no pacote do aplicativo, que usa um URI ms-local-stream:// ou que seja carregado usando o método [NavigateToString](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.navigatetostring). 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Acessando o Windows Runtime em um modo de exibição da Web

Você pode usar o método [AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) para injetar uma instância de uma classe nativa de um componente do Tempo de Execução do Windows no contexto do JavaScript do modo de exibição da Web. Isso permite acesso completo aos métodos, propriedades e eventos nativos desse objeto no conteúdo JavaScript desse modo de exibição da Web. A classe deve ser decorada com o atributo [AllowForWeb](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute). 

Por exemplo, este código insere uma instância do `MyClass` importado de um componente do Windows Runtime em um modo de exibição da Web.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Para obter mais informações, consulte [WebView.AddWebAllowedObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject). 

Além disso, o conteúdo confiável do JavaScript em um modo de exibição da Web pode ter permissão para acessar diretamente as APIs do Windows Runtime. Isso proporciona poderosas capacidades nativas para aplicativos Web hospedados em um modo de exibição da Web. Para habilitar esse recurso, o URI do conteúdo confiável deve estar na lista de permissões e no ApplicationContentUriRules do aplicativo em Package.appxmanifest, com WindowsRuntimeAccess especificamente definido para "all". 

Este exemplo mostra uma seção do manifesto do aplicativo. Aqui, um URI local recebe acesso ao Windows Runtime. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>Opções de hospedagem do conteúdo da Web

Você pode usar a propriedade [WebView.Settings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.settings) (do tipo [WebViewSettings](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings)) para controlar se o JavaScript e IndexedDB estão habilitados. Por exemplo, se você usar um modo de exibição da Web para exibir conteúdo estritamente estático, convém desativar o JavaScript para obter melhor desempenho.

### <a name="capturing-web-view-content"></a>Captura de conteúdo do modo de exibição da Web

Para habilitar o compartilhamento de conteúdo do modo de exibição da Web com outros aplicativos, use o método [CaptureSelectedContentToDataPackageAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync), que retorna o conteúdo selecionado como um [DataPackage](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Esse método é assíncrono, então você deve usar um adiamento para impedir que seu manipulador de evento [DataRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) retorne antes da chamada assíncrona ser concluída. 

Para obter uma imagem de visualização do conteúdo atual do modo de exibição da Web, use o método [CapturePreviewToStreamAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync). Esse método cria uma imagem do conteúdo atual e grava o fluxo especificado. 

### <a name="threading-behavior"></a>Comportamento de threading

Por padrão, o conteúdo de modo de exibição da Web é hospedado no thread da interface do usuário em dispositivos na família de dispositivos da área de trabalho e fora do thread de interface do usuário em todos os outros dispositivos. Você pode usar a propriedade estática [WebView.DefaultExecutionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode) para consultar o comportamento de threading padrão para o cliente atual. Se necessário, você pode usar o construtor [WebView(WebViewExecutionMode)](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webview.) para substituir esse comportamento. 

> **Observação**&nbsp;&nbsp;Pode haver problemas de desempenho ao hospedar conteúdo no thread da interface do usuário em dispositivos móveis, então não deixe de testar em todos os dispositivos de destino quando você alterar o DefaultExecutionMode.

Um modo de exibição da Web que hospeda o conteúdo fora do thread de interface do usuário não é compatível com o controle dos pais que exigem gestos para propagar a partir do controle de modo de exibição da Web ao pai, como [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView), [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)e outros controles relacionados. Esses controles não poderão receber gestos iniciados no modo de exibição da Web fora do thread. Além disso, a impressão de conteúdo da web fora do thread não possui suporte diretamente. Você deve imprimir um elemento com o [WebViewBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) preenchido.

## <a name="recommendations"></a>Recomendações


-   Certifique-se de que o site carregado está formatado corretamente para o dispositivo e utiliza cores, tipografia e navegação consistentes com o restante de seu aplicativo.
-   Os campos de entrada devem ser dimensionados adequadamente. Os usuários podem não perceber que é possível ampliá-los para inserir texto.
-   Se um modo de exibição na Web não se parece com o resto de seu aplicativo, considere a possibilidade de utilizar controles ou meios alternativos para realizar tarefas relevantes. Se o modo de exibição na Web corresponder ao resto de seu aplicativo, os usuários o verão como uma experiência perfeita.



## <a name="related-topics"></a>Tópicos relacionados

* [Classe WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)
 

 




