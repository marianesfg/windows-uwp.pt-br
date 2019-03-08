---
title: Controle de risco versus recursos de pool de blocos
description: Para recursos não streaming, o Direct3D pode impedir determinadas condições de risco durante a renderização, mas como o controle de risco estaria em um nível de bloco para os recursos de streaming, monitorar as condições de risco durante a renderização de streaming de recursos pode ser muito caro.
ms.assetid: 8B0C73D3-3F77-41E8-B17D-C595DEE39E49
keywords:
- Controle de risco versus recursos de pool de blocos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4dec176206aacb946bfd65341c483d8ba61558ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629741"
---
# <a name="hazard-tracking-versus-tile-pool-resources"></a>Controle de risco versus recursos de pool de blocos


Para recursos não streaming, o Direct3D pode impedir determinadas condições de risco durante a renderização, mas como o controle de risco estaria em um nível de bloco para os recursos de streaming, monitorar as condições de risco durante a renderização de streaming de recursos pode ser muito caro.

Por exemplo, para recursos não streaming, o tempo de execução não permite que qualquer sub-recurso determinado seja vinculado como uma entrada (como um modo de exibição de recurso de sombreador) e como uma saída (como uma exibição de destino de renderização) ao mesmo tempo. Se tal caso for encontrado, o tempo de execução desagrupa a entrada. Essa sobrecarga de rastreamento no tempo de execução é barata e é feita no nível do sub-recurso. Um dos benefícios dessa sobrecarga de rastreamento é minimizar as chances de aplicativos acidentalmente dependendo da ordem de execução do sombreador de hardware. A ordem de execução do sombreador de hardware pode variar se não estiver em uma determinada unidade de processamento gráfico (GPU), então certamente entre GPUs diferentes.

Monitorar a forma como os recursos são associados pode ser muito caro para os recursos de streaming, porque o rastreamento está em um nível de bloco. Novos problemas surgem como, possivelmente, validar as tentativas de renderização para uma exibição de destino de renderização com um bloco mapeado em várias áreas na superfície simultaneamente. Se acontecer desse controle de risco por bloco ser muito caro para o tempo de execução, idealmente ele seria pelo menos uma opção na camada de depuração.

Um aplicativo deve informar o driver de vídeo ao emitir uma operação de gravação ou leitura para um recurso de streaming que faz referência à memória do pool de bloco que também será referenciada por recursos de streaming separados nas futuras operações de gravação e leitura que estão esperando que a primeira operação seja concluída antes das operações seguintes poderem começar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamentos estiverem em um pool de bloco](mappings-are-into-a-tile-pool.md)

 

 




