---
description: Use este método na API de análise da Microsoft Store para obter dados de integridade do Xbox Live.
title: Obter dados de integridade do Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análise do Xbox Live, integridade, erros de cliente
ms.localizationpriority: medium
ms.openlocfilehash: 3b996d85776cb49d45cc5b699709b4eb107e7086
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709583"
---
# <a name="get-xbox-live-health-data"></a>Obter dados de integridade do Xbox Live


Use este método na API de análise da Microsoft Store para obter dados de integridade para seu [jogo habilitado para Xbox Live](../xbox-live/index.md). Essas informações também estão disponíveis no [relatório de análise do Xbox](../publish/xbox-analytics-report.md) no Partner Center.

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
| applicationId | sequência | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do jogo para o qual você deseja recuperar os dados de integridade do Xbox Live.  |  Sim  |
| metricType | string | Uma sequência que especifica o tipo de dados de análise do Xbox Live para recuperar. Para este método, especifique o valor **callingpattern**.  |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de integridade a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de integridade a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | Não   |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Se você especificar um ou mais campos *groupby*, qualquer outro campo *groupby* não especificado terá o valor **All** no corpo da resposta. |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de integridade para o seu jogo habilitado para Xbox Live. Substitua o valor de *applicationId* pela ID da Store do seu jogo.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de integridade. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir.                                                                                                                      |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.   |


Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | A ID da Store do jogo do qual você está recuperando dados de integridade.     |
| date                | string | A primeira data no intervalo de datas dos dados de integridade. Se a solicitação tiver especificado um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| deviceType          | string | Uma das sequências a seguir que especifica o tipo de dispositivo no qual seu jogo foi usado:<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (este valor indica um computador)</li><li><strong>Desconhecido</strong></li></ul>  |
| sandboxId     | string |   A ID de área restrita criada para o jogo. Pode ser um valor RETAIL ou a ID de uma área restrita privada.   |
| packageVersion     | string |  A versão do pacote de quatro partes para o jogo.  |
| callingPattern     | object |  Um objeto [callingPattern](#callingpattern) que fornece respostas do serviço, dispositivos e dados do usuário para cada código de status retornado por cada serviço Xbox Live usado pelo jogo no intervalo de datas especificado.     |


### <a name="callingpattern"></a>callingPattern

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| serviço      | string  |   O nome do serviço Xbox Live aos quais os dados de integridade estão relacionados.       |
| endpoint      | string  |   O ponto de extremidade do serviço Xbox Live aos quais os dados de integridade estão relacionados.        |
| httpStatusCode      | string  |  O código de status HTTP para este conjunto de dados de integridade.<p/><p/>**Observação**&nbsp;&nbsp;O código de status **429E** indica que a chamada de serviço foi bem-sucedida somente porque o [limite de taxa bem granular](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) foi isento durante a chamada. Uma taxa bem granular limitada pode ser imposta no futuro caso o serviço experimente um alto volume e, nesse caso, a chamada resultaria em um [código de status HTTP 429](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object).         |
| serviceResponses      | number  | O número de respostas de serviço que retornaram o código de status especificado.         |
| uniqueDevices      | number  |  O número de dispositivos exclusivos que chamaram o serviço e receberam o código de status especificado.       |
| uniqueUsers      | number  |   O número de usuários exclusivos que receberam o código de status especificado.       |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação. Os nomes e pontos de extremidade do serviço mostrados neste exemplo não são reais e são usados apenas para fins de exemplo.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de análise do Xbox Live](get-xbox-live-analytics.md)
* [Obter dados de conquistas do Xbox Live](get-xbox-live-achievements-data.md)
* [Obter dados do Hub de Jogos Xbox Live](get-xbox-live-game-hub-data.md)
* [Obter dados de clube do Xbox Live](get-xbox-live-club-data.md)
* [Obter dados de multijogador do Xbox Live](get-xbox-live-multiplayer-data.md)
