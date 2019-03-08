---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608301"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
Contém os dados agregados para sessões de adequação a um usuário. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
O objeto AggregateSessionsResponse tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| totalDurationInSeconds| inteiro com sinal de 64 bits| Duração total de sessões em segundos durante o período de agregação.| 
| totalJoules| inteiro com sinal de 64 bits| Total de energia consumida — em joules — durante o período de agregação. | 
| totalSessions| inteiro com sinal de 64 bits| Número total de sessões durante o período de agregação.| 
| weightedAverageMets| número de ponto flutuante de precisão simples | Equivalente metabólico média ponderada do valor de tarefa (MET) durante o período de agregação. O valor MET é a proporção da taxa de um indivíduo metabólico durante uma atividade em relação à taxa de metabólico do indivíduo em repouso. Porque a taxa de Metabolic (metabólico) para posicionar é 1.0, independentemente de peso de um indivíduo, e os valores MET são em relação a um indivíduo pairando metabólico taxa, eles podem ser usados para comparar a intensidade de uma atividade que está sendo executada por indivíduos diferentes pesos.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   