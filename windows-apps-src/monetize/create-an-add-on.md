---
author: Xansky
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Use este método na API de envio da Microsoft Store para criar um complemento para um aplicativo que está registrado na sua conta PartnerCenter.
title: Criar um complemento
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, criar complemento, produto in-App, IAP
ms.localizationpriority: medium
ms.openlocfilehash: d262a86c4a177095015c3f1391b19f1a7719d0a4
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7572088"
---
# <a name="create-an-add-on"></a>Criar um complemento

Use este método na API de envio da Microsoft Store para criar um complemento (também conhecido como produto in-App ou IAP) para um aplicativo que está registrado à sua conta do Partner Center.

> [!NOTE]
> Este método cria um complemento sem nenhum envio. Para criar um envio para um complemento, veja os métodos em [Gerenciar envios de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.

|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  applicationIds  |  array  |  Uma matriz que contém a ID da Loja do aplicativo ao qual esse complemento está associado. Essa matriz é compatível com apenas um item.   |  Sim  |
|  productId  |  string  |  A ID do produto do complemento. Este é um identificador que pode ser usado no código para fazer referência ao complemento. Para obter mais informações, consulte [Definir seu tipo de produto e a ID do produto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sim  |
|  productType  |  string  |  O tipo de produto do complemento. Há suporte para os seguintes valores: **Durável** e **Consumíveis**.  |  Sim  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo complemento consumível para um aplicativo.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, veja [recurso do complemento](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição                                                                                                                                                                           |
|--------|------------------|
| 400  | A solicitação é inválida. |
| 409  | O complemento não pôde ser criado por causa do estado atual ou o complemento usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Obter todos os complementos](get-all-add-ons.md)
* [Obter um complemento](get-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)
