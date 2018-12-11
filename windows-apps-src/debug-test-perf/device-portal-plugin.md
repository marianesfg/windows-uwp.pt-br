---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Escrever um plug-in personalizado para o Device Portal
description: Saiba como escrever um aplicativo UWP que use o Windows Device Portal para hospedar uma página da Web e fornecer informações de diagnóstico.
ms.date: 03/24/2017
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: d9e11445d77434320c8842608bf8183a078c0660
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8899444"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Criar um plug-in personalizado para o Device Portal

Saiba como escrever um aplicativo UWP que use o Windows Device Portal para hospedar uma página da Web e fornecer informações de diagnóstico.

A partir da versão Atualização para Criadores, você poderá usar o Device Portal para hospedar interfaces de diagnóstico do seu app. Este artigo aborda as três partes necessárias para criar um DevicePortalProvider para seu app – as alterações appxmanifest, a configuração da conexão do seu app ao serviço Device Portal e a manipulação de uma solicitação de entrada. Um app de exemplo também é fornecido como introdução (em breve). 

## <a name="create-a-new-uwp-app-project"></a>Criar um novo projeto aplicativo UWP
Neste guia, criaremos tudo em uma única solução por questão de simplicidade.

No Microsoft Visual Studio 2017, crie um projeto de aplicativo UWP. Vá para Arquivo > Novo Projeto e selecione Modelos > Visual C# > Universal do Windows > Aplicativo em branco (Universal do Windows). Chame-o de "DevicePortalProvider". Esse será o aplicativo que conterá o serviço de aplicativo. Escolha o SDK da Atualização para Criadores para dar suporte.  Talvez seja necessário atualizar o Visual Studio ou instalar o novo SDK – veja [aqui](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/) para obter detalhes. 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>Adicionar a extensão devicePortalProvider ao seu arquivo package.appxmanifest
Você precisará adicionar algum código ao seu arquivo *package.appxmanifest* para torná-lo funcional como um plug-in do Device Portal. Primeiro, adicione as seguintes definições de namespace à parte superior do arquivo. Além disso, adicione-as ao atributo `IgnorableNamespaces`.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Para declarar que seu app é um Provedor do Device Portal, será necessário criar um serviço de app e uma nova extensão de Provedor de Device Portal que o utilize. Adicione a extensão windows.appService e a extensão windows.devicePortalProvider ao elemento `Extensions`sob `Application`. Verifique se os atributos `AppServiceName`correspondem em cada extensão. Isso indica ao serviço Device Portal que esse serviço de app pode ser iniciado para manipular solicitações no namespace do manipulador. 

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

O atributo `HandlerRoute`faz referência ao namespace REST declarado pelo seu app. Todas as solicitações HTTP feitas nesse namespace (implicitamente seguidas por um caractere curinga) recebidas pelo serviço device Portal serão enviadas para seu app para serem manipuladas. Nesse caso, qualquer HTTP autenticada com êxito para `<ip_address>/MyNamespace/api/*`será enviada para seu app. Os conflitos entre as rotas do manipulador são liquidados por meio de uma verificação "a mais longa vence": qualquer rota que corresponda a mais solicitações será selecionada, o que significa que uma solicitação para /MyNamespace/api/foo" corresponderá a um provedor com "/MyNamespace/api" em vez de um com "/MyNamespace".  

