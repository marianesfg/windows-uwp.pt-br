---
title: User (JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " User (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641351"
---
# <a name="user-json"></a>User (JSON)
Contém dados de placar de líderes de usuário. 
<a id="ID4EN"></a>

 
## <a name="user"></a>Usuário
 
O objeto de usuário tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| gamertag| cadeia de caracteres| Gamertag do player (máximo de 15 caracteres). O cliente deve usar esse valor na interface do usuário ao identificar o player.| 
| classificação| inteiro com sinal de 32 bits| A classificação do usuário em relação ao usuário que está solicitando os dados de placar de líderes.| 
| rating| cadeia de caracteres| Classificação do usuário.| 
| xuid| inteiro sem sinal de 64 bits| O Xbox usuário ID (XUID) do usuário.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   