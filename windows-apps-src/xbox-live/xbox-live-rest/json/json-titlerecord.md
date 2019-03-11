---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603981"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
Informações sobre um título, incluindo seu nome e um carimbo de hora da última modificação. 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
Um TitleRecord deve conter um DeviceRecord ou um LastSeenRecord, mas não pode conter ambos.
 
O objeto TitleRecord tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| id| inteiro sem sinal de 32 bits| TitleId do registro.| 
| name| cadeia de caracteres| Nome localizado do título.| 
| atividade| [ActivityRecord](json-activityrecord.md)| A atividade do usuário no título. Retornado se a profundidade é "tudo".| 
| lastModified| DateTime| Timestamp UTC quando o registro foi atualizado pela última vez.| 
| posicionamento| cadeia de caracteres| O local do aplicativo dentro da interface do usuário. As possibilidades incluem "fill", "completo", "encaixado" ou "em segundo plano". O padrão é "full" para dispositivos sem a capacidade de colocar os aplicativos.| 
| Estado| cadeia de caracteres| O estado do título. Pode ser "active" ou "inativos" (o padrão). O título define o estado com base em seus próprios critérios para a atividade e inatividade.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>Referência 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   