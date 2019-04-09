---
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Use esse método na API de envio a Microsoft Store para criar um novo envio de complemento para um aplicativo que está registrado para o Partner Center.
title: Criar um envio de complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, criar envio de complemento, produto no app, IAP
ms.localizationpriority: medium
ms.openlocfilehash: cbb093576badf5cd84b132cfb139db9da7d31991
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334922"
---
# <a name="create-an-add-on-submission"></a>Criar um envio de complemento

Use esse método na API de envio a Microsoft Store para criar um novo envio de complemento (também conhecido como no aplicativo produto ou IAP) para um aplicativo que é registrado em sua conta no Partner Center. Depois de criar um novo envio com êxito usando esse método, [atualize o envio](update-an-add-on-submission.md) para fazer as alterações necessárias para os dados de envio e depois [confirme o envio](commit-an-add-on-submission.md) para inclusão e publicação.

Para obter mais informações sobre como esse método se adapta ao processo de criação de um envio de complemento, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de complemento](manage-add-on-submissions.md).

> [!NOTE]
> Este método cria um envio para um complemento existente. Para criar um complemento, use o método [Criar um complemento](create-an-add-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Crie um complemento para um dos seus aplicativos. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o [criar um complemento](create-an-add-on.md) método.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions` |

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |

### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadeia de caracteres | Obrigatório. A ID da Loja do complemento para o qual você deseja criar um envio. A ID de Store está disponível no Partner Center e ele é incluído nos dados de resposta para solicitações para [criar um complemento](create-an-add-on.md) ou [obter detalhes de complemento](get-all-add-ons.md).  |

### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo envio para um complemento.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o novo envio. Para obter mais detalhes sobre os valores no corpo da resposta, veja [recurso do envio de complemento](manage-add-on-submissions.md#add-on-submission-object).

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
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
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
| 400  | O envio não pôde ser criado porque a solicitação não é válida. |
| 409  | O envio de mensagens não pôde ser criado devido ao estado atual do aplicativo ou o aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Obtenha um envio de complemento](get-an-add-on-submission.md)
* [Confirmar envio um complemento](commit-an-add-on-submission.md)
* [Atualizar uma apresentação de complemento](update-an-add-on-submission.md)
* [Excluir um envio de complemento](delete-an-add-on-submission.md)
* [Obter o status de envio de um complemento](get-status-for-an-add-on-submission.md)
