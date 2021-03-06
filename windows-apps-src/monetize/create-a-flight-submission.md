---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Use esse método na API de envio a Microsoft Store para criar um novo voo o envio de pacote para um aplicativo que é registrado em sua conta no Partner Center.
title: Criar um envio de pacote de pré-lançamento
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, criar envio de versão de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: b6474c566795043a435e70b2b41d4f5b59280c15
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371987"
---
# <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

Use este método na API de envio da Microsoft Store para criar um novo envio para um pacote de pré-lançamento para um app. Depois de criar um novo envio com êxito usando esse método, [atualize o envio](update-a-flight-submission.md) para fazer as alterações necessárias para os dados de envio e depois [confirme o envio](commit-a-flight-submission.md) para inclusão e publicação.

Para obter mais informações sobre como esse método se encaixa no processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Microsoft Store, consulte [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md).

> [!NOTE]
> Este método cria um envio para um pacote de pré-lançamento existente. Para criar um pacote de pré-lançamento, use o método [criar um pacote de pré-lançamento](create-a-flight.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Crie um voo de pacote para um aplicativo. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o [criar um voo de pacote](create-a-flight.md) método.

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo para o qual você deseja criar um envio do pacote de pré-lançamento. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadeia de caracteres | Obrigatório. A ID do pacote de pré-lançamento para o qual você deseja adicionar o envio. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como criar um novo envio de pacote de pré-lançamento para um aplicativo que tem a ID da Loja 9WZDNCRD91MD.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o novo envio. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso de envio de pacote de pré-lançamento](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | Não foi possível criar o envio do pacote de pré-lançamento porque a solicitação é inválida. |
| 409  | O envio de voo do pacote não pôde ser criado devido ao estado atual do aplicativo ou o aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de voo do pacote](manage-flight-submissions.md)
* [Obter o envio de voo de um pacote](get-a-flight-submission.md)
* [O envio de voo de um pacote de confirmação](commit-a-flight-submission.md)
* [O envio de voo de um pacote de atualização](update-a-flight-submission.md)
* [Excluir um envio de voo do pacote](delete-a-flight-submission.md)
* [Obter o status de envio de voo de um pacote](get-status-for-a-flight-submission.md)
