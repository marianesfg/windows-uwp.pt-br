---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Use este método na API de análise da Microsoft Store para obter os dados de aquisição agregados de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter aquisições de app
ms.date: 03/23/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, aquisições de aplicativo
ms.localizationpriority: medium
ms.openlocfilehash: 4ef5c9cedcb828f6c7df8a294fc4aad87e9f74ae
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662171"
---
# <a name="get-app-acquisitions"></a>Obter aquisições de app


Use este método na API de análise da Microsoft Store para obter dados agregados de aquisição em formato JSON de um aplicativo durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis na [relatório de aquisições](../publish/acquisitions-report.md) no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de aquisição.  |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filter=market eq 'US' and gender eq 'm'*. <p/><p/>Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul> | Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores de dados resultantes de cada aquisição. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>Data</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li><strong>Data</strong></li><li><strong>ApplicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>Data</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não  |

### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações de obtenção de dados de aquisição do aplicativo. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Uma matriz de objetos que contém dados agregados de aquisição do aplicativo. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de aquisição](#acquisition-values) a seguir.                                                                                                                      |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de aquisição para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                 |


### <a name="acquisition-values"></a>Valores de aquisição

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | cadeia de caracteres | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId       | cadeia de caracteres | A ID da Loja do aplicativo do qual você está recuperando dados de aquisição.     |
| applicationName     | cadeia de caracteres | O nome de exibição do app.   |
| deviceType          | cadeia de caracteres | Uma das seguintes sequências que especifica o tipo de dispositivo no qual ocorreu a aquisição:<ul><li><strong>PC</strong></li><li><strong>Telefone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| orderName           | cadeia de caracteres | O nome do pedido.  |
| storeClient         | cadeia de caracteres | Uma das sequências a seguir que indica a versão da Store onde ocorreu a aquisição:<ul><li>**Windows Phone Store (cliente)**</li><li>**Microsoft Store (cliente)** (ou **Windows Store (cliente)** se estiver consultando dados antes de 23 de março de 2018)</li><li>**Microsoft Store (Web)** (ou **Windows Store (Web)** se estiver consultando dados antes de 23 de março de 2018)</li><li>**Compra de volume por organizações**</li><li>**Outros**</li></ul>                                                                                            |
| osVersion           | cadeia de caracteres | Uma das seguintes sequências que especifica a versão do sistema operacional no qual ocorreu a aquisição:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Desconhecido</strong></li></ul>  |
| market              | cadeia de caracteres | O código de país ISO 3166 do mercado onde ocorreu a aquisição.  |
| gender              | cadeia de caracteres | Uma das sequências a seguir que especifica o gênero do usuário que fez a aquisição:<ul><li><strong>M</strong></li><li><strong>f</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| ageGroup            | cadeia de caracteres | Uma das sequências a seguir que especifica a faixa etária do usuário que fez a aquisição:<ul><li><strong>menos de 13</strong></li><li><strong>13-17</strong></li><li><strong>18 a 24</strong></li><li><strong>25-34</strong></li><li><strong>35 44</strong></li><li><strong>55 44</strong></li><li><strong>maior que 55</strong></li><li><strong>Desconhecido</strong></li></ul>  |
| acquisitionType     | cadeia de caracteres | Uma das sequências a seguir que indica o tipo de aquisição:<ul><li><strong>livre</strong></li><li><strong>Versão de avaliação</strong></li><li><strong>Pago</strong></li><li><strong>código promocional</strong></li><li><strong>IAP</strong></li><li><strong>Iap da assinatura</strong></li><li><strong>Público particular</strong></li><li><strong>Ordem de pré</strong></li><li><strong>Xbox Game Pass</strong> (ou <strong>Game Pass</strong> se estiver consultando dados anteriores a 23 de março de 2018)</li><li><strong>disco</strong></li><li><strong>Código pré-pago</strong></li></ul>   |
| acquisitionQuantity | number | O número de aquisições que ocorreram durante o nível de agregação especificado.    |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de aquisições](../publish/acquisitions-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de funil de aquisição de aplicativo](get-acquisition-funnel-data.md)
* [Obter as conversões de aplicativo por canal](get-app-conversions-by-channel.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
