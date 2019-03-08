---
ms.assetid: ''
description: Use este método na API de análise da Microsoft Store para baixar o arquivo CAB para um erro em seu app.
title: Baixar o arquivo CAB de um erro em seu aplicativo
ms.date: 06/16/2017
ms.topic: article
keywords: windows 10, uwp, API de análise da Microsoft Store, baixar CAB
ms.localizationpriority: medium
ms.openlocfilehash: a4643f94236e62c46c12fd656ab5ddba5e1e0632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594531"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Baixar o arquivo CAB de um erro em seu aplicativo

Use esse método na API de análise de Microsoft Store para baixar o arquivo CAB que está associado um determinado erro em seu aplicativo que foi relatado ao Partner Center. Este método pode apenas baixar o arquivo CAB de um erro de app que ocorreu nos últimos 30 dias. Downloads de arquivos CAB também estão disponíveis na **falhas** seção o [relatório de integridade](../publish/health-report.md) no Partner Center.

Antes de usar esse método, primeiro você deverá usar o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar a ID do arquivo CAB que deseja baixar.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabId** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID da Loja do aplicativo para o qual você deseja baixar um arquivo CAB. A ID de Store está disponível na [página de aplicativo de identidade](../publish/view-app-identity-details.md) do Partner Center. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8. |  Sim  |
| cabId | cadeia de caracteres | A ID exclusiva do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabId** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter baixar um arquivo CAB usando esse método. Substitua os parâmetros *applicationId* e *cabId* pelos valores apropriados para seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/cabdownload?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esse método retorna um código de resposta 302 (redirecionamento) e o cabeçalho **Local** na resposta é atribuído à URI de assinatura do acesso compartilhado (SAS) do arquivo CAB. O chamador é redirecionado para essa URI a fim de baixar automaticamente o arquivo CAB.

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter detalhes sobre um erro em seu aplicativo](get-details-for-an-error-in-your-app.md)
* [Obter o rastreamento de pilha para um erro em seu aplicativo](get-the-stack-trace-for-an-error-in-your-app.md)
