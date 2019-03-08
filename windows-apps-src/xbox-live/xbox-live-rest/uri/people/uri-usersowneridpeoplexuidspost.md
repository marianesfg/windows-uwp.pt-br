---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589861"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
Obtém as pessoas por XUID de pessoas do chamador de coleção. O domínio para esses URIs é `social.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4E5)
  * [Autorização](#ID4EJB)
  * [Cabeçalhos de solicitação necessários](#ID4ERC)
  * [Cabeçalhos de solicitação opcionais](#ID4EBE)
  * [Corpo da solicitação](#ID4EHF)
  * [Códigos de status HTTP](#ID4EKH)
  * [Cabeçalhos de resposta necessária](#ID4ENBAC)
  * [Corpo da resposta](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
POST operações não modificará todos os recursos, portanto, isso produzirá os mesmos resultados se executada uma vez ou várias vezes.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| ownerId| cadeia de caracteres| Identificador do usuário cujo recurso está sendo acessado. Deve corresponder ao usuário autenticado. Os valores possíveis são "me", xuid({xuid}) ou gt({gamertag}).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Autorização
 
| Tipo| Obrigatório| Descrição| Resposta se ausente| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| sim| Chamador tiver a ID de usuário na Xbox do (XUID usuário).| 401 não autorizado| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorização| Cadeia de caracteres. Dados de autorização do Xbox LIVE. Isso normalmente é um token XSTS criptografado. Valor de exemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>.| 
| Content-Length| inteiro sem sinal de 32 bits. Comprimento, em bytes, do corpo da solicitação. Valor de exemplo: 22.| 
| Content-Type| Cadeia de caracteres. Tipo MIME do corpo da solicitação. Isso deve ser <b>application/json</b>.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor padrão: 1.| 
| Aceitar| Cadeia de caracteres. Tipos de conteúdo que o chamador aceita na resposta. Todas as respostas são <b>application/json</b>.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>Membros necessários
 
| Membro| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| Matriz de XUIDs que identificam as pessoas a serem retornados da coleção de pessoas do chamador. Ver [XuidList (JSON)](../../json/json-xuidlist.md).| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>Membros opcionais
 
Não há nenhum membro opcional para esta solicitação.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>Membros proibidos
 
Todos os outros membros são proibidos em uma solicitação.
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>Exemplo de solicitação
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Êxito ao método é "get".| 
| 204| Sem conteúdo| Êxito ao método é "Adicionar" ou "remover".| 
| 400| Solicitação Inválida| Parâmetro de método estava ausente ou malformado ou as IDs de usuário foram malformadas.| 
| 403| Proibido| Não foi possível analisar a declaração XUID do cabeçalho de autorização.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| inteiro sem sinal de 32 bits| Comprimento, em bytes, do corpo da resposta. Valor de exemplo: 22.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Isso sempre será <b>application/json</b>.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Um corpo de resposta é enviado somente quando o método de solicitação é "get". Não há nenhum corpo de resposta para "Adicionar" ou "remover".
 
Se uma chamada de método "get" for bem-sucedida, o serviço retorna o número total de pessoas em pessoas do chamador, coleção e uma matriz que contém a coleção de pessoas do chamador. Nenhuma resposta é retornada para métodos de "Adicionar" e "remover". Ver [PeopleList (JSON)](../../json/json-peoplelist.md).
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   