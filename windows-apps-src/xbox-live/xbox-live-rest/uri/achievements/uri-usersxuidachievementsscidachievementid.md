---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599441"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
Retorna detalhes sobre a realização, incluindo seus metadados configurado e dados específicos do usuário. 

> [!NOTE] 
> Só tem suporte para a plataforma. 

 
O domínio para esses URIs é `achievements.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| xuid| inteiro sem sinal de 64 bits| Xbox usuário ID (XUID) do usuário cujo recurso está sendo acessado. Deve corresponder a XUID do usuário autenticado.| 
| scid| GUID| Identificador exclusivo da configuração do serviço cuja medalha está sendo acessada.| 
| achievementid| inteiro sem sinal de 32 bits| Identificador exclusivo (dentro de SCID especificado) da realização que está sendo acessada.| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;Obtém os detalhes de realização.
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>Parent 

[Realizações URIs](atoc-reference-achievementsv2.md)

   