---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661751"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
Permite pesquisar itens dentro de um único grupo de mídia. Observe que páginas de dados retornados da pesquisa podem ser acessadas sem sequência usando o parâmetro skipItems em vez de usar o token de continuação. Essa API aceita Refiners de consulta.
 
O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[OBTER (media / {marketplaceId} / singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;Permite pesquisar itens dentro de um único grupo de mídia. 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   