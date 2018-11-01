---
author: Xansky
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Use esse método na API de análise da Microsoft Store para obter dados de desempenho da campanha publicitária agregados do aplicativo especificado durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de desempenho da campanha publicitária
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, campanhas publicitárias
ms.localizationpriority: medium
ms.openlocfilehash: 337342378fc42b33c0bddb980b143ab195305458
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5881679"
---
# <a name="get-ad-campaign-performance-data"></a>Obter dados de desempenho da campanha publicitária


Use esse método na API de análise da Microsoft Store para obter um resumo agregado dos dados de desempenho da campanha publicitária promocional dos aplicativos durante um determinado intervalo de datas e outros filtros opcionais. Este método retorna os dados no formato JSON.

Esse método retorna os mesmos dados fornecidos pelo [Relatório de anúncios de instalação de aplicativos](../publish/app-install-ads-reports.md) no painel do Centro de Desenvolvimento do Windows. Para obter mais informações sobre campanhas publicitárias, consulte [Criar uma campanha publicitária para seu aplicativo](../publish/create-an-ad-campaign-for-your-app.md).

Para criar, atualizar ou recuperar detalhes de campanhas publicitárias, você pode usar os métodos de [Gestão de campanhas publicitárias](manage-ad-campaigns.md) na [API de promoções da Microsoft Store](run-ad-campaigns-using-windows-store-services.md).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                |
|---------------|--------|---------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Para recuperar dados de desempenho da campanha publicitária de um aplicativo específico, use o parâmetro *applicationId*. Para recuperar dados de desempenho do anúncio de todos os aplicativos associados à conta de desenvolvedor, omita o parâmetro *applicationId*.

| Parâmetro     | Tipo   | Descrição     | Obrigatório |
|---------------|--------|-----------------|----------|
| applicationId   | string    | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app cujos dados de desempenho da campanha publicitária você deseja recuperar. |    Não      |
|  startDate  |  date   |  A data de início no intervalo de datas dos dados de desempenho da campanha publicitária a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos 30 dias.   |   Não    |
| endDate   |  date   |  A data de término no intervalo de datas dos dados de desempenho da campanha publicitária a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos um dia.   |   Não    |
| top   |  int   |  O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.   |   Não    |
| skip   | int    |  O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.   |   Não    |
| filter   |  string   |  Uma ou mais instruções que filtram as linhas na resposta. O único filtro compatível é **campaignId**. Cada instrução pode usar os operadores **eq** ou **ne**, e as instruções podem ser combinadas usando **and** ou **or**.  Aqui está um parâmetro *filter* de exemplo: ```filter=campaignId eq '100023'```.   |   Não    |
|  aggregationLevel  |  string   | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>.    |   Não    |
| orderby   |  string   |  <p>Uma instrução que ordena os valores de dados do resultado dos dados de desempenho da campanha publicitária. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,campaignId</em></p>   |   Não    |
|  groupby  |  string   |  <p>Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby = applicationId&amp;aggregationLevel = week</em></p>   |   Não    |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações para obter dados de desempenho da campanha publicitária.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição  |
|------------|--------|---------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de desempenho da campanha publicitária agregados. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [Objeto de desempenho da campanha](#campaign-performance-object) abaixo.          |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 5, mas houver mais de 5 itens de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>Objeto de desempenho da campanha

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor               | Tipo   | Descrição            |
|---------------------|--------|------------------------|
| date                | string | A primeira data no intervalo de datas dos dados de desempenho da campanha publicitária. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId       | string | A ID da Loja do aplicativo do qual você está recuperando dados de desempenho da campanha publicitária.                     |
| campaignId     | string | A ID da campanha publicitária.           |
| lineId     | string |    A ID da [linha de entrega](manage-delivery-lines-for-ad-campaigns.md) da campanha publicitária que gerou esses dados de desempenho.        |
| currencyCode              | string | O código da moeda do orçamento da campanha.              |
| spend          | string |  O valor no orçamento gasto na campanha publicitária.     |
| impressions           | long | O número de impressões de anúncios da campanha.        |
| installs              | long | O número de instalações de aplicativos relacionados à campanha.   |
| clicks            | long | O número de cliques em anúncios da campanha.      |
| iapInstalls            | comprimento | O número de instalações de complemento (também chamado de compra no aplicativo ou IAP) relacionado à campanha.      |
| activeUsers            | comprimento | O número de usuários que clicaram em um anúncio que faz parte da campanha e retornaram ao aplicativo.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Criar uma campanha publicitária para seu app](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Veicular campanhas publicitárias usando os serviços da Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
