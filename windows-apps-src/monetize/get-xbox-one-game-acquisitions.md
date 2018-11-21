---
author: Xansky
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Use este método na API de análise da Microsoft Store para obter os dados de aquisição agregados de um jogo Xbox One durante um determinado intervalo de datas e outros filtros opcionais.
title: Obter aquisições de jogo Xbox One
ms.author: mhopkins
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, serviços da Store, API de análise da Microsoft Store, aquisições de jogo Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 200f18e443e8a130a7e5c673b03c146b73c9083b
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7438335"
---
# <a name="get-xbox-one-game-acquisitions"></a>Obter aquisições de jogo Xbox One

Use este método na análise da Microsoft Store API para obter dados de aquisição agregados no formato JSON para um Xbox One jogo que ingerido por meio do Portal de desenvolvedor do Xbox (XDP) e está disponível no painel de análise XDP.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este método, primeiro você precisa do seguinte:

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md#prerequisites) para a API de análise da Microsoft Store.
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expira, você pode obter um novo.

## <a name="request"></a>Solicitação


### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho        | Tipo   | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorização | string | Obrigatório. O token de acesso do Azure AD no formulário **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro        | Tipo   |  Descrição      |  Obrigatório  
|---------------|--------|---------------|------|
| applicationId | string | A ID do produto do jogo Xbox One do qual você está recuperando dados de aquisição. Para obter a ID do produto do jogo, navegue até seu jogo no programa de análise XDP e recupere a ID do produto da URL. Como alternativa, se você baixar os dados de aquisições do relatório de análise do Partner Center, a ID do produto está incluída no arquivo. tsv.  |  Sim  |
| startDate | date | A data de início no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| endDate | date | A data final no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual. |  Não  |
| top | int | O número de linhas de dados para retornar. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. |  Não  |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. |  Não  |
| filter | string  | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Os valores de sequência devem estar entre aspas simples no parâmetro *filter*. Por exemplo, *filter=market eq 'US' and gender eq 'm'*. <p/><p/>Você pode especificar os campos a seguir do corpo da resposta:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>idade</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | Não   |
| aggregationLevel | string | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: <strong>day</strong>, <strong>week</strong> ou <strong>month</strong>. Se não for especificado, o padrão será <strong>day</strong>. | Não |
| orderby | string | Uma instrução que classifica os valores de dados resultantes de cada aquisição. A sintaxe é <em>orderby=field [order],field [order],...</em>. O parâmetro <em>field</em> pode ser uma das seguintes cadeias de caracteres:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>idade</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>O parâmetro <em>order</em> é opcional, e pode ser <strong>asc</strong> ou <strong>desc</strong> para especificar a ordem crescente ou decrescente de cada campo. O padrão é <strong>asc</strong>.</p><p>Este é um exemplo de cadeia de caracteres <em>orderby</em>: <em>orderby=date,market</em></p> |  Não  |
| groupby | string | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>As linhas de dados retornados conterão os campos especificados no parâmetro <em>groupby</em>, bem como o seguinte:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>O parâmetro <em>groupby</em> pode ser usado com o parâmetro <em>aggregationLevel</em>. Por exemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Não  |


### <a name="request-example"></a>Exemplo de solicitação

O exemplo a seguir demonstra várias solicitações de obtenção de dados de aquisição do jogo Xbox One. Substitua o valor *applicationId* com a ID do produto para o seu jogo.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta


### <a name="response-body"></a>Corpo da resposta

