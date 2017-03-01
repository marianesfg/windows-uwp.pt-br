---
author: mcleanbyron
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: "Use este método na API de envio da Windows Store para criar um complemento para um app que está registrado à sua conta do Centro de Desenvolvimento do Windows."
title: Criar um complemento usando a API de envio da Windows Store
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Windows Store, criar complemento, produto in-App, IAP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0492398872142aabd32d3a4d68d55b4e326f027e
ms.lasthandoff: 02/07/2017

---

# <a name="create-an-add-on-using-the-windows-store-submission-api"></a>Criar um complemento usando a API de envio da Windows Store




Use este método na API de envio da Windows Store para criar um complemento (também conhecido como produto no app ou IAP) para um app que está registrado para sua conta do Centro de Desenvolvimento do Windows.

>**Observação**&nbsp;&nbsp;Este método cria um complemento sem nenhum envio. Para criar um envio para um complemento, veja os métodos em [Gerenciar envios de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Windows Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

>**Observação**&nbsp;&nbsp;Este método só pode ser usado para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da Solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.
 
|  Parâmetro  |  Tipo  |  Descrição  |  Obrigatório  |
|------|------|------|------|
|  applicationIds  |  array  |  Uma matriz que contém a ID da Loja do app ao qual esse complemento está associado. Essa matriz é compatível com apenas um item.   |  Sim  |
|  productId  |  cadeia de caracteres  |  A ID do produto do complemento. Este é um identificador que pode ser usado no código para fazer referência ao complemento. Para obter mais informações, consulte [Definir seu tipo de produto e a ID do produto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sim  |
|  productType  |  cadeia de caracteres  |  O tipo de produto do complemento. Há suporte para os seguintes valores: **Durável** e **Consumíveis**.  |  Sim  |

<span/>

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo complemento consumível para um app.

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
| 409  | O complemento não pôde ser criado por causa de seu estado atual ou o complemento usa um recurso de painel do Centro de Desenvolvimento é [atualmente não é compatível com a API de envio da Windows Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Obter todos os complementos](get-all-add-ons.md)
* [Obter um complemento](get-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)

