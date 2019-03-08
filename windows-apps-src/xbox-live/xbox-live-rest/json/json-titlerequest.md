---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593791"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
Solicitação para obter informações sobre um título. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
O objeto TitleRequest tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| id| inteiro sem sinal de 32 bits| Identificador do título.| 
| atividade| [ActivityRequest](json-activityrequest.md)| No título informações, incluindo informações de presença e mídia avançadas, se disponível.| 
| Estado| cadeia de caracteres| Se um usuário está ativo ou não. Este campo é obrigatório para marcar um usuário como inativo. O padrão é "ativo".| 
| posicionamento| cadeia de caracteres| O modo de posicionamento do título. Os valores possíveis incluem "full", "fill", "encaixado" ou "em segundo plano". O padrão é "full".| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Referência 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   