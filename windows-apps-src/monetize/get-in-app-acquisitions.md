---
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Use este método na API de análise da Microsoft Store para obter os dados de aquisição agregados de um complemento durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter aquisições de complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, aquisições de complemento
ms.localizationpriority: medium
ms.openlocfilehash: cd7e907994943dbce83d195e80a15770833f7e4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650981"
---
# <a name="get-add-on-acquisitions"></a>Obter aquisições de complemento

Use este método na API de análise da Microsoft Store para obter dados agregados de aquisição para complementos para seu app em formato JSON durante um determinado intervalo de datas e outros filtros opcionais. Essas informações também estão disponíveis na [relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md) no Partner Center.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição          |
|---------------|--------|--------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

O parâmetro *applicationId* ou *inAppProductId* é obrigatório. Para recuperar dados de aquisição para todos os complementos registrados no aplicativo, especifique o parâmetro *applicationId*. Para recuperar dados de aquisição de um único complemento, especifique o parâmetro *inAppProductId*. Se você especificar ambos, o parâmetro *applicationId* será ignorado.

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja recuperar dados de aquisição do complemento.  |  Sim  |
| inAppProductId | cadeia de caracteres | A [ID da Store](in-app-purchases-and-trials.md#store-ids) do complemento para o qual você deseja recuperar dados de aquisição.  | Sim  |
| startDate | date | A data de início no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |cadeia de caracteres  | Uma ou mais instruções que filtram as linhas na resposta. Para saber mais, consulte a seção [campos de filtro](#filter-fields) a seguir. | Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores de dados resultantes de cada aquisição de complemento. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>Data</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li><strong>Data</strong></li><li><strong>ApplicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>Data</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não  |


### <a name="filter-fields"></a>Campos de filtro

O parâmetro *filter* da solicitação contém uma ou mais instruções que filtram as linhas da resposta. Cada instrução contém um campo e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Estes são alguns exemplos dos parâmetros *filter*:

-   *filtro = mercado eq eq 'US' e sexo estou '*
-   *filtro = (ne de colocação no mercado 'US') e (sexo ne 'Desconhecido') e (estou ne gênero ') e (mercado ne 'Não') e (Grupoetário ne 'maior que 55' ou Grupoetário ne 'menos de 13')*

Para obter uma lista dos campos com suporte, consulte a tabela a seguir. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro *filter*.

| Campos        |  Descrição        |
|---------------|-----------------|
| acquisitionType | Uma das seguintes cadeias de caracteres:<ul><li><strong>livre</strong></li><li><strong>Versão de avaliação</strong></li><li><strong>Pago</strong></li><li><strong>código promocional</strong></li><li><strong>IAP</strong></li></ul> |
| ageGroup | Uma das seguintes cadeias de caracteres:<ul><li><strong>menos de 13</strong></li><li><strong>13-17</strong></li><li><strong>18 a 24</strong></li><li><strong>25-34</strong></li><li><strong>35 44</strong></li><li><strong>55 44</strong></li><li><strong>maior que 55</strong></li><li><strong>Desconhecido</strong></li></ul> |
| storeClient | Uma das seguintes cadeias de caracteres:<ul><li><strong>Windows Phone Store (cliente)</strong></li><li><strong>Microsoft Store (cliente)</strong></li><li><strong>Microsoft Store (web)</strong></li><li><strong>Compra de volume por organizações</strong></li><li><strong>Outros</strong></li></ul> |
| gender | Uma das seguintes cadeias de caracteres:<ul><li><strong>M</strong></li><li><strong>f</strong></li><li><strong>Desconhecido</strong></li></ul> |
| market | Uma cadeia de caracteres que contém o código de país ISO 3166 do mercado onde ocorreu a aquisição. |
| osVersion | Uma das seguintes cadeias de caracteres:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Desconhecido</strong></li></ul> |
| deviceType | Uma das seguintes cadeias de caracteres:<ul><li><strong>PC</strong></li><li><strong>Telefone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul> |
| orderName | Uma cadeia de caracteres que especifica o nome do pedido do código promocional que foi usado para adquirir o complemento (só se aplicará se o usuário adquiriu o complemento resgatando um código promocional). |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações de obtenção de dados de aquisição do complemento. Substitua os valores de *inAppProductId* e *applicationId* pela ID da Loja apropriada para seu complemento ou aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição         |
|------------|--------|------------------|
| Valor      | matriz  | Uma matriz de objetos que contém dados agregados de aquisição de complemento. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de aquisição de complemento](#add-on-acquisition-values) a seguir.                                                                                                              |
| @nextLink  | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados de aquisição de complemento para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valores de aquisição de complemento

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo    | Descrição        |
|---------------------|---------|---------------------|
| date                | cadeia de caracteres  | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| inAppProductId      | cadeia de caracteres  | A ID da Loja do complemento do qual você está recuperando dados de aquisição.                                                                                                                                                                 |
| inAppProductName    | cadeia de caracteres  | O nome de exibição do complemento. Este valor só será exibido nos dados de resposta se o parâmetro *aggregationLevel* for definido como **day**, a menos que você especifique o campo **inAppProductName** no parâmetro *groupby*.                                                                                                                                                                                                            |
| applicationId       | cadeia de caracteres  | A ID da Loja do aplicativo para o qual você deseja recuperar dados de aquisição do complemento.                                                                                                                                                           |
| applicationName     | cadeia de caracteres  | O nome de exibição do app.                                                                                                                                                                                                             |
| deviceType          | cadeia de caracteres  | O tipo de dispositivo que concluiu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                                  |
| orderName           | cadeia de caracteres  | O nome do pedido.                                                                                                                                                                                                                   |
| storeClient         | cadeia de caracteres  | A versão da Loja onde ocorreu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                            |
| osVersion           | cadeia de caracteres  | A versão do sistema operacional no qual ocorreu a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                                   |
| market              | cadeia de caracteres  | O código de país ISO 3166 do mercado onde ocorreu a aquisição.                                                                                                                                                                  |
| gender              | cadeia de caracteres  | O sexo do usuário que fez a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                                    |
| ageGroup            | cadeia de caracteres  | A faixa etária do usuário que fez a aquisição. Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                                 |
| acquisitionType     | cadeia de caracteres  | O tipo de aquisição (grátis, pago e assim por diante). Para obter uma lista das cadeias de caracteres com suporte, consulte a seção [Campos de filtro](#filter-fields) acima.                                                                                                    |
| acquisitionQuantity | número inteiro | O número de aquisições que ocorreram.                        |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Relatório de aquisições de complemento](../publish/add-on-acquisitions-report.md)
* [Dados de análise de acesso usando os serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obter as conversões de complemento por canal](get-add-on-conversions-by-channel.md)
* [Obter aquisições de aplicativo](get-app-acquisitions.md)
* [Obter dados de funil de aquisição de aplicativo](get-acquisition-funnel-data.md)
* [Obter as conversões de aplicativo por canal](get-app-conversions-by-channel.md)

 

 
