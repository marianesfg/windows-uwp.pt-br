---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606721"
---
# <a name="get-usersme"></a>GET (/users/me)
Obter o usuário atual [PresenceRecord](../../json/json-presencerecord.md) sem precisar saber XUID do usuário.
O domínio para esses URIs é `userpresence.xboxlive.com`.

  * [Parâmetros de cadeia de caracteres de consulta](#ID4EZ)
  * [Autorização](#ID4EIC)
  * [Cabeçalhos de solicitação necessários](#ID4ELD)
  * [Cabeçalhos de solicitação opcionais](#ID4EPF)
  * [Corpo da solicitação](#ID4EPG)
  * [Corpo da resposta](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| level| cadeia de caracteres| Opcional. <ul><li><b>user</b>: Retorna somente o nó do usuário.</li><li><b>dispositivo</b>: Retorna nós de nó e o dispositivo de usuário.</li><li><b>title</b>: Padrão. Retorna a árvore inteira, exceto a atividade.</li><li><b>all</b>: Retorna a árvore inteira, inclusive a presença de nível de atividade.</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| Sim| ID de usuário do Xbox (XUID) do chamador| 403 Proibido|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. A única com suporte pela presença é application/json, mas ele deve ser especificado no cabeçalho.|
| Accept-Language| cadeia de caracteres| Localidade aceitável para cadeias de caracteres na resposta. Valores de exemplo: en-US.|
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: presencebeta.xboxlive.com.|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4E1G"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EAH"></a>


### <a name="sample-response"></a>Resposta de exemplo

Esse método retorna um [PresenceRecord](../../json/json-presencerecord.md).


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EQH"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4ESH"></a>


##### <a name="parent"></a>Parent

[/users/me](uri-usersme.md)
