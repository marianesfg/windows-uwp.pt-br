---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649411"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
Contém as novas pontuações de reputação base ao qual as pontuações de um usuário existente devem ser alteradas. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
O objeto ResetReputation tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| fairplayReputation| number| Os detalhes desejados novos base pontuação de reputação de Fairplay para o usuário (intervalo válido 0 a 75).| 
| commsReputation| number| Os detalhes desejados novos base pontuação de reputação de comentários do usuário (intervalo válido 0 a 75).| 
| userContentReputation| number| Os detalhes desejados novos base pontuação UserContent reputação do usuário (intervalo válido 0 a 75).| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   