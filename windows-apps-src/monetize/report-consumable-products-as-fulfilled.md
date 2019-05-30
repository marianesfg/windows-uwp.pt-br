---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Use esse método na API da coleção da Microsoft Store para declarar um produto consumível como providenciado para um determinado cliente. Para que um usuário possa recomprar um produto consumível, seu aplicativo ou serviço deve declarar que o produto consumível já foi providenciado para esse usuário.
title: Declarar produtos consumíveis como providenciados
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API de coleção da Microsoft Store, providenciado, consumível
ms.localizationpriority: medium
ms.openlocfilehash: 994113abc34a0a5f7905bff00aa77c6785409927
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372769"
---
# <a name="report-consumable-products-as-fulfilled"></a>Declarar produtos consumíveis como providenciados

Use esse método na API da coleção da Microsoft Store para declarar um produto consumível como providenciado para um determinado cliente. Para que um usuário possa recomprar um produto consumível, seu aplicativo ou serviço deve declarar que o produto consumível já foi providenciado para esse usuário.

Há duas maneiras de usar esse método para declarar um produto consumível como providenciado:

* Forneça a ID do item do produto consumível (conforme retornado no parâmetro **itemId** de uma [consulta por produtos](query-for-products.md)) e uma ID de rastreamento exclusivo que você forneça. Se a mesma ID de rastreamento for usada em várias tentativas, o mesmo resultado será retornado mesmo se o item já tiver sido consumido. Se você não tiver certeza se uma solicitação de produto consumível foi bem-sucedida, o serviço deve reenviar as solicitações de produto consumível com a mesma ID de rastreamento. A ID de rastreamento sempre será vinculada a essa solicitação de produto consumível e poderá ser reenviada indefinidamente.
* Forneça a ID do produto (como retornado no parâmetro **productId** de uma [consulta por produtos](query-for-products.md)) e uma ID de transação que é obtida de uma das fontes listadas na descrição do parâmetro **transactionId** na seção do corpo da solicitação abaixo.

## <a name="prerequisites"></a>Pré-requisitos


Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave ID da Microsoft Store que representa a identidade do usuário para o qual você quer declarar um produto consumível como providenciado.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                   |
|--------|---------------------------------------------------------------|
| POSTAR   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorização  | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.                           |
| Host           | cadeia de caracteres | Deve ser definido como o valor **collections.mp.microsoft.com**.                                            |
| Content-Length | number | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | cadeia de caracteres | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-body"></a>Corpo da solicitação

| Parâmetro     | Tipo         | Descrição         | Obrigatório |
|---------------|--------------|---------------------|----------|
| beneficiário   | UserIdentity | O usuário para o qual este item está sendo consumido. Para obter mais informações, consulte a tabela a seguir.        | Sim      |
| itemId        | cadeia de caracteres       | O valor *itemID* retornado por uma [consulta por produtos](query-for-products.md). Use esse parâmetro com *trackingId*      | Não       |
| trackingId    | guid         | Uma ID de rastreamento exclusiva fornecida pelo desenvolvedor. Use esse parâmetro com *itemId*.         | Não       |
| productId     | cadeia de caracteres       | O valor de *productId* retornado por uma [consulta por produtos](query-for-products.md). Use esse parâmetro com *transactionId*   | Não       |
| transactionId | guid         | Um valor de ID de transação que é obtido de uma das seguintes fonte. Use esse parâmetro com *productId*.<ul><li>A propriedade [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) da classe [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults).</li><li>O recibo do aplicativo ou produto que é retornado por [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ou [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync).</li><li>O parâmetro *transactionId* retornado por uma [consulta por produtos](query-for-products.md).</li></ul>   | Não       |


O objeto UserIdentity contém os parâmetros a seguir.

| Parâmetro            | Tipo   | Descrição       | Obrigatório |
|----------------------|--------|-------------------|----------|
| identityType         | cadeia de caracteres | Especifique o valor de cadeia de caracteres **b2b**.    | Sim      |
| identityValue        | cadeia de caracteres | Uma [chave ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário para o qual você quer declarar um produto consumível como providenciado.      | Sim      |
| localTicketReference | cadeia de caracteres | O identificador solicitado para resposta retornada. É recomendável que você use o mesmo valor que o *userId*[declaração](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) na chave de ID do Microsoft Store. | Sim      |


### <a name="request-examples"></a>Exemplos de solicitação

O exemplo a seguir usa *itemId* e *trackingId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

O exemplo a seguir usa *productId* e *transactionId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>Resposta

Nenhum conteúdo será retornado se o consumo for executado com êxito.

### <a name="response-example"></a>Exemplo de resposta

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>Códigos de erro


| Código | Erro        | Código de erro interno           | Descrição           |
|------|--------------|----------------------------|-----------------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | PartnerAadTicketRequired   | Um token de acesso do Azure AD não foi passado para o serviço no cabeçalho de autorização.                                                                                                   |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave de ID da Microsoft Store no corpo da solicitação e a declaração *appid* no token de acesso do Azure AD no cabeçalho de autorização não coincidem.                     |

<span/> 

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar direitos de produto de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta de produtos](query-for-products.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Renovar uma chave de ID do Microsoft Store](renew-a-windows-store-id-key.md)
