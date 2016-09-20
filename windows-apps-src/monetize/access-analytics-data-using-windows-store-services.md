---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Use a API de análise da Windows Store para recuperar de forma programática dados analíticos de aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows."
title: "Acesse dados analíticos usando serviços da Windows Store."
ms.sourcegitcommit: 204bace243fb082d3ca3b4259982d457f9c533da
ms.openlocfilehash: 30388a975e9623c5511abe608aa1b21956e2c974

---

# Acesse dados analíticos usando serviços da Windows Store.


\[Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Uso da *API de análise da Windows Store* para recuperar, de forma programática, dados analíticos de aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows. Essa API permite que você recupere dados de aplicativo e aquisições IAP, erros e classificações e avaliações de aplicativo. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

## Pré-requisitos para usar a API de análise da Windows Store


-   Você (ou sua organização) deve ter um diretório do Azure AD. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Ou você pode [fazer isso gratuitamente](http://go.microsoft.com/fwlink/p/?LinkId=703757).
-   Você deve ter uma [conta de usuário](https://azure.microsoft.com/documentation/articles/active-directory-create-users/) no diretório do Azure AD que você deseja associar à sua conta do Centro de Desenvolvimento do Windows.

## Usando a API de análise da Windows Store


Antes de poder usar a API de análise da Windows Store, você deve associar um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento e obter um token de acesso do Azure AD. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de análise da Windows Store. Depois que tiver um token de acesso, você poderá chamar a API de análise da Windows Store a partir do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo de ponta a ponta:

1.  
            [Associe um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento do Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account).
2.  
            [Obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token).
3.  
            [Chame a API de análise da Windows Store](#call-the-windows-store-analytics-api).


### Associe um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento do Windows

1.  No Centro de Desenvolvimento, acesse suas **Configurações de conta**, clique em **Gerenciar usuários** e associe a sua conta do Centro de Desenvolvimento ao diretório do Azure AD da sua organização. Para obter instruções detalhadas, consulte [Gerenciar usuários de conta](https://msdn.microsoft.com/library/windows/apps/mt489008). Você pode, de forma alternativa, adicionar outros usuários do diretório do Azure AD da sua organização para que eles também possam acessar a conta no Centro de Desenvolvimento.

    > 
            **Observação**  Apenas uma conta do Centro de Desenvolvimento pode ser associada ao Active Directory do Azure. Da mesma forma, apenas um Active Directory do Azure pode ser associado uma conta do Centro de Desenvolvimento. Depois de estabelecer essa associação, você não poderá removê-la sem contatar o suporte.

     

2.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos do Azure AD**, adicione o aplicativo do Azure AD que representa o aplicativo ou o serviço que você usará para acessar dados analíticos de sua conta do Centro de Desenvolvimento e atribua-o a função **Gerente**. Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**. Para saber mais, consulte a seção sobre o gerenciamento de aplicativos do Azure AD em [Manage account users](https://msdn.microsoft.com/library/windows/apps/mt489008).

3.  Volte para a página **Gerenciar usuários**, clique no nome do seu aplicativo do Azure AD para ir para as configurações do aplicativo e clique em **Adicionar nova chave**. Na tela seguinte, copie os valores **ID do cliente** e **Chave**. Para saber mais, consulte a seção sobre o gerenciamento de aplicativos do Azure AD em [Manage account users](https://msdn.microsoft.com/library/windows/apps/mt489008). Você precisa da ID e da chave do cliente para obter um token de acesso do Azure AD a ser usado ao chamar a API de análise da Windows Store. Você não poderá acessar essas informações novamente depois que você sair desta página.


### Obtenha um token de acesso do Azure AD

Depois de associar um aplicativo do Azure AD à sua conta no Centro de Desenvolvimento e recuperar o ID do cliente e a chave do aplicativo, você pode usar essas informações para obter o token de acesso do Azure AD. Você precisa de um token de acesso antes de poder chamar qualquer um dos métodos na API de análise da Windows Store. Depois de criar um token de acesso, você terá 60 minutos para usá-lo antes que ele expire.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://msdn.microsoft.com/library/azure/dn645543.aspx) para enviar um HTTP POST para o seguinte ponto de extremidade do Azure AD.

```syntax
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   Para obter seu ID de locatário, faça logon no [Portal de gerenciamento do Azure](http://manage.windowsazure.com/), navegue até **Active Directory** e clique no diretório que você vinculou à sua conta do Centro de Desenvolvimento. A ID de locatário para este diretório é incorporada à URL dessa página, conforme mostrado pela cadeia de caracteres *your\_tenant\_ID* no exemplo a seguir.

  ```syntax
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   Para os parâmetros *client\_id* e *client\_secret*, especifique a ID do cliente e a chave do seu aplicativo que você recuperou anteriormente do Centro de Desenvolvimento.
-   Para o parâmetro *resource*, especifique o seguinte URI: ```https://manage.devcenter.microsoft.com```.


### Chame a API de análise da Windows Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de análise da Windows Store. Para saber mais sobre a sintaxe de cada método, consulte os artigos a seguir. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

-   [Obtenha aquisições de aplicativo](get-app-acquisitions.md)
-   [Obtenha aquisições IAP](get-in-app-acquisitions.md)
-   [Obtenha dados de relatório de erros](get-error-reporting-data.md)
-   [Obtenha classificações de aplicativo](get-app-ratings.md)
-   [Obtenha avaliações de aplicativo](get-app-reviews.md)

## Exemplo de código


O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de análise da Windows Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](http://www.newtonsoft.com/json) do Newtonsoft para desserializar os dados JSON devolvidos pela API de análise da Windows Store.

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get IAP acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## Respostas de erro


A API de análise da Windows Store devolve respostas de erro em um objeto JSON que contém códigos de erro e mensagens. O exemplo a seguir demonstra uma resposta de erro causada por um parâmetro inválido.

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## Tópicos relacionados

* [Obtenha aquisições de aplicativo](get-app-acquisitions.md)
* [Obtenha aquisições IAP](get-in-app-acquisitions.md)
* [Obtenha dados de relatório de erros](get-error-reporting-data.md)
* [Obtenha classificações de aplicativo](get-app-ratings.md)
* [Obter avaliações de aplicativo](get-app-reviews.md)
 



<!--HONumber=Jun16_HO5-->


