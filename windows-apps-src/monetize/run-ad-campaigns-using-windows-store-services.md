---
author: mcleanbyron
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: "Use a API de promoções da Windows Store para gerenciar de forma programática campanhas publicitárias promocionais para aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows."
title: "Veicular campanhas publicitárias usando os serviços da Loja"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de promoções da Windows Store, campanhas publicitárias"
ms.openlocfilehash: 4db2904ff23d52eb58fbe74f7591f7f05f2e4961
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2017
---
# <a name="run-ad-campaigns-using-store-services"></a>Veicular campanhas publicitárias usando os serviços da Loja

Use a *API de promoções da Windows Store* para gerenciar de forma programática campanhas publicitárias promocionais para aplicativos que são registrados na conta da sua organização no Centro de Desenvolvimento do Windows. Essa API permite que você crie, atualize e monitore suas campanhas e outros ativos relacionados, como direcionamento e criativos. Essa API é especialmente útil para desenvolvedores que criam grandes volumes de campanhas e que desejam fazer isso sem usar o painel do Centro de Desenvolvimento do Windows. Essa API usa o Azure Active Directory (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de promoções da Windows Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você terá 60 minutos para usá-lo em chamadas à API de promoções da Windows Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
3.  [Chame a API de promoções da Windows Store](#call-the-windows-store-promotions-api).

Como alternativa, você pode criar e gerenciar as campanhas publicitárias usando o painel do Centro de Desenvolvimento do Windows e todas as campanhas publicitárias que você criar programaticamente através da API de promoções da Windows Store também poderão ser acessadas no painel. Para obter mais informações sobre o gerenciamento de campanhas publicitárias no painel, consulte [Criar uma campanha publicitária para seu aplicativo](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Qualquer desenvolvedor com uma conta no Centro de Desenvolvimento do Windows pode usar a API de promoções da Windows Store para gerenciar as campanhas publicitárias de seus aplicativos. Agências de mídia também podem solicitar acesso a essa API para executar campanhas publicitárias em nome dos seus anunciantes. Se você for uma agência de mídia que deseja saber mais sobre essa API ou solicitar acesso à ela, envie sua solicitação para storepromotionsapi@microsoft.com.

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-promotions-api"></a>Etapa 1: complete os pré-requisitos para usar a API de promoções da Windows Store

Antes de começar a escrever o código para chamar a API de promoções da Windows Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Antes de poder criar e iniciar uma campanha publicitária usando essa API com êxito, primeiro você deve [criar uma campanha de anúncios pagos usando a página **Promover seu aplicativo** no painel do Centro de Desenvolvimento](../publish/create-an-ad-campaign-for-your-app.md) e, em seguida, é necessário adicionar pelo menos um método de pagamento nesta página. Depois que você fizer isso, é possível criar linhas de entrega faturáveis para campanhas publicitárias usando essa API. As linhas de entrega de campanhas publicitárias criadas usando essa API cobram automaticamente o instrumento de pagamento padrão escolhido na cobrança na página **Promover seu aplicativo** no painel.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Centro de Desenvolvimento](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account) sem nenhum custo adicional.

* Você deve associar um aplicativo Azure AD à sua conta do Centro de Desenvolvimento, recuperar a ID do locatário e a ID de cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de promoções da Windows Store. Você precisa da ID do locatário, ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.
    > [!NOTE]
    > Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo Azure AD à sua conta do Centro de Desenvolvimento e recuperar os valores necessários:

1.  No Centro de Desenvolvimento, acesse suas **Configurações de conta**, clique em **Gerenciar usuários** e [associe a sua conta do Centro de Desenvolvimento ao diretório do Azure AD da sua organização](../publish/associate-azure-ad-with-dev-center.md).

2.  Na página **Gerenciar usuários**, clique em **Adicionar apps do Azure AD**, adicione o app do Azure AD que representa o app ou o serviço que você usará para gerenciar campanhas promocionais de sua conta do Centro de Desenvolvimento e atribua a ele a função **Gerente**. Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**. Para obter mais informações, consulte [Adicionar aplicativos do Azure AD à sua conta do Centro de Desenvolvimento](../publish/add-users-groups-and-azure-ad-applications.md#azure-ad-applications).

3.  Volte para a página **Gerenciar usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de promoções da Windows Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

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

Para os valor de *tenant\_id* no POST URI e os parâmetros *client\_id* e *client\_secret*, especifique a ID do locatário, a ID do cliente e a chave para o aplicativo que você recuperou do Centro de Desenvolvimento na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-promotions-api" />
## <a name="step-3-call-the-windows-store-promotions-api"></a>Etapa 3: Chame a API de promoções da Windows Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de promoções da Windows Store. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

No contexto da API de promoções da Windows Store, uma campanha publicitária consiste em um objeto de *campanha* que contém informações de alto nível sobre a campanha e outros objetos que representam as *linhas de entrega*, *perfis de direcionamento*, e *criativos* para a campanha publicitária. A API inclui diferentes conjuntos de métodos que são agrupados por esses tipos de objetos. Para criar uma campanha, você normalmente chama um método POST diferente para cada um desses objetos. A API também fornece métodos GET, que você pode usar para recuperar qualquer objeto e métodos PUT, que você pode usar para editar a campanha, linha de entrega e objetos de perfil de direcionamento.

Para obter mais informações sobre esses objetos e seus métodos relacionados, consulte a tabela a seguir.


| Objeto       | Descrição   |
|---------------|-----------------|
| Campanhas |  Esse objeto representa a campanha publicitária e fica na parte superior da hierarquia de modelos de objeto das campanhas publicitárias. Esse objeto identifica o tipo de campanha que você está executando (pago, doméstico ou comunidade), o objetivo de campanha, as linhas de entrega da campanha e outros detalhes. Cada campanha pode ser associada a apenas um aplicativo.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar as campanhas publicitárias](manage-ad-campaigns.md).<br/><br/>**Observação**&nbsp;&nbsp;depois de criar uma campanha publicitária, você pode recuperar dados de desempenho para a campanha usando o método [Obter dados de desempenho de campanha publicitária](get-ad-campaign-performance-data.md) na [API de análise da Windows Store](access-analytics-data-using-windows-store-services.md).  |
| Linhas de entrega | Cada campanha tem uma ou mais linhas de entrega que são usadas para comprar inventário e fornecer seus anúncios. Para cada linha de entrega, você pode definir direcionamento, definir o preço da oferta e decidir quanto você deseja gastar definindo um orçamento e vinculando a criativos que você deseja usar.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md). |
| Perfis de direcionamento | Cada linha de entrega tem um perfil de direcionamento que especifica os usuários, regiões geográficas e tipos de inventário que você deseja direcionar. Perfis de direcionamento podem ser criados como um modelo e compartilhados entre as linhas de entrega.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md). |
| Criativos | Cada linha de entrega tem um ou mais criativos que representam os anúncios que são mostrados para os clientes como parte da campanha. Um criativo pode ser associado a uma ou mais linhas de entrega, mesmo em campanhas publicitárias, dado que ele sempre representa o mesmo aplicativo.<br/><br/>Para obter mais informações sobre os métodos relacionados a esse objeto, consulte [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md). |


O diagrama a seguir ilustra a relação entre campanhas, linhas de entrega, perfis de direcionamento, e criativos.

![Hierarquia de campanha publicitária](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Exemplo de código

O exemplo de código a seguir demonstra como obter um token de acesso do Azure AD e chamar a API de promoções da Windows Store de um aplicativo de console C#. Para usar este exemplo de código, atribua as variáveis *tenantId*, *clientId*, *clientSecret* e *appID* aos valores adequados ao seu cenário. Este exemplo exige o [pacote Json.NET](http://www.newtonsoft.com/json) do Newtonsoft para desserializar os dados JSON devolvidos pela API de promoções da Windows Store.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar campanhas publicitárias](manage-ad-campaigns.md)
* [Gerenciar linhas de entrega de campanhas publicitárias](manage-delivery-lines-for-ad-campaigns.md)
* [Gerenciar perfis de direcionamento de campanhas publicitárias](manage-targeting-profiles-for-ad-campaigns.md)
* [Gerenciar criativos de campanhas publicitárias](manage-creatives-for-ad-campaigns.md)
* [Obter dados de desempenho da campanha publicitária](get-ad-campaign-performance-data.md)


 