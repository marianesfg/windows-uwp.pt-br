---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: "Use esse método na API de análise da Windows Store para obter dados de desempenho da campanha publicitária agregados do aplicativo especificado durante um determinado intervalo de datas e outros filtros opcionais."
title: "Obter dados de desempenho da campanha de anúncios"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 8859658675d31deccc3403e4862c47a54ca25b1a

---

# Obter dados de desempenho da campanha de anúncios


Use esse método na API de análise da Windows Store para obter um resumo agregado dos dados de desempenho da campanha publicitária dos aplicativos durante um determinado intervalo de datas e outros filtros opcionais. Este método retorna os dados no formato JSON.

Esse método retorna os mesmos dados fornecidos pelo [Relatório de anúncios de instalação de aplicativos](../publish/app-install-ads-reports.md) no painel do Centro de Desenvolvimento do Windows. Para obter mais informações sobre campanhas publicitárias, consulte [Criar uma campanha publicitária para seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

Para recuperar detalhes completos de todas as campanhas publicitárias que foram criadas para a conta do Centro de Desenvolvimento, é possível usar o [método Obter detalhes de todas as campanhas publicitárias](#get-details-for-all-ad-campaigns) descrito posteriormente neste artigo.

## Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## Solicitação


### Sintaxe da solicitação

| Método | URI da solicitação                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |

<span />

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                |
|---------------|--------|---------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span />

### Parâmetros solicitados

Para recuperar dados de desempenho da campanha publicitária de um aplicativo específico, use o parâmetro *applicationId*. Para recuperar dados de desempenho do anúncio de todos os aplicativos associados à conta de desenvolvedor, omita o parâmetro *applicationId*.

| Parâmetro     | Tipo   | Descrição     | Necessário |
|---------------|--------|-----------------|----------|
| applicationId   | string    | A ID da Loja do aplicativo cujos dados de desempenho da campanha publicitária você deseja recuperar. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9NBLGGH4R315. |    Não      |
|  startDate  |  date   |  A data de início no intervalo de datas dos dados de desempenho da campanha publicitária a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos 30 dias.   |   Não    |
| endDate   |  date   |  A data de término no intervalo de datas dos dados de desempenho da campanha publicitária a ser recuperada, no formato AAAA/MM/DD. O padrão é a data atual menos um dia.   |   Não    |
| top   |  int   |  O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.   |   Não    |
| skip   | int    |  O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.   |   Não    |
| filter   |  string   |  Uma ou mais instruções que filtram as linhas na resposta. O único filtro compatível é **campaignId**. Cada instrução pode usar os operadores **eq** ou **ne**, e as instruções podem ser combinadas usando **and** ou **or**.  Aqui está um parâmetro *filter* de exemplo: ```filter=campaignId eq '100023'```.   |   Não    |
|  aggregationLevel  |  string   | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>.    |   Não    |
| orderby   |  string   |  <p>Uma instrução que ordena os valores de dados do resultado dos dados de desempenho da campanha publicitária. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,campaignId</em></p>   |   Não    |
|  groupby  |  string   |  <p>Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby = applicationId&amp;aggregationLevel = week</em></p>   |   Não    |


<span />
 

### Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações para obter dados de desempenho da campanha publicitária.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta


### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de desempenho da campanha publicitária agregados. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [Objeto de desempenho da campanha](#campaign-performance-object) abaixo.                                                                                                                      |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 5, mas houver mais de 5 itens de dados para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                                                                                                                                                                             |

<span id="campaign-performance-object" />
### Objeto de desempenho da campanha

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor               | Tipo   | Descrição            |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | string | A primeira data no intervalo de datas dos dados de desempenho da campanha publicitária. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, esse valor será a primeira data nesse intervalo de datas. |
| applicationId       | string | A ID da Loja do aplicativo do qual você está recuperando dados de desempenho da campanha publicitária.                     |
| campaignId     | string | A ID da campanha publicitária.           |
| currencyCode              | string | O código da moeda do orçamento da campanha.              |
| spend          | string |  O valor no orçamento gasto na campanha publicitária.     |
| impressions           | long | O número de impressões de anúncios da campanha.        |
| installs              | long | O número de instalações de aplicativos relacionados à campanha.   |
| clicks            | long | O número de cliques em anúncios da campanha.      |

<span />

### Exemplo de resposta

O exemplo a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day& startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

<span id="get-details-for-all-ad-campaigns" />
## Obter detalhes de todas as campanhas publicitárias


Use esse método para obter detalhes de todas as campanhas publicitárias que foram criadas para aplicativos registrados na conta do Centro de Desenvolvimento do Windows. Este método retorna os dados no formato JSON. Para obter mais informações sobre campanhas publicitárias, consulte [Criar uma campanha publicitária para seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

### Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

### Solicitação


#### Sintaxe da solicitação

| Método | URI da solicitação                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign``` |

<span />

#### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span />

#### Parâmetros solicitados

| Parâmetro     | Tipo   | Descrição     | Necessário |
|---------------|--------|-----------------|----------|
| top           | int    | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 1000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |    Não      |
| skip           | int    | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=100 e skip=0 recuperam as primeiras 100 linhas de dados, top=100 e skip=100 recuperam as próximas 100 linhas de dados e assim por diante. |    Não      |


<span />
 

#### Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações para obter detalhes de todas as campanhas publicitárias.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign?top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>
```

### Resposta


#### Corpo da resposta

| Valor      | Tipo   | Descrição  |
|------------|--------|--------------|
| Valor      | array  | Uma matriz de objetos que contêm detalhes das campanhas publicitárias. Para obter mais informações sobre os dados em cada objeto, consulte a seção [Objeto da campanha](#campaign-object) abaixo.                        |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 5, mas houver mais de 5 itens de dados para a consulta. |                                   |

<span id="campaign-object" />
#### Objeto da campanha

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor               | Tipo   | Descrição          |
|---------------------|--------|------------------|
| id     | string | A ID da campanha publicitária. |
| name     | string | O nome da campanha publicitária. |
| createDate     | string | A data em que a campanha publicitária foi criada, no fuso horário UTC. |
| startDate     | string | A data de início da campanha publicitária, no fuso horário UTC. |
| endDate     | string | A data de término da campanha publicitária, no fuso horário UTC. |
| applicationId       | string | A ID da Loja do aplicativo à qual a campanha publicitária está associada. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9NBLGGH4R315.                    |
| budget     | number | O orçamento da campanha publicitária.           |
| budgetType    | string | O tipo de orçamento da campanha publicitária. Pode ser uma das seguintes cadeias de caracteres: **Monthly** ou **Total**.           |
| currencyCode              | string | O código da moeda do orçamento da campanha.              |
| type              | string | O tipo de campanha publicitária. Pode ser uma das seguintes cadeias de caracteres: **Paid**, **House** ou **Community**.             |
| status              | string | O status da campanha publicitária. Pode ser uma das seguintes cadeias de caracteres: **Active**, **UserPaused**, **Pending**, **Ended** ou **SystemPaused**.             |
| targetingType              | string | O tipo de segmentação da campanha publicitária. Pode ser uma das seguintes cadeias de caracteres: **Auto**, **Manual** ou **Segment**.             |
| target              | dicionário | Um dicionário de pares de chave e valor que contêm informações de segmentação da campanha. Para obter mais informações sobre esse objeto, consulte a seção [Objeto de destino](#target-object) abaixo.             |

<span />

<span id="target-object" />
#### Objeto de destino

Este recurso é um dicionário de pares de chave e valor a seguir.

| Chave               | Tipo   | Valor          |
|---------------------|--------|------------------|
| Country     | array |   Uma ou mais cadeias de caracteres que contêm os códigos alfa 2 ISO 3166 dos países ou das regiões segmentados pela campanha publicitária. |
| OsVersion     | array | Uma ou mais das seguintes cadeias de caracteres que especificam as versões de sistema operacional segmentadas pela campanha publicitária: **10** ou **8. x**.  |
| Age     |  array |  Uma ou mais das seguintes cadeias de caracteres que especifica a faixa etária segmentada: **Age13To17**, **Age18To24**, **Age25To34**, **Age35To49** ou **Age50AndAbove**. |
| Gender     |  array |  Uma ou mais das seguintes cadeias de caracteres que especifica o sexo segmentado: **Female** ou **Male**. |
| DeviceType       |  array  |  Uma ou mais das seguintes cadeias de caracteres que especifica o tipo de dispositivo de destino: **PC/Tablet** ou **Phone**.                  |
| Segment     |  array |    Um ou mais cadeias de caracteres que contêm as IDs dos segmentos de clientes.       |


<span />

#### Exemplo de resposta

O exemplo a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
   "Value": [
      {
         "id":31010026,
         "name": "Contoso App 1 Campaign",
         "createDate":"2016-07-14T10:01:21Z",
         "startDate":"2016-07-21T11:23:36Z",
         "applicationId":"9NBLGGH0XK8Z",
         "budget": 100,
         "budgetType":"Monthly",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Active",
         "targetingType": "Auto",
         "target": null
      },
      {
         "id":31010025,
         "name":"Contoso App 2 Campaign",
         "createDate":"2016-07-14T10:02:21Z",
         "startDate":"2016-07-21T11:18:44Z",
         "endDate":"2016-08-21T11:18:44Z",
         "applicationId":"9WZDNCRDX48S",
         "budget": 50,
         "budgetType":"Total",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Ended",
         "targetingType": "Manual",
         "target": {
            "Country": [
               "US",
               "BR",
               "FR",
               "IN",
               "IT"
            ],
            "OsVersion": [
               "8.X"
            ],
            "Age": [
               "Age18To24",
               "Age25To34",
               "Age35To49",
               "Age50AndAbove"
            ],
            "Gender": [
               "Male"
            ],
            "DeviceType": [
               "PC/Tablet",
               "Phone"
            ]
         }
      }
   ]
}
```

## Tópicos relacionados

* [Relatório de anúncios de instalação de aplicativos](../publish/app-install-ads-reports.md)
* [Criar uma campanha publicitária para seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


