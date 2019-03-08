---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
assetID: 0fcbef77-4607-765e-72e1-d2e7620e2c61
permalink: en-us/docs/xboxlive/rest/uri-medialocalemediaitemtypequeryrefinersubqueryrefinervaluesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 183d15a306d0f15fcb5f9f7fad8411772f763c13
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661811"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinersubqueryrefinervalues"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
Obter a lista de valores inferiores para um valor de refinamento de determinada consulta (por exemplo, "subgenres em um determinado gênero"). O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4EDB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
O valor de refinamento da consulta é passado como um parâmetro de cadeia de caracteres de consulta chamado **queryRefinerValue**, que é feito para permitir que consulta os valores de refinamento com caracteres proibidos em troncos URI a ser passado.
 
Essa API tem suporte apenas para música.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EOB"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EQB"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

  
<a id="ID4E1B"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   