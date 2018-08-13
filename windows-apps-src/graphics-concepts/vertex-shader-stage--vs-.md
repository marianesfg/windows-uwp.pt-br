---
title: Estágio do sombreador de vértice (VS)
description: O estágio do Sombreador de Vértice (VS) processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Estágio do sombreador de vértice (VS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 521b3254fd8f3f86e8b55923627b8560d2991ae6
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044105"
---
# <a name="vertex-shader-vs-stage"></a>Estágio do sombreador de vértice (VS)


O estágio do Sombreador de Vértice (VS) processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


O estágio de sombreador de vértice (VS) é usado para processamento de vértice individual, como:

-   Transformações
-   Aplicação de capas
-   Metamorfose
-   Iluminação por vértice

O estágio de sombreador de vértice é um estágio de sombreador programável; ele é mostrado como um bloco arredondado no diagrama de [pipeline gráfico](graphics-pipeline.md). Esse estágio de sombreador usa o modelo de sombreador 4.0 [núcleo comum de sombreador](https://msdn.microsoft.com/library/windows/desktop/bb509580).

O estágio de sombreador de vértice (VS) processa vértices de assembler de entrada. Os sombreadores de vértice sempre operam em um vértice de entrada único e produzem um vértice de saída único. O estágio do sombreador de vértice sempre deve estar ativo para que o pipeline seja executado. Se nenhuma modificação de vértice ou transformação for necessária, um sombreador de vértice de passagem deve ser criado e definido para o pipeline.

Cada vértice de entrada de sombreador de vértice pode ser composto de até 16 vetores de 32 bits (até 4 componentes cada). Cada vértice de saída pode ser composto de até 16 vetores de 4 componentes de 32 bits. Todos os sombreadores de vértice devem ter pelo menos uma entrada e uma saída, que podem ter um valor escalar.

O estágio de sombreador de vértice pode consumir dois valores gerados por sistema do assembler de entrada: VertexID e InstanceID (veja os valores do sistema e a semântica). Como VertexID e InstanceID são significativos a um nível de vértice, as IDs geradas pelo hardware podem somente ser alimentadas no primeiro estágio que as entende, esses valores de ID só podem ser alimentados no estágio de sombreador de vértice.

Os sombreadores de vértice sempre são executados em todos os vértices, incluindo vértices adjacentes nas topologias primitivas de entrada com adjacência. O número de vezes que o sombreador de vértice foi executado pode ser consultado na CPU usando a estatística de pipeline VSInvocations.

Um sombreador de vértice pode executar operações de amostragem de textura e carregamento onde não há necessidade de elementos derivados do espaço da tela (usando funções intrínsecas do HLSL: [amostra (objeto de textura do DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509695), [SampleCmpLevelZero (objeto de textura do DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509697), e [SampleGrad (objeto de textura do DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509698)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Um único vértice, com valores VertexID e InstanceID gerados pelo sistema. Cada vértice de entrada de sombreador de vértice pode ser composto de até 16 vetores de 32 bits (até 4 componentes cada).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


Um único vértice. Cada vértice de saída pode ser composto de até 16 vetores de 4 componentes de 32 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 



