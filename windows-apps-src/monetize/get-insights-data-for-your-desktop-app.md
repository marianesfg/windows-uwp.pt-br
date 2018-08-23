---
author: mcleanbyron
description: Use este método no API de análise do Microsoft Store para obter dados ideias para seu aplicativo de área de trabalho.
title: Obter dados de ideias para o seu aplicativo de área de trabalho
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, serviços de repositório, análise do Microsoft Store API, insights
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2820115"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obter dados de ideias para o seu aplicativo de área de trabalho

Use este método no API de análise do Microsoft Store para obter ideias dados relacionados aos métricas de integridade para um aplicativo de área de trabalho que você adicionou para o [programa do aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Esses dados também estão disponíveis no [relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) para aplicativos da área de trabalho no painel do Centro de desenvolvimento do Windows.

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
| applicationId | string | A ID de produto do aplicativo da área de trabalho para o qual você deseja obter ideias de dados. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise do Centro de Desenvolvimento para o seu aplicativo da área de trabalho](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como o **Relatório de integridade**) e recupere a ID do produto da URL. Se você não especificar esse parâmetro, o corpo da resposta conterá dados ideias para todos os aplicativos registrados em sua conta.  |  Não  |
| startDate | date | A data de início do intervalo de data dos dados ideias para recuperar. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data de término do intervalo de data dos dados ideias para recuperar. O padrão é a data atual. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filtro = dataType eq 'aquisição'*. <p/><p/>Atualmente esse método suporta apenas a **integridade**do filtro.  | Não   |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação para obtenção de dados ideias. Substitua o valor de *applicationId* com o valor apropriado para seu aplicativo de área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | string | A ID de produto do aplicativo da área de trabalho para o qual você recuperou dados ideias.     |
| insightDate                | string | A data em que podemos identificado a alteração em uma métrica específica. Essa data representa o fim da semana em que podemos detectado um aumento significativo ou diminuir em uma métrica comparada à semana antes que. |
| tipo de dados     | string | Uma string que especifica a área de análise gerais que informa essa percepção. Atualmente, este método só oferece suporte a **integridade**.    |
| insightDetail          | array | Um ou mais [valores InsightDetail](#insightdetail-values) que representam os detalhes para insight atual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Uma string que indica a métrica que descreve a percepção atual ou a dimensão atual. Atualmente, esse método suporta apenas o valor de **contagem de ocorrências**.  |
| SubDimensions         | array |  Um ou mais objetos que descrevem uma métrica única para o insight.   |
| Alteraçãopercentual            | string |  Porcentagem na métrica alterou em sua base de clientes inteira.  |
| DimensionName           | string |  O nome da métrica descrito na dimensão atual. Exemplos incluem **EventType**, **mercado**, **DeviceType**e **PackageVersion**.   |
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

* [Programa de aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Relatório de integridade](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
