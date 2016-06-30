---
author: mcleanbyron
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: "Use esse método na API da coleção da Windows Store para obter todos os produtos que pertence a um cliente em relação a aplicativos que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros."
title: Consulta por produtos
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: b8661d73487dde61b207159d11a0583700fa22bc

---

# Consulta por produtos


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Use esse método na API da coleção da Windows Store para obter todos os produtos que pertence a um cliente em relação a aplicativos que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros.

Esse método é projetado para ser chamado pelo seu serviço em resposta a uma mensagem de seu aplicativo. Seu serviço não deve sondar regularmente todos os usuários em um agendamento.

## Pré-requisitos


Para usar esse método, você precisará:

-   Ter um token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com`.
-   Uma chave ID da Windows Store que tenha sido gerada chamando-se o método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) no código do lado do cliente em seu aplicativo.

Para saber mais, consulte [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md).

## Solicitação

### Sintaxe da solicitação

| Método | URI da solicitação                                                 |
|--------|-------------------------------------------------------------|
| POST   | `https://collections.mp.microsoft.com/v6.0/collections/query` |

<br/>
 
### Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorização  | cadeia de caracteres | Obrigatório. O token de acesso do Azure AD no formulário **Bearer**&lt;*token*&gt;.                           |
| Host           | string | Deve ser definido como o valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |

 <br/>

### Corpo da solicitação

| Parâmetro         | Tipo         | Descrição                                                                                                                                                                                                                                                          | Obrigatório |
|-------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| beneficiários     | UserIdentity | Um objeto UserIdentity que representa o usuário está sendo consultado por produtos.                                                                                                                                                                                           | Sim      |
| continuationToken | string       | Se houver vários conjuntos de produtos, o corpo da resposta retornará um token de continuação quando o limite de página for atingido. Forneça esse token de continuação em chamadas subsequentes para recuperar produtos restantes.                                                      | Não       |
| maxPageSize       | número       | O número máximo de produtos para retornar uma resposta. O valor máximo e padrão é 100.                                                                                                                                                                      | Não       |
| modifiedAfter     | datetime     | Se especificado, o serviço retorna apenas produtos que foram modificados após essa data.                                                                                                                                                                             | Não       |
| parentProductId   | string       | Se especificado, o serviço retorna apenas IAPs que correspondem ao aplicativo especificado.                                                                                                                                                                                    | Não       |
| productSkuIds     | ProductSkuId | Se especificado, o serviço retornará apenas os produtos aplicáveis aos pares Produto/SKU fornecidos.                                                                                                                                                                        | Não       |
| productTypes      | string       | Se especificado, o serviço retorna apenas produtos que correspondam aos tipos de produto especificados. Os tipos de produto com suporte são **Application**, **Durable** e **UnmanagedConsumable**.                                                                                       | Não       |
| validityType      | string       | Quando definido como **All**, todos os produtos para um usuário serão retornados, incluindo itens expirados. Quando definido como **Valid**, somente os produtos que são válidos serão retornados nesse momento (ou seja, eles têm um status ativo, data de início &lt; agora e data de término é &gt; agora). | Não       |

<br/> 

O objeto UserIdentity contém os parâmetros a seguir.

| Parâmetro            | Tipo   | Descrição                                                                                                                                                                                                                  | Obrigatório |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | string | Especifique o valor de cadeia de caracteres **b2b**.                                                                                                                                                                                            | Sim      |
| identityValue        | string | Valor cadeia de caracteres da chave ID da Windows Store ID.                                                                                                                                                                                    | Sim      |
| localTicketReference | string | Identificador solicitado para produtos retornados. Itens retornados no corpo da resposta terão uma correspondência *localTicketReference*. Recomendamos que você use o mesmo valor que a declaração *userId* na chave ID da Windows Store. | Sim      |

<br/> 

O objeto ProductSkuId contém os parâmetros a seguir.

| Parâmetro | Tipo   | Descrição                                                                                                                                                                                                                                                                                                            | Obrigatório |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| productId | cadeia de caracteres | A ID da Loja do catálogo da Windows Store. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8. | Sim      |
| skuID     | cadeia de caracteres | A ID da SKU do catálogo da Windows Store. Um ID de SKU de exemplo é “0010”.                                                                                                                                                                                                                                                | Sim      |

