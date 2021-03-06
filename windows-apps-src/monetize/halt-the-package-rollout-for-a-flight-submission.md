---
description: Use esse método na API de envio da Microsoft Store a fim de parar uma distribuição de pacote para um pacote de pré-lançamento.
title: Interromper a distribuição da versão de pré-lançamento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, distribuição de pacote, envio de versão de pré-lançamento, interromper
ms.assetid: f8ee0687-a421-48e7-a6eb-3fd5633c352b
ms.localizationpriority: medium
ms.openlocfilehash: 43b721a608996b904d6c68b7db350490d77755dc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372701"
---
# <a name="halt-the-rollout-for-a-flight"></a>Interromper a distribuição da versão de pré-lançamento

Use esse método na API de envio da Microsoft Store a fim de [parar a distribuição](../publish/gradual-package-rollout.md#completing-the-rollout) para o envio de um pacote de pré-lançamento. Para obter mais informações sobre o processo de criação de um envio de pacote de pré-lançamento usando a API de envio da Microsoft Store, consulte [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

> [!NOTE]
> Se você interromper a distribuição de um envio de versão de pré-lançamento do pacote e depois [criar um novo envio de versão de pré-lançamento do pacote](create-a-flight-submission.md), o novo envio será um clone do envio interrompido.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para a API de envio da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Crie um envio para um dos seus aplicativos. Você pode fazer isso no Partner Center, ou você pode fazer isso usando o [criar um envio de aplicativo](create-an-app-submission.md) método.
* Habilite uma distribuição de pacote gradual para o envio. Você pode fazer isso [no Partner Center](../publish/gradual-package-rollout.md), ou você pode fazer isso [usando a API de envio da Microsoft Store](manage-flight-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Solicitação

Esse método tem a seguinte sintaxe. Veja as seções a seguir para obter exemplos de uso e descrições dos parâmetros do cabeçalho e da solicitação.

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POSTAR   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Nome        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadeia de caracteres | Obrigatório. A ID da Loja do aplicativo que contém o envio do pacote de pré-lançamento com a porcentagem da distribuição de pacote que você deseja parar. Para obter mais informações sobre a ID da Loja, consulte [Exibir detalhes de identidade do aplicativo](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadeia de caracteres | Obrigatório. A ID do pacote de pré-lançamento que contém o envio com a porcentagem da distribuição de pacote que você deseja parar. Essa ID está disponível nos dados de resposta de solicitações para [criar um pacote de pré-lançamento](create-a-flight.md) e [obter pacotes de pré-lançamento para um aplicativo](get-flights-for-an-app.md). Para um voo que foi criado no Partner Center, essa ID também está disponível na URL para a página de voo no Partner Center.   |
| submissionId | cadeia de caracteres | Obrigatório. A ID do envio com a distribuição de pacote a ser parada. Esse ID está disponível nos dados de resposta para solicitações para [criar um envio de pacote de pré-lançamento](create-a-flight-submission.md). Para um envio que foi criado no Partner Center, essa ID também está disponível na URL para a página de envio no Partner Center.  |


### <a name="request-body"></a>Corpo da solicitação

Não forneça um corpo da solicitação para esse método.

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como parar a distribuição de pacote para um envio do pacote de pré-lançamento.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O exemplo a seguir demonstra o corpo da resposta JSON para uma chamada bem-sucedida para esse método. Para obter mais detalhes sobre os valores no corpo da resposta, consulte [Recurso da distribuição de pacote](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Códigos de erro

Se não for possível concluir a solicitação, a resposta conterá um dos seguintes códigos de erro HTTP.

| Código de erro |  Descrição   |
|--------|------------------|
| 404  | O envio do pacote de pré-lançamento não foi encontrado. |
| 409  | Este código indica um dos seguintes erros:<br/><br/><ul><li>O envio não está em um estado válido para a operação de distribuição gradual (antes de chamar esse método, o envio deve ser publicado, e o valor [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) deve ser definido como **PackageRolloutInProgress**).</li><li>O envio não pertence ao aplicativo especificado.</li><li>O aplicativo usa um recurso do Partner Center que está [atualmente não tem suporte da API de envio a Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Tópicos relacionados

* [Distribuição gradual do pacote](../publish/gradual-package-rollout.md)
* [Gerenciar envios de voo de pacote usando a API de envio da Microsoft Store](manage-flight-submissions.md)
* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
