---
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Use este método na API de análises da Microsoft Store para determinar se você pode responder a uma análise específica, ou se você pode responder a qualquer revisão para um determinado aplicativo.
title: Obter informações de resposta para análises
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análises da Microsoft Store, informações de resposta
ms.localizationpriority: medium
ms.openlocfilehash: 0497b5eec67f9204139cd10d4523b534d6c8779f
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8756550"
---
# <a name="get-response-info-for-reviews"></a>Obter informações de resposta para análises

Se você quiser responder de forma programática a uma análise feita pelo cliente ao seu aplicativo, você pode usar este método na API de análises da Microsoft Store para determinar primeiro se você tem permissão para responder à análise. Você não poderá responder às análises enviadas por clientes que optaram por não receber respostas a análises. Depois de confirmar que você pode responder à análise, você pode usar o método [enviar respostas às análises do aplicativo](submit-responses-to-app-reviews.md) para responder de forma programática à ela.


## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](respond-to-reviews-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID da análise que você deseja verificar para determinar se você pode responder a ela. As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   | Descrição                                     |  Obrigatório  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | cadeia | A ID da Loja do aplicativo que contém a análise para a qual você deseja determinar se pode responder. A ID da loja está disponível na [página de identidade de aplicativo](../publish/view-app-identity-details.md) no Partner Center. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8. |  Sim  |
| reviewId | cadeia | A ID da revisão que você deseja responder (este é um GUID). As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md). <br/>Se você omitir esse parâmetro, o corpo da resposta para esse método indicará se você tem permissões para responder a quaisquer análises para o aplicativo especificado. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir mostram como usar esse método para determinar se você pode responder a uma determinada análise.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição    |  
|------------|--------|-----------------------|
| CanRespond      | Booliano  | O valor **true** indica que você pode responder à análise especificado, ou que você tem permissões para responder a qualquer análise do aplicativo especificado. Caso contrário, esse valor é **false**.       |
| DefaultSupportEmail  | cadeia |  O [endereço de email de suporte](../publish/enter-app-properties.md#support-contact-info) do seu aplicativo, conforme especificado na listagem da Loja do seu aplicativo. Se você não especificar um endereço de email de suporte, esse campo ficará vazio.    |

 
### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar respostas às análises usando a API de análise da Microsoft Store](submit-responses-to-app-reviews.md)
* [Responder às críticas dos clientes usando o Partner Center](../publish/respond-to-customer-reviews.md)
* [Responder às críticas usando serviços da Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obter avaliações de aplicativo](get-app-reviews.md)
