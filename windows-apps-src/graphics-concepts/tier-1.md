---
title: Nível 1
description: Esta seção descreve o suporte de nível 1.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- Nível 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0ae76111f6feefa0bb63fd18516e033050cc06fc
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7844504"
---
# <a name="tier-1"></a>Nível 1


Esta seção descreve o suporte de nível 1.

## <a name="span-idtier1generallimitationsspanspan-idtier1generallimitationsspanspan-idtier1generallimitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Limitações gerais do Nível 1


-   Hardware em nível de recurso mínimo 11.0.
-   Não há suporte para quilting.
-   Não há suporte para Texture1D ou Texture3D.
-   Não há suporte para suavização multisample (MSAA) para 2, 8 ou 16 amostras. Somente 4x é necessário, exceto nenhum formato de 128 bpp.
-   Nenhum padrão swizzle (o layout em blocos de 64 KB e na compactação MIP final depende do fornecedor de hardware).
-   Limitação sobre como os blocos podem ser acessados quando há mapeamentos duplicados. Consulte [Limitações de acesso a blocos com mapeamentos duplicados](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Limitações específicas que afetam apenas o nível 1


### <a name="span-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Leitura/gravação em recursos de streaming que têm mapeamentos NULOS

Os recursos de streaming pode ter mapeamentos **NULOS**, mas ler a partir de ou gravar nesses recursos gera resultados indefinidos, incluindo dispositivo removido. Os aplicativos podem resolver isso mapeando uma única página fictícia para todas as áreas vazias. Tome cuidado se você for gravar e renderização em uma página mapeada para vários locais de destino de renderização, pois a ordem de gravações será indefinida.

### <a name="span-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>Não há instruções do sombreador para fixação de nível de detalhe e feedback de status mapeado

As instruções do sombreador para fixação de nível de detalhe e feedback de status mapeado não estão disponíveis. Consulte [Exposição de recursos de streaming HLSL](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Restrições de alinhamento para formas de bloco padrão

É possível garantir apenas que os mips (a partir dos mais finos) cujas dimensões sejam múltiplos do tamanho do bloco padrão tenham suporte para as formas de bloco padrão e possam ter blocos individuais arbitrariamente mapeados/desmapeados. O primeiro mipmap em um recurso de streaming que tenha qualquer dimensão que não seja um múltiplo do tamanho do bloco padrão, juntamente com todos os mipmaps mais grosseiros, pode ter uma forma de bloco não padrão, cabendo em blocos N de 64 KB para esse conjunto de mips de uma vez (N relatado ao aplicativo). Esses blocos N são considerados compactados como uma unidade, que deve ser totalmente mapeada ou totalmente desmapeada pelo aplicativo a qualquer momento, embora os mapeamentos de cada um dos blocos N possam ser em locais arbitrariamente separados em um pool de blocos.

### <a name="span-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Matriz de mipmaps que não são um múltiplo do tamanho do bloco padrão

Os recursos de streaming com qualquer mipmap que não seja um múltiplo do tamanho do bloco padrão em todas as dimensões não podem ter um tamanho de matriz maior que 1.

### <a name="span-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Alternando entre referências de blocos em um pool de blocos por meio de um recurso Buffer e Texture

Para alternar entre a referência de blocos em um pool de blocos por meio de um recurso [Buffer](introduction-to-buffers.md) e a referência aos mesmos blocos por meio de um recurso [Texture](introduction-to-textures.md) ou vice versa, a atualização mais recente dos mapeamentos de blocos ou a cópia dos mapeamentos de blocos que define os mapeamentos para quem blocos do pool devem ter a mesma dimensão de recurso (Buffer versus Texture\*) como a dimensão de recurso que será usada para acessar os blocos. Caso contrário, o comportamento será indefinido, incluindo a chance de restauração do dispositivo.

Por exemplo, é inválido atualizar os mapeamentos de blocos para defini-los para um Buffer e depois atualizar os mapeamentos de blocos para os mesmos blocos no pool via [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) e, em seguida, acessar os blocos via Buffer. As operações alternativas são redefinir os mapeamentos de blocos para um recurso ao alternar entre Buffer e Texture (ou vice-versa) compartilhando os blocos ou simplesmente nunca compartilhar blocos de um pool entre recursos Buffer e recursos Texture.

### <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtragem de redução mínima/máxima

Não há suporte para a filtragem de redução mínima/máxima. Consulte [Recursos de amostragem de textura de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Níveis de recursos de streaming](streaming-resources-features-tiers.md)

 

 




