---
author: Xansky
ms.assetid: E3DF5D11-8791-4CFC-8131-4F59B928A228
description: Use este método na API de envio da Microsoft Store para obter dados para um envio de complemento existente.
title: Obter um envio de complemento
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de complemento, produto no app, IAP
ms.localizationpriority: medium
ms.openlocfilehash: e126ce53da515724f5fb1f9992ed8ca27c02ce06
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5162512"
---
# <a name="get-an-add-on-submission"></a>Obter um envio de complemento

Use este método na API de envio da Microsoft Store para obter dados para um envio de complemento (também conhecido como produto no aplicativo ou IAP) existente. Para obter mais informações sobre o processo de criação de um envio de complemento, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de complemento para um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [Criar um envio de complemento](create-an-add-on-submission.md).

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obrigatório. A ID da Loja do complemento que contém o envio que você deseja obter. A ID da Loja está disponível no painel do Centro de Desenvolvimento, e ela está incluída nos dados de resposta de solicitações para [Criar um complemento](create-an-add-on.md) ou [obter detalhes de complemento](get-all-add-ons.md).  |
| submissionId | string | Obrigatório. A ID do envio a ser obtida. Essa ID está disponível nos dados de resposta para solicitações para [criar um envio de complemento](create-an-add-on-submission.md). Para um envio criado no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de envio no painel.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter um envio de complemento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o envio especificado. Para obter mais detalhes sobre os valores no corpo da resposta, veja [recurso do envio de complemento](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },

    "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
    "friendlyName": "Submission 2"
}
```


## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio não foi encontrado. |
| 409  | O envio não pertence ao complemento especificado, ou o complemento usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Confirmar um envio de complemento](commit-an-add-on-submission.md)
* [Atualizar um envio de complemento](update-an-add-on-submission.md)
* [Excluir um envio de complemento](delete-an-add-on-submission.md)
* [Obter o status de um envio de complemento](get-status-for-an-add-on-submission.md)
