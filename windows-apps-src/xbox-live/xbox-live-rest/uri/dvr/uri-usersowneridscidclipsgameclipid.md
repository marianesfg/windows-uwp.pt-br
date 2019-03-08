---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658331"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
Acesse um único clipe de jogo do sistema se todas as IDs para localizá-lo são conhecidas. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Parâmetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| ownerId| cadeia de caracteres| Identidade de usuário do usuário cujo recurso está sendo acessado. Formatos com suporte: "me" ou "xuid(123456789)". Comprimento máximo: 16.| 
| scid| cadeia de caracteres| ID de configuração de serviço do recurso que está sendo acessado. Deve corresponder a SCID do usuário autenticado.| 
| gameClipId| cadeia de caracteres| ID do clipe de jogo do recurso que está sendo acessado.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;Obtenha um clipe de jogo único do sistema se todas as IDs para localizá-lo são conhecidas.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Parent 

[URIs DVR de jogos](atoc-reference-dvr.md)

   