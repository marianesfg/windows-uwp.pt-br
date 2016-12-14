---
author: mcleanbyron
ms.assetid: 
description: "Use este método na API de análise da Windows Store para obter dados detalhados de um erro específico do seu app."
title: Obter detalhes de um erro em seu app
translationtype: Human Translation
ms.sourcegitcommit: 767097f068630e5ec171415c05d6dc395c8b26b3
ms.openlocfilehash: cd72e58ec252c556cf0f2e5ce071744e8fac6c9e

---

# <a name="get-details-for-an-error-in-your-app"></a>Obter detalhes de um erro em seu app

Use este método na API de análise da Windows Store para obter dados detalhados de um erro específico do seu app no formato JSON. Este método só pode recuperar detalhes dos erros que ocorreram nos últimos 30 dias. Os dados de erros detalhados também estão disponíveis na seção **Falhas** do [Relatório de integridade](../publish/health-report.md) no painel do Centro de Desenvolvimento do Windows.

Antes de usar este método, primeiro você deve usar o método [obter dados de relatório de erros](get-error-reporting-data.md) para recuperar a ID do erro para o qual você deseja obter informações detalhadas.

## <a name="prerequisites"></a>Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.
* Obtenha a ID do erro para o qual você deseja obter informações detalhadas. Para obter essa ID, use o método [obter dados de relatório de erros](get-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |

<span/> 

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID da Loja do app para o qual você deseja recuperar dados de erros detalhados. A ID da Loja está disponível na [página de identidade do app](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Um exemplo de ID da Loja é 9WZDNCRFJ3Q8. |  Sim  |
| failureHash | string | A ID exclusiva do erro para o qual você deseja obter informações detalhadas. Para obter esse valor para o erro no qual você está interessado, use o método [obter dados de relatório de erros](get-error-reporting-data.md) e o valor **failureHash** no corpo da resposta desse método. |  Sim  |
| startDate | date | A data de início no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é 30 dias antes da data atual. |  Não  |
| endDate | date | A data de término no intervalo de datas dos dados de erros detalhados a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10 e skip=0 recuperam as primeiras 10 linhas de dados, top=10 e skip=10 recuperam as próximas 10 linhas de dados e assim por diante. |  Não  |
| filter |string  | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. | Não   |
| orderby | string | Uma instrução que ordena os valores dos dados de resultado. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |

<span/>
 
### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Estes são alguns exemplos dos parâmetros *filter*:

-   *filter=market eq 'US' e osVersion eq 'Windows 10'*
-   *filter=market ne 'US' e osVersion ne 'Windows 8'*

Para obter uma lista dos campos com suporte, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campos        |  Descrição        |
|---------------|-----------------|
| osVersion | Uma das seguintes cadeias de caracteres:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| osBuild | O número da compilação do sistema operacional no qual o app estava em execução quando o erro ocorreu. |
| market | Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado do dispositivo no qual o app era executado quando o erro ocorreu. |
| deviceType | Uma das seguintes cadeias de caracteres que especifica o tipo do dispositivo no qual o app era executado quando o erro ocorreu:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceModel | Uma cadeia de caracteres que especifica o modelo do dispositivo no qual o app era executado quando o erro ocorreu. |
| cabId | A ID exclusiva do arquivo CAB que está associado a esse erro. |
| cabExpirationTime | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601. |
| packageVersion | O versão do pacote do aplicativo que está associado a esse erro. |

<span/> 

### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de erros detalhados. Substitua o valor de *applicationId* pela ID da Loja de seu app.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo    | Descrição    |
|------------|---------|------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de erros detalhados. Para saber mais sobre os dados em cada objeto, consulte a seção de [Valores de detalhes de erros](#error-detail-values) a seguir.          |
| @nextLink  | string  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10, mas houver mais de 10 linhas de erros para a consulta. |
| TotalCount | inumber | O número total de linhas no resultado dos dados da consulta.        |

<span id="error-detail-values"/>
### <a name="error-detail-values"></a>Valores de detalhes de erros

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor           | Tipo    | Descrição     |
|-----------------|---------|----------------------------|
| date            | string  | A primeira data no intervalo de datas dos dados do erro. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId   | string  | A ID da Loja do app para o qual você recuperou dados de erros detalhados.      |
| failureName     | string  | O nome do erro. Esse é o mesmo nome que aparece na seção **Falhas** do [Relatório de integridade](../publish/health-report.md) no painel do Centro de Desenvolvimento do Windows.            |
| failureHash     | string  | O identificador exclusivo do erro.     |
| osVersion       | string  | A versão do sistema operacional no qual o erro ocorreu.    |
| market          | string  | O código de país ISO 3166 do mercado do dispositivo.     |
| deviceType      | string  | O tipo de dispositivo no qual o erro ocorreu.     |
| packageVersion  | string  | O versão do pacote do aplicativo que está associado a esse erro.    |
| osBuild         | string  | O número de versão do sistema operacional no qual o erro ocorreu.       |
| cabId           | string  | A ID exclusiva do arquivo CAB que está associado a esse erro.   |
| cabExpirationTime  | string  | A data e a hora em que o arquivo CAB expirou e não pôde mais ser baixado, no formato ISO 8601.   |
| deviceModel           | string  | Uma cadeia de caracteres que especifica o modelo do dispositivo no qual o app era executado quando o erro ocorreu.   |
| cabDownloadable           | booliano  | Indica se o arquivo CAB está disponível para download para esse usuário.   |

<span/> 

### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2015-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "deviceType": "Windows.Desktop",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter o rastreamento de pilha de um erro em seu app](get-the-stack-trace-for-an-error-in-your-app.md)



<!--HONumber=Dec16_HO1-->


