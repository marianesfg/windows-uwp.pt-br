---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655701"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
Retorna oferece detalhes e os metadados sobre um ou mais itens. O domínio para esses URIs é `eds.xboxlive.com`.
 
Os detalhes da API é diferente da API relacionadas e a API de procurar (quando a passagem em uma ID) como essas APIs para retornar informações sobre os outros itens que estão associados com a ID de fiven, explícita ou implicitamente, enquanto a API de detalhes retorna informações adicionais sobre o mesmo item.
 
Várias IDs dos tipos de item de mídia diferentes podem ser passadas em uma única chamada (contanto que eles não são do tipo ProviderContentID - veja abaixo), mas todos eles devem pertencer ao mesmo grupo de mídia. No entanto, há alguns cenários de cliente em que o chamador não saiba o grupo de mídia. A API dá suporte a isso, permitindo que um valor de sepcial "Desconhecido" para o grupo de mídia nas seguintes situações:
 
   * idType = XboxHexTitle, que produzirá o tipo de aplicativo ou GameType itens
   * idType = ProviderContentId, que produzirá MovieType ou TVType itens
  
O gráfico a seguir resume o mapeamento de inteiro de qual ID tipos podem ser fornecidos com quais grupos de mídia:
 
| Tipo de ID| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Desconhecido| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Canonical| Y| Y| Y| Y| Y| Y| Y| N| 
| ZuneCatalog| N| N| Y| Y| Y| Y| N| N| 
| ZuneMediaInstance| N| N| Y| N| Y| Y| N| N| 
| Oferta| Y| Y| Y| N| Y| Y| N| N| 
| AMG| N| N| N| N| Y| N| N| N| 
| MediaNet| N| N| N| N| Y| N| N| N| 
| XboxHexTitle| Y| Y| N| N| N| N| N| Y| 
| ProviderContentId| N| N| Y| N| N| Y| N| Y| 
 
  * [Notas de parâmetro](#ID4EEH)
  * [Parâmetros de URI](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>Notas de parâmetro
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
Isso é usado para o provedor de pesquisa ID específica. Ex. Netflix Id ou ID Hulu.
 
Quando idType ProviderContentId, apenas um único valor é aceito. Isso ocorre porque ProviderContentIds são o único tipo de ID que pode conter o '.' caracteres. Uma vez que o '.' caractere também é o separador que usamos entre IDs, há ambiguidade entre o que é um delimieter entre IDs e o que é parte da própria ID. O restante da API funciona da mesma forma para ProviderContentIds, exceto para a funcionalidade de pesquisa em massa.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[OBTER (/media/ {marketplaceId} / detalhes)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;Retorna oferece detalhes e os metadados sobre um ou mais itens. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   