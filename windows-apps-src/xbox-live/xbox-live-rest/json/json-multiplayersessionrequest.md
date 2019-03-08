---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628341"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
O objeto JSON de solicitação é passada para uma operação em um **MultiplayerSession** objeto. 
<a id="ID4EQ"></a>

  
 
O objeto JSON MultiplayerSessionRequest tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| Constantes| objeto| Configurações de somente leitura que são mescladas com o modelo de sessão para produzir as constantes para a sessão. | 
| propriedades | objeto | Alterações a ser mesclado com as propriedades de sessão.| 
| members.me | objeto| Constantes e as propriedades que são muito parecidas, como suas contrapartes de nível superior. Qualquer método PUT exige que o usuário seja membro da sessão e adiciona o usuário se necessário. Se "me" é especificado como nulo, o membro que está fazendo a solicitação será removido da sessão. | 
| membros | objeto| Outros objetos que representam os usuários a adicionar à sessão, inseridos por um índice baseado em zero. O número de membros em uma solicitação sempre começa com 0, mesmo se a sessão já contém membros. Os membros são adicionados à sessão na ordem em que aparecem na solicitação. Propriedades de membro só podem ser definidas pelo usuário aos quais eles pertencem. | 
| servidores | objeto| Valores que indicam as atualizações e adições à sessão do conjunto de participantes do servidor associado. Se um servidor for especificado como null, essa entrada do servidor será removida da sessão. | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>Estrutura de solicitação
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>Referência 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   