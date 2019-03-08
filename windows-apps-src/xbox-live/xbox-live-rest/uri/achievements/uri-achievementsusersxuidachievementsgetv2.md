---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655261"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
Obtém a lista de realizações definidos no título, desbloqueado por usuário ou que o usuário tem em andamento. O domínio para esses URIs é `achievements.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4ECB)
  * [Autorização](#ID4ENF)
  * [Cabeçalhos de solicitação necessários](#ID4ESG)
  * [Cabeçalhos de solicitação opcionais](#ID4ESH)
  * [Corpo da solicitação](#ID4EIBAC)
  * [Corpo da resposta](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário cujo (recurso) está sendo acessado. Deve corresponder a XUID do usuário autenticado.| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Obrigatório| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| Não| inteiro com sinal de 32 bits| Retorne itens começando depois determinado número de itens. Por exemplo, <b>skipItems = "3"</b> irá recuperar itens que começa com o quarto item recuperado. | 
| <b>continuationToken</b>| Não| cadeia de caracteres| Retorne os itens começando com o token de continuação fornecido. | 
| <b>maxItems</b>| Não| inteiro com sinal de 32 bits| Número máximo de itens a serem retornados da coleção, que pode ser combinada com <b>skipItems</b> e <b>continuationToken</b> para retornar um intervalo de itens. O serviço pode fornecer um valor padrão se <b>maxItems</b> não estiver presente e pode retornar menos de <b>maxItems</b>, mesmo se a última página de resultados ainda não foi retornada. | 
| <b>titleId</b>| Não| cadeia de caracteres| Um filtro para os resultados retornados. Aceita um ou mais identificadores de título de decimal, delimitada por vírgulas.| 
| <b>unlockedOnly</b>| Não| Valor booliano| Filtrar os resultados retornados. Se definido como <b>verdadeira</b>, será somente retornar as realizações desbloqueadas para o usuário. O padrão é <b>falsos</b>.| 
| <b>possibleOnly</b>| Não| Valor booliano| Filtrar os resultados retornados. Se definido como <b>verdadeira</b>, retornará todos os resultados possíveis, mas não desbloqueado metadados – apenas as informações de medalha de XMS. O padrão é <b>falsos</b>.| 
| <b>types</b>| Não| cadeia de caracteres| Um filtro para os resultados retornados. Pode ser "Persistent" ou "Desafio". O padrão é todos os tipos suportados.| 
| <b>orderBy</b>| Não| cadeia de caracteres| Especifica a ordem na qual retornar os resultados. Pode ser "Fora de ordem", "Title", "UnlockTime" ou "EndingSoon". O padrão é "Desordenados".| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>Autorização
 
| Declaração| Necessário?| Descrição| Comportamento se ausente| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Usuário| O chamador é um usuário autorizado do Xbox LIVE.| O chamador precisa ser um usuário válido no Xbox LIVE.| 403 Proibido| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor padrão: 1.| 
| <b>x-xbl-contract-version</b>| inteiro sem sinal de 32 bits| Se estiver presente e definido como 2, a versão V2 dessa API será usada. Otherwise, V1.| 
| <b>Accept-Language</b>| cadeia de caracteres| Lista de localidades desejadas e fallbacks (por exemplo, "fr-FR", "fr, en-GB, en-WW, en-US"). O serviço realizações funcionará por meio da lista até encontrar a correspondência de cadeias de caracteres localizadas. Se nenhum for encontrado, ele tenta corresponder ao local definido no token de usuário, que é proveniente do endereço IP do usuário. Se ainda não há correspondência cadeias de caracteres localizadas são encontradas, ele usa as cadeias de caracteres padrão fornecidas pelo editor/desenvolvedor de título. | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Se a chamada for bem-sucedida, o serviço retorna uma matriz de [medalha (JSON)](../../json/json-achievementv2.md) objetos e uma [PagingInfo (JSON)](../../json/json-paginginfo.md) objeto.
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "achievements":
    [{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "achievementState":"Achieved",
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>Referência 

[Conquista (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Parâmetros de paginação](../../additional/pagingparameters.md)

   