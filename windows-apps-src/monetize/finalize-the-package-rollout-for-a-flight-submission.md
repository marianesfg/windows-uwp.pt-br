---
author: mcleanbyron
description: Use esse método na API de envio da Microsoft Store a fim de finalizar a distribuição de pacote para o envio de um pacote de pré-lançamento.
title: Finalizar a distribuição para um envio de versão de pré-lançamento
ms.author: mcleans
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, distribuição de pacote, envio de versão de pré-lançamento, finalizar
ms.assetid: e4a645f6-1f00-4af5-80d6-d2ee179acc8a
ms.localizationpriority: medium
ms.openlocfilehash: 4fe00d07773d52c5a80161ec8c9620c12f5b4b0e
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815801"
---
# <a name="finalize-the-rollout-for-a-flight-submission"></a>Finalizar a distribuição para um envio de versão de pré-lançamento


Use esse método na API de envio da Microsoft Store a fim de [finalizar a distribuição de pacote](../publish/gradual-package-rollout.md#completing-the-rollout) para o envio de um pacote de pré-lançamento. Para obter mais informações sobre o processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Microsoft Store, consulte [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).


## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.
* Crie um envio de aplicativo para um aplicativo em sua conta do Centro de Desenvolvimento. Você pode fazer isso no painel do Centro de Desenvolvimento ou usando o método [criar um envio de aplicativo](create-an-app-submission.md).
* Habilite uma distribuição de pacote gradual para o envio. Você pode fazer isso no [painel do Centro de Desenvolvimento](../publish/gradual-package-rollout.md) ou [usando a API de envio da Microsoft Store](manage-flight-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições dos parâmetros do cabeçalho e da solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento com a distribuição de pacote que você deseja finalizar. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade de aplicativo](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Necessário. A ID do pacote de pré-lançamento que contém o envio com a distribuição de pacote que você deseja finalizar. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md). Para uma versão de pré-lançamento criada no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de pré-lançamento no painel. |
| submissionId | string | Necessário. A ID do envio com a distribuição de pacote que você deseja finalizar. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). Para um envio criado no painel do Centro de Desenvolvimento, essa ID também está disponível na URL da página de envio no painel.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como finalizar a distribuição de pacote para um envio do pacote de pré-lançamento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso da distribuição de pacote](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100.0,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio do pacote de pré-lançamento não foi encontrado. |
| 409  | Este código indica um dos seguintes erros:<br/><br/><ul><li>O envio não está em um estado válido para a operação de distribuição gradual (antes de chamar esse método, o envio deve ser publicado, e o valor [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) deve ser definido como **PackageRolloutInProgress**).</li><li>O envio não pertence ao aplicativo especificado.</li><li>O aplicativo usa um recurso de painel do Centro de Desenvolvimento que [atualmente não é compatível com a API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Tópicos relacionados

* [Distribuição de pacote gradual](../publish/gradual-package-rollout.md)
* [Gerenciar envios de pacote de pré-lançamento usando a API de envio da Microsoft Store](manage-flight-submissions.md)
* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)