---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter dados detalhados de um erro específico para seu Xbox One jogo.
title: Obter detalhes de um erro em seu Xbox One jogo
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, erros, detalhes
ms.localizationpriority: medium
ms.openlocfilehash: 33733af7f323817bc82d49800c2dc17c5f7b9887
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978345"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Obter detalhes de um erro em seu Xbox One jogo

Use este método na análise da Microsoft Store API para obter dados detalhados de um erro específico para seu Xbox One jogo que ingerido por meio do Portal de desenvolvedor do Xbox (XDP) e está disponível no painel do Centro de desenvolvimento de análise XDP. Este método só pode recuperar detalhes dos erros que ocorreram nos últimos 30 dias.

Antes de usar esse método, primeiro você deve usar o método [obter dados para o seu jogo Xbox One de relatório de erros](get-error-reporting-data-for-your-xbox-one-game.md) para recuperar a ID do erro para o qual você deseja obter informações detalhadas.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do erro para o qual você deseja obter informações detalhadas. Para obter essa ID, use o método [obter dados de relatório de erro para o seu Xbox One jogo](get-error-reporting-data-for-your-xbox-one-game.md) e use o valor de **failureHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do jogo Xbox One para o qual você está recuperando detalhes do erro. Para obter a ID do produto do jogo, navegue até seu jogo no Portal de Desenvolvedor do Xbox (XDP) e recupere a ID do produto da URL. Como alternativa, se você baixar os dados de integridade do relatório de análise do Centro de desenvolvimento do Windows, a ID do produto está incluída no arquivo. tsv. |  Sim  |
| failureHash | string | A ID exclusiva do erro para o qual você deseja obter informações detalhadas. Para obter esse valor para o erro que você está interessado, use o método [obter dados de relatório de erro para o seu Xbox One jogo](get-error-reporting-data-for-your-xbox-one-game.md) e use o valor de **failureHash** no corpo da resposta desse método. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante. |  Não  |
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações de obtenção de dados de erros detalhados para um Xbox One jogo. Substitua o valor *applicationId* com a ID do produto para o seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
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
| applicationId   | string  | A ID do produto do jogo Xbox One para o qual você recuperou dados de erros detalhados.      |
| failureHash     | string  | O identificador exclusivo do erro.     |
| failureName     | string  | O nome de falha, que é composto de quatro partes: uma ou mais classes de problema, um código de verificação de bug/exceção, o nome da imagem em que ocorreu a falha e o nome da função associada.           |
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| cabId           | string  | A ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabExpirationTime  | string  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.     |
| osBuild         | string  | O número de versão do sistema operacional no qual o erro ocorreu.       |
| packageVersion  | string  | A versão do pacote do jogo que está associado esse erro.    |
| deviceModel           | string  | Uma das seguintes cadeias de caracteres que especifica o console Xbox One no qual o jogo estava em execução quando o erro ocorreu.<p/><ul><li><strong>Xbox da Microsoft um</strong></li><li><strong>Um S Xbox da Microsoft</strong></li><li><strong>X de um Xbox da Microsoft</strong></li></ul>  |
| osVersion       | string  | A versão do sistema operacional no qual ocorreu o erro. Isso é sempre o valor **Windows 10**.    |
| osRelease       | string  |  Uma das seguintes cadeias de caracteres que especifica a versão do sistema operacional Windows 10 ou anel de liberação (como uma subpopulação na versão do sistema operacional) no qual ocorreu o erro.<p/><ul><li><strong>Versão1507</strong></li><li><strong>Versão1511</strong></li><li><strong>Versão1607</strong></li><li><strong>Versão1703</strong></li><li><strong>Versão1709</strong></li><li><strong>Versão1803</strong></li><li><strong>Versão prévia de lançamento</strong></li><li><strong>Participante do Programa Windows Insider - Modo Rápido</strong></li><li><strong>Participante do Programa Windows Insider - Modo Lento</strong></li></ul><p>Se a versão do sistema operacional ou anel de liberação de versão de pré-lançamento for desconhecida, este campo terá o valor <strong>Desconhecido</strong>.</p>    |
| deviceType      | string  | O tipo de dispositivo no qual o erro ocorreu. Isso é sempre o valor **Console**.     |
| cabDownloadable           | booliano  | Indica se o arquivo CAB está disponível para download para esse usuário.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório para seu Xbox One erros jogo](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obter o rastreamento de pilha de um erro em seu Xbox One jogo](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Baixar o arquivo CAB para um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
