---
description: Use este método na API de análise da Microsoft Store para obter dados de conquistas do Xbox Live.
title: Obter dados de conquistas do Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, conquistas
ms.localizationpriority: medium
ms.openlocfilehash: 23a99c637dfd466ba21169626315803dec60e4e8
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8829929"
---
# <a name="get-xbox-live-achievements-data"></a>Obter dados de conquistas do Xbox Live

Use este método na API de análise da Microsoft Store para obter o número de clientes que desbloquearam cada conquista para seu [jogo habilitado para Xbox Live](../xbox-live/index.md) durante o dia mais recente para o qual os dados de conquista estão disponíveis, os 30 dias anteriores a esse dia e o tempo de vida total do seu jogo até esse dia. Essas informações também estão disponíveis no [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

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
| applicationId | string | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de conquistas do Xbox Live.  |  Sim  |
| metricType | string | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **achievements**.  |  Sim  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de conquistas para clientes que desbloquearam as 10 primeiras conquistas para o seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contem dados sobre cada conquista em seu jogo. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.                                                                                                                      |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 100, mas houver mais de 100 linhas de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.  |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | A ID da Store do jogo do qual você está recuperando dados de conquistas.     |
| reportDateTime     | string |  A data para os dados de conquistas.    |
| achievementId          | number |  A ID da conquista. |
| achievementName           | string | O nome da conquista.  |
| pontuação do jogador           | number |  A recompensa de pontuação do jogador para a conquista.  |
| dailyUnlocks           | number |  O número de clientes que desbloquearam a conquista no dia especificado por *reportDateTime*.  |
| monthlyUnlocks              | number |  O número de clientes que desbloquearam a conquista nos 30 dias anteriores ao dia especificado por *reportDateTime*.   |
| totalUnlocks | number |  O número de clientes que desbloquearam a conquista durante o tempo de vida do seu jogo, até o dia especificado por *reportDateTime*.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados do Hub de Jogos Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
* [Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)
