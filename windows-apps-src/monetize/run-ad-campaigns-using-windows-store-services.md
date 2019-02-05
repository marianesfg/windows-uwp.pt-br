---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Use a API de promoções da Microsoft Store para gerenciar de forma programática campanhas publicitárias promocionais para aplicativos que estão registrados em Partner Center conta do sua organização.
title: Veicular campanhas publicitárias usando os serviços da Store
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API de promoções da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 58325074a2f59dcd146a9534054b302b3ce9956b
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/04/2019
ms.locfileid: "9044673"
---
# <a name="run-ad-campaigns-using-store-services"></a>Veicular campanhas publicitárias usando os serviços da Store

Use a *API de promoções da Microsoft Store* para gerenciar de forma programática campanhas publicitárias promocionais para aplicativos que estão registrados em Partner Center conta do sua organização. Essa API permite que você crie, atualize e monitore suas campanhas e outros ativos relacionados, como direcionamento e criativos. Essa API é especialmente útil para desenvolvedores que criam grandes volumes de campanhas e que desejam fazer isso sem usar o Partner Center. Essa API usa o Azure Active Directory (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de promoções da Microsoft Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você terá 60 minutos para usá-lo em chamadas à API de promoções da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
3.  [Chame a API de promoções da Microsoft Store](#call-the-windows-store-promotions-api).

Como alternativa, você pode criar e gerenciar campanhas publicitárias usando o Partner Center e todas as campanhas publicitárias que você criar programaticamente através de promoções da Microsoft Store que API também pode ser acessado no Partner Center. Para obter mais informações sobre como gerenciar as campanhas publicitárias no Partner Center, consulte [criar uma campanha publicitária para seu aplicativo](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Qualquer desenvolvedor com uma conta do Partner Center pode usar a API de promoções da Microsoft Store para gerenciar as campanhas publicitárias de seus aplicativos. Agências de mídia também podem solicitar acesso a essa API para executar campanhas publicitárias em nome dos seus anunciantes. Se você for uma agência de mídia que deseja saber mais sobre essa API ou solicitar acesso à ela, envie sua solicitação para storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Etapa 1: Concluir os pré-requisitos para usar a API de promoções da Microsoft Store

Antes de começar a escrever o código para chamar a API de promoções da Microsoft Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Antes de poder criar e iniciar uma campanha publicitária usando essa API com êxito, você deve primeiro [criar uma campanha publicitária usando a página de **campanhas publicitárias** no Partner Center](../publish/create-an-ad-campaign-for-your-app.md), e você deve adicionar pelo menos um método de pagamento nesta página. Depois que você fizer isso, é possível criar linhas de entrega faturáveis para campanhas publicitárias usando essa API. Linhas de entrega de campanhas publicitárias criadas usando essa API cobram automaticamente o instrumento de pagamento padrão escolhido na página **campanhas publicitárias** no Partner Center.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](https://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sem nenhum custo adicional.

* Você deve associar um aplicativo Azure AD à sua conta do Partner Center, recuperar a ID do locatário e ID do cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de promoções da Microsoft Store. Você precisa da ID do locatário, ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.
    > [!NOTE]
    > Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo Azure AD à sua conta do Partner Center e recuperar os valores necessários:

1.  No Partner Center, [associe a conta do Partner Center da sua organização com o diretório do Azure AD da sua organização](../publish/associate-azure-ad-with-partner-center.md).

2.  Em seguida, na página **usuários** na seção **configurações da conta** do Partner Center, [Adicione o aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa o aplicativo ou serviço que você usará para gerenciar campanhas promocionais de sua conta do Partner Center. Certifique-se de atribuir esse aplicativo à **Manager**. Se o aplicativo ainda não existe no diretório do Azure AD, você pode [criar um novo aplicativo Azure AD no Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Volte para a página **Usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de promoções da Microsoft Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```syntax
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

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Etapa 3: Chamar a API de promoções da Microsoft Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de promoções da Microsoft Store. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

No contexto da API de promoções da Microsoft Store, uma campanha publicitária consiste em um objeto de *campanha* que contém informações de alto nível sobre a campanha e outros objetos que representam as *linhas de entrega*, *perfis de direcionamento*, e *criativos* para a campanha publicitária. A API inclui diferentes conjuntos de métodos que são agrupados por esses tipos de objetos. Para criar uma campanha, você normalmente chama um método POST diferente para cada um desses objetos. A API também fornece métodos GET, que você pode usar para recuperar qualquer objeto e métodos PUT, que você pode usar para editar a campanha, linha de entrega e objetos de perfil de direcionamento.

Para obter mais informações sobre esses objetos e seus métodos relacionados, consulte a tabela a seguir.


| Objeto       | Descrição   |
|---------------|-----------------|
| Campanhas |  Esse objeto representa a campanha publicitária e fica na parte superior da hierarquia de modelos de objeto das campanhas publicitárias. Esse objeto identifica o tipo de campanha que você está executando (pago, doméstico ou comunidade), o objetivo de campanha, as linhas de entrega da campanha e outros detalhes. Cada campanha pode ser associada a apenas um aplicativo.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar as campanhas publicitárias](manage-ad-campaigns.md).<br/><br/>**Observação**&nbsp;&nbsp;depois de criar uma campanha publicitária, você pode recuperar dados de desempenho para a campanha usando o método [Obter dados de desempenho de campanha publicitária](get-ad-campaign-performance-data.md) na [API de análise da Microsoft Store](access-analytics-data-using-windows-store-services.md).  |
| Linhas de entrega | Cada campanha tem uma ou mais linhas de entrega que são usadas para comprar inventário e fornecer seus anúncios. Para cada linha de entrega, você pode definir direcionamento, definir o preço da oferta e decidir quanto você deseja gastar definindo um orçamento e vinculando a criativos que você deseja usar.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md). |
| Perfis de direcionamento | Cada linha de entrega tem um perfil de direcionamento que especifica os usuários, regiões geográficas e tipos de inventário que você deseja direcionar. Perfis de direcionamento podem ser criados como um modelo e compartilhados entre as linhas de entrega.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md). |
| Criativos | Cada linha de entrega tem um ou mais criativos que representam os anúncios que são mostrados para os clientes como parte da campanha. Um criativo pode ser associado a uma ou mais linhas de entrega, mesmo em campanhas publicitárias, dado que ele sempre representa o mesmo aplicativo.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md). |


O diagrama a seguir ilustra a relação entre campanhas, linhas de entrega, perfis de direcionamento, e criativos.

![Hierarquia de campanha publicitária](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de promoções da Microsoft Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](https://www.newtonsoft.com/json) do Newtonsoft para desserializar os dados JSON retornados pela API de promoções da Microsoft Store.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md)
* [Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)


 
