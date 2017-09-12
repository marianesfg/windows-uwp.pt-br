---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "Se você tiver um catálogo de aplicativos e complementos, poderá usar a API de coleção da Windows Store e a API de compra da Windows Store para acessar informações de propriedade para esses produtos de seus serviços."
title: "Gerenciar direitos a produtos de um serviço"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de coleção da Windows Store, API de compra da Windows Store, exibir produtos, conceder produtos"
ms.openlocfilehash: 6ecc9d6014692cac52f5554f78a0773dfee3fb81
ms.sourcegitcommit: e7e8de39e963b73ba95cb34d8049e35e8d5eca61
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2017
---
# <a name="manage-product-entitlements-from-a-service"></a>Gerenciar direitos a produtos de um serviço

Se você tiver um catálogo de aplicativos e complementos, poderá usar a *API de coleção da Windows Store* e a *API de compra da Windows Store* para acessar informações de direitos para esses produtos de seus serviços. Um *direito* representa um direito do consumidor de usar um aplicativo ou complemento que está publicado na Windows Store.

Essas APIs consistem em métodos REST que são projetados para serem usados por desenvolvedores com catálogos de complementos que são suportados pelos serviços para várias plataformas. Essa API permite:

-   API de coleção da Windows Store: [consulta por aplicativos pertencentes a um usuário](query-for-products.md) e [declarar que um produto de consumo foi providenciado](report-consumable-products-as-fulfilled.md).
-   API de compras da Windows Store: [Conceda um produto gratuito a um usuário](grant-free-products.md), [obtenha assinaturas para um usuário](get-subscriptions-for-a-user.md) e [altere o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> A API de coleção e a API de compra da Windows Store usam a autenticação do Azure Active Directory (Azure AD) para acessar informações de propriedade do cliente. Para usar essas APIs, você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD.

## <a name="overview"></a>Visão geral

As etapas a seguir descrevem o processo de ponta a ponta para usar a API de coleção da Windows Store e a API de compra:

1.  [Configure um aplicativo Web no Azure AD](#step-1).
2.  [Associe sua ID de cliente do Azure AD ao seu aplicativo no painel do Centro de Desenvolvimento do Windows](#step-2).
3.  Em seu serviço, [crie tokens de acesso do Azure AD](#step-3) que representem sua identidade de fornecedor.
4.  No código do lado do cliente em seu aplicativo do Windows, [crie uma chave ID da Windows Store](#step-4) que represente a identidade do usuário atual e passe a chave ID da Windows Store de volta para seu serviço.
5.  Depois de obter o token de acesso do Azure AD e a chave ID da Windows Store, [chame a API de coleção ou a API de compra da Windows Store de seu serviço](#step-5).

As seções a seguir fornecem mais detalhes sobre cada uma dessas etapas.

<span id="step-1"/>
## <a name="step-1-configure-a-web-application-in-azure-ad"></a>Etapa 1: configurar um aplicativo Web no Azure AD

Antes de poder usar a API de coleção da Windows Store ou API de compra, você deve criar um aplicativo Web do Azure AD, recuperar a ID de locatário e a ID de cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de coleção da Windows Store ou API de compra. Você precisa da ID do locatário, ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.

> [!NOTE]
> Você precisa executar somente as tarefas nesta seção uma vez. Depois que você atualizar o manifesto de aplicativo do Azure AD e ter a ID de locatário, a ID de cliente e o segredo de cliente, você poderá reutilizar esses valores sempre que precisar criar um novo token de acesso do Azure AD.

1.  Siga as instruções em [Integrando aplicativos com o Active Directory do Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=722502) para adicionar um aplicativo Web ao Azure AD.
    > [!NOTE]
    > Na página **Conte-nos sobre o seu aplicativo**, certifique-se de escolher **Aplicativo Web e/ou API da Web**. Isso é necessário para que você possa recuperar uma chave (também chamada de *segredo do cliente*) para seu aplicativo. Para chamar a API de coleção ou a API de compra da Windows Store, você deverá fornecer o segredo do cliente quando solicitar um token de acesso do Azure AD em uma etapa posterior.

2.  No [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/), navegue até **Active Directory**. Selecione seu diretório, clique na guia **Aplicativos** na parte superior e selecione seu aplicativo.
3.  Clique na guia **Configurar**. Nessa guia, obtenha a ID do cliente para seu aplicativo e solicite uma chave (chamada de *segredo do cliente* nas etapas posteriores).
4.  Na parte inferior da tela, clique em **Gerenciar manifesto**. Baixe o manifesto do aplicativo do Azure AD e substitua a seção `"identifierUris"` pelo texto a seguir.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Essas cadeias de caracteres representam os públicos permitidos por seu aplicativo. Em uma etapa posterior, você criará tokens de acesso do Azure AD que serão associados a cada um desses valores público. Para obter mais informações sobre como baixar o manifesto de seu aplicativo, consulte [Noções básicas sobre o manifesto de aplicativo do Active Directory do Azure](http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Salve o manifesto do aplicativo e carregue-o em seu aplicativo no [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/).

<span id="step-2"/>
## <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>Etapa 2: associe sua ID de cliente do Azure AD ao seu aplicativo no Centro de Desenvolvimento do Windows

Antes de poder usar a API de coleção ou API de compra da Windows Store para operar em um aplicativo ou complemento, você deve associar sua ID de cliente do Azure AD ao aplicativo (ou o aplicativo que contém o complemento) no painel do Centro de Desenvolvimento.

> [!NOTE]
> Você só precisa executar essa tarefa uma vez.

1.  Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview) e selecione o aplicativo.
2.  Vá até a página **Serviços** &gt; **Coleções e compras de produto** e digite sua ID de cliente do Azure AD em um dos campos disponíveis.

<span id="step-3"/>
## <a name="step-3-create-azure-ad-access-tokens"></a>Etapa 3: criar tokens de acesso do Azure AD

Para poder recuperar uma chave ID da Windows Store ou chamar a API de coleção ou API de compra da Windows Store, seu serviço deve criar diversos tokens de acesso do Azure AD diferentes que representem sua identidade de fornecedor. Cada token será usado com uma API diferente. A vida útil desses tokens é de 60 minutos, e você pode atualizá-los depois que expirarem.

> [!IMPORTANT]
> Crie tokens de acesso do Azure AD somente no contexto de seu serviço, e não em seu aplicativo. O segredo do cliente pode ser comprometido se ele for enviado para seu aplicativo.

<span id="access-tokens" />
### <a name="understanding-the-different-tokens-and-audience-uris"></a>Noções básicas sobre os diferentes tokens e URIs de público

Dependendo dos métodos com os quais você deseja chamar a API de compra ou API de coleção da Windows Store, você deve criar dois ou três tokens diferentes. Cada token de acesso é associado a um URI de público diferente (esses são os mesmos URIs que você adicionou anteriormente na seção `"identifierUris"` do manifesto do aplicativo do Azure AD).

  * Em todos os casos, você deve criar um token com o `https://onestore.microsoft.com` URI de público. Em uma etapa posterior, você irá passar esse token para o cabeçalho de **autorização** dos métodos na API de compra ou API de coleção da Windows Store.
      > [!IMPORTANT]
      > Use o `https://onestore.microsoft.com` público apenas com tokens de acesso que sejam armazenados com segurança dentro de seu serviço. Expor tokens de acesso com esse público fora de seu serviço poderia tornar seu serviço vulnerável a ataques de repetição.

  * Se você deseja chamar um método na API de coleção da Windows Store para [procurar por produtos pertencentes a um usuário](query-for-products.md) ou [declarar um produto consumível como providenciado](report-consumable-products-as-fulfilled.md), você também deve criar um token com o `https://onestore.microsoft.com/b2b/keys/create/collections` URI de público. Em uma etapa posterior, você passará esse token para um método de cliente no SDK do Windows para solicitar uma chave ID da Windows Store que você pode usar com a API de coleção da Windows Store.

  * Se você deseja chamar um método na API de compras da Windows Store para [conceder um produto gratuito a um usuário](grant-free-products.md), [obter assinaturas para um usuário](get-subscriptions-for-a-user.md) ou [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md), também é necessário criar um token com o `https://onestore.microsoft.com/b2b/keys/create/purchase`URI de público. Em uma etapa posterior, você passará esse token para um método de cliente no SDK do Windows para solicitar uma chave ID da Windows Store que você pode usar com a API de compra da Windows Store.

<span />
### <a name="create-the-tokens"></a>Criar os tokens

Para criar os tokens de acesso, use a API OAuth 2.0 em seu serviço, seguindo as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service) para enviar uma solicitação HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

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

* Para os parâmetros *client\_id* e *client\_secret*, especifique a ID e o segredo do cliente para seu aplicativo que você obteve do [Portal de Gerenciamento do Azure](http://manage.windowsazure.com). Esses dois parâmetros são necessários para criar um token de acesso com o nível de autenticação exigido pela API de compra ou API de coleção da Windows Store.

* Para o parâmetro de *recurso*, especifique um dos URIs de audiência listados na [seção anterior](#access-tokens), dependendo do tipo de token de acesso que você está criando.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obter mais detalhes sobre a estrutura de um token de acesso, consulte [Tipos de declaração e token com suporte](http://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>
## <a name="step-4-create-a-windows-store-id-key"></a>Etapa 4: criar uma chave ID da Windows Store

Antes de chamar qualquer método na API de coleção ou a API de compra da Windows Store, o aplicativo deve criar uma chave ID da Windows Store e enviá-la para o serviço. Esta chave é um JSON Web Token (JWT) que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Para obter mais informações sobre as declarações nessa chave, consulte [Declarações em uma chave de ID da Windows Store](#claims).

Atualmente, a única forma de criar uma chave de ID da Windows Store é chamando uma API da Plataforma Universal do Windows (UWP) no código do cliente em seu aplicativo. A chave gerada representa a identidade do usuário que fez logon na Windows Store no dispositivo.

> [!NOTE]
> Cada chave ID da Windows Store é válida por 90 dias. Depois que uma chave expira, você pode [renovar a chave](renew-a-windows-store-id-key.md). Nós recomendamos que você renove as chaves de ID da Windows Store em vez de criar chaves novas.

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>Para criar uma chave de ID da Windows Store para a API de coleção da Windows Store

Siga estas etapas para criar uma chave de ID da Windows Store que você pode usar com a API de coleção da Windows Store para [procurar por produtos pertencentes a um usuário](query-for-products.md) ou [declarar um produto consumível como providenciado](report-consumable-products-as-fulfilled.md).

1.  Passe o token de acesso do Azure AD criado com o `https://onestore.microsoft.com/b2b/keys/create/collections` URI de público de seu serviço para o aplicativo cliente.

2.  Em seu código de aplicativo, chame um desses métodos para recuperar uma chave de ID da Windows Store:

  * Se seu aplicativo usa a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar as compras no aplicativo, use o método [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx).

  * Se seu aplicativo usa a classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) no namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para gerenciar as compras no aplicativo, use o método [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674).

    Passe o token de acesso do Azure AD para o parâmetro *serviceTicket* do método. Opcionalmente, você pode passar uma ID para o parâmetro *publisherUserId* que identifica o usuário atual no contexto de seus serviços. Se você mantiver IDs de usuário para seus serviços, você poderá usar esse parâmetro para correlacionar essas IDs de usuário com as chamadas que fizer da API de coleção da Windows Store.

3.  Depois que seu aplicativo criar com êxito uma chave de ID da Windows Store, repasse a chave para seu serviço.

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>Para criar uma chave de ID da Windows Store para a API de compra da Windows Store

Siga estas etapas para criar uma chave de ID da Windows Store que você pode usar com a API de compras da Windows Store a fim de [conceder um produto gratuito a um usuário](grant-free-products.md), [obter assinaturas para um usuário](get-subscriptions-for-a-user.md) ou [alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Passe o token de acesso do Azure AD criado com o `https://onestore.microsoft.com/b2b/keys/create/purchase` URI de público de seu serviço para o aplicativo cliente.

2.  Em seu código de aplicativo, chame um desses métodos para recuperar uma chave de ID da Windows Store:

  * Se seu aplicativo usa a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar as compras no aplicativo, use o método [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx).

  * Se seu aplicativo usa a classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) no namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para gerenciar as compras no aplicativo, use o método [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675).

    Passe o token de acesso do Azure AD para o parâmetro *serviceTicket* do método. Opcionalmente, você pode passar uma ID para o parâmetro *publisherUserId* que identifica o usuário atual no contexto de seus serviços. Se você mantiver IDs de usuário para seus serviços, você poderá usar esse parâmetro para correlacionar essas IDs de usuário com as chamadas que fizer da API de compra da Windows Store.

3.  Depois que seu aplicativo criar com êxito uma chave de ID da Windows Store, repasse a chave para seu serviço.

<span id="step-5"/>
## <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>Etapa 5: chamar a API de coleção ou a API de compra da Windows Store a partir de seu serviço

Depois que o serviço tiver uma chave ID da Windows Store que o permita acessar informações de propriedade de produto de um usuário específico, seu serviço poderá chamar a API de compra ou a API de coleção da Windows Store seguindo essas instruções:

* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Obter assinaturas para um usuário](get-subscriptions-for-a-user.md)
* [Alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md)

Para cada cenário, passe as seguintes informações para a API:

-   O token de acesso do Azure AD criado anteriormente com o URI de público `https://onestore.microsoft.com`. Esse token representa sua identidade de fornecedor. Passe esse token ao cabeçalho da solicitação.
-   A chave da ID da Windows Store recuperada do código do lado do cliente em seu aplicativo. Essa chave representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar.

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Declarações em uma chave ID da Windows Store

Uma chave ID da Windows Store é um JSON Web Token JWT () que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Quando decodificada usando Base64, uma chave de ID da Windows Store contém as declarações a seguir.

* `iat`:&nbsp;&nbsp;&nbsp;Identifica o momento em que a chave foi emitida. Essa declaração pode ser usada para determinar a idade do token. Esse valor é expresso como época.
* `iss`:&nbsp;&nbsp;&nbsp;Identifica o emissor. Isso tem o mesmo valor que a reivindicação `aud`.
* `aud`:&nbsp;&nbsp;&nbsp;Identifica o público. Deve ser um dos seguintes valores: `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifica o período durante ou após o tempo de expiração em que a chave não será mais aceita para processamento de qualquer coisa, salvo a renovação de chaves. O valor dessa declaração é expresso como época.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifica o momento em que o token será aceito para processamento. O valor dessa declaração é expresso como época.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;A ID do cliente que identifica o desenvolvedor.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Uma carga opaca (criptografada e codificada em Base64) que contém informações que destinam-se apenas para uso pelos serviços da Windows Store.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Uma ID de usuário que identifica o usuário atual no contexto de seus serviços. É o mesmo valor que você transmite para o parâmetro *publisherUserId* opcional do [método usado para criar a chave](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;O URI que você pode usar para renovar a chave.

Veja aqui um exemplo de um cabeçalho de chave ID da Windows Store decodificado.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Aqui está um exemplo de uma declaração de chave ID da Windows Store decodificada.

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

* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Obter assinaturas para um usuário](get-subscriptions-for-a-user.md)
* [Alterar o estado de cobrança de uma assinatura para um usuário](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renovar uma chave ID da Windows Store](renew-a-windows-store-id-key.md)
* [Integrando aplicativos com o Active Directory do Azure](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Noções básicas sobre o manifesto de aplicativo do Active Directory do Azure]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de declaração e token com suporte](http://go.microsoft.com/fwlink/?LinkId=722501)
