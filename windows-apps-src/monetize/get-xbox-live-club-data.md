---
description: Use este método na API de análise da Microsoft Store para obter dados de clube do Xbox Live.
title: Obter dados de clube do Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, clubes
ms.localizationpriority: medium
ms.openlocfilehash: e5fc116c2b868ddf093aabea09d59934301f49ec
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321863"
---
# <a name="get-xbox-live-club-data"></a>Obter dados de clube do Xbox Live

Use este método na API de análise da Microsoft Store para obter dados de clube para seu [jogo habilitado para Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md). Essas informações também estão disponíveis na [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

> [!IMPORTANT]
> Esse método oferece suporte somente a jogos para Xbox ou que usam os serviços do Xbox Live. Esses jogos devem passar pelo [processo de aprovação de conceito](../gaming/concept-approval.md), que inclui jogos publicados por [parceiros da Microsoft](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners) e jogos enviados por meio do programa [ID@Xbox](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id). Esse método não oferece suporte no momento para jogos publicados pelo [Programa de Criadores do Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| OBTER    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados


| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de clube do Xbox Live.  |  Sim  |
| metricType | cadeia de caracteres | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **communitymanagerclub**.  |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de clube a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de clube a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de clube para o seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz que contém um objeto [ProductData](#productdata) que contém dados de clubes relacionados ao seu jogo e um objeto [XboxwideData](#xboxwidedata) que contém dados de clube para todos os clientes do Xbox Live. Esses dados são incluídos para fins de comparação com os dados para o seu jogo.  |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta. |


### <a name="productdata"></a>ProductData

Esse recurso contém dados de clube para seu jogo.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| date            |  cadeia de caracteres |   A data para os dados de clube.   |
|  applicationId               |    cadeia de caracteres     |  A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você recuperou dados de clube.   |
|  clubsWithTitleActivity               |    int     |  O número de clubes que estão socialmente envolvidos com seu jogo.   |     
|  clubsExclusiveToGame               |   int      |  O número de clubes que estão socialmente envolvidos exclusivamente com seu jogo.   |     
|  clubFacts               |   array      |   Contém um ou mais objetos [ClubFacts](#clubfacts) sobre cada um dos clubes socialmente envolvidos com seu jogo.   |


### <a name="xboxwidedata"></a>XboxwideData

Esse recurso contém dados médios do clube entre todos os clientes do Xbox Live.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|------|
| date            |  cadeia de caracteres |   A data para os dados de clube.   |
|  applicationId  |    cadeia de caracteres     |   No objeto **XboxwideData**, essa sequência sempre será o valor **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   int     |  Em média, o número de clubes com clientes que estão socialmente envolvidos com um jogo habilitado para Xbox Live.    |     
|  clubsExclusiveToGame               |   int      |  Em média, o número de clubes que estão socialmente envolvidos exclusivamente com um jogo habilitado para Xbox Live.   |     
|  clubFacts               |   objeto      |  Contém um objeto [ClubFacts](#clubfacts). Esse objeto não faz sentido no contexto do objeto **XboxwideData** e tem valores padrão.  |


### <a name="clubfacts"></a>ClubFacts

No objeto **ProductData**, este objeto contém os dados de um clube específico com atividades relacionadas ao seu jogo. No objeto **XboxwideData**, este objeto não tem sentido e tem valores padrão.

| Valor           | Tipo    | Descrição        |
|-----------------|---------|--------------------|
|  name            |  cadeia de caracteres  |   No objeto **ProductData**, este é o nome do clube. No objeto **XboxwideData**, sempre terá o valor **XBOXWIDE**.           |
|  memberCount               |    int     | No objeto **ProductData**, este é o número de membros no clube, excluindo não membros que estejam apenas visitando o clube. No objeto **XboxwideData**, sempre será 0.    |
|  titleSocialActionsCount               |    int     |  No objeto **ProductData**, este é o número de ações sociais que os membros do clube executaram em relação ao seu jogo. No objeto **XboxwideData**, sempre será 0   |
|  isExclusiveToGame               |    Booliano     |  No objeto **ProductData**, indica se o clube atual está socialmente envolvido de forma exclusiva com seu jogo. No objeto **XboxwideData**, sempre será verdadeiro.  |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de realizações Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados de hub jogos do Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados para múltiplos jogadores Xbox Live](get-xbox-live-multiplayer-data.md)
