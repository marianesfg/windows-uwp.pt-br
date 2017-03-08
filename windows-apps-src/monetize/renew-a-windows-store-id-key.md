---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "Use esse método para renovar uma chave da Windows Store."
title: Renovar uma chave ID da Windows Store
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de coleção da Windows Store, API de compra da Windows Store, chave ID da Windows Store, renovar"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: b740cf431607f1748a8513a02746a70560d09da2
ms.lasthandoff: 02/07/2017

---

# <a name="renew-a-windows-store-id-key"></a>Renovar uma chave ID da Windows Store


Use esse método para renovar uma chave da Windows Store. Quando você [gera uma chave da ID da Windows Store](view-and-grant-products-from-a-service.md#step-4), a chave é válida por 90 dias. Depois que a chave expira, você pode usar a chave expirada para renegociar uma nova chave usando esse método.

## <a name="prerequisites"></a>Pré-requisitos


Para usar esse método, você precisará:

* Ter um token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com`.
* Uma chave da ID da Windows Store expirada que foi [gerada com base no código do lado do cliente no app](view-and-grant-products-from-a-service.md#step-4).

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Tipo de chave    | Método | URI da Solicitação                                              |
|-------------|--------|----------------------------------------------------------|
| Coleções | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Compra    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |

<span/>

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | string | Deve ser definido como o valor **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |

<span/>

### <a name="request-body"></a>Corpo da solicitação

| Parâmetro     | Tipo   | Descrição                       | Obrigatório |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | O token de acesso do Azure AD.        | Sim      |
| chave           | string | A chave ID da Windows Store expirada. | Não       |

<span/> 

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

| Parâmetro | Tipo   | Descrição                                                                                                            | Obrigatório |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | string | A chave da Windows Store atualizada que pode ser usada em futuras chamadas das APIs de coleção ou compra da Windows Store. | Não       |

<span/>

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


| Código | Erro        | Código de erro interno           | Descrição                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave ID da Windows Store e a declaração *appid* no token de acesso do Azure AD não correspondem.                                                                     |

<span/>

## <a name="related-topics"></a>Tópicos relacionados


* [Gerenciar direitos a produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)

