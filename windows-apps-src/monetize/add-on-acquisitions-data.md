---
description: Use esse método na API de análise de Microsoft Store para obter dados de aquisição de complemento de agregação no formato JSON para aplicativos UWP e jogos Xbox One que estavam disponíveis no painel do Centro de parceiros de análise de XDP e ingeridos por meio do Portal de desenvolvedor do Xbox (XDP).
title: Obter dados de aquisições de complemento para seus aplicativos e jogos
ms.date: 03/06/2019
ms.topic: article
keywords: windows 10, uwp, rede de publicidade, metadados do app
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829820"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Obter dados de aquisições de complemento para seus aplicativos e jogos 
Use esse método na API de análise de Microsoft Store para obter dados de aquisição de complemento de agregação no formato JSON para aplicativos UWP e jogos Xbox One que estavam disponíveis no painel do Centro de parceiros de análise de XDP e ingeridos por meio do Portal de desenvolvedor do Xbox (XDP). 

## <a name="prerequisites"></a>Pré-requisitos
Para usar este método, primeiro você precisa do seguinte: 

* Se você não tiver feito isso, conclua todos os [pré-requisitos](access-analytics-data-using-windows-store-services.md) para a API de análise da Microsoft Store. 
* [Obtenha um token de acesso do Azure AD](access-analytics-data-using-windows-store-services.md) a ser usado no cabeçalho da solicitação para este método. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá obter um novo. 

> [!NOTE]
> Essa API não fornece dados de agregação diário antes de 1º de outubro de 2016. 

## <a name="request"></a>Solicitação

### <a name="request-syntax"></a>Sintaxe da solicitação
| Método | URI da solicitação |
| --- | --- | 
| OBTER | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>Cabeçalho da solicitação 
| Cabeçalho | Tipo | Descrição | 
| --- | --- | --- |
| Autorização | cadeia de caracteres | Obrigatório. O token de acesso do AD do Azure no formato **portador** `<token>`. |

### <a name="request-parameters"></a>Parâmetros solicitados
O *applicationId* ou *addonProductId* o parâmetro é obrigatório. Para recuperar dados de aquisição para todos os complementos registrados no aplicativo, especifique o parâmetro *applicationId*. Para recuperar dados de aquisição para um único complemento, especifique o *addonProductId* parâmetro. Se você especificar ambos, o parâmetro *applicationId* será ignorado. 

| Parâmetro | Tipo | Descrição | Obrigatório | 
| --- | --- | --- | --- |
| applicationId | cadeia de caracteres | O *productId* do jogo Xbox One para o qual você está recuperando dados de aquisição. Para obter o *productId* do seu jogo, navegue até seu jogo no programa de análise de XDP e recuperar o *productId* da URL. Como alternativa, se você baixar os dados de aquisições do relatório de análise do Partner Center, o *productId* está incluído no arquivo. tsv. | Sim |
| addonProductId | cadeia de caracteres | O *productId* do complemento para o qual você deseja recuperar os dados de aquisição. | Sim |
| startDate | date | A data de início no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. | Não |
| endDate | date | A data final no intervalo de datas de dados de aquisição de complemento a serem recuperados. O padrão é a data atual. | Não |
| top | int | O número de linhas de dados a serem retornadas na solicitação. O valor máximo e o valor padrão; se não forem especificados, será 10.000. Se houver mais linhas na consulta, o corpo da resposta incluirá um link que você poderá usar para solicitar a próxima página de dados. | Não |
| skip | int | O número de linhas a serem ignoradas na consulta. Use este parâmetro para percorrer grandes conjuntos de dados. Por exemplo, top=10000 e skip=0 recuperam as primeiras 10.000 linhas de dados, top=10000 e skip=10000 recuperam as próximas 10.000 linhas de dados e assim por diante. | Não |
| filter | cadeia de caracteres | Uma ou mais instruções que filtram as linhas na resposta. Cada instrução contém um nome de campo do corpo da resposta e valor que estão associados com os operadores eq ou ne e instruções podem ser combinadas usando e ou ou. Valores de cadeia de caracteres devem estar entre aspas simples no parâmetro de filtro. For example, filter=market eq 'US' and gender eq 'm'. <br/> Você pode especificar os campos a seguir do corpo da resposta: <ul><li>**acquisitionType**</li><li>**Idade**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | Não |
| aggregationLevel | cadeia de caracteres | Especifica o intervalo de tempo para o qual recuperar dados agregados. Pode ser uma das seguintes cadeias de caracteres: **day**, **week** ou **month**. Se não for especificado, o padrão será **day**. | Não |
| orderby | cadeia de caracteres | Uma instrução que classifica os valores de dados resultantes de cada aquisição de complemento. A sintaxe é *orderby = field [order], o campo [order],...* O parâmetro *field* pode ser uma das seguintes cadeias de caracteres: <ul><li>**date**</li><li>**acquisitionType**</li><li>**Idade**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> O parâmetro de ordem é opcional e pode ser **asc** ou **desc** para especificar em ordem crescente ou decrescente para cada campo. O padrão é **asc**. <br/> Este é um exemplo de cadeia de caracteres *orderby*: *orderby=date,market* | Não |
| groupby | cadeia de caracteres | Uma instrução que aplica a agregação de dados apenas aos campos especificados. Você pode especificar os campos a seguir: <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**Idade**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> As linhas de dados retornados conterão os campos especificados no parâmetro *groupby*, bem como o seguinte: <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> O parâmetro groupby pode ser usado com o *aggregationLevel* parâmetro. Por exemplo: *& groupby = idade, mercado & aggregationLevel = semana* | Não |

