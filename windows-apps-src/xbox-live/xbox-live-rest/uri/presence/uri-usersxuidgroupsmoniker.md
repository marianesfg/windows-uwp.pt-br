---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
description: " /users/xuid({xuid})/groups/{moniker}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25ddc8120f05f04d5285fbbe4efc5a41f98265ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617551"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
Acessa o PresenceRecord para um grupo. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| Xbox usuário ID (XUID) do usuário relacionado aos XUIDs no grupo.| 
| moniker| cadeia de caracteres| Cadeia de caracteres define o grupo de usuários. O moniker aceitação somente no momento é "Pessoas", com uma letra maiuscula 'P'.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker} )](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;Obtém o PresenceRecord para um grupo.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de presença](atoc-reference-presence.md)

   