---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632671"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
Obtém uma pessoa por ID de destino da coleção de pessoas do chamador. O domínio para esses URIs é `social.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4E5)
  * [Autorização](#ID4EJB)
  * [Cabeçalhos de solicitação necessários](#ID4ERC)
  * [Cabeçalhos de solicitação opcionais](#ID4EQD)
  * [Corpo da solicitação](#ID4EWE)
  * [Códigos de status HTTP](#ID4EBF)
  * [Cabeçalhos de resposta necessária](#ID4EDH)
  * [Corpo da resposta](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Operações GET não modificará todos os recursos, portanto, isso produzirá os mesmos resultados se executada uma vez ou várias vezes.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| ownerId| cadeia de caracteres| Identificador do usuário cujo recurso está sendo acessado. Deve corresponder ao usuário autenticado. Os valores possíveis são "me", xuid({xuid}) ou gt({gamertag}).| 
| TargetId| cadeia de caracteres| Identificador do usuário cujos dados estão sendo recuperados na lista de pessoas do proprietário, uma ID de usuário do Xbox (XUID) ou um nome de jogador. Valores de exemplo: xuid(2603643534573581), gt(SomeGamertag).| 
  
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
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais
 
| Cabeçalho| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação só serão ser roteada para esse serviço depois de verificar a validade de cabeçalho, as declarações no token de autenticação, etc. Valor padrão: 1.| 
| Aceitar| Cadeia de caracteres. Tipos de conteúdo que o chamador aceita na resposta. Todas as respostas são <b>application/json</b>.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Corpo da solicitação
 
Não há objetos são enviados no corpo dessa solicitação.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>Códigos de status HTTP
 
O serviço retorna um dos códigos de status nesta seção em resposta a uma solicitação feita com esse método nesse recurso. Para obter uma lista completa de códigos de status HTTP padrão usado com serviços do Xbox Live, consulte [códigos de status HTTP padrão](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| OK| Êxito.| 
| 400| Solicitação Inválida| As IDs de usuário foram malformadas.| 
| 403| Proibido| Não foi possível analisar a declaração XUID do cabeçalho de autorização.| 
| 404| Não encontrado| Usuário de destino não foi encontrado na lista de pessoas do proprietário.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>Cabeçalhos de resposta necessária
 
| Cabeçalho| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| inteiro sem sinal de 32 bits| Comprimento, em bytes, do corpo da resposta. Valor de exemplo: 22.| 
| Content-Type| cadeia de caracteres| Tipo MIME do corpo da resposta. Isso sempre será <b>application/json</b>.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>Corpo da resposta
 
Se a chamada for bem-sucedida, o serviço retornará a pessoa de destino. Ver [Person (JSON)](../../json/json-person.md).
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>Resposta de exemplo
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   