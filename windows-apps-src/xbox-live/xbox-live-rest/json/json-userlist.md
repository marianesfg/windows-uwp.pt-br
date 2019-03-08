---
title: UserList (JSON)
assetID: 894f5a39-4eed-0c5b-fc45-cb0097dc46fd
permalink: en-us/docs/xboxlive/rest/json-userlist.html
description: " UserList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 46e5323f4eae8e91b61295c4112b5bacfc8a1759
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598491"
---
# <a name="userlist-json"></a>UserList (JSON)
Uma coleção de [usuário](json-user.md) objetos. 
<a id="ID4ER"></a>

 
## <a name="userlist"></a>UserList
 
O objeto de UserList tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| usuários| Matriz de [usuário (JSON)](json-user.md)| Consulte o exemplo JSON abaixo.| 
  
<a id="ID4EPB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ] 
}
    
```

  
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   