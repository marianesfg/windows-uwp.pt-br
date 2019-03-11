---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
assetID: 1e2f5fd3-3d14-9a97-ffbf-ab2a18ff4f15
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 356c7d6eb30eb14156e434f717530d6f4f48a990
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611471"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinername"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}
Acessa os valores aceitáveis para o nome de refinamento de determinada consulta e recebem o tipo de item de mídia. O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| cadeia de caracteres| Obrigatório. Um dos valores de [obter (/media/ {marketplaceId} / metadados/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
| queryrefinername| cadeia de caracteres| Obrigatório. Nome do refinamento de consulta para quais valores são necessários, como "Genre" ou "Década". Consulte QueryRefiners.| 
  
<a id="ID4EKC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername})](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinernameget.md)

&nbsp;&nbsp;Lista os valores aceitáveis para o nome de refinamento de determinada consulta e recebem o tipo de item de mídia.
 
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   