---
description: Use esse método na API de análise de Microsoft Store para obter dados de aquisição de agregação no formato JSON para aplicativos UWP e jogos Xbox One que estavam disponíveis no painel de análise de XDP e ingeridos por meio do Portal de desenvolvedor do Xbox (XDP).
title: Obter dados de aquisições para seus aplicativos e jogos
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, rede de publicidade, metadados do app
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829821"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Obter dados de aquisições para seus aplicativos e jogos 
Use esse método na API de análise de Microsoft Store para obter dados de aquisição de agregação no formato JSON para aplicativos UWP e jogos Xbox One que estavam disponíveis no painel de análise de XDP e ingeridos por meio do Portal de desenvolvedor do Xbox (XDP). 

> [!NOTE]
> Essa API não fornece dados de agregação diário antes de 1º de outubro de 2016. 

## <a name="prerequisites"></a>Pré-requisitos
Para usar este método, primeiro você precisa do seguinte: 

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md) para a API de análise da Microsoft Store. 
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo. 

## <a name="request"></a>Solicitação
### <a name="request-syntax"></a>Sintaxe da solicitação

| Método | URI da solicitação |
| --- | --- |
| OBTER | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>Cabeçalho da solicitação

| Cabeçalho | Tipo | Descrição |
| --- | --- | --- |
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** `<token>`. |

### <a name="request-parameters"></a>Parâmetros solicitados

| Parâmetro | Tipo | Descrição | Obrigatório |
| --- | --- | --- | --- |
| applicationId | cadeia de caracteres | A ID do produto do jogo Xbox One do qual você está recuperando dados de aquisição. Para obter a ID do produto do seu jogo, navegue até seu jogo no programa de análise de XDP e recuperar a ID do produto da URL. Como alternativa, se você baixar seus dados de aquisições do relatório de análise do Partner Center, a ID do produto está incluída no arquivo. tsv.  | Sim |
| startDate | date | A data de início no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual.  | Não |
| endDate | date | A data final no intervalo de datas de dados de aquisição a serem recuperados. O padrão é a data atual.  | Não |
| top | número inteiro | O número de linhas de dados para retornar. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados.  | Não |
| skip | número inteiro | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, *superior = 10000 e ignorar = 0* recupera as primeiro 10000 linhas de dados, *superior = 10000 e ignorar = 10000* recupera as próximo 10000 linhas de dados e assim por diante.  | Não |
| filter | cadeia de caracteres | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo de resposta e um valor que estão associados aos operadores **eq** ou **ne**, e as instruções podem ser combinadas usando-se **and** ou **or**. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro de filtro. Por exemplo, *filter=market eq 'US' and gender eq 'm'*.  <br/> Você pode especificar os campos a seguir do corpo da resposta: <ul><li>**acquisitionType**</li><li>**Idade**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Não |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: **day**, **week** ou **month**. Se não for especificado, o padrão será **day**.  | Não |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores de dados resultantes de cada aquisição. A sintaxe é *orderby = field [order], o campo [order],...* O parâmetro *field* pode ser uma das seguintes cadeias de caracteres: <ul><li>**date**</li><li>**acquisitionType**</li><li>**Idade**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> O parâmetro *order* é opcional, e pode ser **asc** ou **desc** para especificar a ordem crescente ou decrescente de cada campo. O padrão é **asc**. Este é um exemplo de cadeia de caracteres *orderby*: *orderby=date,market*  | Não |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir: <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> As linhas de dados retornados conterão os campos especificados no parâmetro *groupby*, bem como o seguinte: <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> O *groupby* parâmetro pode ser usado com o parâmetro aggregationLevel. Por exemplo: *& groupby = Grupoetário, mercado & aggregationLevel = semana*  | Não |

