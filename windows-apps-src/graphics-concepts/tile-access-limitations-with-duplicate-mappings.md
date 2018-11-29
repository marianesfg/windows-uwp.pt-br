---
title: Limitações de acesso a blocos com mapeamentos duplicados
description: Há limitações de acesso a blocos com mapeamentos duplicados, como ao copiar recursos de streaming com origem e destino sobrepostos ou ao renderizar blocos compartilhados na área de renderização.
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords:
- Limitações de acesso a blocos com mapeamentos duplicados
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d5563a9909ba3d6cb3deaae43bcf9e55b4b2c880
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8190763"
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>Limitações de acesso a blocos com mapeamentos duplicados


Há limitações de acesso a blocos com mapeamentos duplicados, como ao copiar recursos de streaming com origem e destino sobrepostos ou ao renderizar blocos compartilhados na área de renderização.

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>Copiando recursos de streaming com origem e destino sobrepostos


Se os blocos na área de origem e destino de uma operação Copy\* tiverem mapeamentos duplicados na área de cópia que ficariam sobrepostos mesmo que ambos os recursos não fossem de streaming e a operação Copy\* tivesse suporte para cópias sobrepostas, a operação Copy\* terá um bom comportamento (como se a origem fosse copiada para um local temporário antes de passar para o destino). Mas, se a sobreposição não for óbvia (por exemplo, os recursos de origem e destino são diferentes, mas compartilham mapeamentos, ou os mapeamentos são duplicados sobre uma determinada superfície), os resultados da operação de cópia nos blocos que são compartilhados serão indefinidos.

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>Copiando para o recurso de streaming com blocos duplicados na área de destino


Copiar para um recurso de streaming com blocos duplicados na área de destino produz resultados indefinidos nesses blocos, a menos que os dados em si sejam idênticos; blocos diferentes podem gravar os blocos em ordens diferentes.

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>Acessos UAV a mapeamentos de blocos duplicados


Suponhamos que uma exibição de acesso não ordenado (UAV) em um recurso de streaming tenha mapeamentos de blocos duplicados em sua área ou com outros recursos associados ao pipeline. A ordenação dos acessos a estes blocos duplicados é indefinida se realizada por threads diferentes, assim como qualquer ordenação de acesso da memória a UAVs em geral é não ordenada.

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>Renderizando após alterações no mapeamento de blocos ou atualizações de conteúdo de mapeamentos externos


Se os mapeamentos de blocos de um recurso de streaming forem alterados ou o conteúdo nos blocos mapeados do pool lado a lado for alterado por outros mapeamentos do recurso de streaming, e o recurso de streaming for renderizado por meio da exibição de destino da renderização ou da exibição do estêncil de profundidade, o aplicativo deverá limpar (usando as APIs Clear de função fixa) ou sobrepor totalmente a cópia usando as APIs Copy\*/Update\* nos blocos que foram alterados na área que está sendo renderizada (mapeados ou não).

Se o aplicativo não limpar ou copiar nesses casos, as estruturas de otimização de hardware para a exibição específica do destino de renderização ou do estêncil de profundidade se tornarão obsoletas, e isso resultará em resultados de renderização inúteis em alguns hardwares e inconsistência entre hardwares diferentes. Essas estruturas de dados de otimização ocultas usadas pelo hardware podem ser mapeamentos locais a individuais e não visíveis para outros mapeamentos na mesma memória.

Quando você limpa uma exibição de recursos (configurando todos os elementos de uma exibição de recursos com um único valor), é possível limpar as exibições do destino de renderização com retângulos. Para hardware que dá suporte a recursos de streaming, limpar uma exibição de recursos também deve dar suporte à limpeza das exibições de estêncil de profundidade com retângulos, somente para superfícies de profundidade (sem estêncil). Essa operação permite que os aplicativos limpem apenas a área necessária de uma superfície.

Se um aplicativo precisar preservar o conteúdo das áreas na memória existente em um recurso de streaming onde os mapeamentos foram alterados, esse aplicativo deverá encontrar uma solução alternativa para o requisito de limpeza. O aplicativo pode chegar a essa solução alternativa salvando primeiro o conteúdo onde os mapeamentos de blocos foram alterados (copiando-os em uma superfície temporária), emitindo o comando de limpeza necessário e copiando o conteúdo de volta. Embora isso conclua a tarefa de preservar o conteúdo da superfície para renderização incremental, a desvantagem é que o desempenho da renderização subsequente na superfície poderá ser prejudicado porque as otimizações de renderização poderão ser perdidas.

Se um bloco for mapeado para vários recursos de streaming ao mesmo tempo e o conteúdo do bloco for manipulado por qualquer meio (renderização, cópia e assim por diante) por meio de um dos recursos de streaming, se for o mesmo bloco tiver de ser renderizado por meio de qualquer outro recurso de streaming, o bloco deverá ser limpo primeiro conforme descrito anteriormente.

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>Renderizando blocos compartilhados fora da área de renderização


