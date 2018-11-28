---
description: Use este método na API de análise da Microsoft Store para obter dados do Hub de Jogos Xbox Live.
title: Obter dados do hub de jogos Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, Hubs de Jogos
ms.localizationpriority: medium
ms.openlocfilehash: 09c2a2c69e32d151c393c5a0652c1d9de7b4360e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7851699"
---
# <a name="get-xbox-live-game-hub-data"></a>Obter dados do Hub de Jogos Xbox Live


Use este método na API de análise da Microsoft Store para obter dados do Hub de Jogos para seu [jogo habilitado para Xbox Live](../xbox-live/index.md). Essas informações também estão disponíveis no [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

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
| applicationId | string | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados do Hub de Jogos Xbox Live.  |  Sim  |
| metricType | string | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **communitymanagergamehub**.  |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados do Hub de Jogos a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados do Hub de Jogos a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados do Hub de Jogos para o seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagergamehub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados do Hub de Jogos para cada data no intervalo de datas especificado. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.                                                                                                                      |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.  |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | string | A data dos dados do Hub de Jogos neste objeto. |
| applicationId       | string | A ID da Store do jogo do qual você está recuperando dados do Hub de Jogos.     |
| gameHubLikeCount     | number |   O número de curtidas adicionadas à página do Hub de Jogos na data especificada.   |
| gameHubCommentCount          | number |  O número de comentários adicionados à página do Hub de Jogos para seu app na data especificada.  |
| gameHubShareCount           | number | O número de vezes que a página do Hub de Jogos para seu app foi compartilhada por clientes na data especificada.   |
| gameHubFollowerCount          | number | O número de seguidores da página do Hub do jogo para seu aplicativo.   |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2018-01-04",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 10,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15924
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 12,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15931
    }
  ],
  "@nextLink": null,
  "TotalCount": 26
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
* [Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)
