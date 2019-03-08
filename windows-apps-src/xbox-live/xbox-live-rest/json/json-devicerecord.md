---
title: DeviceRecord (JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
description: " DeviceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9746706c00a09cd8b64913b4ae8b5c3426551e48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646141"
---
# <a name="devicerecord-json"></a>DeviceRecord (JSON)
Informações sobre um dispositivo, incluindo seu tipo e os títulos de Active Directory nele. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
O objeto DeviceRecord tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| type| cadeia de caracteres| O tipo de dispositivo do dispositivo. As possibilidades incluem "D", "Xbox360", "MoLIVE" (Windows), "WindowsPhone", "WindowsPhone7" e "PC" (G4WL). Se o tipo é desconhecido (por exemplo iOS, Android ou um título inserido em um navegador da web), "Web" será retornada.| 
| títulos| matriz de [TitleRecord](json-titlerecord.md)| A lista de títulos de Active Directory neste dispositivo.| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
[{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>Referência   