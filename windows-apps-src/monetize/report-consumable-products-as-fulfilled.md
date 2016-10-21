---
author: mcleanbyron
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: "Use esse método na API da coleção da Windows Store para declarar um produto consumível como providenciado para um determinado cliente. Para que um usuário possa recomprar um produto consumível, seu aplicativo ou serviço deve declarar que o produto consumível já foi providenciado para esse usuário."
title: "Declarar produtos consumíveis como providenciados"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: dd3e687d49e538187c123b7123c184f9182905de

---

# Declarar produtos consumíveis como providenciados




Use esse método na API da coleção da Windows Store para declarar um produto consumível como providenciado para um determinado cliente. Para que um usuário possa recomprar um produto consumível, seu aplicativo ou serviço deve declarar que o produto consumível já foi providenciado para esse usuário.

Há duas maneiras de usar esse método para declarar um produto consumível como providenciado:

-   Forneça a ID do item do produto consumível (conforme retornado no parâmetro **itemId** de uma [consulta por produtos](query-for-products.md)) e uma ID de rastreamento exclusivo que você forneça. Se a mesma ID de rastreamento for usada em várias tentativas, o mesmo resultado será retornado mesmo se o item já tiver sido consumido. Se você não tiver certeza se uma solicitação de produto consumível foi bem-sucedida, o serviço deve reenviar as solicitações de produto consumível com a mesma ID de rastreamento. A ID de rastreamento sempre será vinculada a essa solicitação de produto consumível e poderá ser reenviada indefinidamente.
-   Forneça a ID do produto (como retornado no parâmetro **productId** de uma [consulta por produtos](query-for-products.md)) e uma ID de transação que é obtida de uma das fontes listadas na descrição do parâmetro **transactionId** na seção do corpo da solicitação abaixo.

## Pré-requisitos


Para usar esse método, você precisará:

-   Ter um token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com`.
-   Uma chave ID da Windows Store que tenha sido gerada chamando-se o método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) no código do lado do cliente em seu aplicativo.

Para saber mais, consulte [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md).

## Solicitação


### Sintaxe da solicitação

| Método | URI da solicitação                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |

<span/> 

### Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorização  | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;.                           |
| Host           | string | Deve ser definido como o valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |

<span/>

### Corpo da solicitação

| Parâmetro     | Tipo         | Descrição         | Obrigatório |
|---------------|--------------|---------------------|----------|
| beneficiário   | UserIdentity | O usuário para o qual este item está sendo consumido.                                                                                                                                                                                                                                                                 | Sim      |
| itemId        | Cadeia de caracteres       | O valor itemID retornado por uma [consulta por produtos](query-for-products.md). Use esse parâmetro com trackingId                                                                                                                                                                                                  | Não       |
| trackingId    | Guid         | Uma ID de rastreamento exclusiva fornecida pelo desenvolvedor. Use esse parâmetro com itemId.                                                                                                                                                                                                                                     | Não       |
| productId     | Cadeia de caracteres       | O valor de productId retornado por uma [consulta por produtos](query-for-products.md). Use esse parâmetro com transactionId                                                                                                                                                                                            | Não       |
| transactionId | Guid         | Um valor de ID de transação que é obtido de uma das seguintes fonte. Use esse parâmetro com productId.  <br/><br/><ul><li>A propriedade [TransactionID](https://msdn.microsoft.com/library/windows/apps/dn263396) da classe [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392).</li><li>O recibo do aplicativo ou produto que é retornado por [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), [RequestAppPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/hh967813) ou [GetAppReceiptAsync](https://msdn.microsoft.com/library/windows/apps/hh967811).</li><li>O parâmetro transactionId retornado por uma [consulta por produtos](query-for-products.md).</li></ul>                                                                                                                                                                                                                                   | Não       |

 
<span/>

O objeto UserIdentity contém os parâmetros a seguir.

| Parâmetro            | Tipo   | Descrição                                                                                                                                 | Obrigatório |
|----------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | string | Especifique o valor de cadeia de caracteres **b2b**.                                                                                                           | Sim      |
| identityValue        | string | Valor cadeia de caracteres da chave ID da Windows Store ID.                                                                                                   | Sim      |
| localTicketReference | string | Identificador solicitado para resposta retornada. Recomendamos que você use o mesmo valor que a declaração *userId* na chave ID da Windows Store. | Sim      |

<span/> 

### Exemplos de solicitação

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

## Resposta


Nenhum conteúdo será retornado se o consumo for executado com êxito.

### Exemplo de resposta

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## Códigos de erro


| Código | Erro        | Código de erro interno           | Descrição                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | PartnerAadTicketRequired   | Um token de acesso do Azure AD não foi passado para o serviço no cabeçalho de autorização.                                                                                                   |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave de ID da Windows Store no corpo da solicitação e a declaração *appid* no token de acesso do Azure AD no cabeçalho de autorização não coincidem.                     |

<span/> 

## Tópicos relacionados

* [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta por produtos](query-for-products.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Renovar uma chave ID da Windows Store](renew-a-windows-store-id-key.md)
 

 



<!--HONumber=Aug16_HO3-->


