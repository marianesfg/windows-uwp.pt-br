---
description: Use esse método na API de envio da Microsoft Store a fim de obter as informações da distribuição de pacote para o envio de um pacote de pré-lançamento.
title: Obter as informações de distribuição de pacote para um envio de versão de pré-lançamento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, distribuição de pacote, envio de versão de pré-lançamento
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
ms.localizationpriority: medium
ms.openlocfilehash: 4e60ecfccecda850a5c83e5840626e1b789a068d
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8697651"
---
# <a name="get-rollout-info-for-a-flight-submission"></a>Obter as informações de distribuição de pacote para um envio de versão de pré-lançamento


Use esse método na API de envio da Microsoft Store a fim de obter as informações da [distribuição de pacote](../publish/gradual-package-rollout.md) para o envio de um pacote de pré-lançamento. Para obter mais informações sobre o processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Microsoft Store, consulte [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de pacote de pré-lançamento para um dos seus aplicativos. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o método [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md) .

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições dos parâmetros do cabeçalho e da solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento com as informações da distribuição de pacote que você deseja obter. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Necessário. A ID do pacote de pré-lançamento que contém o envio com as informações da distribuição de pacote que você deseja obter. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md). Para uma versão de pré-lançamento que foi criado no Partner Center, essa ID também está disponível na URL da página de pré-lançamento no Partner Center.    |
| submissionId | string | Necessário. A ID do envio com as informações da distribuição de pacote a ser obtida. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL da página de envio no Partner Center.   |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter as informações de distribuição de pacote para um envio do pacote de pré-lançamento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método para um envio do pacote de pré-lançamento com a distribuição de pacote gradual habilitada. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso da distribuição de pacote](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Se o envio de pacote de pré-lançamento não tiver distribuição de pacote gradual habilitada, o seguinte corpo de resposta será retornado.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio do pacote de pré-lançamento não foi encontrado. |
| 409  | O envio do pacote de pré-lançamento não pertence ao pacote de pré-lançamento especificado, ou o aplicativo usa um recurso do Partner Center que está [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Distribuição de pacote gradual](../publish/gradual-package-rollout.md)
* [Gerenciar envios de pacote de pré-lançamento usando a API de envio da Microsoft Store](manage-flight-submissions.md)
* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
