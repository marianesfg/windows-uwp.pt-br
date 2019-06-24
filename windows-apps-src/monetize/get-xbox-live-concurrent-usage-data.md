---
description: Use este método na API de análise da Microsoft Store para obter dados de uso simultâneo do Xbox Live.
title: Obter dados de uso simultâneo do Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, uso simultâneo
ms.localizationpriority: medium
ms.openlocfilehash: a1ceef92a533a230c2dca54a835578b56ceb809f
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321760"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obter dados de uso simultâneo do Xbox Live


Use este método na API de análise da Microsoft Store para obter dados de uso quase em tempo real (com latência de 5 a 15 minutos) sobre o número médio de clientes jogando seu [jogo habilitado para Xbox Live](https://docs.microsoft.com/gaming/xbox-live/index.md) a cada minuto, hora ou dia durante um intervalo de tempo especificado. Essas informações também estão disponíveis na [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

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
| applicationId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de uso simultâneo do Xbox Live.  |  Sim  |
| metricType | cadeia de caracteres | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **concurrency**.  |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de uso simultâneo a serem recuperados. Consulte a descrição de *aggregationLevel* para o comportamento padrão. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de uso simultâneo a serem recuperados. Consulte a descrição de *aggregationLevel* para o comportamento padrão. |  Não  |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes sequências: **minute**, **hour** ou **day**. Se não for especificado, o padrão será **day**. <p/><p/>Se você não especificar *startDate* ou *endDate*, o corpo da resposta assume como padrão o seguinte: <ul><li>**minuto**: Os últimos 60 registros de dados disponíveis.</li><li>**hora**: Os últimas 24 registros dos dados disponíveis.</li><li>**dia**: Os últimos 7 registros de dados disponíveis.</li></ul><p/>Os seguintes níveis de agregação têm limites de tamanho no número de registros que podem ser retornado. Os registros serão truncados se o período de tempo solicitado for muito grande. <ul><li>**minuto**: Registros de até 1440 (24 horas de dados).</li><li>**hora**: Registros de até 720 (30 dias de dados).</li><li>**dia**: Registros de até 60 (60 dias de dados).</li></ul>  |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de uso simultâneo para o seu jogo habilitado para Xbox Live. Essa solicitação recupera os dados para cada minuto entre 1º de fevereiro de 2018 e 2 de fevereiro de 2018. Substitua o valor de *applicationId* pela ID da Store do seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

O corpo da resposta contém uma matriz de objetos, cada um com um conjunto de dados de uso simultâneo de um minuto, hora ou dia especificado. Cada objeto que contém os valores a seguir.

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Count      | number  | O número médio de clientes jogando seu jogo habilitado para Xbox Live pelo minuto, hora ou dia especificado. <p/><p/>**Observação**&nbsp;&nbsp;O valor 0 indica que não houve nenhum usuário simultâneo durante o intervalo especificado, ou que ocorreu uma falha durante a coleta de dados de usuários simultâneos para o jogo durante o intervalo especificado. |
| Date  | cadeia de caracteres | A data e hora que especifica o minuto, hora ou dia durante o qual os dados de uso simultâneo ocorreram.  |
| SeriesName | cadeia de caracteres    | Isso sempre tem o valor **UserConcurrency**. |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação com agregação de dados por minuto.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>Tópicos relacionados

* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de realizações Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados de integridade do Xbox Live](get-xbox-live-health-data.md)
* [Obter dados de hub jogos do Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados do clube Xbox Live](get-xbox-live-club-data.md)
* [Obter dados para múltiplos jogadores Xbox Live](get-xbox-live-multiplayer-data.md)
