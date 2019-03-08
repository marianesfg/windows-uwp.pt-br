---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589681"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
Permite a navegação para itens dentro de um único grupo de mídia. A API de navegação permite que os clientes procurar itens de dentro de um único grupo de mídia. Páginas de dados podem ser acessadas usando o parâmetro skipItems não sequencialmente em vez de usar o token de continuação.
 
Essa API também permite a navegação dentro os filhos de um determinado item. Por exemplo, passando uma ID e um parâmetro MediaItemType para um Xbox 360 jogo, isso permite navegar e diltering nos filhos do item, como itens de Avatar ou DLC para o jogo.
 
Essa API aceita Refiners de consulta.
 
Alguns cenários para recuperar filhos incluem:
 
   * Álbum para faixas
   * Série de estações
   * Estações para episódios
   * Acompanhar o vídeo de música
   * Artista nos álbuns
   * Jogos para complementos de jogo (DLC, Avatar, temas, etc.)
  
O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Parâmetros de URI](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[OBTER (media / {marketplaceId} / Procurar)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;Permite a navegação para itens dentro de um único grupo de mídia. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Parent 

[URIs de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   