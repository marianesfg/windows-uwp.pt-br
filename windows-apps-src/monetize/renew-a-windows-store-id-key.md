---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Use esse método para renovar uma chave da Microsoft Store.
title: Renovar uma chave de ID da Microsoft Store
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API de coleção da Microsoft Store, API de compra da Microsoft Store, chave ID da Microsoft Store, renovar
ms.localizationpriority: medium
ms.openlocfilehash: fd4d7ce26e12f7ff939ced8d456390b97d0c8a0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620481"
---
# <a name="renew-a-microsoft-store-id-key"></a>Renovar uma chave de ID da Microsoft Store


Use esse método para renovar uma chave da Microsoft Store. Quando você [gera uma chave da ID da Microsoft Store](view-and-grant-products-from-a-service.md#step-4), a chave é válida por 90 dias. Depois que a chave expira, você pode usar a chave expirada para renegociar uma nova chave usando esse método.

## <a name="prerequisites"></a>Pré-requisitos


Para usar esse método, você precisará:

* Um token de acesso do Azure AD com o URI de audiência `https://onestore.microsoft.com`.
* Uma chave da ID da Microsoft Store expirada que foi [gerada com base no código do lado do cliente no app](view-and-grant-products-from-a-service.md#step-4).

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Tipo de chave    | Método | URI da solicitação                                              |
|-------------|--------|----------------------------------------------------------|
| Coleções | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Adquirir    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | cadeia de caracteres | Deve ser definido como o valor **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | number | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | cadeia de caracteres | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |


### <a name="request-body"></a>Corpo da solicitação

| Parâmetro     | Tipo   | Descrição                       | Obrigatório |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | cadeia de caracteres | O token de acesso do Azure AD.        | Sim      |
| key           | cadeia de caracteres | A chave ID da Microsoft Store expirada. | Sim       |


### <a name="request-example"></a>Exemplo de solicitação

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Parâmetro | Tipo   | Descrição                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | cadeia de caracteres | A chave da Microsoft Store atualizada que pode ser usada em futuras chamadas das APIs de coleção ou compra da Microsoft Store. |


### <a name="response-example"></a>Exemplo de resposta

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>Códigos de erro


| Código | Erro        | Código de erro interno           | Descrição   |
|------|--------------|----------------------------|---------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave ID da Microsoft Store e a declaração *appid* no token de acesso do Azure AD não correspondem.                                                                     |


## <a name="related-topics"></a>Tópicos relacionados


* [Gerenciar direitos de produto de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta de produtos](query-for-products.md)
* [Produtos de consumo de relatório como atendida](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)
