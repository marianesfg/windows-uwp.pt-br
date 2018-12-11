---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Use este método na API de envio da Microsoft Store para atualizar um envio de aplicativo existente.
title: Atualizar um envio de aplicativo
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, envio de aplicativo, atualização
ms.localizationpriority: medium
ms.openlocfilehash: b61508edf2ebc2ab155110189fe67df63e2bab30
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8826734"
---
# <a name="update-an-app-submission"></a>Atualizar um envio de aplicativo

Use este método na API de envio da Microsoft Store para atualizar um envio de aplicativo existente. Depois de atualizar com êxito um envio usando esse método, você deverá [confirmar o envio](commit-an-app-submission.md) para ingestão e publicação.

Para obter mais informações sobre como esse método se adapta ao processo de criação de um envio de app, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de aplicativo](manage-app-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio para um dos seus aplicativos. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o método [criar um envio de aplicativo](create-an-app-submission.md) .

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições do corpo da solicitação e do cabeçalho.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo para o qual você deseja atualizar um envio. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obrigatório. A ID do envio para atualizar. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de aplicativo](create-an-app-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os parâmetros a seguir.

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | string  |   Uma cadeia de caracteres que especifica a [categoria e/ou subcategoria](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) para o aplicativo. Categorias e subcategorias são combinadas em uma única cadeia de caracteres com o caractere de sublinhado '_', como **BooksAndReference_EReader**.      |  
| pricing           |  objeto  | Um objeto que contém informações de preços do aplicativo. Para saber mais, veja a seção [Preços de recurso](manage-app-submissions.md#pricing-object).       |   
| visibility           |  string  |  A visibilidade do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | O modo de publicação do envio. Ele pode ter um dos seguintes valores: <ul><li>Imediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | A data de publicação do envio em formato ISO 8601, se o *targetPublishMode* estiver definido como SpecificDate.  |  
| listings           |   objeto  |  Um dicionário de pares de chave e valor, no qual cada chave é um código de país e cada valor é um objeto [recurso de listagem](manage-app-submissions.md#listing-object) objeto que contém as informações de listagem do aplicativo.       |   
| hardwarePreferences           |  array  |   Uma matriz de cadeias de caracteres que definem as [preferências de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) do aplicativo. Ele pode ter um dos seguintes valores: <ul><li>Touch</li><li>Teclado</li><li>Mouse</li><li>Câmera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonia</li></ul>     |   
| automaticBackupEnabled           |  booliano  |   Indica se o Windows pode incluir dados do aplicativo em backups automáticos no OneDrive. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booliano  |   Indica se os clientes podem instalar o aplicativo em armazenamento removível. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booliano |   Indica se o DVR de jogos está habilitado para o aplicativo.    |   
| gamingOptions           |  object |   Uma matriz que contém um [recurso de opções de jogo](manage-app-submissions.md#gaming-options-object) que define as configurações relacionadas a jogos para o app.     |   
| hasExternalInAppProducts           |     boolean          |   Indica se o aplicativo permite que os usuários façam compras fora do sistema de comércio da Microsoft Store. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booliano           |  Indica se o aplicativo foi testado para atender às diretrizes de acessibilidade. Para obter mais informações, consulte [Declarações de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contém [observações de certificação](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) do aplicativo.    |    
| applicationPackages           |   array  | Contém objetos que fornecem detalhes sobre cada pacote no envio. Para saber mais, veja a seção [Pacote de aplicativos](manage-app-submissions.md#application-package-object). Durante a chamada desse método para atualizar um envio de aplicativo, somente os valores *fileName*, *fileStatus*, *minimumDirectXVersion* e *minimumSystemRam* desses objetos são necessários no corpo da solicitação. Os outros valores são preenchidos pelo Partner Center.   |    
| packageDeliveryOptions    | objeto  | Contém as configurações de distribuição de pacote gradual e de atualização obrigatória para o envio. Para obter mais informações, consulte [Objeto de opções de entrega de pacote](manage-app-submissions.md#package-delivery-options-object).  |
| enterpriseLicensing           |  string  |  Um dos [valores de licenciamento empresarial](manage-app-submissions.md#enterprise-licensing) que indicam o comportamento de licenciamento empresarial para o aplicativo.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booliano   |  Indica se a Microsoft tem permissão para [disponibilizar o aplicativo para as futuras famílias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | booliano   |  Indica se o aplicativo tem permissão para [segmentar famílias de dispositivos Windows 10 futuras](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).     |   
| trailers           |  matriz |   Uma matriz que contém até [recursos de trailer](manage-app-submissions.md#trailer-object) que representam trailers de vídeo para a listagem de apps.   |   


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como atualizar um envio de aplicativo.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
              "epub"
            ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
              "Free ebook reader"
            ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. O corpo da resposta contém informações sobre o envio atualizado. Para obter mais detalhes sobre os valores no corpo da resposta, veja [Recurso do envio de aplicativo](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 400  | O envio não pôde ser atualizado porque a solicitação não é válida. |
| 409  | Não foi possível atualizar o envio por causa do estado atual do aplicativo, ou o aplicativo usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obter um envio de aplicativo](get-an-app-submission.md)
* [Criar um envio de aplicativo](create-an-app-submission.md)
* [Confirmar um envio de aplicativo](commit-an-app-submission.md)
* [Excluir um envio de aplicativo](delete-an-app-submission.md)
* [Obter o status de um envio de aplicativo](get-status-for-an-app-submission.md)
