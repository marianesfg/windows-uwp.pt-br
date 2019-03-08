---
title: Parâmetros de EDS
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " Parâmetros de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655411"
---
# <a name="eds-parameters"></a>Parâmetros de EDS

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>Parâmetros de cadeia de caracteres de consulta

Esses parâmetros de consulta não são necessariamente aceitas por todos os [entretenimento descoberta de serviços (EDS) APIs](../uri/marketplace/atoc-reference-marketplace.md), mas todas são aceitas por mais de uma API.

| Parâmetro| Tipo| Descrição|
| --- | --- | --- |
| combinedContentRating| cadeia de caracteres| Opcional. Ver [obter (/media/ {marketplaceId} / contentRating)](../uri/marketplace/uri-medialocalecontentratingget.md).|
| continuationToken| cadeia de caracteres| Opcional. O token de continuação é um blob opaco que contém informações que o serviço precisa para a paginação em determinados cenários. Se o valor for omitido, a primeira página de resultados será retornada (em que o tamanho da página é determinado pelo parâmetro maxItems), juntamente com um token de continuação que pode ser usado para obter a segunda página de resultados. A segunda página contém o token de continuação para a terceira página de resultados e assim por diante.|
| Desejado| cadeia de caracteres| Opcional. Consulte a API de nome de campo combinado.|
| desiredMediaItemTypes| matriz de cadeia de caracteres| Opcional. Esse parâmetro determina os tipos de itens a serem retornados na resposta.|
| Domínio| cadeia de caracteres| Opcional. O parâmetro de domínio determina o contexto de marketplace do jogo e o aplicativo no qual cliente está chamando para. Por padrão, o domínio é "Moderno", indicando que o cliente só pode solicitar ao conteúdo do Xbox One. Se o cliente deseja mudar o domínio para exibir o conteúdo com o Xbox 360, ele deve especificar o domínio como "Xbox360". No momento, entre domínios resultados não têm suporte. Os valores possíveis são: <ul><li>Xbox360</li><li>Moderno</li></ul> O valor de "Xbox360" só é suportado para singleMediaGroupSearch. Há suporte para navegação e detalhes. crossMediaGroupSearch não tem suporte e retornará um erro 400.|
| campos| cadeia de caracteres| Opcional. Ver [obter (/media/ {marketplaceId} / campos)](../uri/marketplace/uri-medialocalefieldsget.md).|
| firstPartyOnly| Valor booliano| Parâmetro de filtro opcional. Determina se o conteúdo de somente a primeira parte é retornado ou se os dois primeiros - e -conteúdo de terceiros é retornado da consulta. |
| freeOnly| Valor booliano| Parâmetro de filtro opcional. Restringe os resultados para liberar apenas o conteúdo.|
| GroupBy| TK| O parâmetro groupBy é usado para ajudar a categorizar o conjunto de resultados em grupos, em vez de um único conjunto de resultados. Especificar esse parâmetro, você modificará o conjunto de resultados para retornar várias listas de itens, em que o número de itens em cada bucket é determinado pelo parâmetro maxItems. <ul><li>MediaGroup - os resultados são agrupados por MediaGroup.</li></ul> |
| hasTrailer| Valor booliano| Parâmetro de filtro opcional. Determina se os itens retornados devem conter um trailer ou se precisar de um marcador é opcional. Se o valor for true, todos os itens devem ter trailers.|
| id| cadeia de caracteres| Opcional. Se fornecido, restringe os resultados para ser filhos apenas do item com a ID especificada. Se esse parâmetro for fornecido, MediaItemType também deve ser especificado. |
| ids| matriz de cadeia de caracteres| Obrigatório. Todas as IDs (até 10) para os quais detalhes serão retornados. Observe que qualquer ID que contém caracteres ilegais para colocar em uma URL (as IDs de tipo ProviderContentId são as próprias URLs normalmente completos e, portanto, contém caracteres ilegais) deve ser codificada como URL a serem enviados corretamente ao EDS.|
| idType| cadeia de caracteres| Opcional. O tipo de IDs que são passados para o parâmetro de ids. Os valores válidos incluem: <ul><li>Canonical (Bing/Marketplace)</li><li>XboxHexTitle (aplicativo de reprodução no console)</li></ul>  Todos fornecidos que IDs devem compartilhar o mesmo idType. Se esse valor for omitido, todas as IDs são consideradas Canonical.|
| latestOnly| Valor booliano| Parâmetro de filtro opcional. Restringe os resultados para somente aqueles com a data de lançamento mais recente.|
| maxItems| inteiro com sinal de 32 bits| Opcional. Determina o número máximo de itens que devem ser retornados de uma chamada. Os valores válidos são números entre 1 e 25, inclusive. O parâmetro padrão é 25 se ele for omitido.|
| mediaGroup| cadeia de caracteres| Opcional. O grupo de mídia de IDs. Todos fornecidos que IDs devem compartilhar o mesmo grupo de mídia.|
| MediaItemType| cadeia de caracteres| Opcional. O tipo do item cuja ID foi especificado no parâmetro de id. Se o parâmetro id for fornecido, esse parâmetro também deve ser especificado.|
| orderBy| cadeia de caracteres| Obrigatório. O parâmetro orderBy determina como os itens que estão sendo retornados devem ser classificados. Os valores comuns para esse campo são listados aqui, mas algumas APIs podem dar suporte a valores adicionais.<ul><li>playCountDaily - por contagem de horas de mídia reproduzida para o dia mais recente.</li><li>freeAndPaidCountDaily - por contagem de compras gratuitas e pagas, para o dia mais recente.</li><li>paidCountAllTime - por contagem de compras pagas apenas, para o tempo todo.</li><li>paidCountDaily - por contagem de apenas pagas compras, para o dia mais recente.</li><li>digitalReleaseDate - por data disponível para download.</li><li>releaseDate - por data disponível nos repositórios, voltando para a data de lançamento digital (se disponível).</li><li>userRatings - pela classificação média do usuário.</li></ul> |
| preferredProvider| cadeia de caracteres| Opcional. Se o usuário tiver um provedor de conteúdo preferido, como a Comcast Xfinity ou FIOS Verizon, a ID do provedor pode ser passado no. Enquanto a ordem real dos itens não serão alterados, para cada item, informações do provedor especificado serão exibidas na parte superior da lista de provedores (se o provedor de conteúdo preferido tem o item disponível).|
| q| cadeia de caracteres| Obrigatório. Usado na pesquisa de termo da consulta.|
| queryRefiners| matriz de cadeia de caracteres| Opcional. Consulte a lista de [EDS consulta Refiners](edsqueryrefiners.md).|
| relação| cadeia de caracteres| Opcional. Filtro que usa o parâmetro de ID como a base para pesquisar para outros produtos que correspondem ao tipo de relação especificado: <ul><li>bundledWith - Encontre produtos de pacote em que o parâmetro ID é incluído como parte desses pacotes.</li><li>bundledProducts - localizar os produtos que estão incluídos no pacote especificado pelo parâmetro de ID.</li></ul>  Somente os produtos que são visíveis no marketplace (pode ser retornado em uma chamada de procurar) serão retornados com esse parâmetro. Se um pacote tiver um produto oculto, ele ainda é parte do pacote, mas não retornado nesses resultados.|
  | ScopeId | cadeia de caracteres | Esse parâmetro é usado em cenários de pesquisa inversa para a mídia de vídeo. |
  | ScopeIdType | cadeia de caracteres | Esse parâmetro é usado em cenários de pesquisa inversa para a mídia de vídeo. Valores possíveis: Título. |
  | skipItems | inteiro com sinal de 32 bits | Opcional. Para que a paginação em cenários entre grupos, o parâmetro skipItems é usado para determinar quantos itens já vimos (e, portanto, qual item deve ser exibido pela primeira vez no conjunto de resultados). O valor é baseado em 0, portanto, skipItems = 0 (ou simplesmente não fornecendo skipItems) começa a recuperação no início da lista. skipItems = 3 seria ignorar os primeiros três itens na lista e iniciar a recuperação com o quarto item. |
  | subscriptionLevel | matriz de cadeia de caracteres | Parâmetro de filtro opcional. O parâmetro subscriptionLevel determina que o tipo de assinatura, o usuário tem (por exemplo, se um usuário tem uma assinatura paga ou uma assinatura gratuita). Os valores possíveis são da seguinte maneira. <ul><li>ouro: O usuário tem uma assinatura paga</li><li>prata: O usuário tem uma assinatura gratuita.</li></ul>  |
| TargetDevices| cadeia de caracteres| EDS fornece a flexibilidade para filtrar oferece para dispositivos de destino. As ofertas (ProviderContent ou disponibilidade) retornadas para o Item podem ser restritas a um dispositivo de destino.|
| topRatedOnly| Valor booliano| Parâmetro de filtro opcional. Restringe os resultados somente o conteúdo melhores.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>Consulte também

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent

[Referência adicional](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>Informações adicionais

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)
