---
title: Estágio de mosaico (TS)
description: O estágio de mosaico (TS) cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, os pontos ou linhas) que se conectam a essas amostras.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Estágio de mosaico (TS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d57c60e8cba9be75e936c55800bac93f8df3e30
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5745276"
---
# <a name="tessellator-ts-stage"></a>Estágio de mosaico (TS)


O estágio de mosaico (TS) cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, os pontos ou linhas) que se conectam a essas amostras.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


O diagrama a seguir destaca os estágios de pipeline de elementos gráficos do Direct3D.

![diagrama do pipeline do direct3d 11 que destaca os estágios do sombreador de domínio, sombreadores hull e mosaico](images/d3d11-pipeline-stages-tessellation.png)

O diagrama a seguir mostra o progresso por meio dos estágios de mosaico.

![diagrama da progressão de mosaico](images/tess-prog.png)

A progressão começa com a superfície subdividida em poucos detalhes. A progressão a seguir destaca o patch de entrada com o patch de geometria correspondente, amostras de domínio, e triângulos que conectam essas amostras. Por fim, a progressão destaca os vértices que correspondem a essas amostras.

O tempo de execução do Direct3D dá suporte a três estágios que implementam o mosaico, o qual converte superfícies subdivididas em poucos detalhes em primitivos mais detalhados na GPU. O bloco de mosaico organiza (ou decompõe) as superfícies de ordem superior em estruturas adequadas para renderização.

Os estágios de mosaico trabalham juntos para converter superfícies de ordem superior (que mantêm o modelo simples e eficiente) em vários triângulos para uma renderização detalhada dentro do pipeline de elementos gráficos do Direct3D.

O mosaico usa a GPU para calcular uma superfície mais detalhada de uma superfície construída a partir de patches de quadrupleto, patches de triângulo ou isolines. Para aproximar a superfície de ordem superior, cada patch é subdividido em triângulos, pontos ou linhas usando fatores de mosaico. O pipeline de elementos gráficos do Direct3D implementa o mosaico usando três estágios de pipeline:

-   [Estágio de Sombreador Hull (HS)](hull-shader-stage--hs-.md) - Um estágio de sombreador programável que produz um patch de geometria (e constantes de patch) que correspondem a cada patch de entrada (quadrupleto, triângulo ou linha).
-   Estágio de mosaico (TS) - Um estágio de pipeline de função fixa que cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, os pontos ou linhas) que se conectam a essas amostras.
-   [Estágio de Sombreador (DS) do Domínio](domain-shader-stage--ds-.md) - Um estágio de sombreador programável que calcula a posição do vértice que corresponde a cada amostra de domínio.

Implementando o mosaico no hardware, um pipeline gráfico pode avaliar modelos com menos detalhes (contagem inferior de polígonos) e renderizar em maiores detalhes. Embora o mosaico de software possa ser feito, o mosaico implementado por hardware pode gerar uma quantidade incrível de detalhes visuais (incluindo suporte para o mapeamento de deslocamento) sem adicionar os detalhes visuais aos tamanhos de modelo e paralisar as taxas de atualização.

Benefícios do mosaico:

-   O mosaico poupa muita memória e largura de banda, o que permite que um aplicativo renderize superfícies mais detalhadas a partir de modelos de menor resolução. A técnica de mosaico implementada no pipeline de elementos gráficos Direct3D também dá suporte ao mapeamento de deslocamento, o que pode produzir incríveis quantidades de detalhes da superfície.
-   O mosaico dá suporte a técnicas de renderização escalonáveis, como níveis de terreno contínuos ou dependentes do modo de exibição que podem ser calculados em tempo real.
-   O mosaico melhora o desempenho executando cálculos dispendiosos com menor frequência (fazendo cálculos em um modelo de detalhe inferior). Isso pode incluir a mesclagem de cálculos usando formas de mesclagem ou destinos que mudam de forma para uma animação mais realista ou cálculos de física para a detecção de colisão ou dinâmica de corpo virtual.

