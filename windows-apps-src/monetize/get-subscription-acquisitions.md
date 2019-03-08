---
description: Use esse método na API de análise de Microsoft Store para obter dados de aquisição para uma assinatura de complemento durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter aquisições de complemento de assinatura
ms.date: 01/25/18
ms.topic: article
keywords: Windows 10, uwp, serviços de Store, API, as assinaturas de análise da Microsoft Store
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594771"
---
# <a name="get-subscription-add-on-acquisitions"></a>Obter aquisições de complemento de assinatura

Use esse método na API de análise de Microsoft Store para obter dados de agregação de aquisição para assinaturas de complemento para o seu aplicativo durante um determinado intervalo de datas e outros filtros opcionais.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição          |
|---------------|--------|--------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | O [Store ID](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você deseja recuperar os dados de aquisição de complemento de assinatura. |  Sim  |
| subscriptionProductId  | cadeia de caracteres | O [Store ID](in-app-purchases-and-trials.md#store-ids) do complemento de assinatura para o qual você deseja recuperar os dados de aquisição. Se você não especificar esse valor, esse método retorna dados de aquisição para todos os complementos de assinatura para o aplicativo especificado.  | Não  |
| startDate | date | A data de início no intervalo de datas de dados de aquisição de complemento de assinatura a recuperar. O padrão é a data atual. |  Não  |
| endDate | date | Data final no intervalo de datas de dados de aquisição de complemento de assinatura a recuperar. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão se não especificado é 100. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=100 e skip=0 recuperam as primeiras 100 linhas de dados, top=100 e skip=100 recuperam as próximas 100 linhas de dados e assim por diante. |  Não  |
| filter | cadeia de caracteres  | Uma ou mais instruções que filtram o corpo da resposta. Cada instrução pode usar os operadores **eq** ou **ne**, e as instruções podem ser combinadas usando **and** ou **or**. Você pode especificar as seguintes cadeias de caracteres nas instruções de filtro (esses correspondem aos [valores no corpo da resposta](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Aqui está um exemplo *filtro* parâmetro: <em>filtro = Data eq ' 2017-07-08'</em>.</p> | Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | cadeia de caracteres | Uma instrução que ordena o resultado de valores de dados para a aquisição de complemento cada assinatura. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>groupby = mercado&amp;aggregationLevel = semana</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstra como obter dados de assinatura de aquisição de complemento. Substitua os *applicationId* valor com a ID de Store apropriado para seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição         |
|------------|--------|------------------|
| Valor      | matriz  | Uma matriz de objetos que contêm dados de assinatura de agregação de aquisição de complemento. Para obter mais informações sobre os dados em cada objeto, consulte a [valores de aquisição da assinatura](#subscription-acquisition-values) seção abaixo.             |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se a **superior** parâmetro da solicitação for definido como 100, mas há mais de 100 linhas de dados de aquisição de complemento de assinatura para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Valores de aquisição da assinatura

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo    | Descrição        |
|---------------------|---------|---------------------|
| date                | cadeia de caracteres  | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| subscriptionProductId      | cadeia de caracteres  | O [Store ID](in-app-purchases-and-trials.md#store-ids) do complemento de assinatura para o qual você está recuperando dados de aquisição.    |
| subscriptionProductName    | cadeia de caracteres  | O nome de exibição do complemento de assinatura.         |
| applicationId       | cadeia de caracteres  | O [Store ID](in-app-purchases-and-trials.md#store-ids) do aplicativo para o qual você está recuperando dados de assinatura de aquisição de complemento.   |
| applicationName     | cadeia de caracteres  | O nome de exibição do app.     |
| skuId     | cadeia de caracteres  | A ID do [SKU](in-app-purchases-and-trials.md#products-skus) do complemento de assinatura para o qual você está recuperando dados de aquisição.     |
| deviceType          | cadeia de caracteres  |  Uma das sequências a seguir que especifica o tipo de dispositivo que concluiu a aquisição:<ul><li><strong>PC</strong></li><li><strong>Telefone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul>       |
| market           | cadeia de caracteres  | O código de país ISO 3166 do mercado onde ocorreu a aquisição.     |
| currencyCode         | cadeia de caracteres  | O código de moeda no formato ISO 4217 para vendas brutas antes dos impostos.       |
| grossSalesBeforeTax           | número inteiro  | As vendas brutas na moeda local especificada pelo *currencyCode* valor.     |
| totalActiveCount             | número inteiro  | O número de assinaturas ativas total durante o período de tempo especificado. Isso é equivalente à soma do *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*, e *lockedActiveCount* valores.  |
| totalChurnCount              | número inteiro  | A contagem total de assinaturas que foram desativadas durante o período de tempo especificado. Isso é equivalente à soma do *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*, e *otherChurnCount* valores.   |
| newCount            | número inteiro  | O número de novas aquisições de assinatura durante o período de tempo especificado, incluindo avaliações.    |
| renewCount     | número inteiro  | O número de renovações de assinaturas durante o período de tempo especificado, incluindo renovações iniciada pelo usuário e renovações automáticas.        |
| goodStandingActiveCount | número inteiro | O número de assinaturas que estavam ativas durante o período de tempo especificado e em que a data de validade é > = a *endDate* valor para a consulta.    |
| pendingGraceActiveCount | número inteiro | O número de assinaturas que estavam ativas durante o período de tempo especificado, mas teve uma falha de cobrança e em que a data de validade da assinatura é > = a *endDate* valor para a consulta.     |
| graceActiveCount | número inteiro | O número de assinaturas que estavam ativas durante o período de tempo especificado, mas teve uma falha de cobrança, e, em que:<ul><li>É a data de validade da assinatura < a *endDate* valor para a consulta.</li><li>O fim do período de carência é > = a *endDate* valor.</li></ul>        |
| lockedActiveCount | número inteiro | O número de assinaturas que estavam em *dunning* (ou seja, a assinatura está prestes a expirar e Microsoft está tentando adquirir fundos para renovar automaticamente a assinatura) especificado durante o tempo e do período em que:<ul><li>É a data de validade da assinatura < a *endDate* valor para a consulta.</li><li>O fim do período de carência é < = a *endDate* valor.</li></ul>        |
| billingChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado devido a uma falha ao processar uma tarifa de cobrança e em que as assinaturas foram previamente no dunning.        |
| nonRenewalChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado, porque eles não foram renovados.        |
| refundChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado, porque eles foram reembolsados.        |
| chargebackChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado devido a um estorno.        |
| earlyChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado enquanto eram válidas.        |
| otherChurnCount | número inteiro | O número de assinaturas que foram desativadas durante o período de tempo especificado por outros motivos.        |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)

 

 
