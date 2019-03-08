---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612621"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
Coleção de [Person (JSON)](json-person.md) objetos. 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
O objeto PersonSummary tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| Valor booliano| Se o chamador tiver marcado o destino como um favorito. Valores de exemplo: true| 
| hasCallerMarkedTargetAsKnown| Valor booliano| Se o chamador marcou o destino como conhecidos. Valores de exemplo: true| 
| isCallerFollowingTarget| Valor booliano| Se o chamador está seguindo o destino. Valores de exemplo: true| 
| isTargetFollowingCaller| Valor booliano| Se o destino está seguindo o chamador. Valores de exemplo: true| 
| legacyFriendStatus| cadeia de caracteres| Status de amigo herdados do destino, como visto pelo chamador. Pode ser "None", "MutuallyAccepted", "OutgoingRequest" ou "IncomingRequest". Valores de exemplo: "MutuallyAccepted"| 
| recentChangeCount| inteiro sem sinal de 32 bits| Opcional. Número de alterações recentes no grafo social de destino. Esse valor só existirá quando um usuário está exibindo o resumo de sua próprias. Valores de exemplo: 5| 
| targetFollowerCount| > inteiro sem sinal de 32 bits| Número de pessoas que estão seguindo o destino. Valores de exemplo: 1308| 
| targetFollowingCount| inteiro sem sinal de 32 bits| Número de pessoas que o destino está seguindo. Valores de exemplo: 112| 
| Marca d'água| cadeia de caracteres| Opcional. Recente alteração de marca d'água para o destino. Esse valor só existirá quando um usuário está exibindo o resumo de sua próprias. Valores de exemplo: 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   