---
author: Xansky
ms.assetid: ''
description: Use este método na API de análise da Microsoft Store para baixar o arquivo CAB para um erro em seu app.
title: Baixar o arquivo CAB de um erro em seu aplicativo
ms.author: mhopkins
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de análise da Microsoft Store, baixar CAB
ms.localizationpriority: medium
ms.openlocfilehash: 671c5c1b187ac48c12988a00d66acb366cae72f1
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4960707"
---
# <a name="download-the-cab-file-for-an-error-in-your-app"></a>Baixar o arquivo CAB de um erro em seu aplicativo

Use este método na API de análise da Microsoft Store para baixar o arquivo CAB associado a um erro específico em seu app relatado para o Centro de Desenvolvimento. Este método pode apenas baixar o arquivo CAB de um erro de app que ocorreu nos últimos 30 dias. Os downloads de arquivo CAB também estão disponíveis na seção **Falhas** do [Relatório de integridade](../publish/health-report.md) no painel do Centro de Desenvolvimento do Windows.

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
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID da Loja do aplicativo para o qual você deseja baixar um arquivo CAB. A ID da Loja está disponível na [página de identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8. |  Sim  |
| cabId | string | A ID exclusiva do arquivo CAB que você deseja baixar. Para obter essa ID, use o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabId** no corpo da resposta desse método. |  Sim  |

 
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
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatórios de erros](get-error-reporting-data.md)
* [Obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md)
* [Obter o rastreamento de pilha de um erro em seu app](get-the-stack-trace-for-an-error-in-your-app.md)
