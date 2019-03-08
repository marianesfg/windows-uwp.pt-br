---
title: PeopleList (JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
description: " PeopleList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f9ab412088707752d62cc20fd54da2639f26ddc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613321"
---
# <a name="peoplelist-json"></a>PeopleList (JSON)
Coleção de [pessoa](json-person.md) objetos. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
O objeto PeopleList tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| pessoas| matriz de [pessoa](json-person.md)| O [pessoa](json-person.md) objetos que compõem a lista de pessoas.| 
| totalCount| inteiro sem sinal de 32 bits| Número total de [pessoa](json-person.md) disponíveis no conjunto de objetos. Esse valor pode ser usado pelos clientes para a paginação porque ele representa o tamanho do conjunto inteiro, não apenas a resposta mais recente. Valor de exemplo: 680.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>Referência 

[OBTER (/users/ {ownerId} / pessoas)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/users/ {ownerId} / pessoas/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   