---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623021"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
Contém dados do título do usuário. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
O objeto UserTitle tem a seguinte especificação. Todas as propriedades são necessárias.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| lastUnlock| DateTime| A hora em que uma realização foi acumulada pela última vez.| 
| titleId| inteiro sem sinal de 32 bits| O identificador exclusivo para o título.| 
| titleVersion| cadeia de caracteres| A versão do título.| 
| serviceConfigId| cadeia de caracteres| ID do conjunto de configuração de serviço primário associado com o título.| 
| titleType| cadeia de caracteres| O tipo do título.| 
| Plataforma| cadeia de caracteres| A plataforma com suporte.| 
| name| cadeia de caracteres| O nome de texto do título desta. Comprimento máximo 22.| 
| earnedAchievements| inteiro sem sinal de 32 bits| O número de medalhas acumulado para o título, incluindo realizações desbloqueadas e concluída com êxito os desafios.| 
| currentGamerscore| inteiro sem sinal de 32 bits| Os jogos total que deste usuário obteve esse título.| 
| maxGamerscore| inteiro sem sinal de 32 bits| Os jogos possível total para esse título.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   