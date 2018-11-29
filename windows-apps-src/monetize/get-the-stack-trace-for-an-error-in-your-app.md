---
ms.assetid: b556a245-6359-4ddc-a4bd-76f9873ab694
description: Use este método na API de análise da Microsoft Store para obter os rastreamentos de pilha de um erro em seu app.
title: Obter o rastreamento de pilha de um erro em seu aplicativo
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, rastreamento de pilha, erro
ms.localizationpriority: medium
ms.openlocfilehash: ceffc7622f756eb17c8475208852e013df814554
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7986600"
---
# <a name="get-the-stack-trace-for-an-error-in-your-app"></a>Obter o rastreamento de pilha de um erro em seu aplicativo

Use este método na API de análise da Microsoft Store para obter os rastreamentos de pilha de um erro em seu app. Este método pode apenas baixar o rastreamento de pilha de um erro de app que ocorreu nos últimos 30 dias. Rastreamentos de pilha também estão disponíveis na seção **falhas** do [relatório de integridade](../publish/health-report.md) no Partner Center.

Antes de usar este método, primeiro você deve usar o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar a ID do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter essa ID, use o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabId** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID da Store do app para o qual você deseja obter a pilha de rastreamento. A ID da loja está disponível na [página de identidade de aplicativo](../publish/view-app-identity-details.md) no Partner Center. Uma ID da Store de exemplo é 9WZDNCRFJ3Q8. |  Sim  |
| cabId | string | A ID exclusiva do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter essa ID, use o método [obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabId** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter um rastreamento de pilha usando esse método. Substitua o valor de *applicationId* pela ID da Store de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/stacktrace?applicationId=9NBLGGGZ5QDR&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição                  |
|------------|---------|--------------------------------|
| Valor      | array   | Uma matriz de objetos contendo cada um deles um quadro de dados de rastreamento de pilha. Para obter mais informações sobre os dados em cada objeto, consulte a seção [Valores de rastreamento de pilha](#stack-trace-values) a seguir. |
| @nextLink  | cadeia  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta.          |


### <a name="stack-trace-values"></a>Valores de rastreamento de pilha

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição      |
|-----------------|---------|----------------|
| level            | string  |  O número do quadro que esse elemento representa na pilha de chamadas.  |
| image   | string  |   O nome do executável ou da imagem de biblioteca que contém a função que é chamada nesse quadro da pilha.           |
| function | string  |  O nome da função que é chamada nesse quadro da pilha. Estará disponível somente se o seu app incluir símbolos para o executável ou a biblioteca.              |
| offset     | string  |  O deslocamento de byte da instrução atual em relação ao início da função.      |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter detalhes de um erro em seu app](get-details-for-an-error-in-your-app.md)
* [Baixar o arquivo CAB de um erro em seu aplicativo](download-the-cab-file-for-an-error-in-your-app.md)
