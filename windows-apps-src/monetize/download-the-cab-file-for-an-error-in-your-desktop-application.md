---
author: Xansky
description: Use este método na API de análise da Microsoft Store para baixar o arquivo CAB para um erro em seu aplicativo da área de trabalho.
title: Baixar o arquivo CAB de um erro em seu aplicativo da área de trabalho
ms.author: mhopkins
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de análise da Microsoft Store, baixar CAB, aplicativo da área de trabalho
ms.localizationpriority: medium
ms.openlocfilehash: b7f4e8d1dabb701df75e06a9ebda7042fd3f16dd
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4618508"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Baixar o arquivo CAB de um erro em seu aplicativo da área de trabalho

Use este método na da API de análise da Microsoft Store para baixar o arquivo CAB associado a um determinado erro para um aplicativo da área de trabalho que você adicionou ao [programa do aplicativo de área de trabalho do Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este método pode apenas baixar o arquivo CAB de um erro de app que ocorreu nos últimos 30 dias. Os downloads de arquivo CAB também estão disponíveis no [Relatório de integridade](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicativos da área de trabalho no painel do Centro de Desenvolvimento do Windows.

Antes de usar esse método, primeiro você deverá usar o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar o hash de ID do arquivo CAB que deseja baixar.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha o hash da ID do arquivo CAB que você deseja baixar. Para obter esse valor, use o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabIdHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do aplicativo da área de trabalho para o qual você deseja baixar um arquivo CAB. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise do Centro de Desenvolvimento para o seu aplicativo da área de trabalho](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como o **Relatório de integridade**) e recupere a ID do produto da URL. |  Sim  |
| cabIdHash | string | O hash da ID exclusiva do arquivo CAB que você deseja baixar. Para obter esse valor, use o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar os detalhes de um erro específico em seu aplicativo, e use o valor de **cabIdHash** no corpo da resposta desse método. |  Sim  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter baixar um arquivo CAB usando esse método. Substitua os parâmetros *applicationId* e *cabIdHash* pelos valores apropriados para seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

Esse método retorna um código de resposta 302 (redirecionamento) e o cabeçalho **Local** na resposta é atribuído à URI de assinatura do acesso compartilhado (SAS) do arquivo CAB. O chamador é redirecionado para essa URI a fim de baixar automaticamente o arquivo CAB.

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros do seu aplicativo da área de trabalho](get-desktop-application-error-reporting-data.md)
* [Obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md)
* [Obter o rastreamento de pilha de um erro em seu aplicativo da área de trabalho](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
