---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Use este método na API de análises da Microsoft Store para enviar respostas às críticas ao seu aplicativo.
title: Enviar respostas às críticas
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análises da Microsoft Store, aquisições de complemento
ms.localizationpriority: medium
ms.openlocfilehash: c08dcda52940f0218b6fdb5be147f058eca7479a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623831"
---
# <a name="submit-responses-to-reviews"></a>Enviar respostas às críticas


Use este método na API de análises da Microsoft Store para enviar respostas às críticas ao seu aplicativo de forma programada. Quando você chamar esse método, será necessário especificar as IDs das críticas que você deseja responder. As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md).

Quando um cliente envia uma crítica, ele pode optar por não receber respostas para sua crítica. Se você tentar responder a uma crítica para a qual o cliente optou por não receber respostas, o corpo da resposta desse método indicará que a tentativa de resposta não foi bem-sucedida. Antes de chamar esse método, você pode, opcionalmente, determinar se você tem permissão para responder a uma determinada crítica usando o método [obter as informações de resposta para avaliações de aplicativo](get-response-info-for-app-reviews.md).

> [!NOTE]
> Além de usar esse método por meio de programação responder às revisões, como alternativa, você pode responder a revisões [usando o Partner Center](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](respond-to-reviews-using-windows-store-services.md#prerequisites) para a API de análises da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha as IDs das críticas que você deseja responder. As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md).

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Esse método não tem parâmetros de solicitação.


### <a name="request-body"></a>Corpo da solicitação

O corpo da solicitação tem os valores a seguir.

| Valor        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------|
| Respostas | matriz | Uma matriz de objetos que contêm os dados de resposta que você deseja enviar. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir. |


Cada objeto na matriz *Respostas* contém os seguintes valores.

| Valor        | Tipo   | Descrição           |  Obrigatório  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | cadeia de caracteres |  A ID da Loja do aplicativo com a crítica que você deseja responder. A ID de Store está disponível na [página de aplicativo de identidade](../publish/view-app-identity-details.md) do Partner Center. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8.   |  Sim  |
| ReviewId | cadeia de caracteres |  A ID da revisão que você deseja responder (este é um GUID). As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md).   |  Sim  |
| ResponseText | cadeia de caracteres | A resposta que você deseja enviar. Sua resposta deve seguir [estas diretrizes](../publish/respond-to-customer-reviews.md#guidelines-for-responses).   |  Sim  |
| SupportEmail | cadeia de caracteres | Endereço de email de suporte do seu aplicativo, que o cliente pode usar para contatá-lo diretamente. Este deve ser um endereço de email válido.     |  Sim  |
| IsPublic | Booliano |  Se você especificar **verdadeira**, sua resposta será exibida na Store do seu aplicativo listando diretamente abaixo de revisão do cliente e será visível para todos os clientes. Se você especificar **falsos** e o usuário ainda não optou por receber respostas de email, sua resposta será enviada ao cliente por email e não será visível para outros clientes na listagem de Store do seu aplicativo. Se você especificar **falsos** e o usuário tem optou por receber respostas de email, um erro será retornado.   |  Sim  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como usar esse método para enviar respostas às várias avaliações.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor        | Tipo   | Descrição            |
|---------------|--------|---------------------|
| Resultado | matriz | Uma matriz de objetos que contém dados sobre cada resposta que você enviou. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.  |


Cada objeto na matriz *Resultado* contém os seguintes valores.

| Valor        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | cadeia de caracteres |  A ID da Loja do aplicativo com a crítica que você deseja responder. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8.   |
| ReviewId | cadeia de caracteres |  A ID da revisão a que você respondeu. É um GUID.   |
| Bem-sucedido | cadeia de caracteres | O valor **true** indica que a resposta foi enviada com sucesso. O valor **false** indica que a sua resposta não foi enviada com sucesso.    |
| FailureReason | cadeia de caracteres | Se **Bem-sucedido** for **false**, esse valor contém um motivo para a falha. Se **Bem-sucedido** for **true**, esse valor será vazio.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Responder a revisões de cliente usando o Partner Center](../publish/respond-to-customer-reviews.md)
* [Responder às revisões usando os serviços da Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obter informações de resposta para revisões de aplicativo](get-response-info-for-app-reviews.md)
* [Obter revisões de aplicativo](get-app-reviews.md)
