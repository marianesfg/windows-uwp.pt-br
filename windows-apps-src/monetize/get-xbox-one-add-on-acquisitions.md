---
description: Use esse método na API de análise de Microsoft Store para obter dados de aquisição de complemento de agregação.
title: Obter aquisições de complementos do Xbox One
ms.date: 10/18/2018
ms.topic: article
keywords: Windows 10, uwp, serviços de Store, API, Xbox One aquisições de complemento de análise da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633291"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Obter aquisições de complementos do Xbox One

Use esse método na Microsoft Store API de análise de obter dados de aquisição de complemento de agregação no formato JSON para um Xbox One jogo que estava disponível no painel de análise de XDP Partner Center e ingeridos por meio do Portal de desenvolvedor do Xbox (XDP).

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição          |
|---------------|--------|--------------|
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

O *applicationId* ou *addonProductId* o parâmetro é obrigatório. Para recuperar dados de aquisição para todos os complementos registrados no aplicativo, especifique o parâmetro *applicationId*. Para recuperar dados de aquisição para um único complemento, especifique o *addonProductId* parâmetro. Se você especificar ambos, o parâmetro *applicationId* será ignorado.

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  |
|---------------|--------|---------------|------|
| applicationId | cadeia de caracteres | O *productId* do jogo Xbox One para o qual você está recuperando dados de aquisição. Para obter o *productId* do seu jogo, navegue até seu jogo no programa de análise de XDP e recuperar o *productId* da URL. Como alternativa, se você baixar os dados de aquisições do relatório de análise do Partner Center, o *productId* está incluído no arquivo. tsv. |  Sim  |
| addonProductId | cadeia de caracteres | O *productId* do complemento para o qual você deseja recuperar os dados de aquisição.  | Sim  |
| startDate | date | A data de início no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter |cadeia de caracteres  | <p>Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo da resposta e valor que estão associados com os operadores eq ou ne e instruções podem ser combinadas usando e ou ou. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro de filtro. Por exemplo, filtrar = mercado eq eq 'US' e sexo estou '.</p> <p>Você pode especificar os campos a seguir do corpo da resposta:</p> <ul><li><strong>acquisitionType</strong></li><li><strong>Idade</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| Não   |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores de dados resultantes de cada aquisição de complemento. A sintaxe é <em>orderby = field [order], o campo [order],...</em> O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>Data</strong></li><li><strong>acquisitionType</strong></li><li><strong>Idade</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li><strong>Data</strong></li><li><strong>ApplicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>Idade</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>mercado</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>Data</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo:  <em>&amp;groupby = idade, mercado&amp;aggregationLevel = semana</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

Os exemplos a seguir demonstram várias solicitações de obtenção de dados de aquisição do complemento. Substitua os *addonProductId* e *applicationId* valores com a ID de Store apropriado para seu complemento ou aplicativo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
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
| addonProductId      | cadeia de caracteres  | O *productId* do complemento para o qual você está recuperando dados de aquisição.                                                                                                                                                                 |
| addonProductName    | cadeia de caracteres  | O nome de exibição do complemento. Esse valor aparece apenas nos dados de resposta se a *aggregationLevel* parâmetro for definido como **dia**, a menos que você especifique a **addonProductName** campo o *groupby* parâmetro.                                                                                                                                                                                                            |
| applicationId       | cadeia de caracteres  | O *productId* do aplicativo para o qual você deseja recuperar os dados de aquisição de complemento.                                                                                                                                                           |
| applicationName     | cadeia de caracteres  | O nome de exibição do jogo.                                                                                                                                                                                                             |
| deviceType          | cadeia de caracteres  | <p>Uma das sequências a seguir que especifica o tipo de dispositivo que concluiu a aquisição:</p> <ul><li>"PC"</li><li>"Telefone"</li><li>"Console"</li><li>"IoT"</li><li>"Servidor"</li><li>"Tablet"</li><li>"Holographic"</li><li>"Desconhecido"</li></ul>                                                                                                  |
| storeClient         | cadeia de caracteres  | <p>Uma das sequências a seguir que indica a versão da Store onde ocorreu a aquisição:</p> <ul><li>"Store do Windows Phone (cliente)"</li><li>"Microsoft Store (cliente)" (ou "Windows Store (cliente)" se a consulta de dados antes de 23 de março de 2018)</li><li>"Microsoft Store (web)" (ou "Windows Store (web)" se a consulta de dados antes de 23 de março de 2018)</li><li>"A compra de volume por organizações"</li><li>"Outros"</li></ul>                                                                                            |
| osVersion           | cadeia de caracteres  | A versão do sistema operacional no qual ocorreu a aquisição. Para esse método, esse valor é sempre "Windows 10".                                                                                                   |
| market              | cadeia de caracteres  | O código de país ISO 3166 do mercado onde ocorreu a aquisição.                                                                                                                                                                  |
| gender              | cadeia de caracteres  | <p>Uma das sequências a seguir que especifica o gênero do usuário que fez a aquisição:</p> <ul><li>"m"</li><li>"f"</li><li>"Desconhecido"</li></ul>                                                                                                    |
| idade            | cadeia de caracteres  | <p>Uma das sequências a seguir que indica a faixa etária do usuário que fez a aquisição:</p> <ul><li>"menos de 13"</li><li>"13 a 17"</li><li>"18 a 24"</li><li>"25-34"</li><li>"44 35"</li><li>"44 55"</li><li>"maior que 55"</li><li>"Desconhecido"</li></ul>                                                                                                 |
| acquisitionType     | cadeia de caracteres  | <p>Uma das sequências a seguir que indica o tipo de aquisição:</p> <ul><li>"Gratuito"</li><li>"Avaliação"</li><li>"Pago"</li><li>"Código promocional"</li><li>"Iap"</li><li>"Assinatura Iap"</li><li>"Audience privada"</li><li>"Pre Order"</li><li>"Xbox Game Pass" (ou "Jogo Pass" consulta de dados antes de 23 de março de 2018)</li><li>"Disco"</li><li>"Código pré-pago"</li><li>"Cobrado ordem Pre"</li><li>"Cancelada a pedido Pre"</li><li>"Falha no pré Order"</li></ul>                                                                                                    |
| acquisitionQuantity | número inteiro | O número de aquisições que ocorreram.                        |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
