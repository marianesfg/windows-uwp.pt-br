---
description: Use esse método na API de análise de Microsoft Store para obter dados de insights para seu aplicativo da área de trabalho.
title: Obter dados de insights do seu aplicativo da área de trabalho
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, uwp, serviços de Store, API, insights de análise da Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5545d27668b23e5b7ae91201421dfa4c92f9c8ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618131"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obter dados de insights do seu aplicativo da área de trabalho

Use esse método na API de análise de Microsoft Store para obter insights de dados relacionado às métricas de integridade para um aplicativo da área de trabalho que você adicionou à [programa de aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Esses dados também estão disponíveis na [relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) para aplicativos da área de trabalho no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A ID do produto do aplicativo da área de trabalho para o qual você deseja obter dados do insights. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise para seu aplicativo da área de trabalho no Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como o **relatório de integridade**) e recupere a ID do produto da URL. Se você não especificar esse parâmetro, o corpo da resposta conterá dados para todos os aplicativos registrados para sua conta do insights.  |  Não  |
| startDate | date | A data de início no intervalo de datas de dados do insights para recuperar. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados do insights para recuperar. O padrão é a data atual. |  Não  |
| filter | cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filtro = eq 'aquisição de tipo de dados'*. <p/><p/>Atualmente, este método só aceita o filtro **integridade**.  | Não   |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de insights de dados. Substitua os *applicationId* valor com o valor apropriado para seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contêm dados de insights para o aplicativo. Para obter mais informações sobre os dados em cada objeto, consulte a [valores de Insight](#insight-values) seção abaixo.                                                                                                                      |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                 |


### <a name="insight-values"></a>Valores de Insight

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | cadeia de caracteres | A ID do produto do aplicativo da área de trabalho para o qual você recuperou dados do insights.     |
| insightDate                | cadeia de caracteres | A data em que nós identificamos que a alteração em uma métrica específica. Essa data representa o fim da semana em que foi detectado um aumento significativo ou diminuir em uma métrica em comparação com a semana antes disso. |
| dataType     | cadeia de caracteres | Uma cadeia de caracteres que especifica a área de análise geral que informa a esse insight. Atualmente, este método só aceita **integridade**.    |
| insightDetail          | matriz | Um ou mais [InsightDetail valores](#insightdetail-values) que representam os detalhes para o insight atual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| FactName           | cadeia de caracteres | Uma cadeia de caracteres que indica a métrica que descreve o insight atual ou a dimensão atual. Atualmente, este método só aceita o valor **contagem de ocorrências**.  |
| SubDimensions         | matriz |  Um ou mais objetos que descrevem uma única métrica para o insight.   |
| PercentChange            | cadeia de caracteres |  A porcentagem que a métrica alterou em sua base de todo o cliente.  |
| DimensionName           | cadeia de caracteres |  O nome da métrica descrito na dimensão atual. Os exemplos incluem **EventType**, **mercado**, **DeviceType**, e **PackageVersion**.   |
| DimensionValue              | cadeia de caracteres | O valor da métrica que é descrito na dimensão atual. Por exemplo, se **DimensionName** é **EventType**, **DimensionValue** pode ser **falha** ou **travar** .   |
| FactValue     | cadeia de caracteres | O valor absoluto da métrica na data em que a análise foi detectado.  |
| Direção | cadeia de caracteres |  A direção da alteração (**positivo** ou **negativo**).   |
| Data              | cadeia de caracteres |  A data em que nós identificamos que a alteração relacionada à percepção atual ou a dimensão atual.   |

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

* [Programa de aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
