---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "Use este método na API de análise da Windows Store para obter dados de opinião para um determinado intervalo de datas e outros filtros opcionais."
title: "Obter avaliações de aplicativo"
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: bb0f912bd3380e21e04fa44f2c75244c6585f03a

---

# Obter avaliações de aplicativo


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Use este método na API de análise da Windows Store para obter dados de opinião para um determinado intervalo de datas e outros filtros opcionais. Este método retorna os dados no formato JSON.

## Pré-requisitos


Para usar este método, você precisa do seguinte:

-   Associe o aplicativo do Azure AD que você usará para chamar esse método com sua conta do Centro de Desenvolvimento.

-   Obtenha um token de acesso do Azure AD para seu aplicativo.

Para saber mais, consulte [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md).

## Solicitação


### Sintaxe da solicitação

| Método | URI da solicitação                                                      |
|--------|------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews |

 

### Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer**&lt;*token*&gt;. |

 

### Corpo da solicitação

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
<td align="left">A ID da Loja do aplicativo para o qual você deseja recuperar dados de opinião. A ID da Loja está disponível na [página Identidade do aplicativo](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento. Uma ID da Loja de exemplo é 9WZDNCRFJ3Q8.</td>
<td align="left">Sim</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">A data de início no intervalo de datas dos dados de opinião a serem recuperados. O padrão é a data atual.</td>
<td align="left">Não</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">A data final no intervalo de datas de dados de opinião a serem recuperados. O padrão é a data atual.</td>
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
<td align="left">orderby</td>
<td align="left">string</td>
<td align="left">Uma instrução que classifica os valores dos dados resultantes de cada classificação. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>classificação</strong></li>
</ul>
<p>O parâmetro <em>order</em> é opcional e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p>
<p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">Não</td>
</tr>
</tbody>
</table>

 
### Campos de filtro

O parâmetro *filter* do corpo da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados a operadores **eq** ou **ne** e alguns campos também dão suporte a operadores **contains**, **gt**, **lt**, **ge** e **le**. Instruções podem ser combinadas usando-se **and** ou **or**.

Este é um exemplo de cadeia de caracteres *filtro*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Para obter uma lista dos campos com suporte e os operadores de suporte para cada campo, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Campos</th>
<th align="left">Operadores com suporte</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">Uma cadeia de caracteres que contém o código do país ISO 3166 do mercado do dispositivo.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
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
<td align="left">eq, ne</td>
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
<td align="left">isRevised</td>
<td align="left">eq, ne</td>
<td align="left">Especifique <strong>true</strong> para filtrar por opiniões que foram revisadas; caso contrário, <strong>false</strong>.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">A versão do pacote do aplicativo que foi revisado.</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">O tipo de dispositivo no qual o aplicativo foi avaliado.</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">Uma das seguintes cadeias de caracteres:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">A resolução de tela do dispositivo no formato &quot;<em>largura</em> x <em>altura</em>&quot;.</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">Especifique <strong>true</strong> para filtrar por dispositivos habilitados para toque; caso contrário, <strong>false</strong>.</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">O nome do revisor.</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">O número de vezes que a opinião foi marcada como útil.</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">O número de vezes que a opinião foi marcada como não útil.</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">O título da análise.</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">O conteúdo de texto da opinião.</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">O conteúdo de texto da resposta.</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">A data em que a resposta foi enviada.</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">A RAM física, em MB.</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">A capacidade do disco do armazenamento principal, em GB.</td>
</tr>
<tr class="odd">
<td align="left">classificação</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">A classificação do aplicativo, em estrelas.</td>
</tr>
</tbody>
</table>

 

### Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações para a obtenção de dados de opinião. Substitua o valor de *applicationId* pela ID da Loja de seu aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## Resposta


### Corpo da resposta

| Valor      | Tipo   | Descrição                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contêm dados de opinião. Para saber mais sobre os dados em cada objeto, consulte a seção de [valores de opinião](#review-values) a seguir.                                                                                                                                      |
| @nextLink  | string | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você pode usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de aquisição para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.                                                                                                                                                                                                                             |

 
### Valores de opinião

Os elementos na matriz *Value* contêm os seguintes valores.

| Valor                  | Tipo    | Descrição                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | string  | A primeira data no intervalo de datas dos dados de classificações. Se a solicitação especificou um único dia, esse valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, esse valor será a primeira data nesse intervalo de datas. |
| applicationId          | string  | A ID da Loja do aplicativo do qual você está recuperando dados de classificações.                                                                                                                                                                 |
| applicationName        | string  | O nome de exibição do aplicativo.                                                                                                                                                                                                         |
| market                 | string  | O código do país ISO 3166 do mercado em que a classificação foi enviada.                                                                                                                                                              |
| osVersion              | string  | A versão do sistema operacional no qual a classificação foi enviada. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                               |
| deviceType             | string  | O tipo de dispositivo no qual a classificação foi enviada. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                           |
| isRevised              | Booliano | O valor **true** indica que a opinião foi revisada; caso contrário, **false**.                                                                                                                                                       |
| packageVersion         | string  | A versão do pacote do aplicativo que foi revisado.                                                                                                                                                                                    |
| deviceModel            | string  | O tipo de dispositivo no qual o aplicativo foi avaliado.                                                                                                                                                                                    |
| productFamily          | string  | O nome da família de dispositivos. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [campos de filtro](#filter-fields) acima.                                                                                                                         |
| deviceScreenResolution | string  | A resolução de tela do dispositivo no formato "*largura* x *altura*".                                                                                                                                                                     |
| isTouchEnabled         | Booliano | O valor **true** indica que é habilitado para toque; caso contrário, **false**.                                                                                                                                                             |
| reviewerName           | string  | O nome do revisor.                                                                                                                                                                                                                   |
| helpfulCount           | number  | O número de vezes que a opinião foi marcada como útil.                                                                                                                                                                                   |
| notHelpfulCount        | number  | O número de vezes que a opinião foi marcada como não útil.                                                                                                                                                                               |
| reviewTitle            | string  | O título da análise.                                                                                                                                                                                                             |
| reviewText             | string  | O conteúdo de texto da opinião.                                                                                                                                                                                                     |
| responseText           | string  | O conteúdo de texto da resposta.                                                                                                                                                                                                   |
| responseDate           | string  | A data em que uma resposta foi enviada.                                                                                                                                                                                                   |
| deviceRAM              | number  | A RAM física, em MB.                                                                                                                                                                                                             |
| deviceStorageCapacity  | number  | A capacidade do disco do armazenamento principal, em GB.                                                                                                                                                                                     |
| classificação                 | number  | A classificação do aplicativo, em estrelas.                                                                                                                                                                                                            |

 

### Exemplo de resposta

O exemplo a seguir demonstra o corpo de uma resposta JSON dessa solicitação.

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
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## Tópicos relacionados

* [Acessar dados analíticos usando serviços da Windows Store](access-analytics-data-using-windows-store-services.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter aquisições IAP](get-in-app-acquisitions.md)
* [Obter dados de relatório de erros](get-error-reporting-data.md)
* [Obter classificações de aplicativo](get-app-ratings.md)



<!--HONumber=Jun16_HO4-->


