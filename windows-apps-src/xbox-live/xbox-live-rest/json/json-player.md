---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Player (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622491"
---
# <a name="player-json"></a>Player (JSON)
Contém dados para um player em uma sessão de jogo. 
<a id="ID4EN"></a>

 
## <a name="player"></a>Player
 
O objeto jogador tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| customData| matriz de inteiro sem sinal de 8 bits| 1024 bytes de Base64 codificada em dados de jogadores de jogo específico. Esse valor é opaco para o servidor.| 
| gamertag| cadeia de caracteres| Gamertag — um máximo de 15 caracteres — do player. O cliente deve usar esse valor na interface do usuário ao identificar o player. | 
| isCurrentlyInSession| Valor booliano| Indica se o jogador está atualmente na sessão ou saiu da sessão.| 
| seatIndex| inteiro com sinal de 32 bits| O índice do player na sessão.| 
| xuid| inteiro sem sinal de 64 bits| O Xbox usuário ID (XUID) do player.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>Referência 

[GameSession (JSON)](json-gamesession.md)

   