---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: "Use esse método para renovar uma chave da Windows Store."
title: Renovar uma chave ID da Windows Store
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: 6255346c568ed24e17c795834ab182f73707c4de

---

# Renovar uma chave ID da Windows Store


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Use esse método para renovar uma chave da Windows Store. Quando você gera uma chave ID da Windows Store chamando o método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675), essa chave é válida por 90 dias. Depois que a chave expira, você pode usar a chave expirada para renegociar uma nova chave usando esse método.

## Pré-requisitos


Para usar esse método, você precisará:

-   Ter um token de acesso do Azure AD criado com o URI de público `https://onestore.microsoft.com`.
-   Uma chave ID da Windows Store expirada que foi gerada chamando o método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) ou [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) do código no lado do cliente em seu aplicativo.

Para saber mais, consulte [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md).

## Solicitação


### Sintaxe da solicitação

| Tipo de chave    | Método | URI da solicitação                                              |
|-------------|--------|----------------------------------------------------------|
| Coleções | POST   | `https://collections.mp.microsoft.com/v6.0/b2b/keys/renew` |
| Compra    | POST   | `https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew`    |

<br/> 

### Cabeçalho da solicitação

| Cabeçalho         | Tipo   | Descrição                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | cadeia de caracteres | Deve ser definido como o valor **collections.mp.microsoft.com** ou **purchase.mp.microsoft.com**.           |
| Content-Length | número | O comprimento do corpo da solicitação.                                                                       |
| Content-Type   | string | Especifica o tipo de solicitação e resposta. Atualmente, o único valor com suporte é **application/json**. |

<br/> 

### Corpo da solicitação

| Parâmetro     | Tipo   | Descrição                       | Obrigatório |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | O token de acesso do Azure AD.        | Sim      |
| chave           | string | A chave ID da Windows Store expirada. | Não       |

<br/> 

### Exemplo de solicitação

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

## Resposta


### Corpo da resposta

| Parâmetro | Tipo   | Descrição                                                                                                            | Obrigatório |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| key       | string | A chave da Windows Store atualizada que pode ser usada em futuras chamadas das APIs de coleção ou compra da Windows Store. | Não       |

<br/> 

### Exemplo de resposta

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

## Códigos de erro


| Código | Erro        | Código de erro interno           | Descrição                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | Não autorizado | AuthenticationTokenInvalid | O token de acesso do Azure AD é inválido. Em alguns casos, os detalhes de ServiceError irão conter mais informações, como quando o token está expirado ou falta a declaração *appid*. |
| 401  | Não autorizado | InconsistentClientId       | A declaração *clientId* na chave ID da Windows Store e a declaração *appid* no token de acesso do Azure AD não correspondem.                                                                     |

<br/> 

## Tópicos relacionados


* [Exibir e conceder produtos de um serviço](view-and-grant-products-from-a-service.md)
* [Consulta por produtos](query-for-products.md)
* [Declarar produtos consumíveis como providenciados](report-consumable-products-as-fulfilled.md)
* [Conceder produtos gratuitos](grant-free-products.md)



<!--HONumber=Jun16_HO5-->


