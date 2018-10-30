---
author: Xansky
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: Use esse método na API de análise da Microsoft Store para obter os dados de desempenho do anúncio agregados de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter dados de desempenho de anúncios
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, anúncios, desempenho
ms.localizationpriority: medium
ms.openlocfilehash: 30f24b2494470f60371e466bb4d1e1878475d963
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5756224"
---
# <a name="get-ad-performance-data"></a>Obter dados de desempenho de anúncios


Use esse método na API de análise da Microsoft Store para obter os dados de desempenho dos aplicativos agregados de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Este método retorna os dados no formato JSON.

Esse método retorna os mesmos dados fornecidos pelo [Relatório de desempenho de anúncios](../publish/advertising-performance-report.md) no painel do Centro de Desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

Para saber mais, consulte [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md).

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição           |
|---------------|--------|--------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

Para recuperar dados de desempenho do anúncio de um aplicativo específico, use o parâmetro *applicationId*. Para recuperar dados de desempenho do anúncio de todos os aplicativos associados à conta de desenvolvedor, omita o parâmetro *applicationId*.

| Parâmetro     | Tipo   | Descrição     | Obrigatório |
|---------------|--------|-----------------|----------|
| applicationId   | string    | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app cujos dados de desempenho do anúncio você deseja recuperar.  |    Não      |
| startDate   | date    | A data de início no intervalo de datas dos dados de desempenho do anúncio a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos 30 dias. |    Não      |
| endDate   | date    | A data de término no intervalo de datas dos dados de desempenho do anúncio a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos um dia. |    Não      |
| top   | int    | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |    Não      |
| skip   | int    | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |    Não      |
| filter   | string    | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. |    Não      |
| aggregationLevel   | string    | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. |    Não      |
| orderby   | string    | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitId</strong></li></ul><p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |    Não      |
| groupby   | string    | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:</p><ul><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>date</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby = applicationId&amp;aggregationLevel = week</em></p> |    Não      |


### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* do corpo da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Aqui está um parâmetro *filter* de exemplo:

-   *filter=market eq 'US' and deviceType eq 'phone'*

Para obter uma lista dos campos com suporte, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campo | Descrição                                                              |
|--------|--------------------------------------------------------------------------|
| market    | Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado onde os anúncios foram veiculados. |
| deviceType    | Uma das seguintes cadeias de caracteres: <strong>PC/Tablet</strong> ou <strong>Phone</strong>. |
| adUnitId    | Uma cadeia de caracteres que especifica uma ID da unidade de anúncio a ser aplicada ao filtro. |
| pubCenterAppName    | Uma cadeia de caracteres que especifica o nome do pubCenter do aplicativo atual a ser aplicado ao filtro. |
| adProvider    | Uma cadeia de caracteres que especifica um nome de fornecedor do anúncio a ser aplicado ao filtro. |
| date    | Uma cadeia de caracteres que especifica uma data no formato AAAA/MM/DD a ser aplicada ao filtro. |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações para obter dados de desempenho do anúncio. Substitua o valor de *applicationId* pela ID da Loja de seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de desempenho do anúncio agregados. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de desempenho do anúncio](#ad-performance-values) a seguir.                                                                                                                      |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 5, mas houver mais de 5 itens de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                          |


### <a name="ad-performance-values"></a>Valores de desempenho do anúncio

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor               | Tipo   | Descrição                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | string | A primeira data no intervalo de datas dos dados de desempenho do anúncio. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId       | string | A ID da Loja do aplicativo do qual você está recuperando dados de desempenho do anúncio.     |
| applicationName     | string | O nome de exibição do aplicativo.                         |
| adUnitId           | string | A ID da unidade de anúncio.        |
| adUnitName           | string | O nome da unidade de anúncio, conforme especificado pelo desenvolvedor no painel do Centro de Desenvolvimento.              |
| adProvider           |  string  |  O nome do provedor de anúncios   |
| deviceType          | string | O tipo de dispositivo no qual os anúncios foram veiculados. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                              |
| market              | string | O código de país ISO 3166 do mercado onde os anúncios foram veiculados.             |
| accountCurrencyCode     | string | O código da moeda da conta.        |
| pubCenterAppName       |  string  |   O nome do aplicativo pubCenter associado ao aplicativo no Centro de Desenvolvimento.   |
| adProviderRequests        | int | O número de solicitações de anúncio do provedor de anúncios especificado.                 |
| impressions           | int | O número de impressões do anúncio.        |
| clicks            | int | O número de cliques no anúncio.       |
| revenueInAccountCurrency       | number | A receita, na moeda do país/região da conta.       |
| requests              | int | O número de solicitações de anúncio.                 |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de desempenho de anúncios](../publish/advertising-performance-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
