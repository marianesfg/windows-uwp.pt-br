---
author: Xansky
description: Use este método na API de análise da Microsoft Store para obter os dados de conversões por canal agregados de um complemento durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter conversões de complemento por canal
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, conversões de complemento, canal
ms.localizationpriority: medium
ms.openlocfilehash: d64af06acb730160f7f9f7f0e9eda96074b3eac7
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5543051"
---
# <a name="get-add-on-conversions-by-channel"></a>Obter conversões de complemento por canal

Use este método na API de análise da Microsoft Store para obter as conversões agregadas por canal de um complemento durante um determinado intervalo de datas e outros filtros opcionais.

* Uma *conversão* significa que um cliente (conectado com uma conta da Microsoft) obteve recentemente uma licença para seu complemento (seja paga ou gratuita).
* O *canal* é o método em que um cliente chegou à página de listagem do seu app (por exemplo, por meio da Loja ou de uma [campanha de promoção de apps personalizada](../publish/create-a-custom-app-promotion-campaign.md)).

Essas informações também estão disponíveis no [Relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id) no painel do Centro de Desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de conversão do complemento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8. |  Sim  |
| inAppProductId | string | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do complemento para o qual você deseja recuperar dados de conversão.  | Sim  |
| startDate | date | A data de início no intervalo de datas de dados de conversão a serem recuperados. O padrão é 1/1/2016. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de conversão a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram o corpo da resposta. Cada instrução pode usar os operadores **eq** ou **ne**, e as instruções podem ser combinadas usando **and** ou **or**. Você pode especificar as seguintes cadeias de caracteres nas declarações de filtro.  Para obter descrições, consulte a seção [valores de conversão](#conversion-values) neste artigo. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Veja um exemplo de parâmetro *filter*: <em>filter = deviceType eq 'PC'</em>.</p> | Não   |
| aggregationLevel | string | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | string | Uma instrução que classifica os valores de dados resultantes de cada conversão. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<p/><ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>As linhas de dados retornadas conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações de obtenção de dados de conversão do app. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contém dados de conversão agregados do complemento. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de conversão](#conversion-values) a seguir.                     |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de dados de conversão para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>Valores de conversão

Os objetos na matriz *Valor* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | string | A primeira data no intervalo de datas dos dados de conversão. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| inAppProductId      | string  | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do complemento do qual você está recuperando dados de conversão.     |
| inAppProductName    | string  | O nome de exibição do complemento para o qual você está recuperando dados de conversão.   |
| applicationId       | string | A [ID da Loja](in-app-purchases-and-trials.md#store-ids) do app para o qual você está recuperando dados de conversão.     |
| applicationName     | string | O nome de exibição do app para o qual você está recuperando dados de conversão.        |
| appType          | string |  O tipo do produto para o qual você está recuperando dados de conversão. Para este método, o único valor com suporte é **Complemento**.            |
| customCampaignId           | string |  A cadeia de caracteres de ID para uma [campanha de promoção de apps personalizados](../publish/create-a-custom-app-promotion-campaign.md) associada ao app.   |
| referrerUriDomain           | string |  Especifica o domínio em que a listagem de apps com a ID de campanha de promoção do app personalizada foi ativada.   |
| channelType           | string |  Uma das cadeias de caracteres a seguir que especifica o canal para a conversão:<ul><li><strong>CustomCampaignId</strong></li><li><strong>Tráfego da loja</strong></li><li><strong>Outro</strong></li></ul>    |
| storeClient         | string | A versão da Loja onde ocorreu a conversão. Atualmente, o único valor com suporte é **SFC**.    |
| deviceType          | string | Uma das seguintes cadeias de caracteres:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | string | O código de país ISO 3166 do mercado onde ocorreu a conversão.    |
| clickCount              | number  |     O número de cliques de cliente no link de listagem de seu app.      |           
| conversionCount            | number  |   O número de conversões de cliente.         |         


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso Add-On",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "Add-On",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "CN",
      "clickCount": 1,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
