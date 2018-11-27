---
title: Gerenciador de Contas da Web
description: Este artigo descreve como usar o AccountsSettingsPane para conectar seu aplicativo da Plataforma Universal do Windows (UWP) a provedores de identidade externos, como a Microsoft ou o Facebook, usando as APIs do Gerenciador de Contas da Web do Windows 10.
ms.date: 12/6/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: b0a052d08c6b0816d977d3e86881540194075818
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7711847"
---
# <a name="web-account-manager"></a>Gerenciador de Contas da Web

Este artigo descreve como usar o **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** para conectar seu aplicativo da Plataforma Universal do Windows (UWP) a provedores de identidade externos, como a Microsoft ou o Facebook, usando as APIs do Gerenciador de Contas da Web do Windows 10. Você aprenderá a solicitar a permissão de um usuário para usar a conta da Microsoft dele, obter um token de acesso e usá-lo para realizar operações básicas (como obter dados de perfil ou carregar arquivos na conta do OneDrive). As etapas são semelhantes para obter permissão do usuário e acesso com qualquer provedor de identidade que ofereça suporte ao Gerenciador de Contas da Web.

> [!NOTE]
> Para obter um exemplo de código completo, consulte o [exemplo de WebAccountManagement no GitHub](http://go.microsoft.com/fwlink/p/?LinkId=620621).

## <a name="get-set-up"></a>Preparar-se

Primeiro, crie um aplicativo em branco no Visual Studio. 

Segundo, para conectar a provedores de identidade, será necessário associar seu aplicativo à Loja. Para isso, clique com o botão direito do mouse no projeto, escolha **Loja** > **Associar o aplicativo à Loja** e siga as instruções do assistente. 

Terceiro, crie uma interface do usuário básica que consiste em um botão XAML simples e duas caixas de texto.

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

E um manipulador de eventos conectado ao botão no code-behind:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

Por último, adicione os seguintes namespaces para que você não precise se preocupar posteriormente com problemas de referência: 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>Mostrar o painel de configurações de conta

O sistema fornece uma interface do usuário interna para gerenciar provedores de identidade e contas da Web chamada de **AccountSettingsPane**. Você pode exibi-la da seguinte forma:

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

Se você executar o aplicativo e clicar no botão "Fazer logon", uma janela vazia deverá ser exibida. 

![Painel de configurações da conta](images/tb-1.png)

O painel está vazio porque o sistema fornece somente um shell de interface do usuário - cabe ao desenvolvedor popular o painel de forma programática com os provedores de identidade. 

## <a name="register-for-accountcommandsrequested"></a>Registro para AccountCommandsRequested

Para adicionar comandos ao painel, começamos o registro para o manipulador de eventos AccountCommandsRequested. Isso diz para o sistema executar nossa lógica de compilação quando o usuário pedir para ver o painel (p. ex.: clica no botão XAML). 

No code-behind, substitua os eventos OnNavigatedTo e OnNavigatedFrom e adicione o seguinte código: 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

Os usuários não interagem com contas frequentemente; por isso, fazer o registro e o cancelamento de registro do manipulador de eventos dessa maneira ajuda a prevenir perda de memória. Dessa forma, o painel personalizado estará somente na memória quando houver uma grande possibilidade de que um usuário o solicitará (porque ele está em uma página de "configurações" ou "logon", por exemplo). 

## <a name="build-the-account-settings-pane"></a>Criar o painel de configurações da conta

O método BuildPaneAsync é chamado sempre que o **AccountSettingsPane** é mostrado. Esse é o local onde colocaremos o código para personalizar os comandos exibidos no painel. 

Comece obtendo um adiamento. Isso diz para o sistema adiar a exibição do **AccountsSettingsPane** até que sua criação seja concluída.

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

Em seguida, obtenha um provedor usando o método WebAuthenticationCoreManager.FindAccountProviderAsync. A URL para o provedor varia de acordo com o provedor e pode ser encontrada na documentação do provedor. Para as Contas da Microsoft e o Azure Active Directory, é "https://login.microsoft.com". 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

Observe que também passamos a cadeia de caracteres "consumers" ao parâmetro opcional *authority*. Isso ocorre porque a Microsoft fornece dois tipos diferentes de autenticação - Contas da Microsoft (MSA) para "consumers", e Azure Active Directory (AAD) para "organizations". A autoridade "consumers" indica que queremos a opção MSA. Se você estiver desenvolvendo um aplicativo empresarial, use a string "organizations".

Por fim, adicione o provedor ao **AccountsSettingsPane** criando um novo **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** da seguinte forma: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

O método GetMsaToken que passamos para o novo **WebAccountProviderCommand** ainda não existe (ele será criado na próxima etapa), por isso sinta-se à vontade para adicioná-lo como um método vazio por enquanto.

Execute o código acima, e o painel deve ter a seguinte aparência: 

![Painel de configurações da conta](images/tb-2.png)

### <a name="request-a-token"></a>Solicitar um token

Assim que a opção da Conta da Microsoft for exibida no **AccountsSettingsPane**, será necessário manipular o que acontecerá quando o usuário a selecionar. Registramos nosso método GetMsaToken para ser acionado quando o usuário optar por fazer logon com a Conta da Microsoft, por isso obteremos o token ali. 

Para obter um token, use o método RequestTokenAsync da seguinte forma: 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

Neste exemplo, passamos a cadeia de caracteres ""wl.basic" para o parâmetro de _escopo_. Escopo representa o tipo de informação que você está solicitando do serviço em um usuário específico. Determinados escopos fornecem acesso somente a informações básicas do usuário, como nome e email, enquanto outros escopos podem conceder acesso a informações confidenciais, como as fotos do usuário ou a caixa de entrada de email. Em geral, seu app deve usar o escopo menos permissivo necessário para obter sua função. Os provedores de serviço fornecerão documentação sobre quais escopos são necessários para obter tokens para uso com os serviços. 

* Para os escopos do Office 365 e do Outlook.com, consulte [Autenticar APIs do Office 365 e do Outlook.com usando o ponto de extremidade de autenticação v 2.0](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2). 
* Para escopos do OneDrive, consulte [Autenticação e credenciais no OneDrive](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes). 

> [!TIP]
> Como alternativa, se seu aplicativo usa uma dica de logon (para preencher o campo de usuário com um endereço de email padrão) ou outra propriedade especial relacionados à experiência entrar, listá-lo na propriedade **[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** . Isso fará com que o sistema ignorar a propriedade ao cache de conta da web, o que impede que as diferenças de conta no cache.

Se estiver desenvolvendo um aplicativo empresarial, você provavelmente desejará se conectar a uma instância do Azure Active Directory (AAD) e usar a API do Microsoft Graph em vez de serviços MSA normais. Nesse cenário, use o código a seguir: 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

O restante deste artigo continua descrevendo o cenário MSA, mas o código para AAD é muito semelhante. Para saber mais sobre AAD/Graph, incluindo um exemplo completo no GitHub, consulte a [Documentação do Microsoft Graph](https://graph.microsoft.io/docs/platform/get-started).

## <a name="use-the-token"></a>Usar o token

O método RequestTokenAsync retorna um objeto WebTokenRequestResult, que contém os resultados de sua solicitação. Se a solicitação foi bem-sucedida, conterá um token.  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> Se você receber um erro ao solicitar um token, verifique se associou seu app à Store, conforme descrito na etapa um. Seu aplicativo não conseguirá obter um token se você ignorou essa etapa. 

Quando você tiver um token, poderá usá-lo para chamar a API do provedor. No código abaixo, chamaremos a [API do Microsoft Live de informações do usuário](https://msdn.microsoft.com/library/hh826533.aspx) para obter informações básicas sobre o usuário e exibi-las em nossa interface do usuário. Observe, porém, que na maioria dos casos é recomendável que você armazene o token obtido uma vez e depois o utilize em um método separado.

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

O modo como você chama diversas APIs REST varia de acordo com o provedor; consulte a documentação de API do provedor para obter informações sobre como usar seu token. 

## <a name="store-the-account-for-future-use"></a>Armazenar a conta para uso futuro

Tokens são úteis para obter imediatamente informações sobre um usuário, mas eles geralmente têm tempo de vida variável - os tokens MSA, por exemplo, são válidos por algumas horas apenas. Felizmente, não é necessário mostrar novamente o **AccountsSettingsPane** sempre que um token expirar. Quando um usuário autoriza seu aplicativo uma vez, você pode armazenar as informações da conta do usuário para uso futuro. 

Para fazer isso, use a classe **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)**. A **WebAccount** é retornada pelo mesmo método usado para solicitar o token:

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

Quando você tiver um **WebAccount**, poderá armazená-la com facilidade. No exemplo a seguir, usamos LocalSettings. Para obter mais informações sobre como usar LocalSettings e outros métodos para armazenar dados do usuário, consulte [Armazenar e recuperar configurações e dados do app](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

Em seguida, podemos usar um método assíncrono semelhante ao seguinte para tentar obter um token em segundo plano com a **WebAccount** armazenada.

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

Coloque o método acima pouco antes do código que cria o **AccountsSettingsPane**. Se o token for obtido em segundo plano, não será necessário mostrar o painel. 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

Obter um token silenciosamente é bastante simples, por isso você deve usar esse processo para atualizar o token entre sessões em vez de armazenar em cache um token existente (pois esse token pode expirar a qualquer momento).

> [!NOTE]
> O exemplo acima aborda somente os casos básicos de êxito e falha. Seu aplicativo também deve considerar cenários incomuns (como um usuário que revoga a permissão do aplicativo ou remove sua conta do Windows, por exemplo) e manipulá-los corretamente.  

## <a name="remove-a-stored-account"></a>Remover uma conta armazenada

Se você persistir uma conta da Web, talvez queira oferecer aos usuários a capacidade de desassociar a conta ao app. Dessa forma, eles podem efetivamente "logoff" do aplicativo: suas informações de conta não serão carregadas automaticamente após a inicialização. Para fazer isso, primeiro remova qualquer conta salva e as informações do armazenamento. Em seguida, chame **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** para limpar o cache e invalide qualquer token existente que seu app possa ter. 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>Adicionar provedores que não dão suporte ao WebAccountManager

Se você deseja integrar a autenticação de um serviço ao seu aplicativo, mas esse serviço não dá suporte ao WebAccountManager - Google+ ou Twitter, por exemplo - você ainda pode adicionar manualmente o provedor ao **AccountsSettingsPane**. Para isso, crie um novo objeto WebAccountProvider e forneça um nome e ícone .png, depois adicione-o à lista WebAccountProviderCommands. Veja um código stub: 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> Isso apenas adiciona um ícone ao **AccountsSettingsPane** e executa o método especificado quando o ícone for clicado (GetTwitterTokenAsync, neste caso). Você deve fornecer o código que manipula a autenticação real. Para obter mais informações, consulte (Agente de autenticação da Web)[web-authentication-broker], que fornece métodos auxiliares para autenticação usando serviços REST. 

## <a name="add-a-custom-header"></a>Adicionar um cabeçalho personalizado

Você pode personalizar o painel de configurações da conta usando a propriedade HeaderText, da seguinte forma: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![Painel de configurações da conta](images/tb-3.png)

Não exagere com texto de cabeçalho; seja conciso e direto. Se o processo de logon for complicado e você precisar exibir mais informações, vincule o usuário a uma página separada usando um link personalizado. 

## <a name="add-custom-links"></a>Adicionar links personalizados

Você pode adicionar comandos personalizados ao AccountsSettingsPane, que aparecem como links abaixo de seu WebAccountProviders com suporte. Comandos personalizados são ótimos para tarefas simples relacionadas a contas de usuário, como exibir uma política de privacidade ou iniciar uma página de suporte para os usuários que estão com problemas. 

Aqui está um exemplo: 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![Painel de configurações da conta](images/tb-4.png)

Teoricamente, você pode usar comandos de configurações para tudo. No entanto, sugerimos limitar seu uso a cenários intuitivos relacionados à conta como os descritos acima. 

## <a name="see-also"></a>Consulte também

[Namespace Windows.Security.Authentication.Web.Core](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Namespace Windows.Security.Credentials](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[Classe AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Agente de autenticação da Web](web-authentication-broker.md)

[Exemplo de gerenciamento de contas da Web](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[App Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
