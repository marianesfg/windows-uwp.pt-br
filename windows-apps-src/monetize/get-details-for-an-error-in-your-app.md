---
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico do seu app.
title: Obter detalhes de um erro em seu app
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, detalhes
ms.localizationpriority: medium
ms.openlocfilehash: 5176e123d57b8bcc5d4981acc91b22329ad4c643
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662101"
---
# <a name="get-details-for-an-error-in-your-app"></a>Obter detalhes de um erro em seu app

Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico do seu app no formato JSON. Este método só pode recuperar detalhes dos erros que ocorreram nos últimos 30 dias. Dados de erro detalhadas também estão disponíveis na **falhas** seção o [relatório de integridade](../publish/health-report.md) no Partner Center.

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
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID da Loja do app para o qual você deseja recuperar dados de erros detalhados. A ID de Store está disponível na [página de aplicativo de identidade](../publish/view-app-identity-details.md) no Partner Center. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8. |  Sim  |
| failureHash | cadeia de caracteres | A ID exclusiva do erro para o qual você deseja obter informações detalhadas. Para obter esse valor para o erro no qual você está interessado, use o método [obter dados de relatório de erros](get-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é 30 dias antes da data atual.<p/><p/>**Observação:**&nbsp;&nbsp;esse método só pode recuperar detalhes de erros que ocorreram nos últimos 30 dias. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante. |  Não  |
| filter |cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>mercado</strong></li><li><strong>Data</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>PackageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Não   |
| orderby | cadeia de caracteres | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>mercado</strong></li><li><strong>Data</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>PackageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de erros detalhados. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição    |
|------------|---------|------------|
| Valor      | matriz   | Uma matriz de objetos que contêm dados de erros detalhados. Para saber mais sobre os dados em cada objeto, consulte a seção de [Valores de detalhes de erros](#error-detail-values) a seguir.          |
| @nextLink  | cadeia de caracteres  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valores de detalhes de erros

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|----------------------------|
| applicationId   | cadeia de caracteres  | A ID da Loja do app para o qual você recuperou dados de erros detalhados.      |
| failureHash     | cadeia de caracteres  | O identificador exclusivo do erro.     |
| failureName     | cadeia de caracteres  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.           |
| date            | cadeia de caracteres  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| cabId           | cadeia de caracteres  | A ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabExpirationTime  | cadeia de caracteres  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |
| market          | cadeia de caracteres  | O código de país ISO 3166 do mercado do dispositivo.     |
| osBuild         | cadeia de caracteres  | O número de versão do sistema operacional no qual o erro ocorreu.       |
| packageVersion  | cadeia de caracteres  | O versão do pacote do aplicativo que está associado a esse erro.    |
| deviceModel           | cadeia de caracteres  | Uma cadeia de caracteres que especifica o modelo do dispositivo no qual o app era executado quando o erro ocorreu.   |
| osVersion       | cadeia de caracteres  | Uma das sequências a seguir que indica a versão do sistema operacional no qual ocorreu o erro:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| osRelease       | cadeia de caracteres  |  Uma das sequências a seguir que especifica a versão do sistema operacional ou anel de liberação de versões de pré-lançamento (como uma subpopulação na versão do sistema operacional) no qual o erro ocorreu.<p/><p>No Windows 10:</p><ul><li><strong>Versão 1507</strong></li><li><strong>Versão 1511</strong></li><li><strong>Versão 1607</strong></li><li><strong>Versão 1703</strong></li><li><strong>Versão 1709</strong></li><li><strong>Versão 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>Insider Fast</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para o Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para o Windows Server 2016:</p><ul><li><strong>Versão 1607</strong></li></ul><p>No Windows 8.1:</p><ul><li><strong>Atualização 1</strong></li></ul><p>No Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p>    |
| deviceType      | cadeia de caracteres  | Uma das seguintes cadeias de caracteres que especifica o tipo do dispositivo no qual o app era executado quando o erro ocorreu:<ul><li><strong>PC</strong></li><li><strong>Telefone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul>     |
| cabDownloadable           | Booliano  | Indica se o arquivo CAB está disponível para download para esse usuário.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "deviceType": "PC",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter o rastreamento de pilha para um erro em seu aplicativo](get-the-stack-trace-for-an-error-in-your-app.md)
* [Baixe o arquivo CAB para um erro em seu aplicativo](download-the-cab-file-for-an-error-in-your-app.md)
