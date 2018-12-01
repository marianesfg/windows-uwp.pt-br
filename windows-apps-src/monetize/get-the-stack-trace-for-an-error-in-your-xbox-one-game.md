---
description: Use este método na API de análise da Microsoft Store para obter os rastreamentos de pilha de um erro em seu Xbox One jogo.
title: Obter o rastreamento de pilha de um erro em seu Xbox One jogo
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, rastreamento de pilha, erro
ms.localizationpriority: medium
ms.openlocfilehash: fd43305c54245c3281a0e840d3df4c5c87ff7ad8
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332823"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>Obter o rastreamento de pilha de um erro em seu Xbox One jogo

Use este método na análise da Microsoft Store API para obter os rastreamentos de pilha de um erro em seu Xbox One jogo que ingerido por meio do Portal de desenvolvedor do Xbox (XDP) e está disponível no painel do Centro de parceiro de análise XDP. Este método pode apenas baixar o rastreamento de pilha de um erro que ocorreu nos últimos 30 dias.

Antes de usar esse método, primeiro você deve usar o método [obter detalhes de um erro em seu jogo Xbox One](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar a ID do arquivo CAB que está associado ao erro para o qual você deseja recuperar o rastreamento de pilha.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter essa ID, use o método [obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar os detalhes de um erro específico em seu aplicativo e use o valor de **cabId** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do jogo Xbox One para o qual você está recuperando um rastreamento de pilha. Para obter a ID do produto do jogo, navegue até seu jogo no Portal de Desenvolvedor do Xbox (XDP) e recupere a ID do produto da URL. Como alternativa, se você baixar os dados de integridade do relatório de análise do Partner Center do Windows, a ID do produto estará incluída no arquivo. tsv. |  Sim  |
| cabId | string | A ID exclusiva do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter essa ID, use o método [obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md) para recuperar os detalhes de um erro específico em seu aplicativo e use o valor de **cabId** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter um rastreamento de pilha para um Xbox One jogo usando esse método. Substitua o valor *applicationId* com a ID do produto para o seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
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
| function | string  |  O nome da função que é chamada nesse quadro da pilha. Isso está disponível somente se seu jogo incluir símbolos para o executável ou uma biblioteca.              |
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

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório para seu Xbox One erros jogo](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obter detalhes de um erro em seu Xbox One jogo](get-details-for-an-error-in-your-xbox-one-game.md)
* [Baixar o arquivo CAB para um erro em seu jogo Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
