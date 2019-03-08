---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a39bc9c3302ba26949700774997355a216fe70d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651351"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
Acessos a contagem de usuários difusão especificados pelo moniker grupos relacionados ao XUID que aparece no URI. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| Xbox usuário ID (XUID) do usuário relacionado aos XUIDs no grupo.| 
| moniker| cadeia de caracteres| Cadeia de caracteres define o grupo de usuários. O moniker aceitação somente no momento é "Pessoas", com uma letra maiuscula 'P'.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;Recupera a contagem de usuários difusão especificados pelo moniker grupos relacionado para o XUID que aparece no URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de presença](atoc-reference-presence.md)

   