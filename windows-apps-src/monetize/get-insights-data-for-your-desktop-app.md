---
author: mcleanbyron
description: Use este método na API de análise da Microsoft Store para obter dados de ideias de seu aplicativo da área de trabalho.
title: Obter dados de ideias para seu aplicativo da área de trabalho
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, serviços da loja, API, insights de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119858"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obter dados de ideias para seu aplicativo da área de trabalho

Use este método na API de análise da Microsoft Store para obter insights dados relacionados à avaliação de integridade para um aplicativo da área de trabalho que você adicionou para o [programa do aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Esses dados também estão disponíveis no [relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) para aplicativos da área de trabalho no painel do Centro de desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do aplicativo da área de trabalho para o qual você deseja obter dados de percepções. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise do Centro de Desenvolvimento para o seu aplicativo da área de trabalho](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como o **Relatório de integridade**) e recupere a ID do produto da URL. Se você não especificar esse parâmetro, o corpo da resposta conterá dados insights para todos os aplicativos registrados em sua conta.  |  Não  |
| startDate | date | A data de início no intervalo de datas dos dados insights a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas dos dados insights a serem recuperados. O padrão é a data atual. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filter = dataType eq 'aquisição'*. <p/><p/>No momento este método só oferece suporte a **integridade**do filtro.  | Não   |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de percepções dados. Substitua o valor *applicationId* com o valor apropriado para seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de ideias para o aplicativo. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [Insight valores](#insight-values) abaixo.                                                                                                                      |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                 |


### <a name="insight-values"></a>Valores de informação

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | A ID do produto do aplicativo da área de trabalho para o qual você recuperou dados de percepções.     |
| insightDate                | string | A data em que identificamos a alteração de uma métrica específica. Essa data representa o final da semana em que detectamos um aumento significativo ou diminuir em uma métrica em comparação com a semana anterior. |
| tipo de dados     | string | Uma cadeia de caracteres que especifica a área de análise gerais que informa essa informação. Atualmente, esse método suporta apenas **integridade**.    |
| insightDetail          | array | Um ou mais [valores InsightDetail](#insightdetail-values) que representam os detalhes de visão atual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Uma cadeia de caracteres que indica a métrica que descreve o insight atual ou a dimensão atual. Atualmente, esse método suporta apenas o valor de **contagem de ocorrências**.  |
| SubDimensions         | array |  Um ou mais objetos que descrevem uma métrica única para a visão.   |
| PercentChange            | string |  Porcentagem a métrica alterada em sua base de clientes inteiro.  |
| DimensionName           | string |  O nome da métrica descrito na dimensão atual. Exemplos incluem **EventType**, **mercado**, **DeviceType**e **PackageVersion**.   |
| DimensionValue              | string | O valor da métrica que está descrito na dimensão atual. Por exemplo, se **DimensionName** **EventType**, **DimensionValue** pode ser **travamento** ou **congelamento**.   |
| FactValue     | string | O valor absoluto da métrica na data que o insight foi detectado.  |
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

* [Programa de aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
