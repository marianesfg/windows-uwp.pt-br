---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654121"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
Solicitação para obter informações sobre quais informações de presença avançada devem ser usadas. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
O objeto RichPresenceRequest tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| id| cadeia de caracteres| O <b>friendlyName</b> da cadeia de caracteres para usar recursos avançados de presença.| 
| scid| cadeia de caracteres| Scid que nos informa onde as cadeias de caracteres de recursos avançados de presença são definidas.| 
| param. autom.| matriz de cadeia de caracteres| Matriz de <b>friendlyName</b> cadeias de caracteres com a qual a cadeia de caracteres de recursos avançados de presença de término. Somente os nomes amigáveis de enumeração devem ser especificados, não estatísticas. Deixando vazio nesse removerá qualquer valor anterior.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   