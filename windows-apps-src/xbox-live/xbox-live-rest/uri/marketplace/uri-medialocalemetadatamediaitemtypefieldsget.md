---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields)
assetID: 0ecf27d8-905d-af52-4060-43353d7a3e29
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypefieldsget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 70b43719f3daa86f8e34289545e454cb5d209545
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656421"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypefields"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields)
Lista os campos do qual um pode esperar dados para um determinado mediaitemtype e uma determinada versão de EDS. O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| cadeia de caracteres| Obrigatório. Um dos valores de [obter (/media/ {marketplaceId} / metadados/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   