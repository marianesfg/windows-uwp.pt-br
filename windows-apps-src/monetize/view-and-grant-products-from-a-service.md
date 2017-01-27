---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "Se você tiver um catálogo de aplicativos e complementos, poderá usar a API de coleção da Windows Store e a API de compra da Windows Store para acessar informações de propriedade para esses produtos de seus serviços."
title: "Exibir e conceder produtos de um serviço"
translationtype: Human Translation
ms.sourcegitcommit: 1a2e856cddf9998eeb8b0132c2fb79f5188c218b
ms.openlocfilehash: d7677c85408af22e882444119dc0231eebe4fa9a

---

# <a name="view-and-grant-products-from-a-service"></a>Exibir e conceder produtos de um serviço

Se você tiver um catálogo de aplicativos e complementos (também conhecidos como produtos no aplicativo ou IAPs), poderá usar a *API de coleção da Windows Store* e a *API de compra da Windows Store* para acessar informações de propriedade para esses produtos de seus serviços.

Essas APIs consistem em métodos REST que são projetados para serem usados por desenvolvedores com catálogos de complementos que são suportados pelos serviços para várias plataformas. Essa API permite:

-   API de coleção da Windows Store: consulta por aplicativos e complementos pertencentes a um determinado usuário ou declarar que um produto de consumo foi providenciado.
-   API de compra da Windows Store: concessão de um aplicativo ou complemento gratuito a um determinado usuário.

## <a name="using-the-windows-store-collection-api-and-windows-store-purchase-api"></a>Usando a API da coleção da Windows Store e a API de compra da Windows Store


A API de coleção e a API de compra da Windows Store usam a autenticação do Active Directory do Azure (Azure AD) para acessar informações de propriedade do cliente. Para poder chamar essas APIs, você deve aplicar alguns metadados do Azure AD ao seu aplicativo no painel do Centro de Desenvolvimento do Windows e gerar vários tokens e chaves de acesso necessários. As etapas a seguir descrevem o processo completo:

