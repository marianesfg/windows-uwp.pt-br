---
author: mcleanbyron
description: Use este método na API de análise da Microsoft Store para obter dados de ideias de seu aplicativo.
title: Obter dados de insights
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, serviços da loja, API, insights de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4534243"
---
# <a name="get-insights-data"></a>Obter dados de insights

Use este método na API de análise da Microsoft Store para obter dados de ideias relacionado ao aquisições, integridade e métricas de uso de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis no [relatório de ideias](../publish/insights-report.md) no painel do Centro de desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A [ID da loja](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você deseja recuperar dados de ideias. Se você não especificar esse parâmetro, o corpo da resposta conterá dados insights para todos os aplicativos registrados em sua conta.  |  Não  |
| startDate | date | A data de início no intervalo de datas dos dados de percepções a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas dos dados de percepções a serem recuperados. O padrão é a data atual. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filter = dataType eq 'aquisição'*. <p/><p/>Você pode especificar os campos de filtro a seguir:<p/><ul><li><strong>aquisição</strong></li><li><strong>integridade</strong></li><li><strong>uso</strong></li></ul> | Não   |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de ideias. Substitua o valor de *applicationId* pela ID da Loja do seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de ideias para o aplicativo. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [informação valores](#insight-values) abaixo.                                                                                                                      |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                 |


### <a name="insight-values"></a>Valores de informação

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | A ID da loja do aplicativo para o qual você está recuperando dados de ideias.     |
| insightDate                | string | A data em que identificamos a alteração de uma métrica específica. Essa data representa o final da semana em que detectamos um aumento significativo ou diminuir em uma métrica em comparação com a semana anterior. |
| tipo de dados     | string | Uma das seguintes cadeias de caracteres que especifica a área de análise gerais que descreve essa informação:<p/><ul><li><strong>aquisição</strong></li><li><strong>integridade</strong></li><li><strong>uso</strong></li></ul>   |
| insightDetail          | array | Um ou mais [valores de InsightDetail](#insightdetail-values) que representam os detalhes de visão atual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Um dos seguintes valores que indica a métrica que descreve a informação atual ou a dimensão atual, com base no valor de **tipo de dados** .<ul><li>**Integridade**, esse valor é sempre **contagem de ocorrências**.</li><li>Para **aquisição**, esse valor é sempre **AcquisitionQuantity**.</li><li>Para **uso**, esse valor pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Um ou mais objetos que descrevem uma métrica única para a informação.   |
| PercentChange            | string |  Porcentagem a métrica alterada em sua base de clientes inteiro.  |
| DimensionName           | string |  O nome da métrica descrito na dimensão atual. Exemplos incluem **EventType**, **mercado**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** e **sexo**.   |
| DimensionValue              | string | O valor da métrica que está descrito na dimensão atual. Por exemplo, se **DimensionName** **EventType**, **DimensionValue** pode ser **travamento** ou **congelamento**.   |
| FactValue     | string | O valor absoluto da métrica na data que a informação foi detectada.  |
| Direção | string |  A direção da alteração (**positivo** ou **negativo**).   |
| Data              | string |  A data em que identificamos a alteração relacionada a visão atual ou a dimensão atual.   |

### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de insights](../publish/insights-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
