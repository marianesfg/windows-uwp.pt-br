---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658101"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
Um objeto JSON que representa o **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**. 

> [!NOTE] 
> Esse objeto é implementado pelo Multiplayer 2015 e se aplica somente a essa versão com vários participantes e posterior. Ele destina-se para uso com o modelo de contrato 104/105 ou posterior.  

 
<a id="ID4ES"></a>

  
 
O objeto JSON MultiplayerActivityDetails tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| Um <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> objeto que representa informações de identificação para a sessão.| 
| HandleId| inteiro sem sinal de 64 bits| A ID de identificador correspondente para a atividade.| 
| TitleId| inteiro sem sinal de 32 bits| A ID de título que deve ser iniciada para unir a atividade.| 
| Visibilidade| MultiplayerSessionVisibility| Um <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> valor que indica o estado de visibilidade da sessão.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| Um <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> valor que indica a restrição de associação para a sessão. Essa restrição se aplica se o campo de visibilidade está definido como "aberta".| 
| Fechado| Valor booliano| True se a sessão está fechada temporariamente para ingressar e false caso contrário.| 
| OwnerXboxUserId| inteiro sem sinal de 64 bits| ID de usuário do Xbox do membro que é proprietária da atividade.| 
| MaxMembersCount| inteiro sem sinal de 32 bits| Número de slots de total.| 
| MembersCount| inteiro sem sinal de 32 bits| Número de slots de ocupado.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   