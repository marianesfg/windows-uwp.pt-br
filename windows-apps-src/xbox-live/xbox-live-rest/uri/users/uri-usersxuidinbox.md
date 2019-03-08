---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640891"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
Fornece acesso a um usuário do sistema de mensagens da caixa de entrada para serviços do Xbox LIVE. O domínio para esses URIs é `msg.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid | inteiro sem sinal de 64 bits | O Xbox usuário ID (XUID) do player que está fazendo a solicitação. | 
| messageId | cadeia de caracteres [50] | ID da mensagem que está sendo recuperado ou excluído. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>Métodos válidos 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;Recupera um número especificado de resumos de mensagens de usuário do serviço. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;Exclui uma mensagem do usuário na entrada do usuário.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;Recupera o texto de mensagem detalhada para uma mensagem de determinado usuário, marcá-lo como lido no serviço. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Parent  

[URIs de usuários](atoc-reference-users.md)

   