### <a name="request-example"></a>Exemplo de solicitação
Os exemplos a seguir demonstram várias solicitações de obtenção de dados de aquisição do complemento. Substitua os *addonProductId* e *applicationId* valores com a ID de Store apropriado para seu complemento ou aplicativo. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Resposta

### <a name="response-body"></a>Corpo da resposta

| Valor | Tipo | Descrição |
| --- | --- | --- |
| Valor | matriz | Uma matriz de objetos que contém dados agregados de aquisição de complemento. Para obter mais informações sobre os dados em cada objeto, consulte a seção de [valores de aquisição de complemento](#add-on-acquisition-values) a seguir. |
| @nextLink | cadeia de caracteres | Se houver páginas adicionais de dados, essa cadeia de caracteres conterá um URI que você poderá usar para solicitar a próxima página de dados. Por exemplo, esse valor é retornado se o parâmetro **top** da solicitação estiver definido como 10000, mas houver mais de 10000 linhas de dados de aquisição de complemento para a consulta. |
| TotalCount | int | O número total de linhas no resultado dos dados da consulta. |

### <a name="add-on-acquisition-values"></a>Valores de aquisição de complemento
Elementos da matriz de valores contêm os valores a seguir.

| Valor | Tipo | Descrição | 
| --- | --- | --- |
| date | cadeia de caracteres | A primeira data no intervalo de datas dos dados de aquisição. Se a solicitação especificou um único dia, o valor será essa data. Se a solicitação especificou uma semana, um mês ou outro intervalo de datas, o valor será a primeira data nesse intervalo de datas. |
| addonProductId | cadeia de caracteres | O *productId* do complemento para o qual você está recuperando dados de aquisição. |
| addonProductName | cadeia de caracteres | O nome de exibição do complemento. Esse valor aparece apenas nos dados de resposta se a *aggregationLevel* parâmetro for definido como **dia**, a menos que você especifique a **addonProductName** campo o *groupby* parâmetro. |
| applicationId | cadeia de caracteres | O *productId* do aplicativo para o qual você deseja recuperar os dados de aquisição de complemento. |
| applicationName | cadeia de caracteres | O nome de exibição do jogo. |
| deviceType | cadeia de caracteres | Uma das sequências a seguir que especifica o tipo de dispositivo que concluiu a aquisição: <ul><li>"PC"</li><li>"Telefone"</li><li>"Console"</li><li>"IoT"</li><li>"Servidor"</li><li>"Tablet"</li><li>"Holographic"</li><li>"Desconhecido"</li></ul> |
| storeClient | cadeia de caracteres | Uma das sequências a seguir que indica a versão da Store onde ocorreu a aquisição: <ul><li>"Store do Windows Phone (cliente)"</li><li>"Microsoft Store (cliente)" (ou "Windows Store (cliente)" se a consulta de dados antes de 23 de março de 2018)</li><li>"Microsoft Store (web)" (ou "Windows Store (web)" se a consulta de dados antes de 23 de março de 2018)</li><li>"A compra de volume por organizações"</li><li>"Outros"</li></ul> |
| osVersion | cadeia de caracteres | A versão do sistema operacional no qual ocorreu a aquisição. Para esse método, esse valor é sempre "Windows 10". |
| market | cadeia de caracteres | O código de país ISO 3166 do mercado onde ocorreu a aquisição. |
| gender | cadeia de caracteres | Uma das sequências a seguir que especifica o gênero do usuário que fez a aquisição: <ul><li>"m"</li><li>"f"</li><li>"Desconhecido"</li></ul> |
| idade | cadeia de caracteres | Uma das sequências a seguir que indica a faixa etária do usuário que fez a aquisição: <ul><li>"menos de 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"maior que 55"</li><li>"Desconhecido"</li></ul> |
| acquisitionType | cadeia de caracteres | Uma das sequências a seguir que indica o tipo de aquisição: <ul><li>"Gratuito" </li><li>"Avaliação" </li><li>"Pago"</li><li>"Código promocional" </li><li>"Iap"</li><li>"Assinatura Iap"</li><li>"Audience privada"</li><li>"Pre Order"</li><li>"Xbox Game Pass" (ou "Jogo Pass" consulta de dados antes de 23 de março de 2018)</li><li>"Disco"</li><li>"Código pré-pago"</li><li>"Cobrado ordem Pre"</li><li>"Cancelada a pedido Pre"</li><li>"Falha no pré Order"</li></ul> |
| acquisitionQuantity | número inteiro | O número de aquisições que ocorreram. |
| inAppProductId | cadeia de caracteres | ID do produto do produto em que esse complemento é usado.  |
| inAppProductName | cadeia de caracteres | Nome do produto do produto em que esse complemento é usado.  |
| paymentInstrumentType | cadeia de caracteres | Tipo de instrumento de pagamento usado para a aquisição.  |
| sandboxId | cadeia de caracteres | A ID de área restrita criadas para o jogo. Isso pode ser o valor **varejo** ou uma ID de sandbox privada.  |
| xboxTitleId | cadeia de caracteres | ID do Xbox título do produto do XDP, se aplicável.  |
| localCurrencyCode | cadeia de caracteres | Código de moeda local com base no país da conta do Partner Center.  |
| xboxProductId | cadeia de caracteres | ID do produto Xbox do produto do XDP, se aplicável.  |
| availabilityId | cadeia de caracteres | ID de disponibilidade do produto do XDP, se aplicável.  |
| skuId | cadeia de caracteres | ID do SKU do produto do XDP, se aplicável.  |
| skuDisplayName | cadeia de caracteres | Nome de exibição de SKU do produto do XDP, se aplicável.  |
| xboxParentProductId | cadeia de caracteres | ID do produto Xbox pai do produto do XDP, se aplicável.  |
| parentProductName | cadeia de caracteres | Nome do produto pai do produto do XDP, se aplicável.  |
| productTypeName | cadeia de caracteres | Nome do tipo de produto do produto do XDP, se aplicável.  |
| purchaseTaxType | cadeia de caracteres | Tipo de imposto de compra do produto do XDP, se aplicável.  |
| purchasePriceUSDAmount | number | O valor pago pelo cliente para o complemento, convertido em USD.  |
| purchasePriceLocalAmount | number | O valor do imposto aplicado ao complemento.  |
| purchaseTaxUSDAmount | number | O valor do imposto aplicado ao complemento, convertido em USD.  |
| purchaseTaxLocalAmount | number | Valor Local de imposto de compra do produto do XDP, se aplicável.  |

### <a name="response-example"></a>Exemplo de resposta
O código a seguir demonstra um exemplo de corpo de resposta JSON para essa solicitação. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```