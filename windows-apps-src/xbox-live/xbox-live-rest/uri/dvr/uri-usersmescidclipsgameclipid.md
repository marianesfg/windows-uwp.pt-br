---
title: /users/me/scids/{scid}/clips/{gameClipId}
assetID: f5bead69-4fc9-f551-39cb-c8754645ac88
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipid.html
description: " /users/me/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3cbe2d2c996b466fd94287129f1add0868f05b05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661771"
---
# <a name="usersmescidsscidclipsgameclipid"></a>/users/me/scids/{scid}/clips/{gameClipId}
Acessar dados de clipe de jogo e metadados. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Parâmetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
| gameClipId| cadeia de caracteres| ID do clipe de jogo do recurso que está sendo acessado.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipiddelete.md)

&nbsp;&nbsp;Excluir um clipe de jogo

[POST (/users/me/scids/{scid}/clips/{gameClipId})](uri-usersmescidclipsgameclipidpost.md)

&nbsp;&nbsp;Atualize metadados de clipe de jogo para os dados do usuário.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Parent 

[URIs DVR de jogos](atoc-reference-dvr.md)

   