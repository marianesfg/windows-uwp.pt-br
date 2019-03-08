---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641581"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
Obter o token de classificação de conteúdo. O domínio para esses URIs é `eds.xboxlive.com`.
 
  * [Comentários](#ID4EV)
  * [Parâmetros de URI](#ID4ELB)
  * [Parâmetros de cadeia de caracteres de consulta](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Comentários
 
Impor sobre o conteúdo de filhos que têm permissão para ver os controles dos pais é uma tarefa complicada. Não apenas a cada tipo de item de mídia tem seu próprio sistema de classificação, mas esses sistemas de classificação podem variar de país para país. Isso significa que há várias partes distintas de dados que precisam ser especificado para filtrar corretamente todos os itens.
 
Em vez de especificar todos os parâmetros em todas as chamadas de API, essa API gera um valor para passar para **combinedContentRating** parâmetros em outras APIs e se comunicar as mesmas informações. Isso foi projetado para tornar as APIs mais fácil de usar e manter, como vários parâmetros passados para essa API são recolhidos para um valor único e reutilizável para outras APIs.
 
Embora os valores exatos retornados por essa API, eventualmente, podem mudar, eles devem alterar com muito pouca frequência (como entre versões de serviços de descoberta de entretenimento (EDS)) e, portanto, pode ser armazenado em cache por longos períodos de tempo. Qualquer API aceitando uma **combinedContentRating** parâmetro fornecerá uma mensagem de erro significativa se o valor passado é inválido, que é uma indicação que o chamador precisa apenas chamar essa API novamente para receber um valor atualizado. Se uma API aceita uma **combinedContentRating** parâmetro, mas um não for fornecido, nenhuma filtragem de conteúdo será realizada com base em controles dos pais. 

> [!NOTE] 
> Isso não significa que conteúdo somente "seguro" é retornado – isso significa que todo o conteúdo é retornado, incluindo conteúdo potencialmente explícito. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>Parâmetros do URI
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | 
| marketplaceId| cadeia de caracteres| Obrigatório. Valor obtido de cadeia de caracteres a <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta
 
| Parâmetro| Tipo| Descrição| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| Valor booliano| Opcional. Filtra música explícita.| 
| filterFamilyOnlyApps| Valor booliano| Opcional. Filtre os aplicativos não marcados como família amigáveis.| 
| filterUnrated| Valor booliano| Opcional. Determina se o conteúdo que é não classificado deve ser incluído na resposta ou não.| 
| maxGameRating| inteiro com sinal de 32 bits| Opcional. Filtra os jogos.| 
| maxMovieRating| inteiro com sinal de 32 bits| Opcional. Filtra os filmes acima de um nível específico.| 
| maxTVRating| inteiro com sinal de 32 bits| Opcional. Filtros de TV.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Parent 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[Cabeçalhos comuns da EDS](../../additional/edscommonheaders.md)

 [Parâmetros de EDS](../../additional/edsparameters.md)

 [EDS Refiners de consulta](../../additional/edsqueryrefiners.md)

 [URIs de Marketplace](atoc-reference-marketplace.md)

 [Referência adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   