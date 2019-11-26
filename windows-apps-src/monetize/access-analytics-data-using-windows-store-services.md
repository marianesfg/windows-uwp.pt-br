---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use a API de análise de Microsoft Store para recuperar dados de análise programaticamente para aplicativos registrados em sua conta do Windows Partner Center da sua organização.
title: Acessar dados analíticos usando serviços da Store
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 71c59049b76219d6f9360748e9ca11ea84542e47
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259314"
---
# <a name="access-analytics-data-using-store-services"></a>Acessar dados analíticos usando serviços da Store

Use a *API de análise de Microsoft Store* para recuperar dados de análise programaticamente para aplicativos registrados em sua conta do Windows Partner Center da sua organização. Essa API permite que você recupere dados para aquisições de aplicativos e complementos (também conhecidos como produto no aplicativo ou IAP), erros, classificações de aplicativos e avaliações. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de análise da Microsoft Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de análise da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
3.  [Chame a API de análise da Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Etapa 1: Concluir os pré-requisitos para usar a API de análise da Microsoft Store

Antes de começar a escrever o código para chamar a API de análise da Microsoft Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sem custo adicional.

* Você deve associar um aplicativo do Azure AD à sua conta do Partner Center, recuperar a ID do locatário e a ID do cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de análise da Microsoft Store. Você precisa da ID do locatário, ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.
    > [!NOTE]
    > Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo do Azure AD à sua conta do Partner Center e recuperar os valores necessários:

1.  No Partner Center, [associe a conta do Partner Center da sua organização ao diretório do Azure ad da sua organização](../publish/associate-azure-ad-with-partner-center.md).

2.  Em seguida, na página **usuários** na seção **configurações de conta** do Partner Center, [adicione o aplicativo do Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa o aplicativo ou serviço que você usará para acessar dados de análise para sua conta do Partner Center. Certifique-se de atribuir esse aplicativo à **Manager**. Se o aplicativo ainda não existir no diretório do Azure AD, você poderá [criar um novo aplicativo do Azure AD no Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Volte para a página **Usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de análise da Microsoft Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para o valor da *ID de\_do locatário* no URI de postagem e os parâmetros de id de *\_* do cliente e\_de *segredo do cliente* , especifique a ID do locatário, a ID do cliente e a chave do aplicativo que você recuperou do Partner Center na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Etapa 3: Chamar a API de análise da Microsoft Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de análise da Microsoft Store. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

### <a name="methods-for-uwp-apps-and-games"></a>Métodos para aplicativos UWP e jogos
Os métodos a seguir estão disponíveis para aquisições de aplicativos e jogos e aquisições de complemento: 

* [Obtenha dados de aquisições para seus jogos e aplicativos](acquisitions-data.md)
* [Obter dados de aquisições de complemento para seus jogos e aplicativos](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>Métodos para aplicativos UWP 

Os seguintes métodos de análise estão disponíveis para aplicativos UWP no Partner Center.

| Cenário       | Métodos      |
|---------------|--------------------|
| Aquisições, conversões, instalações e uso |  <ul><li>[Obter aquisições de aplicativos](get-app-acquisitions.md) (Herdado)</li><li>[Obter dados de funil de aquisição de aplicativo](get-acquisition-funnel-data.md) (Herdado)</li><li>[Obter conversões de aplicativo por canal](get-app-conversions-by-channel.md)</li><li>[Obter aquisições de complemento](get-in-app-acquisitions.md)</li><li>[Obter aquisições de complemento de assinatura](get-subscription-acquisitions.md)</li><li>[Obter conversões de complemento por canal](get-add-on-conversions-by-channel.md)</li><li>[Obter instalações de aplicativos](get-app-installs.md)</li><li>[Obter uso diário do aplicativo](get-app-usage-daily.md)</li><li>[Obter uso mensal do aplicativo](get-app-usage-monthly.md)</li></ul> |
| Erros de app | <ul><li>[Obter dados de relatório de erros](get-error-reporting-data.md)</li><li>[Obter detalhes de um erro em seu aplicativo](get-details-for-an-error-in-your-app.md)</li><li>[Obter o rastreamento de pilha para obter um erro em seu aplicativo](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Baixe o arquivo CAB para obter um erro em seu aplicativo](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Obter dados de informações para seu aplicativo](get-insights-data-for-your-app.md)</li></ul>  |
| Classificações e opiniões | <ul><li>[Obter classificações de aplicativo](get-app-ratings.md)</li><li>[Obter análises de aplicativos](get-app-reviews.md)</li></ul> |
| Anúncios no app e campanhas publicitárias | <ul><li>[Obter dados de desempenho do AD](get-ad-performance-data.md)</li><li>[Obter dados de desempenho da campanha do AD](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Métodos para aplicativos da área de trabalho

Os métodos de análise a seguir estão disponíveis para uso por contas de desenvolvedor que pertencem ao [programa Aplicativo da Área de Trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program).

| Cenário       | Métodos      |
|---------------|--------------------|
| Instalações |  <ul><li>[Obter instalações de aplicativos da área de trabalho](get-desktop-app-installs.md)</li></ul> |
| Trava |  <ul><li>[Obter blocos de atualização para seu aplicativo de desktop](get-desktop-block-data.md)</li><li>[Obter detalhes do bloco de atualização para seu aplicativo de desktop](get-desktop-block-data-details.md)</li></ul> |
| Erros de aplicativo |  <ul><li>[Obter dados de relatório de erros para seu aplicativo de desktop](get-desktop-application-error-reporting-data.md)</li><li>[Obter detalhes de um erro em seu aplicativo de área de trabalho](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obter o rastreamento de pilha para obter um erro em seu aplicativo de desktop](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Baixe o arquivo CAB para obter um erro em seu aplicativo de desktop](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Obter dados de informações para seu aplicativo de desktop](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Métodos para serviços Xbox Live

Os métodos adicionais a seguir estão disponíveis para uso por contas de desenvolvedor com jogos que usam os [serviços Xbox Live](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md).

| Cenário       | Métodos      |
|---------------|--------------------|
| Análises gerais |  <ul><li>[Obter dados do Xbox Live Analytics](get-xbox-live-analytics.md)</li><li>[Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obter dados de uso simultâneo do Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Análise de integridade |  <ul><li>[Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Análise de comunidade |  <ul><li>[Obter dados do hub de jogos do Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obter dados do clube do Xbox Live](get-xbox-live-club-data.md)</li><li>[Obter dados do Xbox Live multiplayer](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Métodos para jogos Xbox One

Os seguintes métodos adicionais estão disponíveis para uso por contas de desenvolvedor com os jogos do Xbox One que foram ingeridos por meio do portal do desenvolvedor do Xbox (XDP) e disponíveis no painel de análise do XDP.

| Cenário       | Métodos      |
|---------------|--------------------|
| Aquisições |  <ul><li>[Obtenha aquisições de jogos do Xbox One](get-xbox-one-game-acquisitions.md)</li><li>[Obter aquisições de complemento do Xbox One](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Erros |  <ul><li>[Obter dados de relatório de erros para o seu jogo do Xbox One](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Obter detalhes de um erro em seu jogo do Xbox One](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Obter o rastreamento de pilha para obter um erro em seu jogo Xbox One](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Baixe o arquivo CAB para obter um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Métodos para hardware e drivers

As contas de desenvolvedor que pertencem ao [programa do painel de hardware do Windows](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) têm acesso a um conjunto adicional de métodos para recuperar dados de análise de hardware e drivers. Para obter mais informações, consulte [API do painel de hardware](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de análise da Microsoft Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](https://www.newtonsoft.com/json) da Newtonsoft para desserializar os dados JSON retornados pela API de análise da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>Respostas de erro

A API de análise da Microsoft Store retorna respostas de erro em um objeto JSON que contém códigos de erro e mensagens. O exemplo a seguir demonstra uma resposta de erro causada por um parâmetro inválido.

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
