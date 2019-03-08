---
title: Person (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Person (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640861"
---
# <a name="person-json"></a>Person (JSON)
Metadados sobre uma única pessoa no sistema de pessoas. 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
O objeto de pessoa tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid| cadeia de caracteres| Obrigatório. Xbox usuário ID (XUID) no formato decimal. Valor de exemplo: 2603643534573573.| 
| isFavorite| Valor booliano| Obrigatório. Se essa pessoa é aquele que o usuário se preocupa mais. Porque os usuários podem ter um grande número de pessoas em sua lista de pessoas, pessoas favoritas devem ser priorizadas em experiências e mostradas antes de outras pessoas que não são seus favoritos.| 
| isFollowingCaller| Valor booliano| Opcional. Se essa pessoa está seguindo o usuário em cujo nome, a chamada à API foi feita.| 
| socialNetworks| matriz de cadeia de caracteres| Opcional. Dentro de redes externa na qual o usuário e essa pessoa tem uma relação.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Referência 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   