---
title: A necessidade de recursos de streaming
description: Os recursos de streaming são necessários para que a memória da GPU não seja desperdiçada com o armazenamento de regiões de superfícies que não serão acessadas e para dizer para o hardware como filtrar entre os blocos adjacentes.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- A necessidade de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0e0354b0e727e84d562bf63779e74be72f87198f
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709530"
---
# <a name="the-need-for-streaming-resources"></a>A necessidade de recursos de streaming


Os recursos de streaming são necessários para que a memória da GPU não seja desperdiçada com o armazenamento de regiões de superfícies que não serão acessadas e para dizer para o hardware como filtrar entre os blocos adjacentes.

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Recursos de streaming ou texturas esparsas


*Recursos de streaming* (chamados de *recursos lado a lado* no Direct3D 11), são recursos lógicos grandes que usam pequenas quantidades de memória física.

Os recursos de streaming também são chamados de *texturas esparsas*. "Esparso" refere-se à natureza lado a lado dos recursos, bem como ao provável motivo principal do agrupamento lado a lado dos recursos: que nem todos eles precisam ser mapeados de uma vez. Na verdade, um aplicativo poderia perfeitamente criar um recurso de streaming em que nenhum dado é criado para todas as regiões + mips do recurso, intencionalmente. Então, o conteúdo em si poderia ser esparso, e o mapeamento do conteúdo na memória da GPU em um determinado momento seria um subconjunto disso (ainda mais esparso).

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Sem o agrupamento lado a lado, as alocações de memória seriam gerenciadas na granularidade de sub-recurso


Em um sistema gráfico (ou seja, o sistema operacional, driver de vídeo e hardware gráfico) sem suporte para recursos de streaming, o sistema gráfico gerencia todas as alocações de memória do Direct3D em uma granularidade de sub-recurso.

Para um [buffer](introduction-to-buffers.md), todo o buffer é o sub-recurso.

Para uma [textura](textures.md) (por exemplo, [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525)), cada nível de mip é um sub-recurso; para uma matriz de texturas (por exemplo, [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526)), cada nível de mip em uma determinada fatia da matriz é um sub-recurso. O sistema gráfico expõe apenas a capacidade de gerenciar o mapeamento de alocações nessa granularidade de sub-recursos. No contexto de recursos de streaming, "mapeamento" refere-se a tornar os dados visíveis para a GPU.

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Sem o agrupamento lado a lado, não é possível acessar somente uma pequena parte da cadeia de mipmaps


Suponhamos que um aplicativo saiba que uma operação de renderização específica precise acessar apenas uma pequena parte de uma cadeia de mipmaps de imagem (talvez nem mesmo a área completa de um determinado mipmap). O ideal seria que o aplicativo pudesse informar o sistema gráfico sobre essa necessidade. Em seguida, o sistema gráfico se preocuparia apenas em garantir que a memória necessária fosse mapeada para a GPU sem a paginação de muita memória.

Na realidade, sem o suporte para recursos de streaming, o sistema gráfico pode apenas ser informado sobre a memória que precisa ser mapeada para a GPU na granularidade de sub-recurso (por exemplo, uma variedade de níveis de mipmap completos que podem ser acessados). Também não há falha de demanda no sistema gráfico. Portanto, provavelmente deve ser usada muita memória da GPU em excesso para mapear sub-recursos completos antes de um comando de renderização que referencie qualquer parte da memória seja executado. Esse é apenas um problema que dificulta o uso de grandes alocações de memória no Direct3D sem o suporte para recursos de streaming.

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Paginação de software para dividir a superfície em blocos menores


A paginação de software pode ser usada para dividir a superfície em blocos que sejam pequenos o suficiente para o hardware processar.

O Direct3D dá suporte para superfícies [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) com até 16384 pixels em um determinado lado. Uma imagem de 16384 de largura por 16384 de altura e 4 bytes por pixel consumiria 1 GB de memória de vídeo (e adicionar mipmaps duplicaria essa quantidade). Na prática, raramente seria necessário referenciar o volume todo de 1 GB em uma única operação de renderização.