<br/> 

### Exemplo de solicitação

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

## Resposta


### Corpo da resposta

| Parâmetro         | Tipo                     | Descrição                                                                                                                                                                                | Obrigatório |
|-------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| continuationToken | string                   | Se houver vários conjuntos de produtos, esse token será retornado quando o limite de página for atingido. Você pode especificar esse token de continuação em chamadas subsequentes para recuperar produtos restantes. | Não       |
| Itens             | CollectionItemContractV6 | Uma matriz de produtos para o usuário especificado.                                                                                                                                               | Não       |

<br/> 

O objeto CollectionItemContractV6 contém os parâmetros a seguir.

| Parâmetro            | Tipo               | Descrição                                                                                                                                        | Obrigatório |
|----------------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| acquiredDate         | datetime           | A data em que o usuário adquiriu o item.                                                                                                      | Sim      |
| campaignId           | string             | A ID da campanha que foi fornecida para este item no momento da compra.                                                                                  | Não       |
| devOfferId           | cadeia de caracteres             | A ID de oferta de uma compra realizada em aplicativo.                                                                                                              | Não       |
| endDate              | datetime           | A data de término do item.                                                                                                                          | Sim      |
| fulfillmentData      | cadeia de caracteres             | N/A                                                                                                                                                | Não       |
| inAppOfferToken      | string             | A cadeia de caracteres da ID de produto especificada pelo desenvolvedor que é atribuída ao item no painel do Centro de Desenvolvimento do Windows. Uma ID de produto de exemplo é "product123". | Não       |
| itemId               | string             | A ID que identifica esse item de coleção em relação a outros itens que o usuário tem. Essa ID é exclusiva por produto.                                          | Sim      |
| localTicketReference | string             | A ID de `localTicketReference` anteriormente fornecida no corpo da solicitação.                                                                      | Sim      |
| modifiedDate         | datetime           | A data em que este item foi modificado pela última vez.                                                                                                              | Sim      |
| orderId              | string             | Se presente, a ID do pedido do qual este item foi obtido.                                                                                          | Não       |
| orderLineItemId      | string             | Se presente, o item de linha da ordem específica para a qual o item foi obtido.                                                                | Não       |
| ownershipType        | string             | A cadeia de caracteres "OwnedByBeneficiary".                                                                                                                   | Sim      |
| productId            | cadeia de caracteres             | A ID da Loja para o aplicativo no catálogo da Windows Store. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8.                                                            | Sim      |
| productType          | cadeia de caracteres             | Um dos seguintes tipos de produto: **Application**, **Durable** e **UnmanagedConsumable**.                                                     | Sim      |
| purchasedCountry     | string             | N/A.                                                                                                                                               | Não       |
| comprador            | IdentityContractV6 | Se presente, representa a identidade do comprador do item. Veja os detalhes para esse objeto abaixo.                                      | Não       |
| Quantity             | número             | A quantidade do item. Atualmente, sempre será 1.                                                                                        | Não       |
| skuId                | string             | A ID da SKU do catálogo da Windows Store. Um ID de SKU de exemplo é “0010”.                                                                            | Sim      |
| skuType              | string             | O tipo de SKU. Os valores possíveis incluem **Trial**, **Full** e **Rental**.                                                                      | Sim      |
| startDate            | datetime           | A data em que a validade do item é iniciada.                                                                                                         | Sim      |
| Status               | string             | O status do item. Os valores possíveis incluem **Active**, **Expired**, **Revoked** e **Banned**.                                              | Sim      |
| Tags                 | string             | N/A                                                                                                                                                | Sim      |
| transactionId        | guid               | A ID de transação como resultado da compra desse item. Pode ser usada para declarar um item como providenciado.                                       | Sim      |

<br/> 

O objeto IdentityContractV6 contém os parâmetros a seguir.

| Parâmetro     | Tipo   | Descrição                                                                        | Obrigatório |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Contém o valor **"pub"**.                                                      | Sim      |
| identityValue | string | O valor da cadeia de caracteres de *publisherUserId* da chave de ID da Windows Store especificada. | Sim      |

<br/> 

### Exemplo de resposta

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

## Tópicos relacionados

* [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Renovar uma chave ID da Windows Store](renew-a-windows-store-id-key.md)



<!--HONumber=Jun16_HO4-->


