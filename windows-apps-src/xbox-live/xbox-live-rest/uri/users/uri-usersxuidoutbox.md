---
title: /users/xuid({xuid})/outbox
assetID: 0b66b885-15ff-be55-f8be-e6e9d85d087e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidoutbox.html
description: " /users/xuid({xuid})/outbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 88f3f3753aeac99db0a8a53e0a2ddde21d034ac5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607541"
---
# <a name="usersxuidxuidoutbox"></a>/users/xuid({xuid})/outbox
Fornece acesso somente de envio para um usuário, das mensagens da caixa de saída para serviços do Xbox LIVE. O domínio para esses URIs é `msg.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI 
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| xuid | inteiro sem sinal de 64 bits | O Xbox usuário ID (XUID) do player que está fazendo a solicitação. | 
  
<a id="ID4EXB"></a>

 
## <a name="valid-methods"></a>Métodos válidos 

[POST (/users/xuid({xuid})/outbox)](uri-usersxuidoutboxpost.md)

&nbsp;&nbsp;Envia uma mensagem especificada para uma lista de destinatários. 
 
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Parent  

[URIs de usuários](atoc-reference-users.md)

   