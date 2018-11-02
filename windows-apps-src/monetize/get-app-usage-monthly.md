---
author: Xansky
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Use este método na API de análise da Microsoft Store para obter dados mensal de uso de aplicativo para um determinado intervalo de datas e outros filtros opcionais.
title: Obter uso do app mensalmente
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, serviços da loja, API, uso de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 3d42cf9f0ed0827b1d5c451ad9fed077ef6acc6b
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5946107"
---
# <a name="get-monthly-app-usage"></a>Obter uso do app mensalmente

Use este método na API de análise da Microsoft Store para obter dados de uso agregados (não incluindo o Xbox multijogador) no formato JSON para um aplicativo durante um determinado intervalo de datas (últimos 90 dias somente) e outros filtros opcionais. Essas informações também estão disponíveis no [relatório de uso](../publish/usage-report.md) no painel do Centro de desenvolvimento do Windows.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro     | Tipo   |  Descrição                                                                                                    |  Obrigatório  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de opinião. |  Sim       |
| startDate     | date   | A data de início no intervalo de datas dos dados de opinião a serem recuperados. O padrão é a data atual.                   |  Não        |
| endDate       | date   | A data final no intervalo de datas de dados de opinião a serem recuperados. O padrão é a data atual.                     |  Não        |
| top           | int    | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.                          |  Não        |
| skip          | int    | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.                         |  Não        |  
| filter        |string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores eq ou ne, e as instruções podem ser combinadas usando-se and ou or. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro filter. Você pode especificar os campos a seguir do corpo da resposta: <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | Não         |  
| orderby       | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não        |
| groupby       | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não        |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados mensal de uso de aplicativo. Substitua o valor de *applicationId* pela ID da Loja do seu app.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de uso agregados. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir. |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de análise para a consulta.                 |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                          |

 
### <a name="usage-values"></a>Valores de uso

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor                     | Tipo    | Descrição                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | string  | A primeira data no intervalo de datas para os dados de uso. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas.                          |
| applicationId             | string  | A ID da loja do aplicativo para o qual você está recuperando dados de uso.                            |
| applicationName           | string  | O nome de exibição do app.                                                                |
| market                    | string  | O código de país ISO 3166 do mercado em que o cliente usou seu aplicativo.                   |
| packageVersion            | string  | A versão do pacote onde ocorreu o uso.                                            |
| deviceType                | string  | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo onde ocorreu o uso:<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**Tablet**</li><li>**IoT**</li><li>**Servidor**</li><li>**Holographic**</li><li>**Desconhecido**</li></ul>                                                                                                                           |
| subscriptionName          | string  | Indica se o uso foi por meio do Xbox Game Pass.                                              |
| monthlySessionCount       | comprimento    | O número de sessões de usuário durante esse mês.                                              |
| engagementDurationMinutes | double  | Os minutos em que os usuários estão ativamente usando o aplicativo medido por um período distinto, a partir de quando o aplicativo é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade.                               |
| monthlyActiveUsers        | comprimento    | O número de clientes que estão usando o aplicativo naquele mês.                                           |
| monthlyActiveDevices      | comprimento    | O número de dispositivos executando o aplicativo para um período distinto, a partir de quando o aplicativo é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade.                                                        |
| monthlyNewUsers           | comprimento    | O número de clientes que usaram o aplicativo pela primeira vez nesse mês.                    |
| averageDailyActiveUsers   | double  | O número médio de clientes que usam o aplicativo no dia a dia.                             |
| averageDailyActiveDevices | double  | O número médio de dispositivos usados para interagir com seu aplicativo por todos os usuários diariamente. |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter ussage diários do aplicativo](get-app-usage-daily.md)
* [Obter aquisições de app](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
