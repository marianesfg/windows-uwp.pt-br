---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico do seu aplicativo da área de trabalho.
title: Obter detalhes de um erro em seu aplicativo da área de trabalho
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, detalhes, aplicativo da área de trabalho
ms.localizationpriority: medium
ms.openlocfilehash: 922ab18bfebfbe539788ade3caa7626919d6b19a
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7119125"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Obter detalhes de um erro em seu aplicativo da área de trabalho

Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico do seu app no formato JSON. Este método só pode recuperar detalhes dos erros que ocorreram nos últimos 30 dias. Dados de erros detalhados também está disponível no [relatório de integridade](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicativos da área de trabalho no Partner Center.

Antes de usar este método, primeiro você deve usar o método [obter dados de relatório de erros](get-error-reporting-data.md) para recuperar a ID do erro para o qual você deseja obter informações detalhadas.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do erro para o qual você deseja obter informações detalhadas. Para obter essa ID, use o método [obter dados de relatório de erros](get-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID de produto do aplicativo da área de trabalho para o qual você deseja recuperar os detalhes do erro. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise para seu aplicativo da área de trabalho no Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (por exemplo, o **relatório de integridade**) e recupere a ID do produto da URL. |  Sim  |
| failureHash | string | A ID exclusiva do erro para o qual você deseja obter informações detalhadas. Para obter esse valor para o erro no qual você está interessado, use o método [obter dados de relatório de erros](get-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é 30 dias antes da data atual.<p/><p/>**Observação:**&nbsp;&nbsp;este método só pode recuperar detalhes dos erros que ocorreram nos últimos 30 dias. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante. |  Não  |
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de erros detalhados. Substitua o valor de *applicationId* pela ID do produto de seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição    |
|------------|---------|------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de erros detalhados. Para saber mais sobre os dados em cada objeto, consulte a seção de [Valores de detalhes de erros](#error-detail-values) a seguir.          |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valores de detalhes de erros

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|----------------------------|
| applicationId   | string  | A ID de produto do aplicativo da área de trabalho para o qual você recuperou os detalhes do erro.      |
| failureHash     | string  | O identificador exclusivo do erro.     |
| failureName     | string  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.           |
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| cabIdHash           | string  | O hash da ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabExpirationTime  | string  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.     |
| osBuild         | string  | O número de versão do sistema operacional no qual o erro ocorreu.       |
| applicationVersion         | string  |   A versão do executável do aplicativo em que ocorreu o erro.     |
| deviceModel           | string  | Uma sequência que especifica o modelo do dispositivo no qual o app era executado quando o erro ocorreu.   |
| osVersion       | string  | Uma das seguintes sequências que especifica a versão do sistema operacional no qual o aplicativo da área de trabalho está instalado:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| osRelease       | string  |  Uma das sequências a seguir que especifica a versão do sistema operacional ou anel de liberação de versões de pré-lançamento (como uma subpopulação na versão do sistema operacional) no qual o erro ocorreu.<p/><p>No Windows 10:</p><ul><li><strong>Versão1507</strong></li><li><strong>Versão1511</strong></li><li><strong>Versão1607</strong></li><li><strong>Versão1703</strong></li><li><strong>Versão1709</strong></li><li><strong>Versão1803</strong></li><li><strong>Versão prévia de lançamento</strong></li><li><strong>Participante do Programa Windows Insider - Modo Rápido</strong></li><li><strong>Participante do Programa Windows Insider - Modo Lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p>    |
| deviceType      | string  | Uma das seguintes sequências que indica o tipo de dispositivo no qual ocorreu o erro: <p/><ul><li><strong>Computador</strong></li><li><strong>Servidor</strong></li><li><strong>Desconhecido</strong></li></ul>     |
| cabDownloadable           | booliano  | Indica se o arquivo CAB está disponível para download para esse usuário.   |
| fileName           | string  | O nome do arquivo executável do aplicativo da área de trabalho para o qual você recuperou os detalhes do erro.  |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "10238467886765136388",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "NULL_CLASS_PTR_WRITE_c0000005_contoso.exe!unknown_error_in_process",
      "date": "2018-01-28 23:55:29",
      "cabIdHash": "54ffb83a-e159-41d2-8158-f36f306cc01e",
      "cabExpirationTime": "2018-02-27 23:55:29",
      "market": "US",
      "osBuild": "10.0.10240",
      "applicationVersion": "2.2.2.0",
      "deviceModel": "Contoso All-in-one",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "deviceType": "PC",
      "cabDownloadable": false,
      "fileName": "contosodemo.exe"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros do seu aplicativo da área de trabalho](get-desktop-application-error-reporting-data.md)
* [Obter o rastreamento de pilha de um erro em seu aplicativo da área de trabalho](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Baixar o arquivo CAB de um erro em seu aplicativo da área de trabalho](download-the-cab-file-for-an-error-in-your-desktop-application.md)
