---
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: Use esse método na API de compra da Microsoft Store para conceder um aplicativo ou complemento gratuito a um determinado usuário.
title: Conceder produtos gratuitos
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API de compra da Microsoft Store, conceder produtos
ms.localizationpriority: medium
ms.openlocfilehash: 957958891b1052be4ac9ae65d90f97ff8a44ef36
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613191"
---
# <a name="grant-free-products"></a>Conceder produtos gratuitos

Use este método na API de compra da Microsoft Store para conceder um aplicativo ou complemento (também conhecido como produto no aplicativo ou IAP) gratuito a um determinado usuário.

No momento, você pode conceder apenas produtos gratuitos. Se seu serviço tenta usar esse método para conceder um produto que não seja gratuito, esse método retornará um erro.

## <a name="prerequisites"></a>Pré-requisitos

Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave de ID da Microsoft Store que representa a identidade do usuário para o qual você deseja conceder um produto gratuito.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorização  | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;.                           |
| Host           | cadeia de caracteres | Deve ser definido como o valor **purchase.mp.microsoft.com**.                                            |
| Content-Length | number | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | cadeia de caracteres | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-body"></a>Corpo da solicitação

| Parâmetro      | Tipo   | Descrição        | Obrigatório |
|----------------|--------|---------------------|----------|
| availabilityId | cadeia de caracteres | A ID de disponibilidade do produto a ser concedido no catálogo da Microsoft Store.         | Sim      |
| b2bKey         | cadeia de caracteres | A [chave de ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa a identidade do usuário para o qual você deseja conceder um produto gratuito.    | Sim      |
| devOfferId     | cadeia de caracteres | Uma ID de oferta especificada pelo desenvolvedor que irá aparecer no item Coleção após a compra.        |
| language       | cadeia de caracteres | O idioma do usuário.  | Sim      |
| market         | cadeia de caracteres | O mercado do usuário.       | Sim      |
| orderId        | guid   | Uma GUID gerada para o pedido. Esse valor é exclusivo para o usuário, mas não é necessário que seja exclusivo em todos os pedidos.    | Sim      |
| productId      | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) no catálogo da Microsoft Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD. | Sim      |
| quantity       | int    | A quantidade a ser comprada. Atualmente, o único valor com suporte é 1. Se não for especificado, o padrão é 1.   | Não       |
| skuId          | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) do produto no catálogo da Microsoft Store. Um exemplo de ID da loja para SKU é 0010.     | Sim      |


### <a name="request-example"></a>Exemplo de solicitação

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Parâmetro                 | Tipo                        | Descrição             | Obrigatório |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | Informações contextuais do cliente para este pedido. Isso será atribuído ao valor *clientID* no token do Azure AD.    | Sim      |
| createdtime               | datetimeoffset              | A hora em que o pedido foi criado.         | Sim      |
| currencyCode              | cadeia de caracteres                      | Código de moeda para *totalAmount* e *totalTaxAmount*. N/D para itens gratuitos.     | Sim      |
| friendlyName              | cadeia de caracteres                      | O nome amigável do pedido. N/D para pedidos feitos usando-se a API de compra da Microsoft Store. | Sim      |
| isPIRequired              | booliano                     | Indica se um PI (meio de pagamento) é necessário como parte da ordem de compra.  | Sim      |
| language                  | cadeia de caracteres                      | A ID de idioma para a ordem (por exemplo, "en").       | Sim      |
| market                    | cadeia de caracteres                      | A ID de mercado para a ordem (por exemplo, "US").  | Sim      |
| orderId                   | cadeia de caracteres                      | ID que identifica o pedido de um usuário específico.                | Sim      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | A lista de itens de linha do pedido. Normalmente, há um item de linha por pedido.       | Sim      |
| orderState                | cadeia de caracteres                      | O estado do pedido. Os estados válidos são: **Editing**, **CheckingOut**, **Pending**, **Purchased**, **Refunded**, **ChargedBack** e **Cancelled**. | Sim      |
| orderValidityEndTime      | cadeia de caracteres                      | A última vez em que o preço do pedido era válido antes de ser enviado. N/D para aplicativos gratuitos.      | Sim      |
| orderValidityStartTime    | cadeia de caracteres                      | A primeira vez em que o preço do pedido é válido antes de ser enviado. N/D para aplicativos gratuitos.          | Sim      |
| comprador                 | IdentityV6                  | Um objeto que descreve a identidade do comprador.       | Sim      |
| totalAmount               | decimal                     | O valor total da compra de todos os itens no pedido, incluindo imposto.       | Sim      |
| totalAmountBeforeTax      | decimal                     | Valor total da compra de todos os itens no pedido, sem imposto.      | Sim      |
| totalChargedToCsvTopOffPI | decimal                     | Se você estiver usando um meio de pagamento e o valor armazenado (CSV) separados, o valor será carregado no CSV.            | Sim      |
| totalTaxAmount            | decimal                     | O valor total do imposto para todos os itens de linha.    | Sim      |


