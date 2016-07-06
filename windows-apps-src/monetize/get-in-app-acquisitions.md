---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: "Use este método na API de análise da Windows Store para obter dados agregados de aquisição de um produto no aplicativo (IAP) durante um determinado intervalo de datas e outros filtros opcionais."
title: "Obter aquisições IAP"
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 21e634b1d5ab6c3ba7762c1b83c94d076d094af5

---

# Obter aquisições IAP


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Use este método na API de análise da Windows Store para obter dados agregados de aquisição de um produto no aplicativo (IAP) durante um determinado intervalo de datas e outros filtros opcionais. Este método retorna os dados no formato JSON.

## Pré-requisitos


Para usar este método, você precisa do seguinte:

-   Associe o aplicativo do Azure AD que você usará para chamar esse método com sua conta do Centro de Desenvolvimento.

-   Obtenha um token de acesso do Azure AD para seu aplicativo.

Para saber mais, consulte [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md).

## Solicitação


### Sintaxe da solicitação

| Método | URI da solicitação                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions |

 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer**&lt;*token*&gt;. |

 

### Corpo da solicitação

O parâmetro *applicationId* ou *inAppProductId* é obrigatório. Para recuperar dados de aquisição para todos os IAPs registrados no aplicativo, especifique o parâmetro *applicationId*. Para recuperar dados de aquisição de um único IAP, especifique o parâmetro *inAppProductId*. Se você especificar ambos, o parâmetro *inAppProductId* será ignorado.

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
<td align="left">A ID da Loja do aplicativo para o qual você deseja recuperar dados de aquisição de IAP. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8.</td>
<td align="left">Sim</td>
</tr>
<tr class="even">
<td align="left">inAppProductId</td>
<td align="left">string</td>
<td align="left">A ID de produto do IAP para o qual você deseja recuperar dados de aquisição.</td>
<td align="left">Sim</td>
</tr>
<tr class="odd">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">A data de início no intervalo de datas para os dados de aquisição de IAP a serem recuperados. O padrão é a data atual.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">A data de término no intervalo de datas para os dados de aquisição de IAP a serem recuperados. O padrão é a data atual.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">filter</td>
<td align="left">string</td>
<td align="left">Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir.</td>
<td align="left">Não</td>
</tr>
<tr class="even">
<td align="left">aggregationLevel</td>
<td align="left">string</td>
<td align="left">Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Uma instrução que classifica os valores dos dados do resultado de cada aquisição de IAP. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:
<ul>
<li><strong>date</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p>
<p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">Não</td>
</tr>
</tbody>
</table>

 

### Campos de filtro

O parâmetro *filter* do corpo da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Estes são alguns exemplos dos parâmetros *filter*:

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
<th align="left">Campo</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">acquisitionType</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>free</strong></li>
<li><strong>trial</strong></li>
<li><strong>paid</strong></li>
<li><strong>promotional code</strong></li>
<li><strong>iap</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>less than 13</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>greater than 55</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>Windows Phone Store (client)</strong></li>
<li><strong>Windows Store (client)</strong></li>
<li><strong>Windows Store (web)</strong></li>
<li><strong>Volume purchase by organizations</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">market</td>
<td align="left">Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado onde ocorreu a aquisição.</td>
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
<td align="left">orderName</td>
<td align="left">Uma cadeia de caracteres que especifica o nome da ordem do código promocional que foi usado para adquirir o aplicativo (só se aplicará se o usuário adquiriu o aplicativo resgatando um código promocional).</td>
</tr>
</tbody>
</table>

 

### Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações de obtenção de dados de aquisição de IAP. Substitua os valores de *inAppProductId* e *applicationId* com a ID de produto apropriada para seu IAP e a ID da Loja para seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta


### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                                |
|------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contém dados agregados de aquisição de IAP. Para obter mais informações sobre os dados em cada objeto, consulte a seção [Valores de aquisição de IAP](#iap-acquisition-values) a seguir.                                                                                                              |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados de aquisição de IAP para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                                                                                                                                                                                 |


### Valores de aquisição de IAP

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor               | Tipo    | Descrição                                                                                                                                                                                                                              |
|---------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | string  | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, esse valor será a primeira data nesse intervalo de datas. |
| inAppProductId      | string  | A ID de produto do IAP para o qual você está recuperando os dados de aquisição.                                                                                                                                                                 |
| inAppProductName    | string  | O nome de exibição do IAP.                                                                                                                                                                                                             |
| applicationId       | string  | A ID da Loja do aplicativo para o qual você deseja recuperar dados de aquisição de IAP.                                                                                                                                                           |
| applicationName     | string  | O nome de exibição do aplicativo.                                                                                                                                                                                                             |
| deviceType          | string  | O tipo de dispositivo que concluiu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                  |
| orderName           | string  | O nome do pedido.                                                                                                                                                                                                                   |
| storeClient         | string  | A versão da Loja onde ocorreu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                            |
| osVersion           | string  | A versão do sistema operacional no qual ocorreu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                   |
| market              | string  | O código de país ISO 3166 do mercado onde ocorreu a aquisição.                                                                                                                                                                  |
| gender              | string  | O sexo do usuário que fez a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                    |
| ageGroup            | string  | A faixa etária do usuário que fez a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                 |
| acquisitionType     | string  | O tipo de aquisição (grátis, pago e assim por diante). Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                    |
| acquisitionQuantity | inumber | O número de aquisições que ocorreram.                                                                                                                                                                                                |

 

### Exemplo de resposta

O exemplo a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso IAP 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "Iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## Tópicos relacionados

* [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter classificações de aplicativo](get-app-ratings.md)
* [Obter avaliações de aplicativo](get-app-reviews.md)

 

 



<!--HONumber=Jun16_HO5-->


