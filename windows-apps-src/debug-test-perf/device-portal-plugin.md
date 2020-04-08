---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Escrever um plug-in personalizado para o portal de dispositivos
description: Saiba como escrever um aplicativo UWP que use o Portal de Dispositivos do Windows para hospedar uma página da Web e fornecer informações de diagnóstico.
ms.date: 03/24/2017
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 4881fe961979243849728d3f835c449e0f71f4b4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683839"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Escrever um plug-in personalizado para o Portal de Dispositivos

Saiba como escrever um aplicativo UWP que use o Portal de Dispositivos do Windows para hospedar uma página da Web e fornecer informações de diagnóstico.

Da versão Atualização para Criadores em diante, você poderá usar o Portal de Dispositivos para hospedar interfaces de diagnóstico do seu aplicativo. Este artigo aborda as três partes necessárias para criar um DevicePortalProvider para seu aplicativo – as alterações appxmanifest, a configuração da conexão do seu aplicativo ao serviço Portal de Dispositivos e a manipulação de uma solicitação de entrada. Um aplicativo de exemplo também é fornecido como introdução (em breve). 

## <a name="create-a-new-uwp-app-project"></a>Criar um projeto de aplicativo UWP
Neste guia, criaremos tudo em uma só solução por questão de simplicidade.

No Microsoft Visual Studio 2019, crie um projeto de aplicativo UWP. Vá para Arquivo > Novo > Projeto e selecione Aplicativo em Branco (Windows Universal) para C# e clique em Avançar. Na caixa de diálogo Configurar seu novo projeto, nomeie o projeto "DevicePortalProvider" e clique em Criar. Esse será o aplicativo que conterá o serviço de aplicativo. Escolha "Atualização do Windows 10 para Criadores (10.0; Build 15063)" para dar suporte.  Talvez seja necessário atualizar o Visual Studio ou instalar o novo SDK – veja [aqui](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/) para obter detalhes. 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>Adicionar a extensão devicePortalProvider ao seu arquivo package.appxmanifest
Você precisará adicionar algum código ao seu arquivo *package.appxmanifest* para torná-lo funcional como um plug-in do Portal de Dispositivos. Primeiro, adicione as definições de namespace a seguir à parte superior do arquivo. Além disso, adicione-as ao atributo `IgnorableNamespaces`.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Para declarar que seu aplicativo é um Provedor do Portal de Dispositivos, será necessário criar um serviço de aplicativo e uma extensão de Provedor do Portal de Dispositivos que o utilize. Adicione a extensão windows.appService e a extensão windows.devicePortalProvider ao elemento `Extensions` sob `Application`. Verifique se os atributos `AppServiceName` correspondem em cada extensão. Isso indica ao serviço Portal de Dispositivos que esse serviço de aplicativo pode ser iniciado para manipular solicitações no namespace do manipulador. 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

O atributo `HandlerRoute` faz referência ao namespace REST declarado pelo seu aplicativo. Todas as solicitações HTTP feitas nesse namespace (implicitamente seguidas por um caractere curinga) recebidas pelo serviço Portal de Dispositivos serão enviadas ao seu aplicativo para serem manipuladas. Nesse caso, qualquer solicitação HTTP autenticada com êxito para `<ip_address>/MyNamespace/api/*` será enviada para seu aplicativo. Os conflitos entre as rotas do manipulador são liquidados por meio de uma verificação "a mais longa vence": qualquer rota que corresponda a mais solicitações será selecionada, o que significa que uma solicitação para /MyNamespace/api/foo" corresponderá a um provedor com "/MyNamespace/api" em vez de um com "/MyNamespace".  