Suponhamos que uma área de um recurso de streaming esteja sendo renderizada e os blocos do pool de blocos referenciados pela área de renderização também sejam mapeados fora da área de renderização (inclusive por meio de outros recursos de streaming, ao mesmo tempo ou não). Não há garantira de que os dados renderizados nesses blocos aparecerão corretamente quando visualizados por meio de outros mapeamentos, mesmo que o layout de memória subjacente seja compatível. Isso se deve às estruturas de dados de otimização que alguns hardwares usam que podem ser mapeamentos locais a individuais para superfícies renderizáveis e não visíveis para outros mapeamentos no mesmo local de memória.

Você pode usar uma solução alternativa para essa restrição copiando do mapeamento renderizado para todos os outros mapeamentos na mesma memória que pode ser acessada (ou limpando essa memória ou copiando outros dados para ela se o conteúdo antigo não for mais necessário). Embora essa solução alternativa pareça redundante, isso faz com que todos os outros mapeamentos para a mesma memória entendam corretamente como acessar seu conteúdo, e pelo menos a economia de memória de se ter apenas uma única memória física de backup permanece intacta.

Além disso, quando você alterna o uso de recursos diferentes de streaming que compartilham mapeamentos (a menos que sejam somente leitura), é preciso especificar uma restrição (barreira) de ordenação de acesso aos dados entre vários recursos lado a lado, entre as alternâncias.

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>Renderizando blocos compartilhados dentro da área de renderização


Se uma área de um recurso de streaming estiver sendo renderizada dentro da área de renderização em que vários blocos são mapeados para o mesmo local do pool de blocos, os resultados da renderização serão indefinidos nesses blocos.

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>Compatibilidade de dados entre blocos de compartilhamento de recursos de streaming


Suponhamos que vários recursos de streaming tenham mapeamentos para os mesmos locais de pools de blocos e que cada recurso seja usado para acessar os mesmos dados. Esse cenário é válido somente quando as outras regras sobre como evitar problemas com estruturas de otimização de hardware são evitadas, barreiras apropriadas são especificadas (especificando uma restrição da ordenação de acesso aos dados entre vários recursos lado a lado) e os recursos de streaming são compatíveis entre si.

O último é descrito aqui em termos do que significa a incompatibilidade de recursos de streaming que compartilham blocos. As condições de incompatibilidade de acesso aos mesmos dados em mapeamentos de blocos duplicados são o uso de diferentes dimensões ou formatos de superfície, ou diferenças na presença de sinalizadores de associação do destino de renderização ou do estêncil de profundidade nos recursos. A gravação na memória com um tipo de mapeamento gera resultados indefinidos quando, subsequentemente, você lê ou renderiza usando um mapeamento de um recurso incompatível.

Se os outros mapeamentos de compartilhamento de recursos forem inicializados primeiro com novos dados (reciclando a memória para um objetivo diferente), a operação de leitura ou renderização subsequente funcionará bem, pois não haverá sangria dos dados entre interpretações incompatíveis. Mas, quando você alterna o acesso a mapeamentos incompatíveis dessa forma, é preciso especificar barreiras (especificando uma restrição de ordenação de acesso aos dados entre vários recursos lado a lado).

Se o sinalizador de associação do destino de renderização ou do estêncil de profundidade não estiver definido em qualquer um dos recursos que compartilham mapeamentos entre si, haverá bem menos restrições. Contanto que os tipos de formato e superfície (por exemplo, Texture2D) sejam os mesmos, os blocos poderão ser compartilhados. Exemplos de casos de formatos diferentes que são compatíveis são: superfícies BC\* superfícies e o formato descompactado de tamanho equivalente de 32 bits ou 16 bits por componente, como BC6H e R32G32B32A32. Muitos formatos de 32 bits por elemento também podem ser associados a R32\_\* (R10G10B10A2\_\*, R8G8B8A8\_\*, B8G8R8A8\_\*,B8G8R8X8\_\*,R16G16\_\*); essa operação sempre foi permitida para recursos que não são de streaming.

Não há problema no compartilhamento entre blocos compactados e não compactados quando os formatos são compatíveis e os blocos são preenchidos com cor sólida.

Por fim, se não houver nada em comum nos mapeamentos de blocos de compartilham recursos, exceto que nenhum apresente sinalizadores de associação do destino de renderização ou do estêncil de profundidade, somente a memória preenchida com 0 poderá ser compartilhada com segurança; o mapeamento aparecerá como o valor que o 0 decodificar para a definição do formato de recurso específico (normalmente 0).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