O pipeline de elementos gráficos do Direct3D implementa o mosaico em hardware, suavizando o trabalho da CPU para a GPU. Isso pode levar a melhorias de desempenho muito grandes se um aplicativo implementar um grande número de destinos que mudam de forma e/ou modelos de aplicação de capas/deformação mais sofisticados.

O mosaico é um estágio de funções fixas inicializado associando um [sombreador hull](hull-shader-stage--hs-.md) ao pipeline. (veja [Tutorial: Inicializar o Estágio de Mosaico](https://msdn.microsoft.com/library/windows/desktop/ff476341)). O objetivo do estágio de mosaico é subdividir um domínio (quad, tri ou linha) em muitos objetos menores (triângulos, pontos ou linhas). O mosaico organiza um domínio canônico em um sistema de coordenadas normalizado (zero-para-um). Por exemplo, um domínio quadrupleto é transformado em um quadrado de unidade.

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Fases no estágio de mosaico (TS)

O estágio de mosaico (TS) funciona em duas fases:

-   A primeira fase processa os fatores de mosaico, corrigindo problemas de arredondamento, manipulando fatores muito pequenos, reduzindo e combinando fatores, usar aritmética de ponto flutuante de 32 bits.
-   A segunda fase gera listas de pontos ou topologias com base no tipo de particionamento selecionado. Isso é a tarefa principal do estágio de mosaico e usa frações de 16 bits com aritmética de ponto fixo. A aritmética de ponto fixo permite a aceleração de hardware, mantendo a precisão aceitável. Por exemplo, dado um patch com 64 metros de largura, essa precisão pode colocar pontos em uma resolução de 2 mm.

    | Tipo de particionamento | Intervalo                       |
    |----------------------|-----------------------------|
    | Fractional\_odd      | \[1...63\]                  |
    | Fractional\_even     | TessFactor range: \[2..64\] |
    | Inteiro              | TessFactor range: \[1..64\] |
    | Pow2                 | TessFactor range: \[1..64\] |

     

O mosaico é implementado com dois estágios de sombreador programável: um [sombreador hull](hull-shader-stage--hs-.md) e um [sombreador de domínio](domain-shader-stage--ds-.md). Esses estágios de sombreador são programados com o código HLSL que é definido no modelo de sombreador 5. Os destinos de sombreador são: hs\_5\_0 and ds\_5\_0. O título cria o sombreador e código para o hardware é extraído dos sombreadores compilados que são passados para o tempo de execução quando os sombreadores são associados ao pipeline.

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Habilitar/desabilitar mosaico

Habilite o mosaico criando um sombreador hull e vinculando-o ao estágio de sombreador hull (isso configura automaticamente o estágio de mosaico). Para gerar as posições de vértice final dos patches de mosaico, você também precisará criar um [sombreador domínio](domain-shader-stage--ds-.md) e associá-lo ao estágio do sombreador de domínio. Depois que o mosaico é habilitado, a entrada de dados no estágio do Assembler de Entrada (IA) deve ser dados de patch. A topologia de assembler de entrada deve ser uma topologia constante de patch.

Para desabilitar o mosaico, defina os sombreadores hull e o sombreador de domínio **NULL**. Nem o [estágio do Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md) nem o [estágio de Saída de Fluxo (SO)](stream-output-stage--so-.md) podem ler pontos de controle de saída de sombreador hull ou dados de patch.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


O mosaico opera uma vez por patch usando os fatores de mosaico (que especificam o quão delicadamente o domínio será transformado em mosaico) e o tipo de particionamento (que especifica o algoritmo usado para dividir um patch) que são transmitidos do estágio de sombreador hull.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


O mosaico gera coordenadas uv (e, opcionalmente, w) e a topologia de superfície para o estágio do sombreador de domínio.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