Dois novos recursos são necessários para essa funcionalidade. eles também devem ser adicionados ao seu arquivo *package.appxmanifest*.

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
> A funcionalidade "devicePortalProvider" é restrita ("rescap"), o que significa que você deve obter aprovação anterior da Loja para que seu app possa ser publicado lá. No entanto, isso não impede que você teste seu app localmente por meio de sideload. Para sabe rmais sobre funcionalidades restritas, veja [Declarações de funcionalidades do app](https://msdn.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities).

## <a name="set-up-your-background-task-and-winrt-component"></a>Configurar sua tarefa em segundo plano e o Componente WinRT
Para configurar a conexão do Device Portal, seu app deve vincular uma conexão de serviço de app do serviço Device Portal com a instância do Device Portal em execução no app. Para fazer isso, adicione um novo componente WinRT ao seu aplicativo com uma classe que implemente [**IBackgroundTask**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtask).

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Certifique-se de que seu nome corresponda ao namespace e ao nome da classe configurados pelo AppService EntryPoint ("MySampleProvider.SampleProvider"). Quando você fizer sua primeira solicitação ao provedor Device Portal, o Device Portal ocultará a solicitação, iniciará a tarefa em segundo plano do seu app, chamará seu método **Run** e passará um [**IBackgroundTaskInstance**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Seu app o utilizará então para configurar uma instância de [**DevicePortalConnection**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection).

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

Há dois eventos que devem ser manipulados pelo aplicativo para concluir a solicitação de manipulação de loop: **Closed**, para sempre que o serviço Device Portal for desligado, e solicitações de [**RequestReceived**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), que expõe HTTP de entrada e fornece a principal funcionalidade do provedor Device Portal. 

## <a name="handle-the-requestreceived-event"></a>Manipular o evento RequestReceived
O evento **RequestReceived** será gerado assim que que cada solicitação HTTP seja feita na Rota do Manipulador especificada em seu plug-in. A solicitação de manipulação de loop para provedores do Device Portal é semelhante à do NodeJS Express: os objetos de solicitação e resposta são fornecidos junto com o evento e o manipulador responde preenchendo o objeto de resposta. Em provedores do Device Portal, o evento **RequestReceived** e seus manipuladores usam os objetos [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httprequestmessage) e [**HttpResponseMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpresponsemessage).   

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

Nesse manipulador de solicitações de exemplo, primeiro fazemos pull dos objetos de solicitação e de resposta do parâmetro *args*, então criamos uma cadeia de caracteres com a URL de solicitação e uma formatação HTML adicional. Isso é adicionado no objeto Response como uma instância de [**HttpStringContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpstringcontent). Outras classes [**IHttpContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.ihttpcontent), como as para "String" e "Buffer", também são permitidas.

A resposta é então definida como uma resposta HTTP e recebe um código de status 200 (OK). Ela deve ser renderizada conforme o esperado no navegador que fez a chamada original. Observe que, quando o manipulador de eventos **RequestReceived** retorna, a mensagem de resposta é automaticamente retornada ao agente do usuário: nenhum método "send" adicional é necessário.

![mensagem de resposta do device portal](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Fornecimento de conteúdo estático
O conteúdo estático pode ser servido diretamente de uma pasta dentro de seu pacote, facilitando a adição de uma interface do usuário ao seu provedor.  A maneira mais fácil de servir conteúdo estático é criar uma pasta de conteúdo em seu projeto que possa ser mapeada para uma URL.

![pasta de conteúdo estático do device portal](images/device-portal/plugin-static-content.png)
 
Em seguida, adicione um manipulador de rotas ao manipulador de eventos **RequestReceived** que detecte rotas de conteúdo estático e mapeie uma solicitação de forma adequada.  

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

![configuração de cópia do arquivo de conteúdo estático](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Uso de APIs e recursos existentes no Device Portal
O conteúdo estático servido por um fornecedor do Device Portal é servido na mesma porta do que o serviço principal do Device Portal.  Isso significa que você pode fazer referência ao JS e ao CSS incluídos no Device Portal com marcas `<link>`e `<script>` simples em seu HTML. Em geral, sugerimos o uso de *rest.js*, que encapsula todas as principais APIs do Device Portal em um objeto webbRest conveniente, e o arquivo *common.css*, que permitirá que você defina o estilo de seu conteúdo para ajustá-lo ao restante da interface do usuário do Device Portal. Você pode ver um exemplo nona página *index.html* incluída no exemplo. Ele usa *rest.js* para recuperar o nome do dispositivo e os processos em execução do Device Portal. 

![saída do plug-in do device portal](images/device-portal/plugin-output.png)
 
Importante: o uso dos métodos HttpPost/DeleteExpect200 em webbRest executará automaticamente a [manipulação de CSRF](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) para você, o que permite que a sua página da Web chame APIs REST de alteração de estado.  

> [!NOTE] 
> O conteúdo estático incluído no Device Portal não vem com uma garantia em relação a alterações significativas. Embora não se espere que as APIs mudem com frequência, elas poderão fazer isso, especialmente nos arquivos *common.js* e *controls.js*, que o seu provedor não deverá usar. 

## <a name="debugging-the-device-portal-connection"></a>Depuração a conexão do Device Portal
Para depurar a tarefa em segundo plano, você deve alterar a maneira como o Visual Studio executa seu código. Siga as etapas abaixo para depuração de uma conexão de serviço de app para verificar como seu provedor está manipulando as solicitações HTTP:

1.  No menu Depurar, selecione Propriedades de DevicePortalProvider. 
2.  Na guia Depuração, na seção da ação Iniciar, selecione "Não executar, mas depurar o meu código quando ele for iniciado".  
![colocar o plug-in em modo de depuração](images/device-portal/plugin-debug-mode.png)
3.  Defina um ponto de interrupção na função do manipulador RequestReceived.
![ponto de interrupção no manipulador requestreceived](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Verifique se a arquitetura de compilação corresponde exatamente à arquitetura do destino. Se estiver usando um computador de 64 bits, você deve implantar usando uma compilação AMD64. 
4.  Pressione F5 para implantar seu app
5.  Desative o Desativar Device Portal e então o ative novamente para que ele localize seu app (isso só será necessário quando o manifesto do app for alterado - nas outras ocasiões, você poderá simplesmente reimplantar e ignorar esta etapa). 
6.  Em seu navegador, acesse o namespace do provedor e o ponto de interrupção deverá ser alcançado.

## <a name="related-topics"></a>Tópicos relacionados
* [Visão geral do Windows Device Portal](device-portal.md)
* [Criar e consumir um serviço de app](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


