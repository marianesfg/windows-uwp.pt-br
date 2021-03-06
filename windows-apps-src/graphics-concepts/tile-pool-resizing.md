---
title: Redimensionamento de pool de blocos
description: Redimensione um pool de blocos para ampliar um pool de blocos se o app precisar de mais trabalho para os recursos de streaming mapeados neles ou reduzir se precisar de menos espaço.
ms.assetid: A54A06DC-BDDB-42DC-85E8-C64241100ED5
keywords:
- Redimensionamento de pool de blocos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e08447c575e99178e503e99eb651cd5e225a898
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607771"
---
# <a name="tile-pool-resizing"></a>Redimensionamento de pool de blocos


Redimensione um pool de blocos para ampliar um pool de blocos se o app precisar de mais trabalho para os recursos de streaming mapeados neles ou reduzir se precisar de menos espaço. Outra opção para os aplicativos é alocar pools de blocos adicionais para novos recursos de streaming. Mas se algum recurso de streaming precisar de mais espaço do que o inicialmente disponível no bloco de pools, ampliar o pool de blocos será uma boa opção. Um recurso de streaming não pode ter mapeamentos para vários pools de blocos ao mesmo tempo.

Quando um pool de blocos é expandido, blocos adicionais são adicionados ao final por meio de uma ou mais alocações novo pelo driver de vídeo. Essa divisão em alocações não fica visível para o aplicativo. A memória existente no pool de blocos deve ser deixada como está, e os mapeamentos de recursos de streaming existentes na memória permanecem intactos.

Quando um pool de blocos é reduzido, os blocos são removidos do final. Os blocos são removidos até mesmo abaixo do tamanho de alocação inicial, até 0, o que significa que novos mapeamentos não poderão ser feitos após o novo tamanho. Porém, os mapeamentos existentes após o final do novo tamanho permanecem intactos e utilizáveis. O driver de vídeo manterá a memória ativa enquanto permanecerem mapeamentos para qualquer parte das alocações que o driver usa para a memória do pool de blocos. Se, após a redução, parte da memória for mantida ativa porque mapeamentos de blocos são apontando para ela e, em seguida, o pool de blocos for ampliado novamente (em qualquer valor), a memória existente será reutilizada primeiro antes que qualquer alocação adicional ocorra para atender ao tamanho da operação de ampliação.

Para poder economizar memória, o aplicativo precisa não só reduzir um pool de blocos, mas também remover/remapear os mapeamentos existentes após o final do novo tamanho de pool de blocos menor.

O ato de reduzir (e remover mapeamentos) não necessariamente gera economia de memória de imediato. A liberação da memória depende de quão granulares as alocações subjacentes do driver de vídeo são para o pool de blocos. Quando a redução é suficiente para fazer uma alocação de driver de vídeo não utilizado, o driver de vídeo pode liberá-la. Se um pool de blocos foi ampliado, a redução aos tamanhos anteriores (e remover/remapear os mapeamentos de blocos de forma correspondente) tem grandes chances de economizar memória, embora não haja garantia caso os tamanhos não se alinhem exatamente aos tamanhos de alocação subjacentes escolhidos pelo driver de vídeo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamentos estiverem em um pool de bloco](mappings-are-into-a-tile-pool.md)

 

 




