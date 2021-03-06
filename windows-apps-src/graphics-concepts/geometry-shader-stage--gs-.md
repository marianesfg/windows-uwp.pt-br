---
title: Estágio de sombreador de geometria (GS)
description: O estágio de sombreador de geometria (GS) processa triângulos primitivos inteiros, linhas e pontos, juntamente com seus vértices adjacentes.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Estágio de sombreador de geometria (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0ea3e7ec73b042eeef560af3d88754afdfa5b441
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370466"
---
# <a name="geometry-shader-gs-stage"></a>Estágio de sombreador de geometria (GS)


O estágio de sombreador de geometria (GS) processa triângulos primitivos inteiros, linhas e pontos, juntamente com seus vértices adjacentes. Ele é útil para algoritmos incluindo expansão de sprites de ponto, sistemas de partículas dinâmicas e geração de volume de sombra. Ele suporta amplificação e cancelamento de amplificação de geometria.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


O estágio do sombreador de geometria processa primitivos inteiros: triângulos (3 vértices com até 3 vértices adjacentes), linhas (2 vértices com até 2 vértices adjacentes) e pontos (1 vértice).

![ilustração de um triângulo e uma linha com vértices adjacentes](images/d3d10-gs.png)

O sombreador de geometria também dá suporte à amplificação e cancelamento de amplificação de geometria. Dado um primitivo de entrada, o sombreador de geometria pode descartar o primitivo ou emitir um ou mais primitivos novos.

O estágio de sombreador de geometria (GS) é um estágio de sombreador programável; ele é mostrado como um bloco arredondado no diagrama de [pipeline gráfico](graphics-pipeline.md). Esse estágio do sombreador expõe sua própria funcionalidade exclusiva, criada nos modelos de sombreador (veja [núcleo comum de sombreador](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)).

O estágio do sombreador de geometria é adequado para algoritmos incluindo:

-   Expansão de sprites de ponto
-   Sistemas de partículas dinâmicas
-   Geração de pele/barbatana
-   Geração de volume de sombra
-   Única passagem de renderização para Cubemap
-   Troca de material por primitivo
-   Configuração de material por primitivo - essa funcionalidade inclui a geração de coordenadas baricêntricas como dados primitivos para que um sombreador de pixel possa realizar a interpolação de atributo personalizado.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrada


O estágio do sombreador de geometria executa o código de sombreador específico do aplicativo com primitivos inteiros como entrada e a capacidade de gerar vértices na saída. Ao contrário de sombreadores de vértice que operam em um único vértice, as entradas do sombreador de geometria são os vértices de um primitivo completo (três vértices para triângulos, dois vértices para linhas ou um único vértice para ponto). Os sombreadores de geometria também podem trazer os dados de vértice para os primitivos adjacentes de borda como entrada (três vértices adicionais para um triângulo, e dois vértices adicionais para uma linha).

O estágio de sombreador de geometria pode consumir os **VA\_PrimitiveID** valor gerado pelo sistema que é gerado automaticamente pelo [estágio do Assembler de entrada (IA)](input-assembler-stage--ia-.md). Isso permite que os dados por primitivo sejam buscados ou calculados, se desejado.

Quando um sombreador de geometria está ativo, ele é chamado uma vez para cada primitivo passado para baixo ou gerado anteriormente no pipeline. Cada invocação do sombreador de geometria vê como entrada os dados para a invocação do primitivo, independentemente de ser um único ponto, uma única linha ou um único triângulo. Uma faixa de triângulos de versões anteriores no pipeline resultaria em uma chamada de sombreador de geometria de cada triângulo individual na faixa (como se a faixa fosse expandida em uma lista de triângulos). Todos os dados de entrada para cada vértice do primitivo individual está disponível (ou seja, 3 vértices para um triângulo), além de dados de vértice adjacentes, se aplicáveis e disponíveis.

Abreviações de vértice comuns:

|     |                 |
|-----|-----------------|
| TV  | Vértice do triângulo |
| LV  | Vértice de linha     |
| AV  | Vértice adjacentes |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


O estágio de sombreador de geometria (GS) é capaz de gerar vários vértices formando uma única topologia selecionada. As topologias de saída do sombreador de geometria disponíveis são **tristrip**, **linestrip**, e **pointlist**. O número de primitivos emitidos pode variar livremente dentro de qualquer chamada do sombreador de geometria, embora o número máximo de vértices que podem ser emitidos deva ser declarado estaticamente. Os tamanhos de faixa emitidos a partir de uma chamada de sombreador de geometria podem ser arbitrários, e novas faixas podem ser criadas por meio da função HLSL [RestartStrip](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip).

A execução de uma instância do sombreador de geometria é atômica das outras chamadas, exceto pelo fato dos dados adicionados aos fluxos serem seriais. As saídas de uma determinada chamada de um sombreador de geometria são independentes de outras chamadas (porém a ordem é respeitada). Um sombreador de geometria gerando faixas de triângulos iniciará uma faixa tira em cada chamada.

A saída do sombreador de geometria pode ser alimentada no estágio do rasterizador e/ou em um buffer de vértice na memória através do estágio de saída de fluxo. A saída alimentada na memória é expandida em listas de linhas/pontos/triângulos individuais (exatamente da forma como seriam passados para o rasterizador).

Um sombreador de geometria produz um vértice de dados por vez, acrescentando vértices a um objeto de fluxo de saída. A topologia dos fluxos é determinada por uma declaração fixa, escolhendo um **TriangleStream**, **LineStream** e **PointStream** como saída para o estágio de GS.

Há três tipos de objetos de fluxo disponíveis: **TriangleStream**, **LineStream** e **PointStream**, que são modelados de todos os objetos. A topologia da saída é determinada pelo seu tipo de objeto correspondente, enquanto o formato dos vértices acrescentado ao fluxo é determinado pelo tipo de modelo.

Quando a saída do sombreador de geometria é identificada como um valor de interpretado do sistema (por exemplo, **VA\_RenderTargetArrayIndex** ou **VA\_posição**), hardware analisa esses dados e executa algum comportamento dependente do valor, além de ser capaz de passar os dados em si para o próximo estágio de sombreador para entrada. Quando essa saída de dados de sombreador de geometria tem um significado para o hardware em uma base por primitivo (como **VA\_RenderTargetArrayIndex** ou **VA\_ViewportArrayIndex**), em vez de em uma base por vértice (como **VA\_ClipDistance\[n\]**  ou **VA\_posição**), os dados por primitivo retirado do vértice à esquerda emitido para o primitivo.

Os primitivos parcialmente concluídos podem ser gerados pelo sombreador de geometria, se o sombreador de geometria terminar e o primitivo estiver incompleto. Primitivos incompletos são descartados silenciosamente. Isso é semelhante à maneira como o IA trata primitivos parcialmente concluídas.

O sombreador de geometria pode executar operações de amostragem de textura e carregamento onde não há necessidade de elementos derivados do espaço da tela (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




