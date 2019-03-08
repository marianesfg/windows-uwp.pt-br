---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627911"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
Lista de XUIDs na qual executar uma operação. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
O objeto XuidList tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| xuids| matriz de cadeia de caracteres| Lista de valores de ID de usuário do Xbox (XUID) em que uma operação deve ser executada ou dados devem ser retornados.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>Referência 

[POST (/users/ {ownerId} / pessoas/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   