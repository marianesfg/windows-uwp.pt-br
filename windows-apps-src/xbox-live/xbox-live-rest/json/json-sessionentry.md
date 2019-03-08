---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589731"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
Contém dados para uma sessão de adequação. 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
O objeto SessionEntry tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| durationInSeconds| inteiro com sinal de 32 bits | Duração, em segundos, da sessão. | 
| Joules| inteiro com sinal de 32 bits | Energia — em joules — gravado na sessão. | 
| atendidos| número de ponto flutuante de precisão simples| Valor médio atendem ao longo da duração da sessão. O valor MET é a proporção da taxa de um indivíduo metabólico durante uma atividade em relação à taxa de metabólico do indivíduo em repouso. Porque a taxa de Metabolic (metabólico) para posicionar é 1.0, independentemente de peso de um indivíduo, e os valores MET são em relação a um indivíduo pairando metabólico taxa, eles podem ser usados para comparar a intensidade de uma atividade que está sendo executada por indivíduos diferentes pesos.| 
| serverTimestamp| DateTime| Tempo — com base em UTC — entrada foi inserida no servidor. | 
| Código-fonte| inteiro sem sinal de 8 bits| Origem da sessão.| 
| carimbo de hora| DateTime| Tempo — com base no tempo Universal Coordenado (UTC) — entrada foi criada no cliente. | 
| titleId| inteiro sem sinal de 64 bits| Título – em decimal — que criou a entrada.| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

   