---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "Use a API de análise da Windows Store para recuperar de forma programática dados analíticos de aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows."
title: "Acessar dados analíticos usando serviços da Loja"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, serviços da Loja, API de análise da Windows Store"
ms.openlocfilehash: aa33af63a49d890b3c60ec1bee32528cfc78af93
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="access-analytics-data-using-store-services"></a>Acessar dados analíticos usando serviços da Loja

Uso da *API de análise da Windows Store* para recuperar, de forma programática, dados analíticos de aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows. Essa API permite que você recupere dados para aquisições de aplicativos e complementos (também conhecidos como produto no aplicativo ou IAP), erros, classificações de aplicativos e avaliações. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de análise da Windows Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de análise da Windows Store antes que ele expire. Depois que o token expirar, você poderá gerar um novo.
3.  [Chame a API de análise da Windows Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-analytics-api"></a>Etapa 1: complete os pré-requisitos para usar a API de análise da Windows Store

Antes de começar a escrever o código para chamar a API de análise da Windows Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sem nenhum custo adicional.

* Você deve associar um aplicativo Azure AD à sua conta do Centro de Desenvolvimento, recuperar a ID do locatário e a ID de cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de análise da Windows Store. Você precisa a ID do locatário, a ID do cliente e a chave para obter um token de acesso do Azure AD que você passa para a API.

  >**Observação**&nbsp;&nbsp;Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo Azure AD à sua conta do Centro de Desenvolvimento e recuperar os valores necessários:

1.  No Centro de Desenvolvimento, acesse suas **Configurações de conta**, clique em **Gerenciar usuários** e associe a sua conta do Centro de Desenvolvimento ao diretório do Azure AD da sua organização. Para obter instruções detalhadas, consulte [Gerenciar usuários de conta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos do Azure AD**, adicione o aplicativo do Azure AD que representa o aplicativo ou o serviço que você usará para acessar dados analíticos de sua conta do Centro de Desenvolvimento e atribua-o a função **Gerente**. Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**. Para obter mais informações, consulte [Adicionar e gerenciar aplicativos Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Volte para a página **Gerenciar usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte as informações sobre o gerenciamento de chaves em [Adicionar e gerenciar aplicativos Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de análise da Windows Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para os valor de *tenant\_id* no POST URI e os parâmetros *client\_id* e *client\_secret*, especifique a ID do locatário, a ID do cliente e a chave para o aplicativo que você recuperou do Centro de Desenvolvimento na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />
## <a name="step-3-call-the-windows-store-analytics-api"></a>Etapa 3: Chame a API de análise da Windows Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de análise da Windows Store. Para saber mais sobre a sintaxe de cada método, consulte os artigos a seguir. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

| Cenário       | Métodos      |
|---------------|--------------------|
| Aquisições e instalações |  <ul><li>[Obter aquisições de app](get-app-acquisitions.md)</li><li>[Obter aquisições de complemento](get-in-app-acquisitions.md)</li><li>[Obter as instalações do aplicativo](get-app-installs.md)</li></ul> |
| Erros de app | <ul><li>[Obter dados de relatórios de erros](get-error-reporting-data.md)</li><li>[Obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md)</li><li>[Obter o rastreamento de pilha de um erro em seu app](get-the-stack-trace-for-an-error-in-your-app.md)</li></ul> |
| Classificações e opiniões | <ul><li>[Obter classificações de app](get-app-ratings.md)</li><li>[Obter avaliações de app](get-app-reviews.md)</li></ul> |
| Anúncios no app e campanhas publicitárias | <ul><li>[Obter dados de desempenho de anúncios](get-ad-performance-data.md)</li><li>[Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)</li></ul> |

Os métodos adicionais a seguir estão disponíveis para uso por contas de desenvolvedor que pertencem ao [programa do Centro de Desenvolvimento de Hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

| Cenário       | Descrição      |
|---------------|--------------------|
| Erros nos drivers do Windows 10 (para IHVs) |  <ul><li>[Obter dados de relatório de erro para drivers do Windows 10](get-error-reporting-data-for-windows-10-drivers.md)</li><li>[Obter detalhes sobre um erro de driver do Windows 10](get-details-for-a-windows-10-driver-error.md)</li><li>[Baixe o arquivo CAB para um erro de driver do Windows 10](download-the-cab-file-for-a-windows-10-driver-error.md)</li></ul> |
| Erros nos drivers do Windows 7/Windows 8.x (para IHVs) |  <ul><li>[Obter dados de relatório de erro para drivers do Windows 7 e Windows 8. x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)</li><li>[Obtenha informações sobre o erro de driver do Windows 7 ou Windows 8. x](get-details-for-a-windows-7-or-windows-8.x-driver-error.md)</li><li>[Baixe o arquivo CAB para um erro de driver do Windows 7 ou Windows 8. x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)</li></ul> |
| Erros de hardware (para OEMs) |  <ul><li>[Obter dados de relatórios de erros de hardware OEM](get-oem-hardware-error-reporting-data.md)</li><li>[Obter detalhes de um erro de hardware OEM](get-details-for-an-oem-hardware-error.md)</li><li>[Baixe o arquivo CAB de um erro de hardware OEM](download-the-cab-file-for-an-oem-hardware-error.md)</li></ul> |

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de análise da Windows Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](http://www.newtonsoft.com/json) do Newtonsoft para desserializar os dados JSON devolvidos pela API de análise da Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Respostas de erro

A API de análise da Windows Store retorna respostas de erro em um objeto JSON que contém códigos de erro e mensagens. O exemplo a seguir demonstra uma resposta de erro causada por um parâmetro inválido.

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
