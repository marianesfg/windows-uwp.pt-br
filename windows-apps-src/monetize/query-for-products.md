---
author: mcleanbyron
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: "Use esse método na API da coleção da Windows Store para obter todos os produtos que pertence a um cliente em relação a apps que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros."
title: Consulta por produtos
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de coleção da Windows Store, exibir produtos"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 29db10862533e7b15c7a676fc3aecd4ba58f9514
ms.lasthandoff: 02/07/2017

---

# <a name="query-for-products"></a>Consulta por produtos


Use esse método na API da coleção da Windows Store para obter todos os produtos que pertence a um cliente em relação a apps que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros.

Esse método é projetado para ser chamado pelo seu serviço em resposta a uma mensagem de seu app. Seu serviço não deve sondar regularmente todos os usuários em um agendamento.

## <a name="prerequisites"></a>Pré-requisitos


Para usar esse método, você precisará:

* Ter um token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com`.
* Uma chave ID da Windows Store que representa a identidade do usuário cujos produtos você deseja obter.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da Solicitação                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |

<span/>
 
### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorização  | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;.                           |
| Host           | string | Deve ser definido como o valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |

<span/>

### <a name="request-body"></a>Corpo da solicitação

| Parâmetro         | Tipo         | Descrição         | Obrigatório |
|-------------------|--------------|---------------------|----------|
| beneficiários     | UserIdentity | Um objeto UserIdentity que representa o usuário está sendo consultado por produtos. Para obter mais informações, consulte a tabela abaixo.    | Sim      |
| continuationToken | cadeia de caracteres       | Se houver vários conjuntos de produtos, o corpo da resposta retornará um token de continuação quando o limite de página for atingido. Forneça esse token de continuação em chamadas subsequentes para recuperar produtos restantes.       | Não       |
| maxPageSize       | número       | O número máximo de produtos para retornar uma resposta. O valor máximo e padrão é 100.                 | Não       |
| modifiedAfter     | datetime     | Se especificado, o serviço retorna apenas produtos que foram modificados após essa data.        | Não       |
| parentProductId   | string       | Se especificado, o serviço retorna apenas complementos que correspondem ao app especificado.      | Não       |
| productSkuIds     | lista&lt;ProductSkuId&gt; | Se especificado, o serviço retornará apenas os produtos aplicáveis aos pares produto/SKU fornecidos. Para obter mais informações, consulte a tabela abaixo.      | Não       |
| productTypes      | cadeia de caracteres       | Se especificado, o serviço retorna apenas produtos que correspondam aos tipos de produto especificados. Os tipos de produto com suporte são **Application**, **Durable** e **UnmanagedConsumable**.     | Não       |
| validityType      | string       | Quando definido como **All**, todos os produtos para um usuário serão retornados, incluindo itens expirados. Quando definido como **Valid**, somente os produtos que são válidos serão retornados nesse momento (ou seja, eles têm um status ativo, data de início &lt; agora e data de término é &gt; agora). | Não       |

<span/>

O objeto UserIdentity contém os parâmetros a seguir.

| Parâmetro            | Tipo   |  Descrição      | Obrigatório |
|----------------------|--------|----------------|----------|
| identityType         | string | Especifique o valor de cadeia de caracteres **b2b**.    | Sim      |
| identityValue        | cadeia de caracteres | O [chave ID da Windows Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cujos produtos você deseja consultar.  | Sim      |
| localTicketReference | cadeia de caracteres | O identificador solicitado para os produtos retornados. Itens retornados no corpo da resposta terão uma correspondência *localTicketReference*. Recomendamos que você use o mesmo valor que a declaração *userId* na chave ID da Windows Store. | Sim      |

<span/> 

O objeto ProductSkuId contém os parâmetros a seguir.

| Parâmetro | Tipo   | Descrição          | Obrigatório |
|-----------|--------|----------------------|----------|
| productId | cadeia de caracteres | O [ID da loja](in-app-purchases-and-trials.md#store-ids) para um [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) no catálogo da Windows Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD. | Sim      |
| skuID     | cadeia de caracteres | O [ID da loja](in-app-purchases-and-trials.md#store-ids) para [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) de produto no catálogo da Windows Store. Um exemplo de ID da loja para SKU é 0010.       | Sim      |

<span/>

### <a name="request-example"></a>Exemplo de solicitação

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
  "maxPageSize": 100,
  "beneficiaries": [
    {
      "localTicketReference": "1055521810674918",
      "identityValue": "eyJ0eXAiOiJ……",
      "identityType": "b2b"
    }
  ],
  "modifiedAfter": "\/Date(-62135568000000)\/",
  "productSkuIds": [
    {
      "productId": "9NBLGGH5WVP6",
      "skuId": "0010"
    }
  ],
  "productTypes": [
    "UnmanagedConsumable"
  ],
  "validityType": "All"
}
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Parâmetro         | Tipo                     | Descrição          | Obrigatório |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | string                   | Se houver vários conjuntos de produtos, esse token será retornado quando o limite de página for atingido. Você pode especificar esse token de continuação em chamadas subsequentes para recuperar produtos restantes. | Não       |
| itens             | CollectionItemContractV6 | Uma matriz de produtos para o usuário especificado. Para obter mais informações, consulte a tabela abaixo.        | Não       |

<span/> 

O objeto CollectionItemContractV6 contém os parâmetros a seguir.

| Parâmetro            | Tipo               | Descrição            | Obrigatório |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | datetime           | A data em que o usuário adquiriu o item.                  | Sim      |
| campaignId           | string             | A ID da campanha que foi fornecida para este item no momento da compra.                  | Não       |
| devOfferId           | string             | A ID de oferta de uma compra realizada em app.              | Não       |
| endDate              | datetime           | A data de término do item.              | Sim      |
| fulfillmentData      | string             | N/A         | Não       |
| inAppOfferToken      | string             | A cadeia de caracteres da ID de produto especificada pelo desenvolvedor que é atribuída ao item no painel do Centro de Desenvolvimento do Windows. Um exemplo de ID de produto é *product123*. | Não       |
| itemId               | cadeia de caracteres             | A ID que identifica esse item de coleção em relação a outros itens que o usuário tem. Essa ID é exclusiva por produto.   | Sim      |
| localTicketReference | cadeia de caracteres             | A ID de *localTicketReference* anteriormente fornecida no corpo da solicitação.                  | Sim      |
| modifiedDate         | datetime           | A data em que este item foi modificado pela última vez.              | Sim      |
| orderId              | string             | Se presente, a ID do pedido do qual este item foi obtido.              | Não       |
| orderLineItemId      | string             | Se presente, o item de linha da ordem específica para a qual o item foi obtido.              | Não       |
| ownershipType        | cadeia de caracteres             | A cadeia de caracteres *OwnedByBeneficiary*.   | Sim      |
| productId            | cadeia de caracteres             | O [ID da loja](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) no catálogo da Windows Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD.          | Sim      |
| productType          | cadeia de caracteres             | Um dos seguintes tipos de produto: **Application**, **Durable** e **UnmanagedConsumable**.        | Sim      |
| purchasedCountry     | cadeia de caracteres             | N/A   | Não       |
| comprador            | IdentityContractV6 | Se presente, representa a identidade do comprador do item. Veja os detalhes para esse objeto abaixo.        | Não       |
| quantity             | número             | A quantidade do item. Atualmente, sempre será 1.      | Não       |
| skuId                | cadeia de caracteres             | O [ID da loja](in-app-purchases-and-trials.md#store-ids) para [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) do produto no catálogo da Windows Store. Um exemplo de ID da loja para SKU é 0010.     | Sim      |
| skuType              | cadeia de caracteres             | O tipo de SKU. Os valores possíveis incluem **Trial**, **Full** e **Rental**.        | Sim      |
| startDate            | datetime           | A data em que a validade do item é iniciada.       | Sim      |
| status               | string             | O status do item. Os valores possíveis incluem **Active**, **Expired**, **Revoked** e **Banned**.    | Sim      |
| marcas                 | string             | N/A    | Sim      |
| transactionId        | guid               | A ID de transação como resultado da compra desse item. Pode ser usada para declarar um item como providenciado.      | Sim      |

<span/> 

O objeto IdentityContractV6 contém os parâmetros a seguir.

| Parâmetro     | Tipo   | Descrição                                                                        | Obrigatório |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | cadeia de caracteres | Contém o valor *pub*.                                                      | Sim      |
| identityValue | cadeia de caracteres | O valor da cadeia de caracteres de *publisherUserId* da chave de ID da Windows Store especificada. | Sim      |

<span/> 

### <a name="response-example"></a>Exemplo de resposta

```syntax
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
  "items" : [
    {
      "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
      "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
      "endDate" : "9999-12-31T23:59:59.9999999+00:00",
      "fulfillmentData" : [],
      "inAppOfferToken" : "consumable2",
      "itemId" : "4b8fbb13127a41f299270ea668681c1d",
      "localTicketReference" : "1055521810674918",
      "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
      "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
      "ownershipType" : "OwnedByBeneficiary",
      "productId" : "9NBLGGH5WVP6",
      "productType" : "UnmanagedConsumable",
      "purchaser" : {
        "identityType" : "pub",
        "identityValue" : "user123"
      },
      "skuId" : "0010",
      "skuType" : "Full",
      "startDate" : "2015-09-22T19:22:51.2068724+00:00",
      "status" : "Active",
      "tags" : [],
      "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
    }
  ]
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Renovar uma chave ID da Windows Store](renew-a-windows-store-id-key.md)

