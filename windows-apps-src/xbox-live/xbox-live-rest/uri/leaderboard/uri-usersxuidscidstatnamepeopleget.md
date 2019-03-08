---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650761"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
Retorna um placar de líderes de redes sociais, classificação que de stat valores (pontuações) para qualquer um dos conhecidos e todos os contatos do usuário atual ou somente os contatos designados como favoritas pessoas que o usuário.
O domínio para esses URIs é `leaderboards.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EAB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ELB)
  * [Autorização](#ID4EQD)
  * [Cabeçalhos de solicitação necessários](#ID4EGE)
  * [Cabeçalhos de solicitação opcionais](#ID4EXF)
  * [Corpo da solicitação](#ID4ETG)
  * [Códigos de status HTTP](#ID4ECEAC)
  * [Cabeçalhos de resposta necessária](#ID4E1HAC)
  * [Cabeçalhos de resposta opcional](#ID4EDJAC)
  * [Corpo da resposta](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

APIs de placar de líderes são todos somente leitura e, portanto, só há suporte para o verbo GET (via HTTPS). Eles refletem com classificação e classificadas "páginas" de estatísticas de player indexadas que são derivadas de estatísticas de usuário individuais que foram escritas por meio da plataforma de dados. Índices de placar de líderes completo podem ser muito grandes e chamadores nunca irá pedir para ver um em sua totalidade, portanto esse URI dá suporte a vários argumentos de cadeia de caracteres de consulta que permitem que um chamador ser específico sobre o tipo de modo que ele deseja ver em relação a esse placar de líderes.

Operações GET não modificará todos os recursos, portanto, isso produzirá os mesmos resultados se executada uma vez ou várias vezes.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| xuid| cadeia de caracteres| Identificador do usuário.|
| scid| cadeia de caracteres| Identificador da configuração do serviço que contém o recurso que está sendo acessado.|
| statname| cadeia de caracteres| Identificador exclusivo do recurso stat usuário que está sendo acessado.|
| all|favorito| enumeração| Se deve classificar o stat valores (pontuações) para todos os contatos de conhecidos do usuário atual ou somente os contatos designados como favoritas pessoas que o usuário.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- |
| maxItems| inteiro sem sinal de 32 bits| Número máximo de registros de placar de líderes para retornar em uma página de resultados. Se não for especificado, um número padrão será retornado (10). O valor máximo de maxItems é ainda indefinido, mas queremos evitar grandes conjuntos de dados, portanto, esse valor deve ser direcionado provavelmente o maior conjunto que um sintonizador de que interface do usuário podia manipular por chamada. |
| skipToRank| inteiro sem sinal de 32 bits| Retorne uma página de resultados, começando com a classificação de placar de líderes especificado. O restante dos resultados será na ordem de classificação por classificação. Essa cadeia de caracteres de consulta pode retornar um token de continuação que pode ser alimentado de volta em uma consulta subsequente para "a próxima página" dos resultados. |
| skipToUser| cadeia de caracteres| Retorne uma página de resultados do placar de líderes em torno de xuid o jogador especificado, independentemente do que o usuário classificação ou pontuação. A página será ordenada por classificação de percentil com o usuário especificado na última posição da página de exibições predefinidas, ou no meio de exibições de placar de líderes de stat. Não há nenhuma <b>continuationToken</b> fornecido para esse tipo de consulta. |
| continuationToken| cadeia de caracteres| Se uma chamada anterior retornado uma <b>continuationToken</b>, em seguida, o chamador pode devolver esse token "como está" em uma cadeia de caracteres de consulta para obter a próxima página de resultados. |
| sort| cadeia de caracteres| Especifique se deseja classificar a lista de jogadores de ordem do menor ao maior valor ("crescente") ou ordem ("decrescente") de valor alto para baixo. Este é um parâmetro opcional; o padrão é a ordem decrescente.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>Autorização

Xuid autorização é necessária.

Lógica de autorização é implementada para fins de isolamento de conteúdo e cenários de controle de acesso.

Estatísticas de placares de líderes e o usuário podem ser lido de clientes em qualquer plataforma, desde que o chamador envia um token válido do XSTS com sua solicitação. Gravações são limitadas (obviamente) para clientes com suporte pela plataforma de dados.

Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| Cadeia de caracteres. Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| Content-Type| Cadeia de caracteres. O tipo MIME do corpo da solicitação. Valor de exemplo: "application/json".|
| X-RequestedServiceVersion| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|
| Aceitar| Cadeia de caracteres. Valores aceitáveis do tipo de conteúdo. Valor de exemplo: "application/json".|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| Cadeia de caracteres. Marca da entidade - usada se o cliente dá suporte ao cache. Valor de exemplo: "686897696a7c876b7e".|

<a id="ID4ETG"></a>


## <a name="request-body"></a>Corpo da solicitação

Para maximizar a capacidade de entender os dados que ele está voltando para a exibição correta do chamador, cada valor stat para cada usuário será retornado como uma cadeia de caracteres no formato em que ele deve ser exibido e formatado de acordo com a localidade especificada no accept-language cabeçalho na solicitação. Um localizada "nome de exibição" também será retornado para statname para esse placar de líderes.

<a id="ID4E2G"></a>


### <a name="required-members"></a>Membros necessários

| Membro| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| Seção| Opcional. Retornado quando a classificação da última entrada na página é menor do que totalItems. Esta seção também não é retornada quando skipToUser é especificado na solicitação.|
| continuationToken| cadeia de caracteres| Obrigatório. Especifica que o valor para alimentar o parâmetro de consulta "continuationToken" para obter a próxima página de resultados para aquele URI se desejado. Se nenhum pagingInfo for retornado, em seguida, não há nenhuma "próxima página" de dados a ser obtida.|
| totalItems| inteiro sem sinal de 64 bits| Obrigatório. Número total de entradas no placar de líderes.|
| <b>leaderboardInfo</b>| Seção| Obrigatório. Sempre retornado. Contém os metadados sobre o placar de líderes solicitado.|
| displayName| cadeia de caracteres| Obrigatório. Nome de exibição localizado para o placar de líderes predefinido. Valor de exemplo: "Total de sinalizadores capturados".|
| totalCount| cadeia de caracteres| Obrigatório. Número total de entradas no placar de líderes.|
| Colunas| matriz| Obrigatório.|
| displayName| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.|
| statName| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.|
| type| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.|
| <b>userList</b>| Seção| Obrigatório. Sempre retornado. Contém as pontuações do usuário real para o placar de líderes solicitado.|
| gamertag| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.|
| xuid| inteiro com sinal de 32 bits| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.|
| Percentil| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.|
| classificação| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.|
| Valores| matriz| Obrigatório. Cada valor separado por vírgula corresponde a uma coluna no placar de líderes.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP

O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| OK| A sessão foi recuperada com êxito.|
| 304| Não modificado|  |
| 400| Solicitação Inválida| | Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.|
| 401| Não autorizado| | A solicitação exige autenticação do usuário.|
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.|
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.|
| 406| Não aceitável| Não há suporte para a versão do recurso; deve ser rejeitada pela camada de MVC.|
| 408| Tempo Limite da Solicitação| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| cadeia de caracteres| O tipo mime do corpo da resposta. Valores de exemplo: "application/json".|
| Content-Length| cadeia de caracteres| O número de bytes que estão sendo enviados na resposta. Valor de exemplo: "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>Cabeçalhos de resposta opcional

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| cadeia de caracteres| Usada para otimização de cache. Valor de exemplo: "686897696a7c876b7e".|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>Corpo da resposta

Solicitação para o placar de redes sociais, nenhuma paginação:

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>Resposta de exemplo


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
