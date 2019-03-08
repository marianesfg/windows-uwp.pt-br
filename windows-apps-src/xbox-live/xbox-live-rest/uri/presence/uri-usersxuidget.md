---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
description: " GET (/users/xuid({xuid}))"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc5d3b6a172eccab0656da39e6896b4df50840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650921"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
Descubra a presença de outro usuário ou cliente.
O domínio para esses URIs é `userpresence.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EDB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EOB)
  * [Autorização](#ID4E4C)
  * [Efeito das configurações de privacidade no recurso](#ID4EAE)
  * [Cabeçalhos de solicitação necessários](#ID4EVH)
  * [Cabeçalhos de solicitação opcionais](#ID4E1BAC)
  * [Corpo da solicitação](#ID4E1CAC)
  * [Corpo da resposta](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

A resposta pode ser filtrada para fornecer a parte do [PresenceRecord](../../json/json-presencerecord.md) se o consumidor não estiver interessado em todo o objeto.

> [!NOTE] 
> Os dados retornados é restrito por regras de isolamento de privacidade e de conteúdo.



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- |
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário de destino.|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

| Parâmetro| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- |
| level| cadeia de caracteres| Opcional. <ul><li><b>user</b>: Retorna somente o nó do usuário.</li><li><b>dispositivo</b>: Retorna nós de nó e o dispositivo de usuário.</li><li><b>title</b>: Padrão. Retorna a árvore inteira, exceto a atividade.</li><li><b>all</b>: Retorna a árvore inteira, inclusive a presença de nível de atividade.</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| Sim| ID de usuário do Xbox (XUID) do chamador| 403 Proibido|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Esse método sempre retorna 200 Okey, mas não pode retornar o conteúdo no corpo da resposta.

| Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Me| -| 200 OK|
| Friend| Todas as pessoas| 200 OK|
| Friend| somente os amigos| 200 OK|
| Friend| bloqueado| 200 OK|
| usuário não friend| Todas as pessoas| 200 OK|
| usuário não friend| somente os amigos| 200 OK|
| usuário não friend| bloqueado| 200 OK|
| site de terceiros| Todas as pessoas| 200 OK|
| site de terceiros| somente os amigos| 200 OK|
| site de terceiros| bloqueado| 200 OK|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. A única com suporte pela presença é application/json, mas ele deve ser especificado no cabeçalho.|
| Accept-Language| cadeia de caracteres| Localidade aceitável para cadeias de caracteres na resposta. Valores de exemplo: en-US.|
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: presencebeta.xboxlive.com.|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>Corpo da solicitação

Não há objetos são enviados no corpo dessa solicitação.

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>Resposta de exemplo

Se não houver nenhum registro existente para o usuário, um registro com nenhum dispositivo será retornado.


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


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>Parent

[/users/xuid({xuid})](uri-usersxuid.md)
