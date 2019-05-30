---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Use esse método na API de envio a Microsoft Store para criar um complemento para o aplicativo que está registrado para sua conta PartnerCenter.
title: Criar um complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, criar complemento, produto in-App, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 00eb1a865631ce51cfa065d27ed00b44c66a6757
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371262"
---
# <a name="create-an-add-on"></a>Criar um complemento

Use esse método na API de envio a Microsoft Store para criar um complemento (também conhecido como no aplicativo produto ou IAP) para um aplicativo que é registrado em sua conta no Partner Center.

> [!NOTE]
> Este método cria um complemento sem nenhum envio. Para criar um envio para um complemento, veja os métodos em [Gerenciar envios de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.

|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  applicationIds  |  matriz  |  Uma matriz que contém a ID da Loja do aplicativo ao qual esse complemento está associado. Somente um item é compatível nessa matriz.   |  Sim  |
|  productId  |  cadeia de caracteres  |  A ID do produto do complemento. Este é um identificador que pode ser usado no código para fazer referência ao complemento. Para obter mais informações, consulte [Definir seu tipo de produto e a ID do produto](https://docs.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sim  |
|  productType  |  cadeia de caracteres  |  O tipo de produto do complemento. Há suporte para os seguintes valores: **Durável** e **consumível**.  |  Sim  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo complemento consumível para um aplicativo.

```json
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
| 409  | O complemento não pôde ser criado devido a seu estado atual ou o complemento usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Obter todos os complementos](get-all-add-ons.md)
* [Adquirir um complemento](get-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)
