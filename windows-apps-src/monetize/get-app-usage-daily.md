---
ms.assetid: 99DB5622-3700-4FB2-803B-DA447A1FD7B7
description: Use este método na API de análise da Microsoft Store para obter dados de uso diário do aplicativo para um determinado intervalo de datas e outros filtros opcionais.
title: Obter uso do app diariamente
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, serviços da loja, API, uso de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: d3460b61e6a9a7c36be6fd87c4dc7fcc1ab811d1
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485232"
---
# <a name="get-daily-app-usage"></a>Obter uso do app diariamente

Use este método na API de análise da Microsoft Store para obter dados de uso agregados (não incluindo o Xbox multijogador) no formato JSON para um aplicativo durante um determinado intervalo de datas (últimos 90 dias somente) e outros filtros opcionais. Essas informações também estão disponíveis no [relatório de uso](../publish/usage-report.md) no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                               |
|--------|---------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily``` |


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
| orderby       | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p>                                                                                                   |  Não        |
| groupby       | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta: <ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**dailySessionCount**</li><li>**engagementDurationMinutes**</li><li>**dailyActiveUsers**</li><li>**dailyActiveDevices**</li><li>**dailyNewUsers**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p>                                                                                                             |  Não        |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação de obtenção de dados de uso diário do aplicativo. Substitua o valor de *applicationId* pela ID da Loja do seu app.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagedaily?applicationId=XXXXXXXXXXXX&startDate=2018-08-10&endDate=2018-08-14 HTTP/1.1
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

| Valor                     | Tipo    | Descrição                                                               |
|---------------------------|---------|---------------------------------------------------------------------------|
| date                      | string  | A primeira data no intervalo de datas para os dados de uso. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas.        |
| applicationId             | string  | A ID da loja do aplicativo para o qual você está recuperando dados de uso.          |
| applicationName           | string  | O nome de exibição do app.                                              |
| deviceType                | string  | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo onde ocorreu o uso:<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**Tablet**</li><li>**IoT**</li><li>**Servidor**</li><li>**Holographic**</li><li>**Desconhecido**</li></ul>                                                                                                         |
| packageVersion            | string  | A versão do pacote onde ocorreu o uso.                          |
| market                    | string  | O código de país ISO 3166 do mercado em que o cliente usou seu aplicativo. |
| subscriptionName          | string  | Indica se o uso foi por meio do Xbox Game Pass.                            |
| dailySessionCount         | comprimento    | O número de sessões de usuário em um dia.                                  |
| engagementDurationMinutes | double  | Os minutos em que os usuários estão ativamente usando o aplicativo medido por um período distinto, a partir de quando o aplicativo é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade.             |
| dailyActiveUsers          | comprimento    | O número de clientes usando o aplicativo nesse dia.                           |
| dailyActiveDevices        | comprimento    | O número de dispositivos diários usados para interagir com seu aplicativo por todos os usuários.  |
| dailyNewUsers             | comprimento    | O número de clientes que usaram o aplicativo pela primeira vez nesse dia.    |
| monthlyActiveUsers        | comprimento    | O número de clientes usando o aplicativo nesse mês.                         |
| monthlyActiveDevices      | comprimento    | O número de dispositivos executando o aplicativo para um período distinto, a partir de quando o aplicativo é iniciado (início do processo) e termina quando ele é encerrado (final do processo) ou após um período de inatividade.                                      |
| monthlyNewUsers           | comprimento    | O número de clientes que usaram o aplicativo pela primeira vez nesse mês.  |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2018-08-10",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 17718,
      "engagementDurationMinutes": 582540.2,
      "dailyActiveUsers": 7078,
      "dailyActiveDevices": 6964,
      "dailyNewUsers": 1727,
      "monthlyActiveUsers": 123609,
      "monthlyActiveDevices": 116723,
      "monthlyNewUsers": 72271
    },
    {
      "date": "2018-08-11",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 19899,
      "engagementDurationMinutes": 655379.7,
      "dailyActiveUsers": 7877,
      "dailyActiveDevices": 7789,
      "dailyNewUsers": 2062,
      "monthlyActiveUsers": 123666,
      "monthlyActiveDevices": 116770,
      "monthlyNewUsers": 72276
    },
    {
      "date": "2018-08-12",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 21349,
      "engagementDurationMinutes": 735640.5,
      "dailyActiveUsers": 8456,
      "dailyActiveDevices": 8309,
      "dailyNewUsers": 2433,
      "monthlyActiveUsers": 124241,
      "monthlyActiveDevices": 117289,
      "monthlyNewUsers": 72732
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter mensal ussage de aplicativo](get-app-usage-monthly.md)
* [Obter aquisições de app](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
