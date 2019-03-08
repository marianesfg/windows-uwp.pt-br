---
title: Refinadores de consulta de EDS
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " Refinadores de consulta de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611461"
---
# <a name="eds-query-refiners"></a>Refinadores de consulta de EDS
 
<a id="ID4EO"></a>

  
 
Os parâmetros a seguir podem ser usados para refinar uma consulta de serviços de descoberta de entretenimento (EDS) para um conjunto mais direcionada de itens. Nenhum desses parâmetros são necessários em qualquer API, mas eles aceita qualquer API que aceita refiners de consulta.
 
Os nomes de parâmetro podem ser passados como valores para qualquer parâmetro de "queryRefiners". Este será em seguida, retorna o número de itens que seriam retornados se a solicitação foram repetida com o refinamento de consulta é aplicado, divididas por cada valor de refinamento a consulta.
 
Aqui está como isso poderia funcionar na prática:
 
   * É feita uma chamada para a API de navegação, incluindo o parâmetro "queryRefiners = gênero".
   * A API retorna oito jogos. Além dos itens, uma lista de cada gênero que tem itens será retornada, junto com quantos itens pertencem a daquele gênero. Para um jogo, isso pode ser "Shooter: 3, quebra-cabeça: 5".
   * Uma segunda consulta é feita. Ela é idêntica à primeira, exceto pelo fato de "gênero = Shooter" é adicionado.
   * Agora, a resposta contém apenas três jogos, que pertencem à categoria "Shooter".
  
| Parâmetro| Tipo de dados| Descrição| 
| --- | --- | --- | 
| <b>decade</b>| cadeia de caracteres| A década em que todos os itens devem foram lançados.| 
| <b>genre</b>| matriz de cadeia de caracteres| A lista de gêneros que todos os itens devem ter.| 
| <b>labelOwner</b>| cadeia de caracteres| O rótulo de música associado com o artista, álbum ou faixa.| 
| <b>network</b>| matriz de cadeia de caracteres| A rede que criou os itens.| 
| <b>studio</b>| matriz de cadeia de caracteres| O studio que criou os itens.| 
| <b>xboxAppCategories</b>| matriz de cadeia de caracteres| A lista de categorias que devem ter a todos os aplicativos do Xbox.| 
| <b>xboxAvatarClothes</b>| matriz de cadeia de caracteres| A lista de tipos de vestuário em que todos os itens de Xbox Avatar devem ter.| 
| <b>xboxAvatarStores</b>| matriz de cadeia de caracteres| A lista de repositórios para Xbox que todos os itens do avatar devem pertencer.| 
| <b>xboxGamePublisherBits</b>| matriz de cadeia de caracteres| A lista de bits de jogo que devem ser definidas em todos os itens de GameType ou itens de tipo de aplicativo.| 
| <b>xboxIsBrowsable</b>| Valor booliano| Se <b>verdadeira</b>, retornará jogos completos que não são acionáveis diretamente, além de conteúdo acionável. O padrão é <b>falsos</b>.| 
| <b>xboxHasChildMediaItemTypes</b>| matriz de cadeia de caracteres| Todos os itens retornados com um grupo de mídia de jogo devem ter filhos cujo tipo de item de mídia é um dos valores fornecidos.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   