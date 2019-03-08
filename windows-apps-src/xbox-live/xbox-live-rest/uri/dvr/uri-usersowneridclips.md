---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
description: " /users/{ownerId}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cd711777bcdf0b073dd0821222049b03aa35a23c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599511"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
Lista de acesso de clipes do usuário. Os domínios para esses URIs são `gameclipsmetadata.xboxlive.com` e `gameclipstransfer.xboxlive.com`, dependendo da função do URI em questão.
 
  * [Parâmetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| ownerId| cadeia de caracteres| Identidade de usuário do usuário cujo recurso está sendo acessado. Formatos com suporte: "me" ou "xuid(123456789)". Comprimento máximo: 16.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;Recupere a lista de clipes do usuário.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Parent 

[URIs DVR de jogos](atoc-reference-dvr.md)

   