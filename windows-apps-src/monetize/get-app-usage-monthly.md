---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Use esse método na API de análise de Microsoft Store para obter dados de uso mensal do aplicativo para um determinado intervalo de datas e outros filtros opcionais.
title: Obter uso do app mensalmente
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, serviços de Store, API, uso de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 48ad049b3f310f8b375a28d9695dd9280d686c43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662921"
---
# <a name="get-monthly-app-usage"></a>Obter uso do app mensalmente

Use esse método na API de análise do Microsoft Store para obter dados de uso de agregação (não incluindo Xbox com vários participantes) no formato JSON para um aplicativo durante um determinado intervalo de datas (últimos 90 dias apenas) e outros filtros opcionais. Essas informações também estão disponíveis na [relatório de uso](../publish/usage-report.md) no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro     | Tipo   |  Descrição                                                                                                    |  Obrigatório  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de opinião. |  Sim       |
| startDate     | date   | A data de início no intervalo de datas dos dados de opinião a serem recuperados. O padrão é a data atual.                   |  Não        |
| endDate       | date   | A data final no intervalo de datas de dados de opinião a serem recuperados. O padrão é a data atual.                     |  Não        |
| top           | int    | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.                          |  Não        |
| skip          | int    | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.                         |  Não        |  
| filter        |cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo da resposta e valor que estão associados com os operadores eq ou ne e instruções podem ser combinadas usando e ou ou. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro de filtro. Você pode especificar os campos a seguir do corpo da resposta: <ul><li>**mercado**</li><li>**deviceType**</li><li>**PackageVersion**</li></ul>                                                                                                                                              | Não         |  
| orderby       | cadeia de caracteres | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li>**Data**</li><li>**applicationId**</li><li>**ApplicationName**</li><li>**mercado**</li><li>**PackageVersion**</li><li>**deviceType**</li><li>**SubscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não        |
| groupby       | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir do corpo da resposta:<ul><li>**ApplicationName**</li><li>**SubscriptionName**</li><li>**deviceType**</li><li>**PackageVersion**</li><li>**mercado**</li><li>**Data**</li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li>**applicationId**</li><li>**SubscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não        |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra uma solicitação para obtenção de dados de uso mensal do aplicativo. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contêm dados de uso agregado. Para obter mais informações sobre os dados em cada objeto, consulte a tabela a seguir. |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de análise para a consulta.                 |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                          |

 
### <a name="usage-values"></a>Valores de uso

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor                     | Tipo    | Descrição                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | cadeia de caracteres  | A primeira data no intervalo de datas para os dados de uso. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas.                          |
| applicationId             | cadeia de caracteres  | A ID de Store do aplicativo para o qual você está recuperando dados de uso.                            |
| applicationName           | cadeia de caracteres  | O nome de exibição do app.                                                                |
| market                    | cadeia de caracteres  | O código do país ISO 3166 de mercado em que o cliente usou seu aplicativo.                   |
| packageVersion            | cadeia de caracteres  | A versão do pacote no qual ocorreu o uso.                                            |
| deviceType                | cadeia de caracteres  | Uma das seguintes cadeias de caracteres que especifica o tipo de dispositivo no qual ocorreu o uso:<ul><li>**PC**</li><li>**Telefone**</li><li>**Console**</li><li>**Tablet**</li><li>**IoT**</li><li>**Servidor**</li><li>**Holographic**</li><li>**Desconhecido**</li></ul>                                                                                                                           |
| subscriptionName          | cadeia de caracteres  | Indica se o uso for feita através do Xbox Game Pass.                                              |
| monthlySessionCount       | long    | O número de sessões de usuário durante esse mês.                                              |
| engagementDurationMinutes | double  | Os minutos em que os usuários são ativamente usando seu aplicativo medido por um período distinto de tempo, começando quando o aplicativo for iniciado (início do processo) e termina quando ele termina (final do processo) ou após um período de inatividade.                               |
| monthlyActiveUsers        | long    | O número de clientes que usam o aplicativo nesse mês.                                           |
| monthlyActiveDevices      | long    | O número de dispositivos que executam seu aplicativo por um período distinto de tempo, começando quando o aplicativo for iniciado (início do processo) e terminam quando ele termina (final do processo) ou após um período de inatividade.                                                        |
| monthlyNewUsers           | long    | O número de clientes que usaram o aplicativo pela primeira vez nesse mês.                    |
| averageDailyActiveUsers   | double  | O número médio de clientes que usam o aplicativo em uma base diária.                             |
| averageDailyActiveDevices | double  | O número médio de dispositivos usados para interagir com seu aplicativo por todos os usuários em uma base diária. |


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

* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter ussage diária do aplicativo](get-app-usage-daily.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
