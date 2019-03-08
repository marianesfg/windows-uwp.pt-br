---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646201"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
Um objeto JSON que representa as estatísticas para um hopper. 
<a id="ID4EN"></a>

  
 
O objeto JSON HopperStatsResults tem a seguinte especificação.
 
| Membro| Tipo| Descrição| 
| --- | --- | --- | 
| hopperName| cadeia de caracteres| O nome do hopper selecionado.| 
| Tempo de espera| inteiro com sinal de 32 bits| Média de correspondência de tempo para o hopper (um número integral de segundos). | 
| População| inteiro com sinal de 32 bits| O número de pessoas aguardando correspondências no hopper.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Sintaxe de JSON de exemplo 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>Parent 

[Referência de objeto do JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>Referência 

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   