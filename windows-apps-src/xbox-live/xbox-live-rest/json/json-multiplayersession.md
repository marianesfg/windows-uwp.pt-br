---
title: MultiplayerSession (JSON)
assetID: d013af81-bfbf-c50a-5696-2bb561448616
permalink: en-us/docs/xboxlive/rest/json-multiplayersession.html
description: " MultiplayerSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 32eca20af0714c5968cf51fcd568d89f2768f369
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656481"
---
# <a name="multiplayersession-json"></a>MultiplayerSession (JSON)
Um objeto JSON que representa o **MultiplayerSession**. 
<a id="ID4EQ"></a>

  
 
O objeto JSON MultiplayerSession tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| Constantes| objeto| Configurações de somente leitura que são mescladas com o modelo de sessão para produzir as constantes para a sessão. | 
| propriedades | objeto | Alterações a ser mesclado com as propriedades de sessão.| 
| members.me | objeto| Constantes e as propriedades que são muito parecidas, como suas contrapartes de nível superior. Qualquer método PUT exige que o usuário seja membro da sessão e adiciona o usuário se necessário. Se "me" é especificado como nulo, o membro que está fazendo a solicitação será removido da sessão. | 
| membros | objeto| Outros objetos que representam os usuários a adicionar à sessão, inseridos por um índice baseado em zero. O número de membros em uma solicitação sempre começa com 0, mesmo se a sessão já contém membros. Os membros são adicionados à sessão na ordem em que aparecem na solicitação. Propriedades de membro só podem ser definidas pelo usuário aos quais eles pertencem. | 
| servidores | objeto| Valores que indicam as atualizações e adições à sessão do conjunto de participantes do servidor associado. Se um servidor for especificado como null, essa entrada do servidor será removida da sessão. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
      "properties": {
        "system": {
          "turn": []
        },
        "custom": {
          "KANWE": "MGMSY"
        }
      },
      "constants": {
        "system": {
          "visibility": "open"
        },
        "custom": {}
      },
      "servers": {},
      "members": {
        "first": 0,
        "end": 1,
        "count": 1,
        "accepted": 0,
        "0": {
          "next": 1,
          "pending": true,
          "properties": {
            "system": {},
            "custom": {}
          },
          "constants":  {
            "system": {
              "xuid": 5500461
            },
            "custom": {
              "8216": "WXMRJ"
            }
          }
        }
      },
      "key": "8d050174-412b-4d51-a29b-d55a34edfdb7~integration~bcd0088d76a94c60be4b75f139243a1f"
    }
  
    
```

  
<a id="ID4EHB"></a>

 
## <a name="request-structure"></a>Estrutura de solicitação
Para a estrutura de solicitação associada com essa especificação JSON, consulte [MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md).  
<a id="ID4EPB"></a>

 
## <a name="response-structure"></a>Estrutura de resposta
 

```json
{
  // The contract version of this rendering of the session. A function of the contract version of the request and constants/system/version.
  "contractVersion": 107,

  // The branch and change number of the session as of this rendering.
  "branch": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
  "changeNumber": 5,

  // Use for tracing.
  // If an empty session is deleted and then restarted, the correlation ID will remain the same.
  "correlationId": "0fe81338-ee96-46e3-a3b5-2dbbd6c41c3b",

  // The time that the session began.
  // If an empty session is deleted and then restarted, this time will be the time of the restart.
  "startTime": "2009-06-15T13:45:30.0900000Z",

  // If any timeouts are in progress, this is the time that the next one will fire.
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  // Present during member initialization.
  // The "stage" goes from "joining" to "measuring" to "evaluating".
  // If episode #1 fails, then "stage" is set to "failed" and the session cannot be initialized.
  // Otherwise, when an initialization episode completes, the "initializing" object is removed.
  // If "autoEvaluate" is set, "evaluating" is skipped. If neither "metrics" nor "measurementServerAddresses" is set, "measuring" is skipped.
  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  // Set after the "measuring" stage of initialization episode #1, if "peerToHostRequirements" is set and no /properties/system/host is set.
  // Cleared once a /properties/system/host is set.
  // Host candidates are device tokens listed in order, order of preference.
  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },

  "members": {
    // On large sessions, at most one entry is present, "me".
    "1": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
      // Only if the member has accepted. Optional (not set if no gamertag claim was found).
      "gamertag": "stacy",
      // This is set when the member uploads a secure device address. It's a case-insensitive string that can be used for equality comparisons.
      "deviceToken": "9f4032ba7",
      // This is set to "open", "moderate", or "strict" when the member uploads a V1.x secure device address.
      "nat": "strict",
      // This value is removed once the user does their first PUT to the session.
      "reserved": true,
      // If the member is active, this is the title in which they are active, in decimal.
      "activeTitleId": "8397267",
      // The time the user joined the session. If "reserved" is true, the time the reservation was made.
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      // Present if this member is in the /properties/system/turn array, otherwise not.
      "turn": true,

      // Set when transitioning out of the "joining" or "measuring" stage if this member doesn't pass.
      // In order of precedence, could be set to "timeout", "latency", "bandwidthdown", "bandwidthup", "network", "group", or "episode".
      // The "network" value means the network configuration and/or conditions (such as conflicting NAT) prevented the QoS metrics from being measured.
      // The only possible value at the end of "joining" is "group". (On timeout from "joining", the reservation is removed.)
      // The "episode" value is set after a failed "evaluation" stage on all members of the initialization episode that didn't fail during "joining" or "measuring".
      "initializationFailure": "latency",

      // If "memberInitialization" is set and the member was added with "initialize": true, this is set to the initialization episode that the member will participate in.
      // One is a special value used for the members added to a new session at create time.
      // Removed when the initialization episode ends.
      "initializationEpisode": 1,

      // The next member's index, which is the same as 'next' below if there's no more.
      "next": 4
    },
    "4": { "next": 5 /* etc */ }
  },

  "membersInfo": {
    "first": 1,  // The first member's index.
    "next": 5,  // The index that the next member added will get.
    "count": 2,  // The number of members.
    "accepted": 1  // The number of members that are no longer 'reserved'.
  },

  "servers": {
    "name": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ }
    }
  }
}
    
```

  
<a id="ID4EWB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EYB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   