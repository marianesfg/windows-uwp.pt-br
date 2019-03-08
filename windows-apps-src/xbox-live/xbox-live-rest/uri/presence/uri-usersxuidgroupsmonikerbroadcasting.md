---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98eaa60204e3c98eb1b09a13372f7b0c084a6608
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651461"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
Acessa o registro de presença dos usuários difusão especificados pelo moniker grupos relacionados ao XUID que aparece no URI. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| Xbox usuário ID (XUID) do usuário relacionado aos XUIDs no grupo.| 
| moniker| cadeia de caracteres| Cadeia de caracteres define o grupo de usuários. O moniker aceitação somente no momento é "Pessoas", com uma letra maiuscula 'P'.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;Recupera o registro de presença dos usuários difusão especificados pelo moniker grupos relacionado para o XUID que aparece no URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de presença](atoc-reference-presence.md)

   