---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "Use estes métodos na API de envio da Windows Store para recuperar dados dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: Obter dados de aplicativo usando a API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# Obter dados de aplicativo usando a API de envio da Windows Store

Use os métodos a seguir na API de envio da Windows Store para obter dados dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows. Para ver uma introdução da API de envio da Windows Store, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada. Esses métodos só podem ser usados para obter dados de aplicativos. Para criar ou gerenciar envios de aplicativos, consulte os métodos em [Gerenciar envios de aplicativo](manage-app-submissions.md).

| Método        | URI    | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | Recupera os dados de todos os aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows. Para saber mais, veja [Obter todos os aplicativos](get-all-apps.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | Recupera dados sobre um aplicativo específico que está registrado na sua conta do Centro de Desenvolvimento do Windows. Para saber mais, veja [Obter um aplicativo](get-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | Lista os complementos (também conhecidos como produtos no aplicativo ou IAPs) de um aplicativo que está registrado em sua conta do Centro de Desenvolvimento do Windows. Para saber mais, veja [Obter complementos de um aplicativo](get-add-ons-for-an-app.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | Lista os pacotes de pré-lançamento de um aplicativo que está registrado em sua conta do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Obter pacotes de pré-lançamento de um aplicativo](get-flights-for-an-app.md). |

<span/>

## Pré-requisitos

Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Windows Store antes de tentar usar qualquer um desses métodos.

## Recursos

Esses métodos usam os recursos a seguir para formatar dados.

<span id="application_object" />
### Aplicativo

Esse recurso representa um aplicativo que está registrado em sua conta. O exemplo a seguir demonstra o formato desse recurso.

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
  }
}
```

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | A ID da Loja do aplicativo. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | O nome principal do aplicativo.                                                                                                                                                   |
| packageFamilyName | string  | O nome da família de pacotes do aplicativo.                                                                                                                                                                                                         |
| packageIdentityName          | string  | O nome da identidade do pacote do aplicativo.                                                                                                                                                              |
| publisherName       | string  | A ID de fornecedor do Windows que está associada ao aplicativo. Isso corresponde ao valor **Package/Identity/Publisher** que aparece na página [Identidade do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) do aplicativo no painel do Centro de Desenvolvimento do Windows.                                                                                             |
| firstPublishedDate      | string  | A data em que o aplicativo foi publicado pela primeira vez, no formato ISO 8601.                                                                                         |
| lastPublishedApplicationSubmission       | object | Um objeto que fornece informações sobre o último envio publicado do aplicativo. Para saber mais, consulte a seção [Envio](#submission_object) a seguir.                                                                                                                                                          |
| pendingApplicationSubmission        | object  |  Um objeto que fornece informações sobre o envio pendente atual do aplicativo. Para saber mais, consulte a seção [Envio](#submission_object) a seguir.  |   |


<span id="add-on-object" />
### Complemento

Esse recurso fornece informações sobre um complemento. O exemplo a seguir demonstra o formato desse recurso.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | string  | A ID da Loja do complemento. Esse valor é fornecido pela Loja. Uma ID da Loja de exemplo é 9NBLGGH4TNMP.   |


<span id="flight-object" />
### Versão de pré-lançamento

Esse recurso fornece informações sobre um pacote de pré-lançamento de um aplicativo. O exemplo a seguir demonstra o formato desse recurso.

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

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | A ID do pacote de pré-lançamento. Esse valor é fornecido pelo Centro de Desenvolvimento.  |
| friendlyName           | string  | O nome do pacote de pré-lançamento, conforme especificado pelo desenvolvedor.   |
| lastPublishedFlightSubmission       | object | Um objeto que fornece informações sobre o último envio publicado do pacote de pré-lançamento. Para saber mais, consulte a seção [Envio](#submission_object) a seguir.  |
| pendingFlightSubmission        | object  |  Um objeto que fornece informações sobre o envio pendente atual do pacote de pré-lançamento. Para saber mais, consulte a seção [Envio](#submission_object) a seguir.  |    
| groupIds           | array  | Uma matriz de cadeias de caracteres que contêm as IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | O nome amigável do pacote de pré-lançamento que ficou imediatamente abaixo do pacote de pré-lançamento atual. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />
### Envio

Esse recurso fornece informações sobre um envio. O exemplo a seguir demonstra o formato desse recurso.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
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
* [Gerenciar envios de aplicativo usando a API de envio da Windows Store](manage-app-submissions.md)
* [Obter todos os aplicativos](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter complementos para um aplicativo](get-add-ons-for-an-app.md)
* [Obter pacotes de pré-lançamento de um aplicativo](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