O objeto ClientContext contém os parâmetros a seguir.

| Parâmetro | Tipo   | Descrição                           | Obrigatório |
|-----------|--------|---------------------------------------|----------|
| cliente    | cadeia de caracteres | A ID do cliente que criou o pedido. | Não       |


O objeto OrderLineItemV6 contém os parâmetros a seguir.

| Parâmetro               | Tipo           | Descrição                                                                                                  | Obrigatório |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agente                   | IdentityV6     | O agente que editou o item de linha pela última vez. Para obter mais informações sobre esse objeto, consulte a tabela a seguir.       | Não       |
| availabilityId          | cadeia de caracteres         | A ID de disponibilidade do produto a ser comprado no catálogo da Microsoft Store.                           | Sim      |
| beneficiário             | IdentityV6     | A identidade do beneficiário do pedido.                                                                | Não       |
| billingState            | cadeia de caracteres         | O estado da cobrança do pedido. Isso é definido como **Charged** quando concluído.                                   | Não       |
| campaignId              | cadeia de caracteres         | A ID de campanha desse pedido.                                                                              | Não       |
| currencyCode            | cadeia de caracteres         | O código de moeda usado nos detalhes do preço.                                                                    | Sim      |
| description             | cadeia de caracteres         | Uma descrição traduzida do item de linha.                                                                    | Sim      |
| devofferId              | cadeia de caracteres         | A ID de oferta do pedido específico, se presente.                                                           | Não       |
| fulfillmentDate         | datetimeoffset | A data em que ocorreu o providência.                                                                           | Não       |
| fulfillmentState        | cadeia de caracteres         | O estado do abastecimento desse item. Isso é definido como **Fulfilled** quando concluído.                      | Não       |
| isPIRequired            | booliano        | Indica se um meio de pagamento é necessário para este item de linha.                                       | Sim      |
| isTaxIncluded           | booliano        | Indica se o imposto está incluído nos detalhes do preço do item.                                        | Sim      |
| legacyBillingOrderId    | cadeia de caracteres         | A ID de cobrança herdada.                                                                                       | Não       |
| lineItemId              | cadeia de caracteres         | A ID do item de linha do item nesse pedido.                                                                 | Sim      |
| listPrice               | decimal        | O preço de lista do item nesse pedido.                                                                    | Sim      |
| productId               | cadeia de caracteres         | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [produto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa o item de linha no catálogo da Microsoft Store. Um exemplo de ID da loja para um produto é 9NBLGGH42CFD.   | Sim      |
| productType             | cadeia de caracteres         | O tipo do produto. Os valores suportados são **Durable**, **Application** e **UnmanagedConsumable**. | Sim      |
| quantity                | int            | A quantidade do item solicitado.                                                                            | Sim      |
| retailPrice             | decimal        | O preço de varejo do item solicitado.                                                                        | Sim      |
| revenueRecognitionState | cadeia de caracteres         | O estado de reconhecimento da receita.                                                                               | Sim      |
| skuId                   | cadeia de caracteres         | A [ID da Store](in-app-purchases-and-trials.md#store-ids) para o [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) do item de linha no catálogo da Microsoft Store. Um exemplo de ID da loja para SKU é 0010.                                                                   | Sim      |
| taxAmount               | decimal        | O valor do imposto do item de linha.                                                                            | Sim      |
| taxType                 | cadeia de caracteres         | O tipo de imposto para os impostos aplicáveis.                                                                       | Sim      |
| Título                   | cadeia de caracteres         | O título traduzido do item de linha.                                                                        | Sim      |
| totalAmount             | decimal        | O valor total da compra do item de linha com imposto.                                                    | Sim      |


O objeto IdentityV6 contém os parâmetros a seguir.

| Parâmetro     | Tipo   | Descrição                                                                        | Obrigatório |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | cadeia de caracteres | Contém o valor **"pub"**.                                                      | Sim      |
| identityValue | cadeia de caracteres | O valor da sequência de *publisherUserId* da chave de ID da Microsoft Store especificada. | Sim      |


### <a name="response-example"></a>Exemplo de resposta

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>Códigos de erro


| Código | Erro        | Código de erro interno           | Descrição   |
|------|--------------|----------------------------|----------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | PartnerAadTicketRequired   | Um token de acesso do Azure AD não foi passado para o serviço no cabeçalho de autorização.   |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave de ID da Microsoft Store no corpo da solicitação e a declaração *appid* no token de acesso do Azure AD no cabeçalho de autorização não coincidem.       |
| 400  | BadRequest   | InvalidParameter           | Os detalhes contêm informações relativas ao corpo da solicitação e aos campos que contêm um valor inválido.           |

<span/> 

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciar direitos de produto de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta de produtos](query-for-products.md)
* [Produtos de consumo de relatório como atendida](report-consumable-products-as-fulfilled.md)
* [Renovar uma chave de ID do Microsoft Store](renew-a-windows-store-id-key.md)