1.  [Configure um aplicativo Web no Azure AD](#step-1). Este aplicativo representa seus serviços de plataforma cruzada no contexto do Azure AD.
2.  [Associe seu ID de cliente do Azure AD ao seu aplicativo no painel do Centro de Desenvolvimento do Windows](#step-2).
3.  Em seu serviço, [gere tokens de acesso do Azure AD](#step-3) que representem sua identidade de fornecedor.
4.  No código do lado do cliente em seu aplicativo do Windows, [gere uma chave ID da Windows Store](#step-4) que represente a identidade do usuário atual e passe a chave ID da Windows Store de volta para seu serviço.
5.  Depois de obter o token de acesso do Azure AD e a chave ID da Windows Store, [chame a API de coleção ou a API de compra da Windows Store de seu serviço](#step-5).

As seções a seguir fornecem mais detalhes sobre cada uma dessas etapas.

<span id="step-1"/>
### <a name="step-1-configure-a-web-application-in-azure-ad"></a>Etapa 1: configure um aplicativo Web no Azure AD

1.  Siga as instruções em [Integrando aplicativos com o Active Directory do Azure](http://go.microsoft.com/fwlink/?LinkId=722502) para adicionar um aplicativo Web ao Azure AD.

    > **Observação**&nbsp;&nbsp;Na página **Conte-nos sobre o seu aplicativo**, certifique-se de escolher **Aplicativo Web e/ou API da Web**. Isso é necessário para que você possa obter uma chave (também chamada de *segredo do cliente*) para seu aplicativo. Para chamar a API de coleção ou a API de compra da Windows Store, você deverá fornecer o segredo do cliente quando solicitar um token de acesso do Azure AD em uma etapa posterior.

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

    Essas cadeias de caracteres representam os públicos permitidos por seu aplicativo. Em uma etapa posterior, você criará tokens de acesso do Azure AD que serão associados a cada um desses valores público. Para obter mais informações sobre como baixar o manifesto de seu aplicativo, consulte [Noções básicas sobre o manifesto de aplicativo do Active Directory do Azure]( http://go.microsoft.com/fwlink/?LinkId=722500).

5.  Salve o manifesto do aplicativo e carregue-o em seu aplicativo no [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/).

<span id="step-2"/>
### <a name="step-2-associate-your-azure-ad-client-id-with-your-application-in-the-windows-dev-center-dashboard"></a>Etapa 2: associe sua ID de cliente do Azure AD ao seu aplicativo no painel do Centro de Desenvolvimento do Windows

A API de coleção e a API de compra da Windows Store fornecem acesso somente a informações de propriedade de um usuário para aplicativos e complementos que você tenha associado à sua ID de cliente do Azure AD.

1.  Entre no [painel do Centro de Desenvolvimento do Windows](https://dev.windows.com/overview) e selecione seu aplicativo.
2.  Vá até a página **Serviços** &gt; **Product collections and purchases** e digite sua ID de cliente do Azure AD em um dos campos disponíveis.

<span id="step-3"/>
### <a name="step-3-retrieve-access-tokens-from-azure-ad"></a>Etapa 3: recupere os tokens de acesso do Azure AD

Para poder recuperar uma chave ID da Windows Store ou chamar a API de coleção ou API de compra da Windows Store, seu serviço deve solicitar três tokens de acesso do Azure AD que representem sua identidade de fornecedor. Cada um desses tokens de acesso é associado a um URI de público diferente, e cada token será usado com uma chamada de API diferente. A vida útil desses tokens é de 60 minutos, e você pode atualizá-la depois que expirarem.

Para obter os tokens de acesso, use a API OAuth 2.0 em seu serviço, seguindo as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar uma solicitação HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Para cada token, especifique os seguintes dados de parâmetro:

* Para os parâmetros *client\_id* e *client\_secret*, especifique a ID e o segredo do cliente para seu aplicativo como obtidos do [Portal de Gerenciamento do Azure](http://manage.windowsazure.com/). Esses dois parâmetros são necessários para gerar um token de acesso com o nível de autenticação exigido pela API de compra ou API de coleção da Windows Store.

* Para o parâmetro *resource*, especifique um dos URIs de ID de aplicativo a seguir (esses são os mesmos URIs que você adicionou anteriormente à seção `"identifierUris"` do manifesto do aplicativo). No final deste processo, você deverá ter três tokens de acesso associados a cada um desses URIs de ID do aplicativo:
  * `https://onestore.microsoft.com/b2b/keys/create/collections`: em uma etapa posterior, você usará o token de acesso que criar com esse URI para solicitar uma chave ID da Windows Store que você pode usar com a API de coleção da Windows Store.
  * `https://onestore.microsoft.com/b2b/keys/create/purchase`: em uma etapa posterior, você usará o token de acesso que criar com esse URI para solicitar uma chave ID da Windows Store que você pode usar com a API de compra da Windows Store.
  * `https://onestore.microsoft.com`: em uma etapa posterior, você usará o token de acesso que criar com esse URI em chamadas diretas para a API de compra ou a API de coleção da Windows Store.

  > **Importante**&nbsp;&nbsp;Use o público de `https://onestore.microsoft.com` apenas com tokens de acesso que sejam armazenados com segurança dentro de seu serviço. Expor tokens de acesso com esse público fora de seu serviço poderia tornar seu serviço vulnerável a ataques de repetição.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens). Para obter mais detalhes sobre a estrutura de um token de acesso, consulte [Tipos de declaração e token com suporte](http://go.microsoft.com/fwlink/?LinkId=722501).

> **Importante**&nbsp;&nbsp;Você deve criar tokens de acesso do Azure AD somente no contexto de seu serviço, e não em seu aplicativo. O segredo do cliente pode ser comprometido se ele for enviado para seu aplicativo.

<span id="step-4"/>
### <a name="step-4-generate-a-windows-store-id-key-from-client-side-code-in-your-app"></a>Etapa 4: Gerar uma chave ID da Windows Store de um código do lado do cliente em seu aplicativo

Antes de chamar a API de coleção ou a API de compra da Windows Store, seu serviço deve obter uma chave ID da Windows Store. Este é um JSON Web Token (JWT) que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Para obter mais informações sobre as declarações nessa chave, consulte [Declarações em uma chave ID da Windows Store](#claims).

Atualmente, a única maneira de obter uma chave ID da Windows Store é chamando uma API da Plataforma Universal do Windows (UWP) do código do lado do cliente em seu aplicativo para recuperar a identidade do usuário que está conectado no momento com o Windows Store. Para gerar uma chave ID da Windows Store:

1.  Passe um dos seguintes tokens de acesso do seu serviço para seu aplicativo cliente:

  * Para obter uma chave ID da Windows Store que você possa usar com a API de coleção da Windows Store, passe o token de acesso do Azure AD criado com o `https://onestore.microsoft.com/b2b/keys/create/collections` URI público.

  * Para obter uma chave ID da Windows Store que você possa usar com a API de compra da Windows Store, passe o token de acesso do Azure AD criado com o `https://onestore.microsoft.com/b2b/keys/create/purchase` URI público.

2.  No código do aplicativo, chame um dos seguintes métodos para recuperar uma chave ID da Windows Store:

  * Se seu aplicativo usa a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para gerenciar as compras no aplicativo, use o método [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) se você pretender usar a API de coleção da Windows Store, ou use o método [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) se você pretender usar a API de compra da Windows Store.

  * Se seu aplicativo usa a classe [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) no namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para gerenciar as compras no aplicativo, use o método [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) se você pretender usar a API de coleção da Windows Store, ou use o método [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) se você pretender usar a API de compra da Windows Store.

    Para cada método, passe o token de acesso do Azure AD para o parâmetro *serviceTicket*. Opcionalmente, você pode passar uma ID para o parâmetro *publisherUserId* que identifica o usuário atual no contexto de seus serviços. Se você mantiver IDs de usuário para seus serviços, você poderá usar esse parâmetro para correlacionar essas IDs de usuário com as chamadas que fizer da API de coleção ou da API de compra da Windows Store.

    >**Observação**&nbsp;&nbsp;Para obter mais informações sobre as diferenças entre os namespaces **Windows.Services.Store** e **Windows.ApplicationModel.Store**, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

3.  Depois que seu aplicativo recuperar com êxito uma chave ID da Windows Store, repasse a chave para seu serviço.

> **Observação**&nbsp;&nbsp;Cada chave de ID da Windows Store é válida por 90 dias. Depois que uma chave expira, você pode [renovar a chave](renew-a-windows-store-id-key.md). Nós recomendamos que você renove as chaves ID da Windows Store em vez de criar novas.

<span id="step-5"/>
### <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>Etapa 5: Chamar a API de coleção ou a API de compra da Windows Store de seu serviço

Depois que o serviço tiver uma chave ID da Windows Store que o permita acessar informações de propriedade de produto de um usuário específico, seu serviço poderá chamar a API de compra ou a API de coleção da Windows Store. Siga as instruções que se aplicam ao seu cenário:

-   [Consulta por produtos](query-for-products.md)
-   [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
-   [Conceder produtos gratuitos](grant-free-products.md)

Para cada cenário, passe as seguintes informações para a API:

-   O token de acesso do Azure AD criado anteriormente com o URI de público `https://onestore.microsoft.com`. Esse token representa sua identidade de fornecedor. Passe esse token ao cabeçalho da solicitação.
-   A chave da ID da Windows Store recuperada do código do lado do cliente em seu aplicativo. Essa chave representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar.

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Declarações em uma chave ID da Windows Store

Uma chave ID da Windows Store é um JSON Web Token JWT () que representa a identidade do usuário cujas informações de propriedade de produto você deseja acessar. Quando decodificada usando Base64, uma chave ID da Windows Store contém as declarações a seguir.

<table>
<colgroup>
<col width="30%" />
<col width="70%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Nome da declaração</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">iat</td>
<td align="left">Identifica o momento em que a chave foi emitida. Essa declaração pode ser usada para determinar a idade do token. Esse valor é expresso como época.</td>
</tr>
<tr>
<td align="left">iss</td>
<td align="left">Identifica o emissor. Tem o mesmo valor que a declaração *aud*.</td>
</tr>
<tr>
<td align="left">aud</td>
<td align="left">Identifica o público. Deve se um dos seguintes valores: `https://collections.mp.microsoft.com/v6.0/keys` ou `https://purchase.mp.microsoft.com/v6.0/keys`.</td>
</tr>
<tr>
<td align="left">exp</td>
<td align="left">Identifica o período durante ou após o tempo de expiração em que a chave não será mais aceita para processamento de qualquer coisa, salvo a renovação de chaves. O valor dessa declaração é expresso como época.</td>
</tr>
<tr>
<td align="left">nbf</td>
<td align="left">Identifica o momento em que o token será aceito para processamento. O valor dessa declaração é expresso como época.</td>
</tr>
<tr>
<td align="left">`http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`</td>
<td align="left">A ID do cliente que identifica o desenvolvedor.</td>
</tr>
<tr>
<td align="left">`http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`</td>
<td align="left">Uma carga opaca (criptografada e codificada em Base64) que contém informações que destinam-se apenas para uso pelos serviços da Windows Store. </td>
</tr>
<tr>
<td align="left">`http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`</td>
<td align="left">Uma ID de usuário que identifica o usuário atual no contexto de seus serviços. É o mesmo valor que você transmite para o parâmetro *publisherUserId* opcional do [método usado para gerar a chave](view-and-grant-products-from-a-service.md#step-4).</td>
</tr>
<tr>
<td align="left">`http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`</td>
<td align="left">O URI que você pode usar para renovar a chave.</td>
</tr>
</tbody>
</table>

Aqui está um exemplo de um cabeçalho de chave ID da Windows Store decodificado.

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
* [Renovar uma chave ID da Windows Store](renew-a-windows-store-id-key.md)
* [Integrando aplicativos com o Active Directory do Azure](http://go.microsoft.com/fwlink/?LinkId=722502)
* [Noções básicas sobre o manifesto de aplicativo do Active Directory do Azure]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [Tipos de declaração e token com suporte](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 



<!--HONumber=Dec16_HO4-->


