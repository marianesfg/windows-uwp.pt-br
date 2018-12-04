---
title: Topologias primitivas
description: O Direct3D dá suporte a várias topologias primitivas, que definem como os vértices são interpretados e renderizados pelo pipeline, como listas de ponto, listas de linha e faixas de triângulos.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Topologias primitivas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 85d1c41fc10f509f3872fb1e4a0af5fa1e1e7c30
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8475111"
---
# <a name="primitive-topologies"></a>Topologias primitivas


O Direct3D dá suporte a várias topologias primitivas, que definem como os vértices são interpretados e renderizados pelo pipeline, como listas de ponto, listas de linha e faixas de triângulos.

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Topologias primitivas básicas


As seguintes topologias primitivas básicas (ou tipos primitivos) são suportados:

-   [Listas de pontos](point-lists.md)
-   [Listas de linhas](line-lists.md)
-   [Faixas de linhas](line-strips.md)
-   [Listas de triângulos](triangle-lists.md)
-   [Listas de triângulos](triangle-strips.md)

Para uma visualização de cada tipo primitivo, consulte o diagrama mais abaixo neste tópico em [Direção da rotação e posições de vértice principais](#winding-direction-and-leading-vertex-positions).

O [estágio do assembler de entrada (IA)](input-assembler-stage--ia-.md) lê os dados de vértice e buffers de índice, reúne os dados nesses primitivos e, em seguida, envia os dados para os estágios de pipeline restantes.

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Adjacência primitiva


Todos os tipos primitivos do Direct3D (exceto a lista de ponto) estão disponíveis em duas versões: um tipo primitivo com adjacência e um tipo primitivo sem adjacência. Primitivas com adjacência contêm alguns dos vértices ao redor, enquanto primitivas sem adjacência contêm apenas os vértices da primitiva de destino. Por exemplo, a primitiva de lista de linha tem uma primitiva de lista de linha correspondente que inclui adjacências.

Primitivas adjacentes foram projetadas para fornecer mais informações sobre sua geometria e só ficam visíveis por meio de um sombreador de geometria. A adjacência é útil para sombreadores de geometria que usam a detecção de silhueta, extrusão de volume de sombra e assim por diante.

Por exemplo, suponha que você deseja desenhar uma lista de triângulos com adjacência. Uma lista de triângulos que contém 36 vértices (com adjacência) produzirá 6 primitivas concluídas. Primitivas com adjacência (exceto faixas de linha) contêm exatamente o dobro dos vértices que a primitiva equivalente sem adjacência, onde cada vértice adicional é um vértice adjacente.

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Direção da rotação e posições de vértice principais


Conforme mostrado na ilustração a seguir, um vértice principal é o primeiro vértice não adjacente em uma primitiva. Um tipo primitivo poderá ter vários vértices principais definidos, desde que cada um deles seja usado para uma primitiva diferente.

-   Para uma faixa de triângulos com adjacência, os vértices principais são 0, 2, 4, 6 e assim por diante.
-   Para uma faixa de linha com adjacência, os vértices principais são 1, 2, 3 e assim por diante.
-   Uma primitiva adjacente, por outro lado, não tem nenhum vértice principal.

A ilustração a seguir mostra a ordem de vértices para todos os tipos primitivos que o assembler de entrada pode produzir.

![diagrama da ordem dos vértices para tipos primitivos](images/d3d10-primitive-topologies.png)

Os símbolos na ilustração anterior estão descritos na tabela a seguir.

| Símbolo                                                                                   | Nome              | Descrição                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![símbolo de um vértice](images/d3d10-primitive-topologies-vertex.png)                     | Vértice            | Um ponto no espaço 3D.                                                                |
| ![símbolo da direção de rotação](images/d3d10-primitive-topologies-winding-direction.png) | Direção de rotação | A ordem de vértice ao montar uma primitiva. Pode ser no sentido horário ou anti-horário. |
| ![símbolo de vértice principal](images/d3d10-primitive-topologies-leading-vertex.png)       | Vértice principal    | O primeiro vértice não adjacente em uma primitiva que contém dados por constante.       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Gerando várias faixas


Você pode gerar várias faixas por meio de corte de faixa. Você pode executar um corte de faixa explicitamente chamando a função HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660), ou inserindo um valor de índice especial para o buffer de índice. Esse valor é -1, que é 0xffffffff para índices de 32 bits ou 0xffff para índices de 16 bits.

Um índice de – 1 indica um "corte" ou "reinício" explícito da faixa atual. O índice anterior conclui a primitiva anterior, ou a faixa, e o próximo índice inicia um novo primitivo ou faixa.

Para obter mais informações sobre a geração de várias faixas, consulte [Estágio de sombreador de geometria (GS)](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Estágio do assembler de entrada (IA)](input-assembler-stage--ia-.md)

[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