### <a name="request-example"></a>Exemplo de solicitação
O exemplo a seguir demonstra várias solicitações de obtenção de dados de aquisição do jogo Xbox One. Substitua os *applicationId* valor com a ID de produto para o seu jogo.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta
| Valor | Tipo | Descrição |
| --- | --- | --- |
| Valor | matriz | Uma matriz de objetos que contém dados agregados de aquisição do jogo. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de aquisição](#acquisition-values) a seguir. |
| @nextLink | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor será retornado se o parâmetro **top** da solicitação estiver definido como 10.000, mas houver mais de 10.000 linhas de dados de aquisição para a consulta. |
| TotalCount | número inteiro | O número total de linhas no resultado dos dados da consulta. |

### <a name="acquisition-values"></a>Valores de aquisição 
Os elementos na matriz *Value* contêm os valores a seguir. 

| Valor | Tipo | Descrição |
| --- | --- | --- |
| date | cadeia de caracteres | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| applicationId | cadeia de caracteres | A ID do produto do jogo Xbox One do qual você está recuperando dados de aquisição. |
| applicationName | cadeia de caracteres | O nome de exibição do jogo. |
| acquisitionType | cadeia de caracteres | Uma das sequências a seguir que indica o tipo de aquisição:  <ul><li>**livre**</li><li>**Versão de avaliação**</li><li>**Pago**</li><li>**código promocional**</li><li>**Iap**</li><li>**Iap da assinatura**</li><li>**Público particular**</li><li>**Ordem de pré**</li><li>**Xbox Game Pass** (ou **Game Pass** se estiver consultando dados anteriores a 23 de março de 2018)</li><li>**disco**</li><li>**Código pré-pago**</li><li>**Ordem de pré cobrado**</li><li>**Ordem de pré cancelada**</li><li>**Ordem de pré com falha**</li></ul> |
| idade | cadeia de caracteres | Uma das sequências a seguir que indica a faixa etária do usuário que fez a aquisição: <ul><li>**menos de 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**maior que 55**</li><li>**Desconhecido**</li></ul> |
| deviceType | cadeia de caracteres | Uma das sequências a seguir que especifica o tipo de dispositivo que concluiu a aquisição: <ul><li>**PC**</li><li>**Telefone**</li><li>**Console**</li><li>**IoT**</li><li>**Servidor**</li><li>**Tablet**</li><li>**Holographic**</li><li>**Desconhecido**</li></ul> |
| gender | cadeia de caracteres | Uma das sequências a seguir que especifica o gênero do usuário que fez a aquisição: <ul><li>**m**</li><li>**f**</li><li>**Desconhecido**</li></ul> |
| market | cadeia de caracteres | O código de país ISO 3166 do mercado onde ocorreu a aquisição. |
| osVersion | cadeia de caracteres | A versão do sistema operacional no qual ocorreu a aquisição. Para esse método, esse valor sempre será **Windows 10**. |
| paymentInstrumentType | cadeia de caracteres | Uma das sequências a seguir que indica a instrução de pagamento usada para a aquisição: <ul><li>**Cartão de crédito**</li><li>**Cartão de débito direto**</li><li>**Compra inferida**</li><li>**Saldo do MS**</li><li>**Operador móvel**</li><li>**Transferência bancária online**</li><li>**PayPal**</li><li>**Dividir a transação**</li><li>**Resgate do token**</li><li>**Zero valor pago**</li><li>**eWallet**</li><li>**Desconhecido**</li></ul> |
| sandboxId | cadeia de caracteres | A ID de área restrita criada para o jogo. Isso pode ser o valor **varejo** ou uma ID de sandbox privada. |
| storeClient | cadeia de caracteres | Uma das sequências a seguir que indica a versão da Store onde ocorreu a aquisição: <ul><li>**Windows Phone Store (cliente)**</li><li>**Microsoft Store (cliente)** (ou **Windows Store (cliente)** se estiver consultando dados antes de 23 de março de 2018) </li><li>**Microsoft Store (Web)** (ou **Windows Store (Web)** se estiver consultando dados antes de 23 de março de 2018) </li><li>**Compra de volume por organizações**</li><li>**Outros**</li></ul> |
| xboxTitleId | cadeia de caracteres | A ID do título do Xbox Live (representado em valor hexadecimal) atribuída pelo Portal de Desenvolvedor do Xbox (XDP) para jogos habilitados para Xbox Live. |
| acquisitionQuantity | number | O número de aquisições que ocorreram durante o nível de agregação especificado. |
| purchasePriceUSDAmount | number | O valor pago pelo cliente pela aquisição, convertido em USD, usando a taxa de câmbio mensal. |
| purchaseTaxUSDAmount | number | O valor do imposto aplicado à aquisição, convertido em USD. |
| localCurrencyCode | cadeia de caracteres | Código de moeda local com base no país da conta do Partner Center.  |
| xboxProductId | cadeia de caracteres | ID do produto Xbox do produto do XDP, se aplicável.  |
| availabilityId | cadeia de caracteres | ID de disponibilidade do produto do XDP, se aplicável.  |
| skuId | cadeia de caracteres | ID do SKU do produto do XDP, se aplicável.  |
| skuDisplayName  | cadeia de caracteres | SKU exibem o nome do produto do XDP, se aplicável.  |
| xboxParentProductId | cadeia de caracteres | ID do produto Xbox pai do produto do XDP, se aplicável.  |
| parentProductName | cadeia de caracteres | Nome do produto pai do produto do XDP, se aplicável.  |
| productTypeName | cadeia de caracteres | Nome do tipo de produto do produto do XDP, se aplicável.  |
| purchaseTaxType | cadeia de caracteres | Tipo de imposto de compra do produto do XDP, se aplicável.  |
| purchasePriceLocalAmount | number | Comprar o valor de Local de preço do produto de XDP, se aplicável.  |
| purchaseTaxLocalAmount | number | Valor Local de imposto de compra do produto do XDP, se aplicável.  |

### <a name="response-example"></a>Exemplo de resposta
O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
