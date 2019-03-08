---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655671"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
Obter a presença de um lote de usuários.
O domínio para esses URIs é `userpresence.xboxlive.com`.

  * [Comentários](#ID4EV)
  * [Autorização](#ID4EAB)
  * [Efeito das configurações de privacidade no recurso](#ID4EDC)
  * [Cabeçalhos de solicitação necessários](#ID4EYF)
  * [Cabeçalhos de solicitação opcionais](#ID4EGAAC)
  * [Corpo da solicitação](#ID4EGBAC)
  * [Corpo da resposta](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Comentários

Esse método deve ser usado por qualquer cliente, o serviço ou o título que desejam aprender sobre informações de presença de um lote de usuários.

As respostas para esta solicitação de lote podem ser filtra por profundidade e caminho. Os consumidores podem usar isso para descobrir e exibir a presença sobre um conjunto de usuários. Os filtros nessa API funcionam como ORs em uma propriedade, mas ANDs em Propriedades.

<a id="ID4EAB"></a>


## <a name="authorization"></a>Autorização

| Tipo| Obrigatório| Descrição| Resposta se ausente|
| --- | --- | --- | --- |
| XUID| Sim| ID de usuário do Xbox (XUID) do chamador| 403 Proibido|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efeito das configurações de privacidade no recurso

Esse método sempre retorna 200 Okey, mas não pode retornar o conteúdo no corpo da resposta.

| Usuário solicitante| Configuração de privacidade do usuário de destino| Comportamento|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>Cabeçalhos de solicitação necessários

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorização| cadeia de caracteres| Credenciais de autenticação para a autenticação HTTP. Valor de exemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| cadeia de caracteres| Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valores de exemplo: 3, vnext.|
| Aceitar| cadeia de caracteres| Tipos de conteúdo que são aceitáveis. A única com suporte pela presença é application/json, mas ele deve ser especificado no cabeçalho.|
| Accept-Language| cadeia de caracteres| Localidade aceitável para cadeias de caracteres na resposta. Valores de exemplo: en-US.|
| Host| cadeia de caracteres| Nome de domínio do servidor. Valor de exemplo: presencebeta.xboxlive.com.|
| Content-Length| cadeia de caracteres| O comprimento do corpo da solicitação. Valor de exemplo: 312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>Cabeçalhos de solicitação opcionais

| Cabeçalho| Tipo| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nome/número do serviço Xbox LIVE para o qual essa solicitação deve ser direcionada de compilação. A solicitação será apenas ser roteada para que o serviço depois de verificar a validade do cabeçalho, as declarações no token de autenticação e assim por diante. Valor padrão: 1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>Membros necessários

| Membro| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| usuários| XUIDs da lista de usuários cuja presença que você deseja saber mais, com um máximo de 1100 XUIDs por vez.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>Membros opcionais

| Membro| Descrição|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| Lista de tipos de dispositivos usados pelos usuários que você deseja conhecer. Se a matriz estiver vazia, ele assume como padrão a todos os tipos de dispositivo possíveis (ou seja, nenhum são filtrados).|
| títulos| Os tipos de lista de dispositivos cujos usuários você deseja conhecer. Se a matriz é deixada em branco, o padrão é todos os títulos de possíveis (ou seja, nenhum são filtrados).|
| level| Valores possíveis: <ul><li>usuário – nós de usuário de get</li><li>dispositivo - usuário get e nós de dispositivo</li><li>título – obter informações de nível básico de título</li><li>todos – Obtenha informações de presença avançada, informações de mídia ou ambos</li></ul><br> O padrão é "title".|
| onlineOnly| Se essa propriedade for true, a operação em lote filtrará os registros para usuários offline (incluindo aqueles encobertos). Se não for fornecido, os usuários online e offline serão retornados.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>Membros proibidos

Todos os outros membros são proibidos em uma solicitação.

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>Exemplo de solicitação


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Parent

[/users/batch](uri-usersbatch.md)
