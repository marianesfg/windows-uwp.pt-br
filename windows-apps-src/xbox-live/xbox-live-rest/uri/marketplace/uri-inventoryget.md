---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632201"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
Fornece o conjunto de inventário associado no momento o usuário fornecido para o chamador.
O domínio para esses URIs é `inventory.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EHB)
  * [Exemplo de solicitação](#ID4EDE)
  * [Corpo da resposta](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

Nenhuma política de verificações, a imposição de, ou filtragem ocorrerá como parte dessa chamada. Os chamadores tem a opção de passar parâmetros de consulta para restringir o escopo dos resultados retornados.

Os chamadores possam percorrer os resultados com um token de continuação conforme fornecido por meio de uma resposta anterior: **/users/me/inventory? continuationToken = continuationTokenString**.

O chamador pode fazer uma chamada para os detalhes da API com uma URL do item específico para ver informações sobre um item específico.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| disponibilidade| cadeia de caracteres| A disponibilidade atual de itens a serem retornados. O padrão é "Disponível" intervalo de datas que retorna os itens para os quais a data atual está entre a data de início e fim. Outros valores incluem "All", que retorna todos os itens e "Não disponível", que retorna os itens para os quais a data atual está fora o intervalo de datas de data e de término do início e, portanto, não está disponível. |
| Contêiner| cadeia de caracteres| Opcional. Se você definir o valor para a ID do produto de um jogo, em seguida, os resultados do inventário do incluem apenas os itens relacionados a esse jogo. Isso é especialmente útil ao chamar o inventário do seu servidor para filtrar resultados para produtos de um jogo específico.|
| expandSatisfyingEntitlements| cadeia de caracteres| Um sinalizador que indica se a resposta inclui todos os direitos satisfatório que o usuário tem nos resultados retornados. O padrão é "false". Quando esse parâmetro é usado com um valor "true", todos os produtos que são concedidas ao usuário por meio do que satisfazem os itens de direitos, como agrupados, Xbox 360 compras migrados para o Xbox One, benefícios da assinatura, etc. são adicionados aos resultados. Quando esse valor é "false", em seguida, apenas os itens pai, como ProductID do grupo são retornados nos resultados e não os itens individuais incluídos. **Observação:** O uso desse parâmetro com um valor "true" é suportado apenas se o parâmetro itemType não está incluído no URI, caso contrário, você receberá um erro HTTP 400. |  
  | productIds | cadeia de caracteres |  Uma coleção de ProductIds que você deseja recuperar especificamente do inventário do usuário, separados por ','.  Se o usuário não tem um ProductID fornecido em seus resultados de inventário, esse item não aparecerá nos resultados da chamada à API. Se você passar o productID de um pacote juntamente com o conjunto de parâmetros expandSatisfyingEntitlements como true, todos os itens incluídos no pacote são retornados nos resultados da chamada (se você especificou sua productIds na sua cadeia de caracteres de consulta ou não).   |
  | Estado | cadeia de caracteres | O estado dos itens a serem retornados. O padrão é "all", que retorna todos os itens. Outros valores são "habilitados", que indica que apenas itemsthat estão habilitados deve ser retornado, "Suspenso", que indica que apenas os itens que são suspensas devem ser retornados, "Expirado", o que indica que somente os itens que expiraram devem ser retornados, " Cancelado", que indica que apenas os itens que serão cancelados devem ser retornados, e"Renewed", que indica que apenas os itens que foram renovados devem ser retornados.  |

Além disso, o recurso oferece suporte a mecânica de paginação padrão.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>Exemplo de solicitação

É o nome de domínio totalmente qualificado para este método URI `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> Quais usuários são considerados depende do token fornecido, que pode incluir vários usuários. Se você quiser que o inventário de um único usuário, você também deve fornecer o hash do usuário para o usuário específico que você queira considerar exclusivamente.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>Corpo da resposta

Se a chamada for bem-sucedida, o serviço retorna uma matriz de itens de inventário. Ver [inventoryItem (JSON)](../../json/json-inventoryitem.md).

<a id="ID4E4E"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EJF"></a>


##### <a name="parent"></a>Parent

[/Users/me/Inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>Informações adicionais

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)
