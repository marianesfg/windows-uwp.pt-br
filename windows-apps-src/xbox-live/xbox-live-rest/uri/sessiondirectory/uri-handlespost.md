---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594741"
---
# <a name="post-handles"></a>POST (/handles)
Define a sessão com vários participantes para a atividade do usuário atual e convida os membros da sessão, se necessário.

> [!IMPORTANT]
> Esse método é usado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele é destinado para uso com o modelo de contrato 104/105 ou posterior e requer um elemento de cabeçalho de X-Xbl-versão de contrato: 104/105 ou posterior em cada solicitação.

  * [Comentários](#ID4ET)
  * [Parâmetros de URI](#ID4EHB)
  * [Códigos de status HTTP](#ID4EPB)
  * [Corpo da solicitação](#ID4EVB)
  * [Corpo da resposta](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Comentários

Esse método HTTP/REST pode ser usado para definir a sessão para a atividade atual. Nesse caso, o método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**. O corpo da solicitação deve definir a sessão de referência, usando o **sessionRef** objeto no arquivo JSON, com o campo de tipo "atividade". Nenhum corpo de resposta é recuperado. Para obter definições dos itens especificados em uma referência de sessão, consulte **Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**.

Esse método POST também pode ser usado para convidar usuários especificados pelos identificadores para uma sessão. Nesse caso, o método pode ser encapsulado por **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**. Esse uso do método POST requer o corpo da solicitação para definir a referência de sessão, mas com o tipo de campo definido como "convidar". O corpo da resposta é um identificador de convite.

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>Parâmetros do URI

Nenhuma

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>Códigos de status HTTP
O serviço retornará um código de status HTTP como ele se aplica ao MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Corpo da solicitação

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>Para configurar a atividade de corpo de solicitação


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>Corpo da solicitação de envio de convites


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>Corpo da resposta

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>Corpo de resposta para a configuração de atividade
Nenhum.  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>Corpo da resposta para enviar convites
Um identificador de convite.   
<a id="ID4EXC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EZC"></a>


##### <a name="parent"></a>Parent

[/handles](uri-handles.md)