| Valor      | Tipo   | Descrição                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Uma matriz de objetos que contém dados agregados de aquisição do jogo. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de aquisição](#acquisition-values) a seguir.                                                                                                                      |
| @nextLink  | cadeia | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de aquisição para a consulta. |
| TotalCount | int    | O número total de linhas no resultado dos dados da consulta.              |


### <a name="acquisition-values"></a>Valores de aquisição

Os elementos na matriz *Value* contêm os valores a seguir.

| Valor               | Tipo   | Descrição                           |
|---------------------|--------|-------------------------------------------|
| date                | string | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId       | string | A ID do produto do jogo Xbox One do qual você está recuperando dados de aquisição. |
| applicationName     | string | O nome de exibição do jogo.       |
| acquisitionType     | string | Uma das sequências a seguir que indica o tipo de aquisição:<ul><li><strong>Grátis</strong></li><li><strong>Avaliação</strong></li><li><strong>Pagos</strong></li><li><strong>Código promocional</strong></li><li><strong>Iap</strong></li><li><strong>Assinatura Iap</strong></li><li><strong>Audiência particular</strong></li><li><strong>Ordem de versões anteriores</strong></li><li><strong>Xbox Game Pass</strong> (ou <strong>Game Pass</strong> se estiver consultando dados anteriores a 23 de março de 2018)</li><li><strong>Disco</strong></li><li><strong>Código pré-pago</strong></li><li><strong>Ordem de pré cobrados</strong></li><li><strong>Ordem de pré cancelada</strong></li><li><strong>Ordem de pré com falha</strong></li></ul>    |
| idade                 | string | Uma das sequências a seguir que indica a faixa etária do usuário que fez a aquisição:<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul>     |
| deviceType          | string | Uma das sequências a seguir que especifica o tipo de dispositivo que concluiu a aquisição:<ul><li><strong>Computador</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Servidor</strong></li><li><strong>Tablet</strong></li><li><strong>Holographic</strong></li><li><strong>Desconhecido</strong></li></ul>  |
| gender              | string | Uma das sequências a seguir que especifica o gênero do usuário que fez a aquisição:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>     |
| market              | string | O código de país ISO 3166 do mercado onde ocorreu a aquisição.  |
| osVersion           | string | A versão do sistema operacional no qual ocorreu a aquisição. Para esse método, esse valor sempre será **Windows 10**.</li></ul>    |
| paymentInstrumentType           | string | Uma das sequências a seguir que indica a instrução de pagamento usada para a aquisição:<ul><li><strong>Cartão de crédito</strong></li><li><strong>Cartão de débito direto</strong></li><li><strong>Compra inferida</strong></li><li><strong>Saldo na MS</strong></li><li><strong>Operadora</strong></li><li><strong>Transferência bancária online</strong></li><li><strong>PayPal</strong></li><li><strong>Transação dividida</strong></li><li><strong>Resgate de token</strong></li><li><strong>Valor zero pago</strong></li><li><strong>eWallet</strong></li><li><strong>Desconhecido</strong></li></ul>    |
| sandboxId              | string | A ID de área restrita criada para o jogo. Pode ser um valor **RETAIL** ou a ID de uma área restrita privada.  |
| storeClient         | string | Uma das sequências a seguir que indica a versão da Store onde ocorreu a aquisição:<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (cliente)** (ou **Windows Store (cliente)** se estiver consultando dados antes de 23 de março de 2018)</li><li>**Microsoft Store (Web)** (ou **Windows Store (Web)** se estiver consultando dados antes de 23 de março de 2018)</li><li>**Volume purchase by organizations**</li><li>**Outro**</li></ul>                             |
| xboxTitleIdHex              | string | A ID do título do Xbox Live (representado em valor hexadecimal) atribuída pelo Portal de Desenvolvedor do Xbox (XDP) para jogos habilitados para Xbox Live.  |
| acquisitionQuantity | number | O número de aquisições que ocorreram durante o nível de agregação especificado.     |
| purchasePriceUSDAmount | number | O valor pago pelo cliente pela aquisição, convertido em USD, usando a taxa de câmbio mensal.    |
| taxUSDAmount     | number | O valor do imposto aplicado à aquisição, convertido em USD. |


### <a name="response-example"></a>Exemplo de resposta

O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação.

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "age": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados analíticos usando serviços da Microsoft Store](access-analytics-data-using-windows-store-services.md)
