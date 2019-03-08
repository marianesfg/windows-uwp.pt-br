---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651241"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
Um objeto JSON que representa o **MultiplayerSessionReference**. 
<a id="ID4EQ"></a>

  
 
O objeto JSON MultiplayerSessionReference tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| GUID| Identificador de configuração de serviço (SCID). Parte 1 do identificador de sessão.| 
| templateName | cadeia de caracteres | Nome da instância atual do modelo de sessão. Parte 2 do identificador de sessão. | 
| name | cadeia de caracteres | Nome da sessão. Parte 3 do identificador de sessão. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>Referência 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   