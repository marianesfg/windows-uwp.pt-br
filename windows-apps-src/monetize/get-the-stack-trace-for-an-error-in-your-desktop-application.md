---
description: Use este método na API de análise da Microsoft Store para obter os rastreamentos de pilha de um erro em seu aplicativo da área de trabalho.
title: Obter o rastreamento de pilha de um erro em seu aplicativo da área de trabalho
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, rastreamento de pilha, erro, aplicativo da área de trabalho
ms.localizationpriority: medium
ms.openlocfilehash: 8cc8aaef2b26af88234efe62bf7cf1cb998e19bc
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8795534"
---
# <a name="get-the-stack-trace-for-an-error-in-your-desktop-application"></a>Obter o rastreamento de pilha de um erro em seu aplicativo da área de trabalho

Use este método na da API de análise da Microsoft Store para obter o rastreamento de pilha para um erro em um aplicativo da área de trabalho que você adicionou ao [programa do aplicativo de área de trabalho do Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este método pode apenas baixar o rastreamento de pilha de um erro que ocorreu nos últimos 30 dias. Rastreamentos de pilha também estão disponíveis no [relatório de integridade](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicativos da área de trabalho no Partner Center.

Antes de usar este método, primeiro você deve usar o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar o hash da ID do arquivo CAB associada ao erro para o qual você deseja recuperar o rastreamento de pilha.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha o hash da ID do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter esse valor, use o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar os detalhes de um erro específico em seu app, e use o valor de **cabIdHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |
 

### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do aplicativo da área de trabalho para o qual você deseja obter um rastreamento de pilha. Para obter a ID do produto de um aplicativo da área de trabalho, abra qualquer [relatório de análise para seu aplicativo da área de trabalho no Partner Center](https://msdn.microsoft.com/library/windows/desktop/mt826504) (por exemplo, o **relatório de integridade**) e recupere a ID do produto da URL. |  Sim  |
| cabIdHash | string | O hash da ID exclusiva do arquivo CAB associado ao erro para o qual você deseja recuperar o rastreamento de pilha. Para obter esse valor, use o método [obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md) para recuperar os detalhes de um erro específico em seu aplicativo, e use o valor de **cabIdHash** no corpo da resposta desse método. |  Sim  |

 
### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra como obter um rastreamento de pilha usando esse método. Substitua os parâmetros *applicationId* e *cabIdHash* pelos valores apropriados para seu aplicativo da área de trabalho.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/stacktrace?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
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
* [Obter dados de relatório de erros do seu aplicativo da área de trabalho](get-desktop-application-error-reporting-data.md)
* [Obter detalhes de um erro em seu aplicativo da área de trabalho](get-details-for-an-error-in-your-desktop-application.md)
* [Baixar o arquivo CAB de um erro em seu aplicativo da área de trabalho](download-the-cab-file-for-an-error-in-your-desktop-application.md)
