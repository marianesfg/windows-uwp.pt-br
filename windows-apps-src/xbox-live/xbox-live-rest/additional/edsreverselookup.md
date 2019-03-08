---
title: Pesquisa inversa de EDS para vídeo
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " Pesquisa inversa de EDS para vídeo"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598431"
---
# <a name="eds-reverse-lookup-for-video"></a>Pesquisa inversa de EDS para vídeo
 
  * [Etapas de pesquisa inversas](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>Etapas de pesquisa inversas
 
Pesquisa inversa de serviços de descoberta (EDS) entretenimento tem suporte para todos os tipos de mídia de vídeo (**MediaItemType.Movie**, **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**, e **MediaItemType.TVShow**), bem como **MediaItemType.Unknown**.
 
Pesquisa inversa exige 4 parâmetros a serem passados: 
   * `idType=ScopedMediaId`
   * `ids=` a ID do provedor de mídia
   * `ScopeIdType=Title`
   * `ScopeId=` a ID do provedor de título
 
 
Normalmente, a pesquisa inversa requer 2 etapas: 
   * Recupere a id do provedor de mídia (por exemplo, de uma chamada de detalhes) se não estiver disponível. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * Emita a chamada para a pesquisa inversa usando o **ProviderMediaId** campo da resposta anterior: 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
Se o **ProviderMediaId** campo não foram recuperado da EDS, em seguida, o campo deve ser codificado de URL a ser passado corretamente para EDS.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   