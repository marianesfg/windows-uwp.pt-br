---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter dados de multijogador do Xbox Live.
title: Obter dados de multijogador do Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, multijogador
ms.localizationpriority: medium
ms.openlocfilehash: 6074f3774d099c63f6c39ac4ef0e95a7b6745912
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7280738"
---
# <a name="get-xbox-live-multiplayer-data"></a>Obter dados de multijogador do Xbox Live


Use este método na API de análise da Microsoft Store para obter dados de multijogador para seu [jogo habilitado para Xbox Live](../xbox-live/index.md) por dia ou por mês. Essas informações também estão disponíveis no [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

> [!IMPORTANT]
> Esse método oferece suporte somente a jogos para Xbox ou que usam os serviços do Xbox Live. Esses jogos devem passar pelo [processo de aprovação de conceito](../gaming/concept-approval.md), que inclui jogos publicados por [parceiros da Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) e jogos enviados por meio do programa [ID@Xbox](../xbox-live/developer-program-overview.md#id). Esse método não oferece suporte no momento para jogos publicados pelo [Programa de Criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados


| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | sequência | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de multijogador do Xbox Live.  |  Sim  |
| metricType | string | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para esse método, especifique o valor **multiplayerdaily** para obter dados de multijogador diários ou **multiplayermonthly** para obter dados de multijogador mensais.  |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados de multijogador a serem recuperados. Para **multiplayerdaily**, o padrão é 3 meses antes da data atual. Para **multiplayermonthly**, o padrão é 1 ano antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de multijogador a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul> | Não   |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Se você especificar um ou mais campos *groupby*, qualquer outro campo *groupby* não especificado terá o valor **All** no corpo da resposta. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de multijogador para o seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de multijogador, onde cada objeto representa um conjunto de dados para o tempo especificado por dia ou por mês e organizado pelos valores **filter** e **groupby** especificados. Para obter mais informações sobre os dados em cada objeto, consulte as seções [Análises de multijogador por dia](#daily-multiplayer-analytics) e [Análise de multijogador por mês](#monthly-multiplayer-analytics).     |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta. |


### <a name="daily-multiplayer-analytics"></a>Análise de multijogador por dia

Os elementos da matriz *Valor* contêm os seguintes valores quando você solicita dados de análise de multijogador por dia (ou seja, quando você especifica **multiplayerdaily** para o parâmetro **metricType**).

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | string | A data para os dados de multijogador. |
| applicationId       | string | A ID da Store do jogo do qual você está recuperando dados de multijogador.     |
| applicationName       | string |  O nome do jogo do qual você está recuperando dados de multijogador.     |
| market       | string | O código de país ISO 3166 de duas letras do mercado de onde vieram os dados de multijogador.       |
| packageVersion     | string |  A versão do pacote de quatro partes para o jogo.  |
| deviceType          | string | Uma das seguintes sequências que especifica o tipo de dispositivo de onde vieram os dados de multijogador:<p/><ul><li><strong>Console</strong></li><li><strong>Computador</strong></li><li>**Desconhecido**</li></ul>  |
| subscriptionName     | string |  O nome da assinatura usada para os dados de multijogador. Os valores possíveis incluem **Xbox Game Pass** e **""** (para quando não houver assinatura).  |
| dailySessionCount     | number |  O número de sessões de multijogador para o jogo na data especificada.  |
| engagementDurationMinutes     | number |  O número total de minutos em que os clientes estavam envolvidos com sessões de multijogador do jogo na data especificada.  |
| dailyActiveUsers     | number |  O número total de usuários multijogador ativos para o jogo na data especificada.  |
| dailyActiveDevices     | number |  O número total de dispositivos ativos que jogaram sessões multijogador para o jogo na data especificada.  |
| dailyNewUsers     | number |  O número total de novos usuários multijogador para o jogo na data especificada.  |
| monthlyActiveUsers     | number |  O número total de usuários multijogador ativos para o mês em que ocorreu a data especificada.  |
| monthlyActiveDevices     | number | O número total de dispositivos ativos que jogaram sessões multijogador para o jogo no mês que ocorreu a data especificada.   |
| monthlyNewUsers     | number |  O número total de novos usuários multijogador para o jogo para o mês em que ocorreu a data especificada.  |


### <a name="monthly-multiplayer-analytics"></a>Análise de multijogador por mês

Os elementos da matriz *Valor* contêm os seguintes valores quando você solicita dados de análise de multijogador por mês (ou seja, quando você especifica **multiplayermonthly** para o parâmetro **metricType**).

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | string | A primeira data no mês para os dados de multijogador. |
| applicationId       | string | A ID da Store do jogo do qual você está recuperando dados de multijogador.     |
| applicationName       | string |  O nome do jogo do qual você está recuperando dados de multijogador.     |
| market       | string | O código de país ISO 3166 de duas letras do mercado de onde vieram os dados de multijogador.       |
| packageVersion     | string |  A versão do pacote de quatro partes para o jogo.  |
| deviceType          | string | Uma das seguintes sequências que especifica o tipo de dispositivo de onde vieram os dados de multijogador:<p/><ul><li><strong>Console</strong></li><li><strong>Computador</strong></li><li>**Desconhecido**</li></ul>  |
| subscriptionName     | string |  O nome da assinatura usada para os dados de multijogador. Os valores possíveis incluem **Xbox Game Pass** e **""** (para quando não houver assinatura).  |
| monthlySessionCount     | number |  O número de sessões de multijogador para o jogo durante o mês especificado.   |
| engagementDurationMinutes     | number |  O número total de minutos em que os clientes estavam envolvidos com sessões de multijogador do jogo durante o mês especificado.  |
| monthlyActiveUsers     | number | O número total de usuários multijogador ativos durante o mês especificado.   |
| monthlyActiveDevices     | number | O número total de dispositivos ativos que jogaram sessões multijogador para o jogo durante o mês especificado.   |
| monthlyNewUsers     | number |  O número total de novos usuários multijogador para o jogo durante o mês especificado.  |
| averageDailyActiveUsers     | number |  O número médio de usuários multijogador ativos por dia para o jogo durante o mês especificado.  |
| averageDailyActiveDevices     | number |  O número médio de dispositivos ativos que jogaram sessões multijogador para o jogo durante o mês especificado.  |


### <a name="response-example"></a>Exemplo de resposta

O exemplo a seguir demonstra um exemplo de corpo de resposta JSON da variante da métrica diária dessa solicitação (ou seja, quando você especificar **multiplayerdaily** para o parâmetro **metricType**).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados do Hub de Jogos Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
