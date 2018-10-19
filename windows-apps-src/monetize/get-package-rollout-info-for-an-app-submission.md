---
author: Xansky
description: Use esse método na API de envio da Microsoft Store a fim de obter informações da distribuição de pacote para um envio de aplicativo.
title: Obter informações de distribuição para um envio de aplicativo
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, distribuição de pacote, envio do aplicativo
ms.assetid: 9ada5ac3-a86e-4bb6-8ebc-915ba9649e3c
ms.localizationpriority: medium
ms.openlocfilehash: ead13a255eb707df2e60907265672d53aab120d9
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4955838"
---
# <a name="get-rollout-info-for-an-app-submission"></a>Obter informações de distribuição para um envio de aplicativo


Use esse método na API de envio da Microsoft Store a fim de obter as informações da [distribuição de pacote](../publish/gradual-package-rollout.md) para o envio de um pacote de pré-lançamento. Para obter mais informações sobre o processo de criação de um envio de aplicativo, usando a API de envio da Microsoft Store, consulte [Gerenciar envios de aplicativo](manage-app-submissions.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de aplicativo para um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [criar um envio de aplicativo](create-an-app-submission.md).

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições dos parâmetros do cabeçalho e da solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout   ``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necessário. A ID da Loja do aplicativo que contém o envio com as informações da distribuição de pacote que você deseja obter. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Necessário. A ID do envio com as informações da distribuição de pacote que você deseja obter. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de aplicativo](create-an-app-submission.md). Para um envio criado no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de envio no painel.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter as informações da distribuição de pacote para um envio de aplicativo.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método para um envio de aplicativo com a distribuição de pacote gradual habilitada. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso da distribuição de pacote](manage-app-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Se o envio de aplicativo não tiver distribuição de pacote gradual habilitada, o seguinte corpo de resposta será retornado.

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
| 404  | O envio não foi encontrado. |
| 409  | O envio não pertence ao aplicativo especificado, ou o aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Tópicos relacionados

* [Distribuição de pacote gradual](../publish/gradual-package-rollout.md)
* [Gerenciar envios de app usando a API de envio da Microsoft Store](manage-app-submissions.md)
* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