Dois novos recursos são necessários para essa funcionalidade. Eles também devem ser adicionados ao seu arquivo *package.appxmanifest*.

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> A funcionalidade "devicePortalProvider" é restrita ("rescap"), o que significa que você deve obter aprovação anterior da Store para que seu aplicativo possa ser publicado lá. No entanto, isso não impede que você teste seu aplicativo localmente por meio de sideload. Para saber mais sobre funcionalidades restritas, confira [Declarações de funcionalidades do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

## <a name="set-up-your-background-task-and-winrt-component"></a>Configurar sua tarefa em segundo plano e o Componente WinRT
Para configurar a conexão do Portal de Dispositivos, seu aplicativo precisa vincular uma conexão de serviço de aplicativo do serviço Portal de Dispositivos com a instância do Portal de Dispositivos em execução no aplicativo. Para fazer isso, adicione um novo componente WinRT ao seu aplicativo com uma classe que implemente [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask).

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Verifique se seu nome corresponde ao namespace e ao nome da classe configurados pelo AppService EntryPoint ("MySampleProvider.SampleProvider"). Quando você fizer sua primeira solicitação ao provedor do Portal de Dispositivos, o Portal de Dispositivos realizará o stash da solicitação, iniciará a tarefa em segundo plano do seu aplicativo, chamará seu método **Run** e passará um [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Seu aplicativo o utilizará então para configurar uma instância [**DevicePortalConnection**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection).

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

Há dois eventos que devem ser manipulados pelo aplicativo para concluir a solicitação de manipulação de loop: **Fechado**, para sempre que o serviço Portal de Dispositivos for desligado e [**RequestReceived**](https://docs.microsoft.com/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), que expõe as solicitações HTTP de entrada e fornece a funcionalidade principal do provedor do Portal de Dispositivos. 

## <a name="handle-the-requestreceived-event"></a>Manipular o evento RequestReceived
O evento **RequestReceived** será gerado assim que cada solicitação HTTP for feita na Rota do Manipulador especificada em seu plug-in. A solicitação de manipulação de loop para provedores do Portal de dispositivos é semelhante à do NodeJS Express: os objetos de solicitação e resposta são fornecidos junto com o evento e o manipulador responde preenchendo o objeto de resposta. Em provedores do Portal de Dispositivos, o evento **RequestReceived** e seus manipuladores usam os objetos [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestmessage) e [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/windows.web.http.httpresponsemessage).   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

Nesse manipulador de solicitações de exemplo, primeiro efetuamos pull dos objetos de solicitação e de resposta do parâmetro *args*, então criamos uma cadeia de caracteres com a URL de solicitação e uma formatação HTML adicional. Isso é adicionado ao objeto Response como uma instância de [**HttpStringContent**](https://docs.microsoft.com/uwp/api/windows.web.http.httpstringcontent). Outras classes [**IHttpContent**](https://docs.microsoft.com/uwp/api/windows.web.http.ihttpcontent), como as para "String" e "Buffer", também são permitidas.

A resposta é então definida como uma resposta HTTP e recebe um código de status 200 (OK). Ela deve ser renderizada conforme o esperado no navegador que fez a chamada original. Observe que, quando o manipulador de eventos **RequestReceived** retorna, a mensagem de resposta é automaticamente retornada ao agente do usuário: nenhum método "send" adicional é necessário.

![mensagem de resposta do portal de dispositivos](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Fornecimento de conteúdo estático
O conteúdo estático pode ser servido diretamente de uma pasta dentro de seu pacote, facilitando a adição de uma interface do usuário ao seu provedor.  A maneira mais fácil de servir conteúdo estático é criando uma pasta de conteúdo em seu projeto que possa ser mapeada para uma URL.

![pasta de conteúdo estático do portal de dispositivos](images/device-portal/plugin-static-content.png)
 
Em seguida, adicione um manipulador de rotas ao manipulador de eventos **RequestReceived** que detecta rotas de conteúdo estático e mapeie uma solicitação de maneira adequada.  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
Verifique se todos os arquivos dentro da pasta de conteúdo estão marcados como "Conteúdo" e defina como "Copiar se mais recente" ou "Copiar sempre" no menu Propriedades do Visual Studio.  Isso garante que os arquivos estejam dentro do seu Pacote AppX quando ele for implantado.  

![configurar cópia do arquivo de conteúdo estático](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Uso de APIs e recursos existentes no Portal de Dispositivos
O conteúdo estático servido por um provedor do Portal de Dispositivos é servido na mesma porta que a do serviço principal do Portal de Dispositivos.  Isso significa que você pode fazer referência ao JS e ao CSS incluídos no Portal de Dispositivos com marcas `<link>` e `<script>` simples em seu HTML. Em geral, sugerimos o uso de *rest.js*, que encapsula todas as principais APIs do Portal de Dispositivos em um objeto webbRest conveniente, e o arquivo *common.css*, que permitirá que você defina o estilo de seu conteúdo para ajustá-lo ao restante da interface do usuário do Portal de Dispositivos. Você pode ver um exemplo disso na página *index.html* incluída no exemplo. Ele usa *rest.js* para recuperar o nome do dispositivo e os processos em execução do Portal de Dispositivos. 

![saída do plug-in do portal de dispositivos](images/device-portal/plugin-output.png)
 
Importante: o uso dos métodos HttpPost/DeleteExpect200 em webbRest executará automaticamente a [manipulação de CSRF](https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) para você, o que permite que a sua página da Web chame APIs REST de alteração de estado.  

> [!NOTE] 
> O conteúdo estático incluído no Portal de Dispositivos não vem com uma garantia em relação a alterações significativas. Embora não se espere que as APIs mudem com frequência, elas poderão fazer isso, especialmente nos arquivos *common.js* e *controls.js*, que o seu provedor não deverá usar. 

## <a name="debugging-the-device-portal-connection"></a>Depuração da conexão do Portal de Dispositivos
Para depurar a tarefa em segundo plano, você precisa alterar a maneira como o Visual Studio executa seu código. Siga as etapas abaixo para depurar uma conexão de serviço de aplicativo a fim de verificar como seu provedor está manipulando as solicitações HTTP:

1.  No menu Depurar, selecione Propriedades de DevicePortalProvider. 
2.  Na guia Depuração, na seção da ação Iniciar, selecione "Não executar, mas depurar o meu código quando ele for iniciado".  
![colocar o plug-in em modo de depuração](images/device-portal/plugin-debug-mode.png)
3.  Defina um ponto de interrupção na função do manipulador RequestReceived.
![ponto de interrupção no manipulador requestreceived](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Verifique se a arquitetura de build corresponde exatamente à arquitetura do destino. Se estiver usando um computador de 64 bits, você precisará implantar usando um build AMD64. 
4.  Pressione F5 para implantar seu aplicativo
5.  Desative o Portal de Dispositivos e ative-o novamente para que ele localize seu aplicativo (isso só será necessário quando o manifesto do aplicativo for alterado – nas outras ocasiões, você poderá simplesmente reimplantar e ignorar esta etapa). 
6.  Em seu navegador, acesse o namespace do provedor e o ponto de interrupção deverá ser alcançado.

## <a name="related-topics"></a>Tópicos relacionados
* [Visão geral do Portal de Dispositivos do Windows](device-portal.md)
* [Criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


