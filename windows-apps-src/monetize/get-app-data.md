---
author: Xansky
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Use estes métodos na API de envio da Microsoft Store para recuperar dados dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows.
title: Obter dados de app
ms.author: mhopkins
ms.date: 02/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, dados do aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 6940c1079c7973bc4fd639345c5d5e3f33b0221f
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5164428"
---
# <a name="get-app-data"></a>Obter dados de app

Use os métodos a seguir na API de envio da Microsoft Store para obter dados para os aplicativos existentes em sua conta do Centro de Desenvolvimento. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

Antes de usar esses métodos, o aplicativo já deve existir na sua conta do Centro de Desenvolvimento. Para criar ou gerenciar envios de aplicativos, consulte os métodos em [Gerenciar envios de aplicativo](manage-app-submissions.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications</td>
<td align="left"><a href="get-all-apps.md">Obter dados para todos os seus apps</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}</td>
<td align="left"><a href="get-an-app.md">Obter dados de um app específico</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts</td>
<td align="left"><a href="get-add-ons-for-an-app.md">Obter complementos para um app</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights</td>
<td align="left"><a href="get-flights-for-an-app.md">Obter pacotes de pré-lançamento de um app</a></td>
</tr>
</tbody>
</table>

<span/>

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

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|---------------------|
| id            | string  | A ID da Loja do aplicativo. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | O nome principal do aplicativo.      |
| packageFamilyName | string  | O nome da família de pacotes do aplicativo.      |
| packageIdentityName          | string  | O nome da identidade do pacote do aplicativo.                       |
| publisherName       | string  | A ID de fornecedor do Windows que está associada ao aplicativo. Isso corresponde ao valor **Package/Identity/Publisher** que aparece na página [Identidade do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) do aplicativo no painel do Centro de Desenvolvimento do Windows.       |
| firstPublishedDate      | string  | A data em que o app foi publicado pela primeira vez, no formato ISO 8601.   |
| lastPublishedApplicationSubmission       | object | Um [recurso de envio](#submission_object) que fornece informações sobre o último envio publicado do app.    |
| pendingApplicationSubmission        | object  |  Um [recurso de envio](#submission_object) que fornece informações sobre o envio atual pendente do app.   |   
| hasAdvancedListingPermission        | boolean  |  Indica se você pode configurar [gamingOptions](manage-app-submissions.md#gaming-options-object) ou [trailers](manage-app-submissions.md#trailer-object) para envios para o app. Este valor é verdadeiro para envios criados depois de maio de 2017. |  |


<span id="add-on-object" />

### <a name="add-on-resouce"></a>Recurso de complemento

Esse recurso fornece informações sobre um complemento.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | A ID da Loja do complemento. Esse valor é fornecido pela Loja. Uma ID da Loja de exemplo é 9NBLGGH4TNMP.   |


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

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição           |
|-----------------|---------|------------------------|
| flightId            | string  | A ID do pacote de pré-lançamento. Esse valor é fornecido pelo Centro de Desenvolvimento.  |
| friendlyName           | string  | O nome do pacote de pré-lançamento, conforme especificado pelo desenvolvedor.   |
| lastPublishedFlightSubmission       | object | Um [recurso de envio](#submission_object) que fornece informações sobre o último envio publicado do pacote de pré-lançamento.   |
| pendingFlightSubmission        | object  |  Um [recurso de envio](#submission_object) que fornece informações sobre o envio atual pendente do pacote de pré-lançamento.  |    
| groupIds           | array  | Uma matriz de cadeias de caracteres que contêm as IDs dos grupos de versão de pré-lançamento que estão associados ao pacote de pré-lançamento. Para saber mais sobre grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | O nome amigável do pacote de pré-lançamento que ficou imediatamente abaixo do pacote de pré-lançamento atual. Para saber mais sobre a classificação de grupos de versão de pré-lançamento, consulte [Pacotes de pré-lançamento](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |


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

Este recurso tem os seguintes valores.

| Valor           | Tipo    | Descrição                 |
|-----------------|---------|------------------------------|
| id            | string  | A ID do envio.    |
| resourceLocation   | string  | Um caminho relativo que você pode acrescentar ao URI básico da solicitação ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar os dados completos do envio.            |
 
<span/>

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de app usando a API de envio da Microsoft Store](manage-app-submissions.md)
* [Obter todos os aplicativos](get-all-apps.md)
* [Obter um aplicativo](get-an-app.md)
* [Obter complementos para um app](get-add-ons-for-an-app.md)
* [Obter pacotes de pré-lançamento de um app](get-flights-for-an-app.md)
