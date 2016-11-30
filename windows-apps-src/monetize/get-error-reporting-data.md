---
author: mcleanbyron
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: "Use este método na API de análise da Windows Store para obter dados de relatório de erros agregados para um determinado intervalo de datas e outros filtros opcionais."
title: "Obter dados de relatório de erros"
translationtype: Human Translation
ms.sourcegitcommit: 7b73682ea36574f8b675193a174d6e4b4ef85841
ms.openlocfilehash: 89b1c9b44aaabb49f78953877ae11d2d7a0a2a2f

---

# Obter dados de relatório de erros

Use este método na API de análise da Windows Store para obter dados de relatório de erros agregados em formato JSON para um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis no [Relatório de integridade](../publish/health-report.md) no painel do Centro de Desenvolvimento do Windows.

## Pré-requisitos


Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Windows Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## Solicitação


### Sintaxe da solicitação

| Método | URI da solicitação                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |

<span/> 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |

<span/> 

### Parâmetros solicitados

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parâmetro</th>
<th align="left">Tipo</th>
<th align="left">Descrição</th>
<th align="left">Obrigatório</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">string</td>
<td align="left">A ID da Loja do aplicativo para o qual você deseja recuperar dados de relatório de erros. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8.</td>
<td align="left">Sim</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">A data de início no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">A data de término no intervalo de datas dos dados do relatório de erros a serem recuperados. O padrão é a data atual.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">string</td>
<td align="left">Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">aggregationLevel</td>
<td align="left">string</td>
<td align="left">Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. Se você especificar <strong>week</strong> ou <strong>month</strong>, os valores <em>failureName</em> e <em>failureHash</em> serão limitados a 1.000 classificações.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">groupby</td>
<td align="left">string</td>
<td align="left">Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:
<ul>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
</ul>
<p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p>
<ul>
<li><strong>date</strong></li>
<li><strong>applicationId</strong></li>
<li><strong>applicationName</strong></li>
<li><strong>deviceCount</strong></li>
<li><strong>eventCount</strong></li>
</ul>
<p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Uma instrução que classifica os valores de dados resultantes de cada aquisição. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:
<ul>
<li><strong>date</strong></li>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
</ul>
<p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p>
<p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">Não</td>
</tr>
</tbody>
</table>

<span/>
 
### Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Estes são alguns exemplos dos parâmetros *filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

Para obter uma lista dos campos com suporte, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Campos</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">failureName</td>
<td align="left">O nome do erro.</td>
</tr>
<tr class="even">
<td align="left">failureHash</td>
<td align="left">O identificador exclusivo para o erro.</td>
</tr>
<tr class="odd">
<td align="left">symbol</td>
<td align="left">O símbolo atribuído a esse erro.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">eventType</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>falha</strong></li>
<li><strong>travar</strong></li>
<li><strong>memória</strong></li>
<li><strong>jse</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">market</td>
<td align="left">Uma cadeia de caracteres que contém o código do país ISO 3166 do mercado do dispositivo.</td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">packageName</td>
<td align="left">O nome exclusivo do pacote do aplicativo que está associado a esse erro.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">O versão do pacote do aplicativo que está associada a esse erro.</td>
</tr>
</tbody>
</table>

<span/> 

### Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de relatório de erros. Substitua o valor de *applicationId* pela ID da Loja de seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta


### Corpo da resposta

| Valor      | Tipo    | Descrição                                                                                                                                                                                                                                                                    |
|------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array   | Uma matriz de objetos que contêm dados de relatório de erros agregados. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de erros](#error-values) a seguir.                                                                                                          |
| @nextLink  | string  | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de erros para a consulta. |
| TotalCount | inumber | O número total de linhas no resultado dos dados da consulta.                                                                                                                                                                                                                     |

<span/>

### Valores de erros

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor           | Tipo    | Descrição                                                                                                                                                                                                                              |
|-----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | string  | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, esse valor será a primeira data nesse intervalo de datas. |
| applicationId   | string  | A ID da Loja do aplicativo para o qual você deseja recuperar dados de aquisição do complemento.                                                                                                                                                           |
| applicationName | string  | O nome de exibição do aplicativo.                                                                                                                                                                                                             |
| failureName     | string  | O nome do erro.                                                                                                                                                                                                                 |
| failureHash     | string  | O identificador exclusivo para o erro.                                                                                                                                                                                                   |
| symbol          | string  | O símbolo atribuído a esse erro.                                                                                                                                                                                                       |
| osVersion       | string  | A versão do sistema operacional no qual ocorreu o erro. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                       |
| eventType       | string  | O tipo de evento de erro. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                                          |
| market          | string  | O código do país ISO 3166 do mercado do dispositivo.                                                                                                                                                                                          |
| deviceType      | string  | O tipo de dispositivo que concluiu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                  |
| packageName     | string  | O nome exclusivo do pacote do aplicativo que está associado a esse erro.                                                                                                                                                                 |
| packageVersion  | string  | O versão do pacote do aplicativo que está associada a esse erro.                                                                                                                                                                     |
| eventCount      | inumber | O número de eventos que são atribuídos a esse erro para o nível de agregação especificado.                                                                                                                                            |
| deviceCount     | inumber | O número de dispositivos exclusivos que correspondem a esse erro para o nível de agregação especificado.                                                                                                                                        |

<span/> 

### Exemplo de resposta

O exemplo a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows Phone 8",
      "eventType": "crash",
      "market": "US",
      "deviceType": "mobile",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 191753
}

```

## Tópicos relacionados

* [Relatório de integridade](../publish/health-report.md)
* [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter aquisições de complemento](get-in-app-acquisitions.md)
* [Obter classificações de aplicativos](get-app-ratings.md)
* [Obter avaliações de aplicativo](get-app-reviews.md)



<!--HONumber=Nov16_HO1-->


