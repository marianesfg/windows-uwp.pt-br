---
title: /users/xuid({xuid})/devices/current/titles/current
assetID: f149c68b-9874-e348-4e1d-6acf5d305c49
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrent.html
description: " /users/xuid({xuid})/devices/current/titles/current"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0b5c17f1791d69ca8a4c902b6c37736c4da28a31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645661"
---
# <a name="usersxuidxuiddevicescurrenttitlescurrent"></a>/users/xuid({xuid})/devices/current/titles/current
Acesse a presença de um título ou usuário do título. O domínio para esses URIs é `userpresence.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário de destino.| 
  
<a id="ID4EUB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentdelete.md)

&nbsp;&nbsp;Remover a presença de um título de fechamento, em vez de aguardar o [PresenceRecord](../../json/json-presencerecord.md) expirar.

[POST (/users/xuid({xuid})/devices/current/titles/current)](uri-usersxuiddevicescurrenttitlescurrentpost.md)

&nbsp;&nbsp;Atualize um título com a presença de um usuário.
 
<a id="ID4EBC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EDC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de presença](atoc-reference-presence.md)

   