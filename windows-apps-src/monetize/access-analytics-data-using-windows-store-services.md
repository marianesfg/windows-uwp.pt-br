---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: Use a API de análise da Microsoft Store para recuperar de forma programática dados analíticos de aplicativos que estão registrados em sua organização ' s conta do Partner Center do Windows.
title: Acessar dados analíticos usando serviços da Store
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0f0df87f8ed5339c977dbd468f8aa2a7877f0d9e
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690554"
---
# <a name="access-analytics-data-using-store-services"></a>Acessar dados analíticos usando serviços da Store

Use a *API de análise da Microsoft Store* para recuperar de forma programática dados analíticos de aplicativos que estão registrados em Partner Center do Windows conta do sua organização. Essa API permite que você recupere dados para aquisições de aplicativos e complementos (também conhecidos como produto no aplicativo ou IAP), erros, classificações de aplicativos e avaliações. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de análise da Microsoft Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de análise da Microsoft Store antes que ele expire. Depois que o token expirar, você poderá gerar um novo.
3.  [Chame a API de análise da Microsoft Store](#call-the-windows-store-analytics-api).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>Etapa 1: Concluir os pré-requisitos para usar a API de análise da Microsoft Store

Antes de começar a escrever o código para chamar a API de análise da Microsoft Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sem nenhum custo adicional.

* Você deve associar um aplicativo Azure AD à sua conta do Partner Center, recuperar a ID do locatário e ID do cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de análise da Microsoft Store. Você precisa a ID do locatário, a ID do cliente e a chave para obter um token de acesso do Azure AD que você passa para a API.
    > [!NOTE]
    > Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo Azure AD à sua conta do Partner Center e recuperar os valores necessários:

1.  No Partner Center, [associe a conta do Partner Center da sua organização com o diretório do Azure AD da sua organização](../publish/associate-azure-ad-with-partner-center.md).

2.  Em seguida, na página **usuários** na seção **configurações da conta** do Partner Center, [Adicione o aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa o aplicativo ou serviço que você usará para acessar dados analíticos de sua conta do Partner Center. Certifique-se de atribuir esse aplicativo à **Manager**. Se o aplicativo ainda não existe no diretório do Azure AD, você pode [criar um novo aplicativo Azure AD no Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).

3.  Volte para a página **Usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de análise da Microsoft Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

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

Para o valor de *tenant\_id* no POST URI e os parâmetros *client\_id* e *client\_secret* , especifique a ID de locatário, ID de cliente e a chave para seu aplicativo que você recuperou do Partner Center na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>Etapa 3: Chamar a API de análise da Microsoft Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de análise da Microsoft Store. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

### <a name="methods-for-uwp-apps"></a>Métodos para aplicativos UWP

Os métodos de análise a seguir estão disponíveis para aplicativos UWP no Partner Center.

| Cenário       | Métodos      |
|---------------|--------------------|
| Aquisições, conversões, instalações e uso |  <ul><li>[Obter aquisições de app](get-app-acquisitions.md)</li><li>[Obter dados de funil de aquisição do app](get-acquisition-funnel-data.md)</li><li>[Obter conversões de app por canal](get-app-conversions-by-channel.md)</li><li>[Obter aquisições de complemento](get-in-app-acquisitions.md)</li><li>[Obter aquisições de complemento de assinatura](get-subscription-acquisitions.md)</li><li>[Obter conversões de complemento por canal](get-add-on-conversions-by-channel.md)</li><li>[Obter as instalações do app](get-app-installs.md)</li><li>[Obter uso do app diariamente](get-app-usage-daily.md)</li><li>[Obter uso do app mensalmente](get-app-usage-monthly.md)</li></ul> |
| Erros de app | <ul><li>[Obter dados de relatórios de erros](get-error-reporting-data.md)</li><li>[Obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md)</li><li>[Obter o rastreamento de pilha de um erro em seu aplicativo](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[Baixar o arquivo CAB de um erro em seu aplicativo](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[Obter dados de insights do seu app](get-insights-data-for-your-app.md)</li></ul>  |
| Classificações e opiniões | <ul><li>[Obter classificações de app](get-app-ratings.md)</li><li>[Obter avaliações de app](get-app-reviews.md)</li></ul> |
| Anúncios no app e campanhas publicitárias | <ul><li>[Obter dados de desempenho de anúncios](get-ad-performance-data.md)</li><li>[Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>Métodos para aplicativos da área de trabalho

Os métodos de análise a seguir estão disponíveis para uso por contas de desenvolvedor que pertencem ao [programa Aplicativo da Área de Trabalho do Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504).

| Cenário       | Métodos      |
|---------------|--------------------|
| Instalações |  <ul><li>[Obter instalações de aplicativo da área de trabalho](get-desktop-app-installs.md)</li></ul> |
| Blocos |  <ul><li>[Obter bloqueios de upgrade para seu aplicativo da área de trabalho](get-desktop-block-data.md)</li><li>[Obter detalhes do bloqueio de upgrade para seu aplicativo da área de trabalho](get-desktop-block-data-details.md)</li></ul> |
| Erros de aplicativo |  <ul><li>[Obter dados de relatório de erros do seu aplicativo da área de trabalho](get-desktop-application-error-reporting-data.md)</li><li>[Obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md)</li><li>[Obter o rastreamento de pilha de um erro em seu aplicativo da área de trabalho](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[Baixar o arquivo CAB de um erro em seu aplicativo da área de trabalho](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[Obter dados de insights do seu aplicativo da área de trabalho](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>Métodos para serviços Xbox Live

Os métodos adicionais a seguir estão disponíveis para uso por contas de desenvolvedor com jogos que usam os [serviços Xbox Live](../xbox-live/developer-program-overview.md).

| Cenário       | Métodos      |
|---------------|--------------------|
| Análises gerais |  <ul><li>[Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)</li><li>[Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)</li><li>[Obter dados de uso simultâneo do Xbox Live](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| Análise de integridade |  <ul><li>[Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)</li></ul> |
| Análise de comunidade |  <ul><li>[Obter dados do Hub de Jogos Xbox Live](get-xbox-live-game-hub-data.md)</li><li>[Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)</li><li>[Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>Métodos para jogos Xbox One

Os métodos adicionais a seguir estão disponíveis para uso por contas de desenvolvedor com jogos Xbox One inseridos por meio do Portal de desenvolvedor do Xbox (XDP) e disponíveis no painel de análise XDP.

| Cenário       | Métodos      |
|---------------|--------------------|
| Aquisições |  <ul><li>[Obter aquisições de jogos Xbox One](get-xbox-one-game-acquisitions.md)</li><li>[Obter aquisições de complementos do Xbox One](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| Erros |  <ul><li>[Obter dados de relatório para seu Xbox One erros jogo](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[Obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[Obter o rastreamento de pilha de um erro em seu Xbox One jogo](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[Baixar o arquivo CAB para um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>Métodos para hardware e drivers

Contas de desenvolvedor que pertencem ao [programa de painel de hardware do Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) têm acesso a um conjunto adicional de métodos para recuperar dados analíticos de hardware e drivers. Para obter mais informações, consulte [o painel de Hardware API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api).

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de análise da Microsoft Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](http://www.newtonsoft.com/json) da Newtonsoft para desserializar os dados JSON retornados pela API de análise da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

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
