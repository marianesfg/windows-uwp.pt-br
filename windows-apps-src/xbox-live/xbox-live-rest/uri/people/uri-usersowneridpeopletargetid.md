---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661741"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
Acessa uma pessoa por ID de destino da coleção de pessoas do chamador. O domínio para esses URIs é `social.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| ownerId| cadeia de caracteres| Identificador do usuário cujo recurso está sendo acessado. Deve corresponder ao usuário autenticado. Os valores possíveis são "me", xuid({xuid}) ou gt({gamertag}).| 
| TargetId| cadeia de caracteres| Identificador do usuário cujos dados estão sendo recuperados na lista de pessoas do proprietário, uma ID de usuário do Xbox (XUID) ou um nome de jogador. Valores de exemplo: xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;Obtém uma pessoa por ID de destino da coleção de pessoas do chamador.
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   