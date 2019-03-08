---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
description: " UserClaims (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21b4322d002747145c3b09e0f3cd7eb03874380b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623341"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
Retorna informações sobre o usuário autenticado atual. 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
O objeto de UserClaims tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| gamertag| cadeia de caracteres| gamertag do usuário.| 
| xuid| inteiro sem sinal de 64 bits| O Xbox usuário ID (XUID) do usuário.| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   