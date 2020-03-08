---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: Use esses métodos na API de envio de Microsoft Store para Gerenciar Complementos para aplicativos que estão registrados em sua conta do Partner Center.
title: Gerenciar complementos
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, complementos, produto no app, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 8e06f8e915466f116692c63df5c53c2a0f97447f
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78852365"
---
# <a name="manage-add-ons"></a>Gerenciar complementos

Use os métodos a seguir na API de envio da Microsoft Store para gerenciar complementos para os aplicativos. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Esses métodos só podem ser usados para obter, criar ou excluir os complementos. Para criar envios para complementos, consulte os métodos em [Gerenciar envios de complemento](manage-add-on-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método</th>
<th align="left">URI</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">Obter todos os Complementos para seus aplicativos</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">Obter um complemento específico</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">Criar um complemento</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">Excluir um complemento</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Se você ainda não tiver feito isso, preencha todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Microsoft Store antes de tentar usar qualquer um desses métodos.

## <a name="data-resources"></a>Recursos de dados

Os métodos da API de envio da Microsoft Store para gerenciar complementos usam os recursos de dados JSON a seguir.

<span id="add-on-object" />

### <a name="add-on-resource"></a>Recurso de complemento

Esse recurso descreve um complemento.

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

Esse recurso tem os valores a seguir.

| {1&gt;Valor&lt;1}      | Tipo   | Descrição        |
|------------|--------|--------------|
| aplicativos      | matriz  | Uma matriz que contém um [recurso de aplicativo](#application-object) que representa o aplicativo ao qual esse complemento está associado. Somente um item é compatível nessa matriz.  |
| {1&gt;id&lt;1} | string  | A ID da Loja do complemento. Esse valor é fornecido pela Loja. Uma ID da Loja de exemplo é 9NBLGGH4TNMP.  |
| productId | string  | A ID do produto do complemento. Essa é a ID que foi fornecida pelo desenvolvedor quando o complemento foi criado. Para obter mais informações, consulte [Definir seu tipo de produto e a ID do produto](https://docs.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | O tipo de produto do complemento. Há suporte para os seguintes valores: **Durável** e **Consumíveis**.  |
| lastPublishedInAppProductSubmission       | objeto | Um [recurso de envio](#submission-object) que fornece informações sobre o envio publicado mais recentemente para o complemento.         |
| pendingInAppProductSubmission        | objeto  |  Um [recurso de envio](#submission-object) que fornece informações sobre o envio pendente atual para o complemento.  |   |

<span id="application-object" />

### <a name="application-resource"></a>Recurso de aplicativo

Esse recurso descreve o aplicativo ao qual um complemento está associado. O exemplo a seguir demonstra o formato desse recurso.

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

Esse recurso tem os valores a seguir.

| {1&gt;Valor&lt;1}           | Tipo    | Descrição        |
|-----------------|---------|-----------|
| {1&gt;Valor&lt;1}            | objeto  |  Um objeto que contém os seguintes valores: <br/><br/> <ul><li>*id*. A ID da Store do app. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do aplicativo.</li></ul>   |
| totalCount   | int  | O número de objetos de aplicativo na matriz *applications* do corpo da resposta.                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>Recurso de envio

Esse recurso fornece informações sobre um envio para um complemento. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Esse recurso tem os valores a seguir.

| {1&gt;Valor&lt;1}           | Tipo    | Descrição     |
|-----------------|---------|------------------|
| {1&gt;id&lt;1}            | string  | A ID do envio.    |
| resourceLocation   | string  | Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do envio.     |
 
<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços Microsoft Stores](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento usando a API de envio de Microsoft Store](manage-add-on-submissions.md)
* [Obter todos os Complementos](get-all-add-ons.md)
* [Obter um complemento](get-an-add-on.md)
* [Criar um complemento](create-an-add-on.md)
* [Excluir um complemento](delete-an-add-on.md)
