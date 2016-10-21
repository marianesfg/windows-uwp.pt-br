---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Use estes métodos na API de envio da Windows Store para gerenciar complementos dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: Gerenciar complementos usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# Gerenciar complementos usando a API de envio da Windows Store




Use os métodos a seguir na API de envio da Windows Store para gerenciar complementos (também conhecidos como produtos no aplicativo ou IAPs) de aplicativos que estão registrados na sua conta do Centro de Desenvolvimento do Windows. Para ver uma introdução da API de envio da Windows Store, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada. Esses métodos só podem ser usados para obter, criar ou excluir os complementos. Para criar envios para complementos, veja os métodos em [Gerenciar envios de complemento](manage-add-on-submissions.md).

| Método        | URI    | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Obtém dados para todos os complementos de todos os aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Obter todos os complementos](get-all-add-ons.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | Obtém dados de um complemento específico. Para saber mais, veja [Obter um complemento](get-an-add-on.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Cria um novo complemento. Para saber mais, veja [Criar um complemento](create-an-add-on.md).  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | Exclui um complemento. Para obter mais informações, consulte [Excluir um complemento](delete-an-add-on.md). |

## Pré-requisitos

Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Windows Store antes de tentar usar qualquer um desses métodos.

## Recursos

Esses métodos usam os recursos a seguir para formatar dados.

<span id="add-on-object" />
### Complemento

Esse recurso representa um complemento. O exemplo a seguir demonstra o formato desse recurso.

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
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

Este recurso tem os seguintes valores.

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | array  | Uma matriz que contém um objeto que representa o aplicativo ao qual esse complemento está associado. Para obter mais informações sobre os dados nesse objeto, consulte a seção [Objeto de aplicativo](#application-object) a seguir. Essa matriz é compatível com apenas um item.  |
| id | string  | A ID da Loja do complemento. Esse valor é fornecido pela Loja. Uma ID da Loja de exemplo é 9NBLGGH4TNMP.  |
| productId | string  | A ID do produto do complemento. Essa é a ID que foi fornecida pelo desenvolvedor quando o complemento foi criado. Para obter mais informações, consulte [Definir seu tipo de produto e a ID do produto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | O tipo de produto do complemento. Há suporte para os seguintes valores: **Durável** e **Consumíveis**.  |
| lastPublishedInAppProductSubmission       | object | Um objeto que fornece informações sobre o último envio publicado do complemento. Para saber mais, consulte a seção [Envio](#submission-object) a seguir.                                                                                                                                                          |
| pendingInAppProductSubmission        | object  |  Um objeto que fornece informações sobre o envio pendente atual do complemento. Para saber mais, consulte a seção [Envio](#submission-object) a seguir.  |   |

<span id="application-object" />
### Aplicativo

Esse recurso representa um aplicativo ao qual um complemento está associado. O exemplo a seguir demonstra o formato desse recurso.

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
}
```

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| value            | object  |  Um objeto que contém os seguintes valores: <br/><br/> <ul><li>*id*. A ID da Loja do aplicativo. Para saber mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do aplicativo.</li></ul>   |
| totalCount   | int  | O número de objetos de aplicativo na matriz *applications* do corpo da resposta.                                                                                                                                                 |

<span id="submission-object" />
### Envio

Esse recurso fornece informações sobre um envio de um complemento. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | A ID do envio.    |
| resourceLocation   | string  | Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do envio.                                                                                                                                               |
 
<span/>

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento usando a API de envio da Windows Store](manage-add-on-submissions.md)
* [Obter todos os complementos](get-all-add-ons.md)
* [Obter um complemento](get-an-add-on.md)
* [Criar um complemento](create-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