Alguns desenvolvedores de jogos modelam superfícies de terreno grandes de até 128 K por 128 K. Eles fazem isso funcionar nas GPUs existentes dividindo a superfície em blocos que sejam pequenos o suficiente para o hardware processar. O aplicativo deve descobrir quais blocos podem ser necessário e carregá-los em um cache de texturas na GPU – um sistema de paginação de software.

Uma desvantagem significativa dessa abordagem é que o hardware não sabe nada sobre a paginação que está acontecendo: quando uma parte de uma imagem precisa ser mostrada na tela que cobre os blocos, o hardware não sabe como executar a filtragem de função fixa (ou seja, eficiente) nos blocos. Isso significa que o aplicativo que gerencia o próprio agrupamento lado a lado de software deve recorrer à filtragem de textura manual no código do sombreador (que se torna muito caro se for desejado um filtro anisotrópico de boa qualidade) e/ou desperdiçar memória criando medianizes em torno de blocos que contenham dados de blocos vizinhos para que a filtragem de hardware de função fixa possa continuar a fornecer alguma ajuda.

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Tornando a representação lado a lado das alocações de superfície um recurso de primeira classe


Se uma representação lado a lado das alocações de superfície for um recurso de primeira classe no sistema gráfico, o aplicativo poderá dizer ao hardware quais blocos devem ser disponibilizados. Dessa forma, menos memória da GPU é desperdiçada com o armazenamento de regiões de superfícies que o aplicativo sabe que não serão acessadas, e o hardware pode saber como filtrar blocos adjacentes, diminuindo os problemas enfrentados por desenvolvedores que executam o agrupamento lado a lado de software por conta própria.

Mas, para fornecer uma solução completa, algo deve ser feito para lidar com o fato de que, independentemente de haver suporte para o agrupamento lado a lado em uma superfície, a dimensão máxima de superfície é atualmente 16384 – não chega nem perto dos mais de 128 K que os aplicativos já querem. Exigir apenas que hardware dê suporte para tamanhos maiores de textura é uma abordagem, mas existem custos e/ou compensações significativos para seguir esse caminho.

O caminho do filtro de textura e o caminho de renderização do Direct3D já estão saturados em termos de precisão no suporte para texturas de 16 K com os outros requisitos, como suporte para extensões do visor que saem da superfície durante a renderização ou suporte para encapsulamento de textura fora da borda da superfície durante a filtragem. Uma possibilidade é definir uma compensação como, por exemplo, à medida que o tamanho da textura ultrapassa 16 K, a funcionalidade/precisão é renunciada de alguma maneira. Mesmo com essa concessão, porém, os custos adicionais de hardware podem ser necessários em termos de endereçamento de funcionalidade em todo o sistema de hardware para chegar a tamanhos de textura maiores.

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problema com texturas grandes: precisão de locais na superfície


Um problema que vem à tona quando as texturas ficam muito grandes é que as coordenadas de textura de ponto flutuante de precisão (e os interpoladores associados para dar suporte à rasterização) perdem a precisão para especificar locais na superfície com exatidão. Pode ocorrer filtragem de textura irregular. Uma opção cara seria exigir duplo suporte para interpoladores de precisão, embora isso possa ser um exagero visto que há uma alternativa razoável.

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Permitindo que vários recursos de dimensões diferentes compartilhem a memória


Outro cenário pode ser servido por recursos de streaming é permitir que vários recursos de diferentes dimensões/formatos compartilhem a mesma memória. Às vezes, os aplicativos têm conjuntos de recursos exclusivos que não são usados ao mesmo tempo ou recursos que são criados apenas para uso muito breve e, em seguida, são destruídos, seguidos pela criação de outros recursos. Uma forma de generalidade que pode não se enquadrar nos "recursos de streaming" é que é possível permitir que o usuário aponte vários recursos diferentes para a mesma memória (sobreposta). Em outras palavras, a criação e a destruição de "recursos" (que definem uma dimensão/formato e assim por diante) podem ser desvinculadas do gerenciamento da memória subjacente aos recursos do ponto de vista do aplicativo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




