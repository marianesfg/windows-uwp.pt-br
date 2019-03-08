---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613371"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
Informações sobre quando o sistema viu um usuário, disponível quando o usuário não tem nenhum DeviceRecord válido pela última vez. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
O objeto LastSeenRecord tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| deviceType| cadeia de caracteres| O tipo de dispositivo no qual o usuário estava presente último.| 
| titleId| inteiro sem sinal de 32 bits| O identificador do título em que o usuário estava presente último.| 
| titleName| cadeia de caracteres| O nome do título em que o usuário estava presente último.| 
| carimbo de hora| DateTime| Carimbo de hora UTC que indica quando o usuário foi o último presente.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Referência   