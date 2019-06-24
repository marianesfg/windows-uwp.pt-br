---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Se você tiver um catálogo de aplicativos e complementos, poderá usar a API de coleção da Microsoft Store e a API de compra da Microsoft Store para acessar informações de propriedade para esses produtos de seus serviços.
title: Gerenciar direitos a produtos de um serviço
ms.date: 08/01/2018
ms.topic: article
keywords: windows 10, uwp, API de coleção da Microsoft Store, API de compra da Microsoft Store, exibir produtos, conceder produtos
ms.localizationpriority: medium
ms.openlocfilehash: 184937133b85ae2cac7a21bb6002af70b06d34da
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319922"
---
# <a name="manage-product-entitlements-from-a-service"></a>Gerenciar direitos a produtos de um serviço

Se você tiver um catálogo de aplicativos e complementos, poderá usar a *API de coleção da Microsoft Store* e a *API de compra da Microsoft Store* para acessar informações de direitos para esses produtos de seus serviços. Um *direito* representa um direito do consumidor de usar um aplicativo ou complemento que está publicado na Microsoft Store.

Essas APIs consistem em métodos REST que são projetados para serem usados por desenvolvedores com catálogos de complementos que são suportados pelos serviços para várias plataformas. Essa API permite:

-   API da coleta do Microsoft Store: [Consulta de produtos que pertence a um usuário](query-for-products.md) e [relatar um produto de consumo, como atendida](report-consumable-products-as-fulfilled.md).
-   API de compra da Microsoft Store: [Conceder a um produto gratuito a um usuário](grant-free-products.md), [obter assinaturas para um usuário](get-subscriptions-for-a-user.md), e [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> A API de coleção e a API de compra da Microsoft Store usam a autenticação do Azure Active Directory (Azure AD) para acessar informações de propriedade do cliente. Para usar essas APIs, você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](https://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD.

## <a name="overview"></a>Visão geral

As etapas a seguir descrevem o processo de ponta a ponta para usar a API de coleção da Microsoft Store e a API de compra:

1.  [Configurar um aplicativo no Azure AD](#step-1).
2.  [Associar ID de aplicativo do Azure AD com o seu aplicativo no Partner Center](#step-2).
3.  Em seu serviço, [crie tokens de acesso do Azure AD](#step-3) que representem sua identidade de fornecedor.
4.  Em seu aplicativo do cliente Windows, [criar uma chave de ID do Microsoft Store](#step-4) que representa a identidade do usuário atual e passe essa chave de volta para seu serviço.
5.  Depois de obter o token de acesso do Azure AD e a chave de ID da Microsoft Store, [chame a API de coleção ou a API de compra da Microsoft Store de seu serviço](#step-5).

Esse processo de ponta a ponta envolve dois componentes de software que executam tarefas diferentes:

* **Seu serviço**. Este é um aplicativo que é executado com segurança no contexto de seu ambiente de negócios, e ele pode ser implementado usando qualquer plataforma de desenvolvimento que você escolher. O serviço é responsável por criar os tokens de acesso do AD do Azure necessários para o cenário de e para chamar os URIs de REST para a coleção da Microsoft Store API e a API de compra.
* **Seu aplicativo do cliente Windows**. Este é o aplicativo para o qual você deseja acessar e gerenciar informações de qualificação de cliente (incluindo complementos para o aplicativo). Este aplicativo é responsável por criar as chaves de ID do Microsoft Store que você precisa chamar a API de coleção do Microsoft Store e comprar a API do seu serviço.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Etapa 1: Configurar um aplicativo no Azure AD

Antes de poder usar a API de coleção do Microsoft Store ou comprar a API, criar um aplicativo Web do Azure AD, recuperar a ID de locatário e ID do aplicativo para o aplicativo e gerar uma chave. O aplicativo Web do Azure AD representa o serviço do qual você deseja chamar a API de coleção do Microsoft Store ou comprar a API. É necessário o ID do locatário, ID do aplicativo e a chave para gerar tokens de acesso do AD do Azure que você precisa chamar a API.

> [!NOTE]
> Você precisa executar somente as tarefas nesta seção uma vez. Depois que você atualizar o manifesto do aplicativo do Azure AD e você tiver sua ID de locatário, o segredo de cliente e a ID do aplicativo, você pode reutilizar esses valores a qualquer momento que você precisa criar um novo token de acesso do AD do Azure.

1.  Se você ainda não fez isso, siga as instruções em [integrando aplicativos com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) para registrar um **aplicativo Web / API** aplicativo com o Azure AD.
    > [!NOTE]
    > Quando você registra seu aplicativo, você deve escolher **aplicativo Web / API** como tipo de aplicativo para que você possa recuperar uma chave (também chamado de um *segredo do cliente*) para seu aplicativo. Para chamar a API de coleção ou a API de compra da Microsoft Store, você deverá fornecer o segredo do cliente quando solicitar um token de acesso do Azure AD em uma etapa posterior.

2.  No [Portal de gerenciamento](https://portal.azure.com/), navegue até **Azure Active Directory**. Selecione seu diretório, clique em **registros de aplicativo** no painel de navegação à esquerda e, em seguida, selecione seu aplicativo.
3.  Você será levado à página de registro principal do aplicativo. Nessa página, copie o **ID do aplicativo** valor para uso posterior.
4.  Criar uma chave que você precisará mais tarde (Isso é chamado um *segredo do cliente*). No painel esquerdo, clique em **as configurações** e, em seguida **chaves**. Nessa página, conclua as etapas a serem [criar uma chave](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copie essa chave para uso posterior.
5.  Adicionar vários URIs de audiência necessários para seu [manifesto do aplicativo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). No painel esquerdo, clique em **manifesto**. Clique em **edite**, substitua o `"identifierUris"` seção com o seguinte texto e, em seguida, clique em **salvar**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Essas cadeias de caracteres representam os públicos permitidos por seu aplicativo. Em uma etapa posterior, você criará tokens de acesso do Azure AD que serão associados a cada um desses valores público.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Etapa 2: Associar ID de aplicativo do Azure AD com o aplicativo cliente no Partner Center

Antes de poder usar a API de coleção do Microsoft Store ou API para configurar a propriedade e compras para seu aplicativo ou o complemento de compra, você deve associar sua ID de aplicativo do Azure AD com o aplicativo (ou o aplicativo que contém o complemento) no Partner Center.

> [!NOTE]
> Você só precisa executar essa tarefa uma vez.

1.  Entrar no [Partner Center](https://partner.microsoft.com/dashboard) e selecione seu aplicativo.
2.  Vá para o **Services** &gt; **coleções de produto e compras** página e insira sua ID de aplicativo do Azure AD em um dos disponíveis **ID do cliente** campos.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Etapa 3: Criar tokens de acesso do AD do Azure

Para poder recuperar uma chave de ID da Microsoft Store ou chamar a API de coleção ou API de compra da Microsoft Store, seu serviço deve criar diversos tokens de acesso do Azure AD diferentes que representem sua identidade de fornecedor. Cada token será usado com uma API diferente. A vida útil desses tokens é de 60 minutos, e você pode atualizá-la depois que expirarem.

> [!IMPORTANT]
> Crie tokens de acesso do Azure AD somente no contexto de seu serviço, e não em seu aplicativo. O segredo do cliente pode ser comprometido se ele for enviado para seu aplicativo.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Noções básicas sobre os diferentes tokens e URIs de audiência

Dependendo dos métodos com os quais você deseja chamar a API de compra ou API de coleção da Microsoft Store, você deve criar dois ou três tokens diferentes. Cada token de acesso é associado a um URI de público diferente (esses são os mesmos URIs que você adicionou anteriormente na seção `"identifierUris"` do manifesto do aplicativo do Azure AD).

  * Em todos os casos, você deve criar um token com o `https://onestore.microsoft.com` URI de público. Em uma etapa posterior, você irá passar esse token para o cabeçalho **Authorization** dos métodos na API de compra ou API de coleção da Microsoft Store.
      > [!IMPORTANT]
      > Use o `https://onestore.microsoft.com` público apenas com tokens de acesso que sejam armazenados com segurança dentro de seu serviço. Expor tokens de acesso com esse público fora de seu serviço poderia tornar seu serviço vulnerável a ataques de repetição.

  * Se você deseja chamar um método na API de coleção da Microsoft Store para [consultar produtos pertencentes a um usuário](query-for-products.md) ou [declarar um produto consumível como providenciado](report-consumable-products-as-fulfilled.md), você também deve criar um token com o URI de audiência `https://onestore.microsoft.com/b2b/keys/create/collections`. Em uma etapa posterior, você passará esse token para um método de cliente no SDK do Windows para solicitar uma chave de ID da Microsoft Store que você pode usar com a API de coleção da Microsoft Store.

  * Se você deseja chamar um método na API de compras da Microsoft Store para [conceder um produto gratuito a um usuário](grant-free-products.md), [obter assinaturas para um usuário](get-subscriptions-for-a-user.md) ou [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md), também é necessário criar um token com o URI de audiência `https://onestore.microsoft.com/b2b/keys/create/purchase`. Em uma etapa posterior, você passará esse token para um método de cliente no SDK do Windows para solicitar uma chave de ID da Microsoft Store que você pode usar com a API de compra da Microsoft Store.

<span />

### <a name="create-the-tokens"></a>Criar os tokens

Para criar os tokens de acesso, use a API OAuth 2.0 em seu serviço, seguindo as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar uma solicitação HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Para cada token, especifique os seguintes dados de parâmetro:

* Para o *client\_id* e *cliente\_segredo* parâmetros, especifique a ID do aplicativo e o segredo do cliente para o aplicativo que você recuperou do [Portal de gerenciamento do azure](https://portal.azure.com/). Esses dois parâmetros são necessários para criar um token de acesso com o nível de autenticação exigido pela API de compra ou API de coleção da Microsoft Store.

* Para o parâmetro de *recurso*, especifique um dos URIs de audiência listados na [seção anterior](#access-tokens), dependendo do tipo de token de acesso que você está criando.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obter mais detalhes sobre a estrutura de um token de acesso, consulte [Tipos de declaração e token com suporte](https://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Etapa 4: Criar uma chave de ID do Microsoft Store

Antes de chamar qualquer método na API de coleção ou a API de compra da Microsoft Store, o aplicativo deve criar uma chave de ID da Microsoft Store e enviá-la para o serviço. Esta chave é um JSON Web Token (JWT) que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Para obter mais informações sobre as declarações nessa chave, consulte [Declarações em uma chave de ID da Microsoft Store](#claims-in-a-microsoft-store-id-key).

Atualmente, a única forma de criar uma chave de ID da Microsoft Store é chamando uma API da Plataforma Universal do Windows (UWP) no código do cliente em seu app. A chave gerada representa a identidade do usuário que fez logon na Microsoft Store no dispositivo.

> [!NOTE]
> Cada chave de ID da Microsoft Store é válida por 90 dias. Depois que uma chave expira, você pode [renovar a chave](renew-a-windows-store-id-key.md). Nós recomendamos que você renove as chaves de ID da Microsoft Store em vez de criar chaves novas.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>Para criar uma chave de ID da Microsoft Store para a API de coleção da Microsoft Store

Siga estas etapas para criar uma chave de ID da Microsoft Store que você pode usar com a API de coleção da Microsoft Store para [procurar por produtos pertencentes a um usuário](query-for-products.md) ou [declarar um produto consumível como providenciado](report-consumable-products-as-fulfilled.md).

1.  Passe o token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com/b2b/keys/create/collections` de seu serviço para o app cliente. Esse é um dos tokens que você criou [anteriormente na etapa 3](#step-3).

2.  No código do seu app, chame um desses métodos para recuperar uma chave de ID da Microsoft Store:

  * Se seu aplicativo usa a classe [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para gerenciar as compras no aplicativo, use o método [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync).

  * Se seu aplicativo usa a classe [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) no namespace [ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para gerenciar as compras no aplicativo, use o método [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync).

    Passe o token de acesso do Azure AD para o parâmetro *serviceTicket* do método. Se você mantiver as IDs de usuário anônimo no contexto de serviços que gerenciam como o Editor do aplicativo atual, você também pode passar uma ID de usuário para o *publisherUserId* parâmetro para associar o usuário atual com a nova ID da Microsoft Store chave (a ID de usuário será inserida na chave). Caso contrário, se você não precisa associar uma ID de usuário com a chave de ID do Microsoft Store, você pode passar qualquer valor de cadeia de caracteres para o *publisherUserId* parâmetro.

3.  Depois que seu app criar com êxito uma chave de ID da Microsoft Store, repasse a chave para seu serviço.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>Para criar uma chave de ID da Microsoft Store para a API de compra da Microsoft Store

Siga estas etapas para criar uma chave de ID da Microsoft Store que você pode usar com a API de compras da Microsoft Store a fim de [conceder um produto gratuito a um usuário](grant-free-products.md), [obter assinaturas para um usuário](get-subscriptions-for-a-user.md) ou [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Passe o token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com/b2b/keys/create/purchase` de seu serviço para o app cliente. Esse é um dos tokens que você criou [anteriormente na etapa 3](#step-3).

2.  No código do seu app, chame um desses métodos para recuperar uma chave de ID da Microsoft Store:

  * Se seu aplicativo usa a classe [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) no namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para gerenciar as compras no aplicativo, use o método [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Se seu aplicativo usa a classe [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) no namespace [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) para gerenciar as compras no aplicativo, use o método [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Passe o token de acesso do Azure AD para o parâmetro *serviceTicket* do método. Se você mantiver as IDs de usuário anônimo no contexto de serviços que gerenciam como o Editor do aplicativo atual, você também pode passar uma ID de usuário para o *publisherUserId* parâmetro para associar o usuário atual com a nova ID da Microsoft Store chave (a ID de usuário será inserida na chave). Caso contrário, se você não precisa associar uma ID de usuário com a chave de ID do Microsoft Store, você pode passar qualquer valor de cadeia de caracteres para o *publisherUserId* parâmetro.

3.  Depois que seu app criar com êxito uma chave de ID da Microsoft Store, repasse a chave para seu serviço.

### <a name="diagram"></a>Diagrama

O diagrama a seguir ilustra o processo de criação de uma chave de ID do Microsoft Store.

  ![Criar chave de ID do Windows Store](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Etapa 5: Chamar a API de coleção do Microsoft Store ou comprar a API do seu serviço

Depois que o serviço tiver uma chave de ID da Microsoft Store que o permita acessar informações de propriedade de produto de um usuário específico, seu serviço poderá chamar a API de compra ou a API de coleção da Microsoft Store seguindo estas instruções:

* [Consulta de produtos](query-for-products.md)
* [Produtos de consumo de relatório como atendida](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Obter assinaturas para um usuário](get-subscriptions-for-a-user.md)
* [Alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md)

Para cada cenário, passe as seguintes informações para a API:

-   No cabeçalho da solicitação, passe o token de acesso do Azure AD com o valor do URI de audiência `https://onestore.microsoft.com`. Esse é um dos tokens que você criou [anteriormente na etapa 3](#step-3). Esse token representa sua identidade de fornecedor.
-   No corpo da solicitação, passe a chave de ID da Microsoft Store recuperada [anteriormente na etapa 4](#step-4) do código do lado do cliente em seu app. Essa chave representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar.

### <a name="diagram"></a>Diagrama

O diagrama a seguir descreve o processo de chamar um método na coleção Microsoft Store API API ou a compra de seu serviço.

  ![Chamar coleções ou compra API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Declarações em uma chave de ID da Microsoft Store

Uma chave de ID da Microsoft Store é um Token Web JSON (JWT) que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Quando decodificada usando Base64, uma chave de ID da Microsoft Store contém as declarações a seguir.

* `iat`:&nbsp;&nbsp;&nbsp;Identifica a hora em que a chave foi emitida. Essa declaração pode ser usada para determinar a idade do token. Esse valor é expresso como época.
* `iss`:&nbsp;&nbsp;&nbsp;Identifica o emissor. Isso tem o mesmo valor que a reivindicação `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifica o público-alvo. Deve se um dos seguintes valores: `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifica o tempo de expiração em ou depois que a chave será não será aceito para processamento de qualquer coisa, exceto para a renovação de chaves. O valor dessa declaração é expresso como época.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifica a hora em que o token será aceito para processamento. O valor dessa declaração é expresso como época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;A ID do cliente que identifica o desenvolvedor.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Uma carga opaca (criptografado e codificada em Base64) que contém informações que são destinadas apenas para uso pelos serviços da Microsoft Store.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Uma ID de usuário que identifica o usuário atual no contexto dos seus serviços. É o mesmo valor que você transmite para o parâmetro *publisherUserId* opcional do [método usado para criar a chave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;O URI que você pode usar para renovar a chave.

Veja aqui um exemplo de um cabeçalho de chave de ID da Microsoft Store decodificado.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Aqui está um exemplo de uma declaração de chave de ID da Microsoft Store decodificada definida.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Consulta de produtos](query-for-products.md)
* [Produtos de consumo de relatório como atendida](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Obter assinaturas para um usuário](get-subscriptions-for-a-user.md)
* [Alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renovar uma chave de ID do Microsoft Store](renew-a-windows-store-id-key.md)
* [Integrando aplicativos com o Active Directory do Azure](https://go.microsoft.com/fwlink/?LinkId=722502)
* [Noções básicas sobre o manifesto do aplicativo do Azure Active Directory]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de declaração e Token com suporte](https://go.microsoft.com/fwlink/?LinkId=722501)
