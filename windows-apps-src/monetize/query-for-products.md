---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Use esse método na API da coleção da Microsoft Store para obter todos os produtos que pertence a um cliente em relação a aplicativos que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros.
title: Consulta por produtos
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API de coleção da Microsoft Store, exibir produtos
ms.localizationpriority: medium
ms.openlocfilehash: a5a986896ca3b2585590e11d17219106a9043204
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252280"
---
# <a name="query-for-products"></a>Consulta por produtos


Use esse método na API da coleção da Microsoft Store para obter todos os produtos que pertence a um cliente em relação a aplicativos que estejam associados com sua ID de cliente do Azure AD. Você pode analisar sua consulta para um determinado produto ou usar outros filtros.

Esse método é projetado para ser chamado pelo seu serviço em resposta a uma mensagem de seu aplicativo. Seu serviço não deve sondar regularmente todos os usuários em um agendamento.

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}


Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave ID da Microsoft Store que representa a identidade do usuário cujos produtos você deseja obter.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI de solicitação                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | string | Obrigatório. O token de acesso do AD do Azure no formulário **portador** &lt;*token*&gt;.                           |
| Host           | string | Deve ser definido como o valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-body"></a>Corpo da solicitação

| Parâmetro         | Tipo         | Descrição         | Obrigatório |
|-------------------|--------------|---------------------|----------|
| beneficiários     | listar&lt;UserIdentity&gt; | Uma lista de objetos UserIdentity que representam os usuários que estão sendo consultados para produtos. Para obter mais informações, consulte a tabela abaixo.    | Sim      |
| continuationToken | string       | Se houver vários conjuntos de produtos, o corpo da resposta retornará um token de continuação quando o limite de página for atingido. Forneça esse token de continuação em chamadas subsequentes para recuperar produtos restantes.       | Não       |
| maxPageSize       | número       | O número máximo de produtos para retornar uma resposta. O valor máximo e padrão é 100.                 | Não       |
| modifiedAfter     | datetime     | Se especificado, o serviço retorna apenas produtos que foram modificados após essa data.        | Não       |
| parentProductId   | string       | Se especificado, o serviço retorna apenas complementos que correspondem ao aplicativo especificado.      | Não       |
| productSkuIds     | list&lt;ProductSkuId&gt; | Se especificado, o serviço retornará apenas os produtos aplicáveis aos pares produto/SKU fornecidos. Para obter mais informações, consulte a tabela abaixo.      | Não       |
| productTypes      | listar&lt;cadeia de caracteres&gt;       | Especifica os tipos de produtos a serem retornados nos resultados da consulta. Os tipos de produtos com suporte são **Application**, **durável**, **Game**e **UnmanagedConsumable**.     | Sim       |
| validityType      | string       | Quando definido como **All**, todos os produtos para um usuário serão retornados, incluindo itens expirados. Quando definido como **Valid**, somente os produtos que são válidos serão retornados nesse momento (ou seja, eles têm um status ativo, data de início &lt; agora e data de término é &gt; agora). | Não       |


O objeto UserIdentity contém os parâmetros a seguir.

| Parâmetro            | Tipo   |  Descrição      | Obrigatório |
|----------------------|--------|----------------|----------|
| identityType         | string | Especifique o valor de cadeia de caracteres **b2b**.    | Sim      |
| identityValue        | string | O [chave ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário cujos produtos você deseja consultar.  | Sim      |
| localTicketReference | string | O identificador solicitado para os produtos retornados. Itens retornados no corpo da resposta terão uma correspondência *localTicketReference*. Recomendamos que você use o mesmo valor do que a declaração *userId* na chave ID da Microsoft Store. | Sim      |


O objeto ProductSkuId contém os parâmetros a seguir.

| Parâmetro | Tipo   | Descrição          | Obrigatório |
|-----------|--------|----------------------|----------|
| productId | string | O [ID da Store](in-app-purchases-and-trials.md#store-ids) para um [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) no catálogo da Microsoft Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD. | Sim      |
| skuID     | string | O [ID da Store](in-app-purchases-and-trials.md#store-ids) para [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) de produto no catálogo da Microsoft Store. Um exemplo de ID da loja para SKU é 0010.       | Sim      |


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


O objeto CollectionItemContractV6 contém os parâmetros a seguir.

| Parâmetro            | Tipo               | Descrição            | Obrigatório |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | datetime           | A data em que o usuário adquiriu o item.                  | Sim      |
| campaignId           | string             | A ID da campanha que foi fornecida para este item no momento da compra.                  | Não       |
| devOfferId           | string             | A ID de oferta de uma compra realizada em aplicativo.              | Não       |
| endDate              | datetime           | A data de término do item.              | Sim      |
| fulfillmentData      | string             | {1&gt;N/A&lt;1}         | Não       |
| inAppOfferToken      | string             | A cadeia de caracteres de ID do produto especificada pelo desenvolvedor que é atribuída ao item no Partner Center. Uma ID de produto de exemplo é *product123*. | Não       |
| itemId               | string             | A ID que identifica esse item de coleção em relação a outros itens que o usuário tem. Essa ID é exclusiva por produto.   | Sim      |
| localTicketReference | string             | A ID de *localTicketReference* anteriormente fornecida no corpo da solicitação.                  | Sim      |
| modifiedDate         | datetime           | A data em que este item foi modificado pela última vez.              | Sim      |
| orderId              | string             | Se presente, a ID do pedido do qual este item foi obtido.              | Não       |
| orderLineItemId      | string             | Se presente, o item de linha da ordem específica para a qual o item foi obtido.              | Não       |
| ownershipType        | string             | A cadeia de caracteres *OwnedByBeneficiary*.   | Sim      |
| productId            | string             | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) no catálogo da Microsoft Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD.          | Sim      |
| productType          | string             | Um dos seguintes tipos de produto: **Application**, **Durable** e **UnmanagedConsumable**.        | Sim      |
| purchasedCountry     | string             | {1&gt;N/A&lt;1}   | Não       |
| comprador            | IdentityContractV6 | Se presente, representa a identidade do comprador do item. Veja os detalhes para esse objeto abaixo.        | Não       |
| quantity             | número             | A quantidade do item. Atualmente, sempre será 1.      | Não       |
| skuId                | string             | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) do produto no catálogo da Microsoft Store. Um exemplo de ID da loja para SKU é 0010.     | Sim      |
| skuType              | string             | O tipo de SKU. Os valores possíveis incluem **Trial**, **Full** e **Rental**.        | Sim      |
| startDate            | datetime           | A data em que a validade do item é iniciada.       | Sim      |
| status               | string             | O status do item. Os valores possíveis incluem **Active**, **Expired**, **Revoked** e **Banned**.    | Sim      |
| marcas                 | string             | {1&gt;N/A&lt;1}    | Sim      |
| transactionId        | guid               | A ID de transação como resultado da compra desse item. Pode ser usada para declarar um item como providenciado.      | Sim      |


O objeto IdentityContractV6 contém os parâmetros a seguir.

| Parâmetro     | Tipo   | Descrição                                                                        | Obrigatório |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Contém o valor *pub*.                                                      | Sim      |
| identityValue | string | O valor da sequência de *publisherUserId* da chave de ID da Microsoft Store especificada. | Sim      |


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

* [Gerenciar direitos de produto de um serviço](view-and-grant-products-from-a-service.md)
* [Relatar produtos consumíveis como atendidos](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
* [Renovar uma chave de ID de Microsoft Store](renew-a-windows-store-id-key.md)
