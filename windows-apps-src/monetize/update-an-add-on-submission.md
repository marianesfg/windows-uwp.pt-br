---
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: Use este método na API de envio da Microsoft Store para atualizar um envio de complemento existente.
title: Atualizar um envio de complemento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de complemento, atualizar, produto no app, IAP
ms.localizationpriority: medium
ms.openlocfilehash: fd0bb8df9b9fc36216da72e4ad01ebd2e650ad1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620611"
---
# <a name="update-an-add-on-submission"></a>Atualizar um envio de complemento


Use este método na API de envio da Microsoft Store para atualizar um envio existente do complemento (também conhecido como produto no app ou IAP). Depois de atualizar com êxito um envio usando esse método, você deverá [confirmar o envio](commit-an-add-on-submission.md) para ingestão e publicação.

Para obter mais informações sobre como esse método se adapta ao processo de criação de um envio de complemento, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de complemento](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Crie um envio de complemento de um dos seus aplicativos. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o [criar um envio de complemento](create-an-add-on-submission.md) método.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadeia de caracteres | Obrigatório. A ID da Loja do complemento para o qual você deseja atualizar um envio. A ID de Store está disponível no Partner Center e ele é incluído nos dados de resposta para solicitações para [criar um complemento](create-an-add-on.md) ou [obter detalhes de complemento](get-all-add-ons.md).  |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio para atualizar. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de complemento](create-an-add-on-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL para a página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | cadeia de caracteres  |  O [tipo de conteúdo](../publish/enter-add-on-properties.md#content-type) fornecido no complemento. Isso pode ter um dos seguintes valores: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | matriz  | Uma matriz de cadeias de caracteres que contenham até 10 [palavras-chave](../publish/enter-add-on-properties.md#keywords) do complemento. O aplicativo pode consultar complementos usando essas palavras-chave.   |
| lifetime           | cadeia de caracteres  |  O tempo de vida do complemento. Isso pode ter um dos seguintes valores: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | objeto  | Um objeto que contém as informações de listagem do complemento. Para obter mais informações, consulte [Recurso de listagem](manage-add-on-submissions.md#listing-object).  |
| pricing           | objeto  | Um objeto que contém as informações de preços do complemento. Para obter mais informações, consulte [Recurso de preços](manage-add-on-submissions.md#pricing-object).  |
| targetPublishMode           | cadeia de caracteres  | O modo de publicação do envio. Isso pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | cadeia de caracteres  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |
| tag           | cadeia de caracteres  |  Os [dados de desenvolvedor personalizados](../publish/enter-add-on-properties.md#custom-developer-data) do complemento (essas informações foram anteriormente chamadas de *marca*).   |
| visibilidade  | cadeia de caracteres  |  A visibilidade do complemento. Isso pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como atualizar um envio de complemento.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
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
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
}
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o envio atualizado. Para obter mais detalhes sobre os valores no corpo da resposta, veja [Recurso do envio de complemento](manage-add-on-submissions.md#add-on-submission-object).

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
    "priceId": "Free"
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
| 400  | O envio não pôde ser atualizado porque a solicitação não é válida. |
| 409  | O envio de mensagens não pôde ser atualizado devido ao estado atual do complemento, ou o complemento usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Obtenha um envio de complemento](get-an-add-on-submission.md)
* [Criar um envio de complemento](create-an-add-on-submission.md)
* [Confirmar envio um complemento](commit-an-add-on-submission.md)
* [Excluir um envio de complemento](delete-an-add-on-submission.md)
* [Obter o status de envio de um complemento](get-status-for-an-add-on-submission.md)
