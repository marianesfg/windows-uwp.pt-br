---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654111"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
Contém informações sobre um grupo de título de cota. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
O objeto quotaInfo tem as seguintes especificações.
 
Para o armazenamento global
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| quotaBytes| inteiro com sinal de 32 bits | Número máximo de bytes utilizáveis pelo título.| 
| usedBytes| inteiro com sinal de 32 bits | Número de bytes usados pelo título.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 
O exemplo a seguir mostra a resposta para o armazenamento global:
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   