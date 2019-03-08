---
title: URIs de DVR de jogos
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " URIs de DVR de jogos"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617841"
---
# <a name="game-dvr-uris"></a>URIs de DVR de jogos
 
Esta seção fornece detalhes sobre os endereços de identificador de recurso Universal (URI) e os métodos de protocolo de transporte de hipertexto (HTTP) associados do Xbox Live Services pela *DVR de jogos*.
 
Consoles somente podem registrar um clipe de jogo, mas qualquer dispositivo que pode acessar pode exibir um clipe.
 
Dependendo da função do URI em questão, os domínios para esses URIs são:
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>Nesta seção

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;Clipes de acesso públicos. Esse URI, na verdade, pode ser especificado em duas formas, `/public/scids/{scid}/clips` e `/public/titles/{titleId}/clips`. Veja abaixo para obter mais detalhes.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;Acessar dados de clipe de jogo.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;Acesso um inicial carregue a solicitação.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;Acessar dados de clipe de jogo e metadados.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;Lista de acesso de clipes do usuário.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;Acesse um único clipe de jogo do sistema se todas as IDs para localizá-lo são conhecidas.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent 

[Referência do Universal Resource Identifier (URI)](../atoc-xboxlivews-reference-uris.md)

   