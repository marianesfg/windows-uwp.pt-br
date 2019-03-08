---
title: GET (/scids/{scid}/leaderboards/{leaderboardname})
assetID: 4adea46c-e910-8709-c28e-ce9178e712ef
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnameget.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0d313262a642ee0db956f6d3264025931e63e8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629491"
---
# <a name="get-scidsscidleaderboardsleaderboardname"></a>GET (/scids/{scid}/leaderboards/{leaderboardname})
 
Obtém um placar de líderes global predefinida.
 
O domínio para esses URIs é `leaderboards.xboxlive.com`.
 
  * [Comentários](#ID4EY)
  * [Parâmetros de URI](#ID4EDB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EOB)
  * [Autorização](#ID4EID)
  * [Efeito das configurações de privacidade no recurso](#ID4EDE)
  * [Cabeçalhos de solicitação necessários](#ID4EME)
  * [Cabeçalhos de solicitação opcionais](#ID4E1F)
  * [Códigos de status HTTP](#ID4E1G)
  * [Cabeçalhos de resposta](#ID4ERCAC)
  * [Corpo da resposta](#ID4EOEAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Comentários
 
APIs de Leaderboard são todos somente leitura e, portanto, só há suporte para o verbo GET. Eles refletem com classificação e classificadas "páginas" de estatísticas de player indexadas que são derivadas de estatísticas de usuário individuais que foram escritas por meio da plataforma de dados. Índices de placar de líderes completo podem ser muito grandes e chamadores nunca irá pedir para ver um em sua totalidade, portanto esse URI dá suporte a vários argumentos de cadeia de caracteres de consulta que permitem que um chamador ser específico sobre o tipo de modo que ele deseja ver em relação a esse placar de líderes.
 
Operações GET não modificará todos os recursos, portanto, isso produzirá os mesmos resultados se executada uma vez ou várias vezes.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| GUID| Identificador da configuração do serviço que contém o recurso que está sendo acessado.| 
| leaderboardname| cadeia de caracteres| Identificador exclusivo do recurso placar de líderes predefinidas que estão sendo acessado.| 
  
<a id="ID4EOB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | 
| maxItems| inteiro sem sinal de 32 bits| Número máximo de registros de placar de líderes para retornar em uma página de resultados. Se não for especificado, um número padrão será retornado (10). O valor máximo de maxItems é ainda indefinido, mas queremos evitar grandes conjuntos de dados, portanto, esse valor deve ser direcionado provavelmente o maior conjunto que um sintonizador de que interface do usuário podia manipular por chamada.| 
| skipToRank| inteiro sem sinal de 32 bits| Retorne uma página de resultados, começando com a classificação de placar de líderes especificado. O restante dos resultados será na ordem de classificação por classificação. Essa cadeia de caracteres de consulta pode retornar um token de continuação que pode ser alimentado de volta em uma consulta subsequente para "a próxima página" dos resultados.| 
| skipToUser| cadeia de caracteres| Retorne uma página de resultados do placar de líderes em torno de xuid o jogador especificado, independentemente do que o usuário classificação ou pontuação. A página será ordenada por classificação de percentil com o usuário especificado na última posição da página de exibições predefinidas, ou no meio de exibições de placar de líderes de stat. Não há nenhum continuationToken fornecido para esse tipo de consulta.| 
| continuationToken| cadeia de caracteres| Se uma chamada anterior retornado um continuationToken, em seguida, o chamador pode devolver esse token "como está" em uma cadeia de caracteres de consulta para obter a próxima página de resultados.| 
  
<a id="ID4EID"></a>

 
## <a name="authorization"></a>Autorização
 
Não há lógica de autorização implementada para cenários de isolamento de conteúdo e controle de acesso.
 
   * Estatísticas de placares de líderes e o usuário podem ser lido de clientes em qualquer plataforma, desde que o chamador envia um token válido do XSTS com a solicitação. Gravações são, obviamente, limitadas a clientes com suporte pela plataforma de dados.
   * Os desenvolvedores de título podem marcar as estatísticas como aberta ou restrita com XDP ou Partner Center. Placar de líderes é abertos de estatísticas. Abrir estatísticas podem ser acessadas por Smartglass, bem como iOS, Android, Windows, Windows Phone e aplicativos web, desde que o usuário está autorizado para a área restrita. Autorização de usuário para uma área restrita é gerenciada por meio de XDP ou Partner Center.
  
O pseudocódigo para a verificação tem esta aparência:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EDE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso
 
Nenhuma verificação de privacidade é executada durante a leitura de dados de placar de líderes.
  
<a id="ID4EME"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Cadeia de caracteres. Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>| 
| X-RequestedServiceVersion| Cadeia de caracteres. Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor padrão: 1.| 
| Aceitar| Cadeia de caracteres. Tipos de conteúdo que são aceitáveis. Valor de exemplo: <b>application/json</b>| 
  
<a id="ID4E1F"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| Cadeia de caracteres. Marca da entidade, usada se o cliente dá suporte ao cache. Valor de exemplo: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4E1G"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| A sessão foi recuperada com êxito.| 
| 304| Não modificado| Recurso não foi modificado desde a última solicitado.| 
| 400| Solicitação Inválida| Serviço não conseguiu compreender a solicitação malformada. Normalmente, um parâmetro inválido.| 
| 401| Não autorizado| A solicitação exige autenticação do usuário.| 
| 403| Proibido| A solicitação não é permitida para o usuário ou serviço.| 
| 404| Não encontrado| O recurso especificado não pôde ser encontrado.| 
| 406| Não aceitável| Não há suporte para a versão do recurso.| 
| 408| Tempo Limite da Solicitação| Não há suporte para a versão do recurso; deve ser rejeitada pela camada de MVC.| 
  
<a id="ID4ERCAC"></a>

 
## <a name="response-headers"></a>Cabeçalhos de resposta
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| cadeia de caracteres| Obrigatório. O tipo MIME do corpo da resposta. Exemplo: <b>application/json</b>.| 
| Content-Length| cadeia de caracteres| Obrigatório. O número de bytes que estão sendo enviados na resposta. Exemplo: <b>232</b>.| 
| ETag| cadeia de caracteres| Opcional. Usada para otimização de cache. Exemplo: <b>686897696a7c876b7e</b>.| 
  
<a id="ID4EOEAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
<a id="ID4EUEAC"></a>

 
### <a name="response-members"></a>Membros de resposta
 
pagingInfo | pagingInfo| Seção| Opcional. Presente somente quando maxItems é especificado na solicitação.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| inteiro sem sinal de 64 bits| Obrigatório. Especifica qual valor deve ser o feed de volta para o <b>skipItems</b> consultar um parâmetro para obter a próxima página de resultados para esse URI, se desejado. Se nenhum <b>pagingInfo</b> é retornado, não haverá nenhum próxima página de dados a ser obtida.| 
| totalCount| inteiro sem sinal de 64 bits| Obrigatório. Número total de entradas no placar de líderes. Valor de exemplo: 1234567890| 
 
leaderboardInfo | leaderboardInfo| Seção| Obrigatório. Sempre retornado. Contém os metadados sobre o placar de líderes solicitado.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| cadeia de caracteres| Não usado.| 
| totalCount| cadeia de caracteres (inteiro sem sinal de 64 bits)| Obrigatório. Número total de entradas no placar de líderes. Valor de exemplo: 1234567890| 
| Colunas| matriz| Obrigatório. Colunas no placar de líderes.| 
| displayName| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.| 
| statName| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.| 
| type| cadeia de caracteres| Obrigatório. Corresponde a uma coluna no placar de líderes.| 
 
userList | userList| Seção| Obrigatório. Sempre retornado. Contém as pontuações do usuário real para o placar de líderes solicitado.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| gamertag| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.| 
| xuid| inteiro sem sinal de 64 bits| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.| 
| Percentil| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.| 
| classificação| cadeia de caracteres| Obrigatório. Corresponde ao usuário na entrada do placar de líderes.| 
| Valores| matriz| Obrigatório. Cada valor separado por vírgula corresponde a uma coluna no placar de líderes.| 
  
<a id="ID4EGKAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 
O seguinte URI de solicitação representa a ignorar a classificação em um placar de líderes global.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured/columns/deaths?maxItems=3&skipToRank=15000
         
```

 
O URI acima retorna a resposta JSON a seguir.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "152",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columns": [
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            },
            {
                "displayName": "Deaths",
                "statName": "deaths",
                "type": "Integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": 1234567890123444,
            "percentile": 0.64,
            "rank": 15000,
            "values": [
                1000,
                47
            ]
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": 1234567890123555,
            "percentile": 0.64,
            "rank": 15001,
            "values": [
                998,
                17
            ]
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": 1234567890123666,
            "percentile": 0.64,
            "rank": 15002,
            "values": [
                996,
                2
            ]
        }
    ]
}
         
```

 
O seguinte URI de solicitação representa pular para o usuário em um placar de líderes global.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/totalflagscaptured?maxItems=3&skipToUser=2343737892734237
         
```

 
O URI acima retorna a resposta JSON a seguir.
 

```cpp
{
    "leaderboardInfo": 
    {   
       "displayName": "Total flags captured",
       "totalCount": 23437,
       "columns": [
              {
                  "displayName": "Flags Captured",
                  "statName": "flagscaptured",
                  "type": "Integer"
              }
       ]
    },
    "userList": [
        {
            "gamertag": "AverageJoe",
            "percentile": 55.00,
            "rank": 11718,
            "value": 1219,
            "xuid": 1234567890123444
        },
        {
            "gamertag": "AreUWatchinMe",
            "percentile": 60.00,
            "rank": 14162,
            "value": 1062,
            "xuid": 2343737892734333
        },
         {
            "gamertag": "WarriorSaint",
            "percentile": 64.39,
            "rank": 15000,
            "value ": 1000,
            "xuid": 1234567890123455
        }
    ]
}
         
```

   
<a id="ID4E5KAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EALAC"></a>

 
##### <a name="parent"></a>Parent 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   