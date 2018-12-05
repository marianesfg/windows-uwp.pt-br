---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Use este método na API de análise da Microsoft Store para obter dados de opinião para um determinado intervalo de datas e outros filtros opcionais.
title: Obter avaliações de aplicativo
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, análises
ms.localizationpriority: medium
ms.openlocfilehash: 084158c0eb20f1d2a03c0e178064ac168c689872
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8705108"
---
# <a name="get-app-reviews"></a>Obter avaliações de aplicativo


Use este método na API de análise da Microsoft Store para obter dados de opinião em formato JSON para um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis no [relatório críticas](../publish/reviews-report.md) no Partner Center.

Depois que você recuperar críticas, você pode usar os métodos [obter as informações de resposta para avaliações de app](get-response-info-for-app-reviews.md) e [enviar respostas às críticas do app](submit-responses-to-app-reviews.md) na API de análises da Microsoft Store para responder às análises de forma programática.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|---------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de opinião.  |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de opinião a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de opinião a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes sequências:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>rating</strong></li></ul><p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |


### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados a operadores **eq** ou **ne** e alguns campos também dão suporte a operadores **contains**, **gt**, **lt**, **ge** e **le**. Instruções podem ser combinadas usando-se **and** ou **or**.

Este é um exemplo de cadeia de caracteres *filtro*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Para obter uma lista dos campos com suporte e os operadores de suporte para cada campo, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campos        | Operadores com suporte   |  Descrição        |
|---------------|--------|-----------------|
| market | eq, ne | Uma cadeia de caracteres que contém o código do país ISO 3166 do mercado do dispositivo. |
| osVersion  | eq, ne  | Uma das seguintes cadeias de caracteres:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | Uma das seguintes cadeias de caracteres:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq, ne  | Especifique <strong>true</strong> para filtrar por análises que foram revisadas; caso contrário, <strong>false</strong>.  |
| packageVersion  | eq, ne  | A versão do pacote do aplicativo que foi revisado.  |
| deviceModel  | eq, ne  | O tipo de dispositivo no qual o aplicativo foi avaliado.  |
| productFamily  | eq, ne  | Uma das seguintes cadeias de caracteres:<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Phone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>Other</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | A RAM física, em MB.  |
| deviceScreenResolution  | eq, ne  | A resolução de tela do dispositivo no formato &quot;<em>largura</em> x <em>altura</em>&quot;.   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | A capacidade do disco do armazenamento principal, em GB.  |
| isTouchEnabled  | eq, ne  | Especifique <strong>true</strong> para filtrar por dispositivos habilitados para toque; caso contrário, <strong>false</strong>.   |
| reviewerName  | eq, ne  |  O nome do revisor. |
| rating  | eq, ne, gt, lt, ge, le  | A classificação do app, em estrelas.  |
| reviewTitle  | eq, ne, contains  | O título da análise.  |
| reviewText  | eq, ne, contains  |  O conteúdo de texto da análise. |
| helpfulCount  | eq, ne  |  O número de vezes que a análise foi marcada como útil. |
| notHelpfulCount  | eq, ne  | O número de vezes que a análise foi marcada como não útil.  |
| responseDate  | eq, ne  | A data em que a resposta foi enviada.  |
| responseText  | eq, ne, contains  | O conteúdo de texto da resposta.  |
| id  | eq, ne  | A ID da revisão (este é um GUID).        |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de análise. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição      |
|------------|--------|------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de opinião. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de opinião](#review-values) a seguir.       |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de análise para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.  |

 
### <a name="review-values"></a>Valores de opinião

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição       |
|-----------------|---------|-------------------|
| date            | string  | A primeira data no intervalo de datas dos dados de análise. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId   | string  | A ID da Loja do app para o qual você está recuperando dados de análise.         |
| applicationName | string  | O nome de exibição do app.    |
| market          | string  | O código do país ISO 3166 do mercado em que a análise foi enviada.        |
| osVersion       | string  | A versão do sistema operacional no qual a análise foi enviada. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.            |
| deviceType      | string  | O tipo de dispositivo no qual a análise foi enviada. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.            |
| isRevised       | booliano | O valor **true** indica que a opinião foi revisada; caso contrário, **false**.   |
| packageVersion  | string  | A versão do pacote do aplicativo que foi revisado.        |
| deviceModel        | string  |O tipo de dispositivo no qual o aplicativo foi avaliado.     |
| productFamily      | string  | O nome da família de dispositivos. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.   |
| deviceRAM       | number  | A RAM física, em MB.    |
| deviceScreenResolution       | string  | A resolução de tela do dispositivo no formato "*largura* x *altura*".    |
| deviceStorageCapacity | number | A capacidade do disco do armazenamento principal, em GB. |
| isTouchEnabled | booliano | O valor **true** indica que é habilitado para toque; caso contrário, **false**. |
| reviewerName | string | O nome do revisor. |
| rating | number | A classificação do app, em estrelas. |
| reviewTitle | string | O título da análise. |
| reviewText | string | O conteúdo de texto da análise. |
| helpfulCount | number | O número de vezes que a análise foi marcada como útil. |
| notHelpfulCount | number | O número de vezes que a análise foi marcada como não útil. |
| responseDate | string | A data em que uma resposta foi enviada. |
| responseText | string | O conteúdo de texto da resposta. |
| id | string | A ID da revisão (este é um GUID). Você pode usar essa ID nos métodos [obter as informações de resposta para avaliações de aplicativo](get-response-info-for-app-reviews.md) e [enviar respostas às críticas do aplicativo](submit-responses-to-app-reviews.md). |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de opiniões](../publish/reviews-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter informações de resposta para opiniões sobre o app](get-response-info-for-app-reviews.md)
* [Enviar respostas às críticas do aplicativo](submit-responses-to-app-reviews.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter classificações de aplicativos](get-app-ratings.md)
