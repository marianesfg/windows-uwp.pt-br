---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Use esses métodos na API de envio a Microsoft Store para recuperar dados para aplicativos que são registrados em sua conta no Partner Center.
title: Obter dados de app
ms.date: 02/28/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, dados do aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 23e392e2064a2a48089d1efadd1461c146e0d343
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598891"
---
# <a name="get-app-data"></a>Obter dados de app

Use os seguintes métodos na API de envio a Microsoft Store para obter dados para aplicativos existentes em sua conta no Partner Center. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Antes de usar esses métodos, o aplicativo já deve existir na sua conta no Partner Center. Para criar ou gerenciar envios de apps, consulte os métodos em [Gerenciar envios de aplicativo](manage-app-submissions.md).

| Método | URI                                                                                             | Descrição                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [Obter dados para todos os seus aplicativos](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [Obter dados para um aplicativo específico](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [Obter voos de pacote para um aplicativo](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver feito isso, preencha todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Microsoft Store antes de tentar usar qualquer um desses métodos.

## <a name="data-resources"></a>Recursos de dados

Os métodos da API de envio da Microsoft Store para obter dados de app usam os recursos de dados JSON a seguir.

<span id="application_object" />

### <a name="application-resource"></a>Recurso de aplicativo

Esse recurso representa um app que está registrado em sua conta.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": true
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|---------------------|
| id            | cadeia de caracteres  | A ID da Loja do aplicativo. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | cadeia de caracteres  | O nome principal do aplicativo.      |
| packageFamilyName | cadeia de caracteres  | O nome da família de pacotes do aplicativo.      |
| packageIdentityName          | cadeia de caracteres  | O nome da identidade do pacote do aplicativo.                       |
| publisherName       | cadeia de caracteres  | A ID de fornecedor do Windows que está associada ao aplicativo. Isso corresponde à **identidade/pacote/Publisher** valor que aparece na [identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) página para o aplicativo no Partner Center.       |
| firstPublishedDate      | cadeia de caracteres  | A data em que o app foi publicado pela primeira vez, no formato ISO 8601.   |
| lastPublishedApplicationSubmission       | objeto | Um [recurso de envio](#submission_object) que fornece informações sobre o último envio publicado do app.    |
| pendingApplicationSubmission        | objeto  |  Um [recurso de envio](#submission_object) que fornece informações sobre o envio atual pendente do app.   |   
| hasAdvancedListingPermission        | booliano  |  Indica se você pode configurar [gamingOptions](manage-app-submissions.md#gaming-options-object) ou [trailers](manage-app-submissions.md#trailer-object) para envios para o app. Este valor é verdadeiro para envios criados depois de maio de 2017. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>Recurso de complemento

Esse recurso fornece informações sobre um complemento.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição         |
|-----------------|---------|----------------------|
| inAppProductId            | cadeia de caracteres  | A ID da Loja do complemento. Esse valor é fornecido pela Loja. Uma ID da Loja de exemplo é 9NBLGGH4TNMP.   |


<span id="flight-object" />

### <a name="flight-resource"></a>Recurso de versão de pré-lançamento

Esse recurso fornece informações sobre um pacote de pré-lançamento de um app.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Esse recurso tem os valores a seguir.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------------------------|
| flightId            | cadeia de caracteres  | A ID do pacote de pré-lançamento. Esse valor é fornecido pelo Centro de parceiros.  |
| friendlyName           | cadeia de caracteres  | O nome do pacote de pré-lançamento, conforme especificado pelo desenvolvedor.   |
| lastPublishedFlightSubmission       | objeto | Um [recurso de envio](#submission_object) que fornece informações sobre o último envio publicado do pacote de pré-lançamento.   |
| pendingFlightSubmission        | objeto  |  Um [recurso de envio](#submission_object) que fornece informações sobre o envio atual pendente do pacote de pré-lançamento.  |    
| groupIds           | matriz  | Uma matriz de cadeias de caracteres que contêm as IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadeia de caracteres  | O nome amigável do pacote de pré-lançamento que ficou imediatamente abaixo do pacote de pré-lançamento atual. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />

### <a name="submission-resource"></a>Recurso de envio

Esse recurso fornece informações sobre um envio. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Esse recurso tem os valores a seguir.

| Valor              | Tipo   | Descrição               |
|--------------------|--------|---------------------------|
| id                 | cadeia de caracteres | A ID do envio. |
| resourceLocation   | cadeia de caracteres | Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do envio. |

 
## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de aplicativo usando a API de envio da Microsoft Store](manage-app-submissions.md)
* [Obter todos os aplicativos](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)
* [Obter voos de pacote para um aplicativo](get-flights-for-an-app.md)
