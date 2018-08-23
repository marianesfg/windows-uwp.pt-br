---
author: mcleanbyron
description: Use este método no API de análise do Microsoft Store para obter dados ideias para seu aplicativo.
title: Obter ideias de dados
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, serviços de repositório, análise do Microsoft Store API, insights
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814578"
---
# <a name="get-insights-data"></a>Obter ideias de dados

Use este método no API de análise Microsoft Store para obter ideias de dados relacionado ao aquisições, integridade e métricas de uso para um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis no [relatório de ideias de pesquisas](../publish/insights-report.md) no painel do Centro de desenvolvimento do Windows.

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
| applicationId | string | A [ID do repositório](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você deseja recuperar dados ideias. Se você não especificar esse parâmetro, o corpo da resposta conterá dados ideias para todos os aplicativos registrados em sua conta.  |  Não  |
| startDate | date | A data de início do intervalo de data dos dados ideias para recuperar. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data de término do intervalo de data dos dados ideias para recuperar. O padrão é a data atual. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filtro = dataType eq 'aquisição'*. <p/><p/>Você pode especificar os seguintes campos de filtro:<p/><ul><li><strong>aquisição</strong></li><li><strong>integridade</strong></li><li><strong>uso</strong></li></ul> | Não   |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação para obtenção de dados ideias. Substitua o valor de *applicationId* pela ID da Store de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de ideias para o aplicativo. Para obter mais informações sobre os dados em cada objeto, consulte a seção [valores Insight](#insight-values) abaixo.                                                                                                                      |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                 |


### <a name="insight-values"></a>Valores de percepção

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | A ID do repositório do aplicativo para o qual você estiver recuperando dados ideias.     |
| insightDate                | string | A data em que podemos identificado a alteração em uma métrica específica. Essa data representa o fim da semana em que podemos detectado um aumento significativo ou diminuir em uma métrica comparada à semana antes que. |
| tipo de dados     | string | Uma das seguintes sequências especificando a área de análise geral que descreve este insight:<p/><ul><li><strong>aquisição</strong></li><li><strong>integridade</strong></li><li><strong>uso</strong></li></ul>   |
| insightDetail          | array | Um ou mais [valores InsightDetail](#insightdetail-values) que representam os detalhes para insight atual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Um dos seguintes valores que indica a métrica que descreve à percepção atual ou a dimensão atual, com base no valor do **tipo de dados** .<ul><li>Para verificar a **integridade**, esse valor é sempre **contagem de ocorrências**.</li><li>Para **aquisição**, esse valor é sempre **AcquisitionQuantity**.</li><li>Para **uso**, esse valor pode ser uma das cadeias de caracteres seguintes:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Um ou mais objetos que descrevem uma métrica única para o insight.   |
| Alteraçãopercentual            | string |  Porcentagem na métrica alterou em sua base de clientes inteira.  |
| DimensionName           | string |  O nome da métrica descrito na dimensão atual. Exemplos incluem **EventType**, **mercado**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** e **Gênero**.   |
| DimensionValue              | string | O valor da métrica descrita na dimensão atual. Por exemplo, se **DimensionName** **EventType**, **DimensionValue** pode ser **Falha** ou **travar**.   |
| FactValue     | string | O valor absoluto da métrica na data que a percepção foi detectada.  |
| Direção | string |  A direção da alteração (**positivo** ou **negativo**).   |
| Data              | string |  A data em que podemos identificado as alterações relacionadas a percepção atual ou a dimensão atual.   |

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

* [Relatório de ideias de pesquisas](../publish/insights-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
