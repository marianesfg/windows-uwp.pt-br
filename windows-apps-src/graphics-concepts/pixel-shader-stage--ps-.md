---
title: "Estágio do sombreador de pixel (PS)"
description: "Estágio do sombreador de pixel (PS) recebe dados interpolados para um primitivo e gera dados por pixel, como cor."
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- "Estágio do sombreador de pixel (PS)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2048a92816de384de0c4be0400338abba9cb275f
ms.lasthandoff: 02/07/2017

---

# <a name="pixel-shader-ps-stage"></a>Estágio do sombreador de pixel (PS)


Estágio do sombreador de pixel (PS) recebe dados interpolados para um primitivo e gera dados por pixel, como cor.

Esse é um estágio de sombreador programável; ele é mostrado como um bloco arredondado no diagrama de [pipeline gráfico](graphics-pipeline.md). Esse estágio do sombreador expõe sua própria funcionalidade exclusiva, criada no modelo de sombreador 4.0 [núcleo comum de sombreador](https://msdn.microsoft.com/library/windows/desktop/bb509580).

O estágio de sombreador de Pixel (PS) permite técnicas de sombreamento avançadas, como iluminação por pixel e pós-processamento. Um sombreador de pixel é um programa que combina variáveis constantes, dados de textura, valores interpolados por vértice e outros dados para produzir saídas por pixel. O [estágio do rasterizador (RS)](rasterizer-stage--rs-.md) invoca um sombreador de pixel uma vez para cada pixel coberto por um primitivo. No entanto, é possível especificar um sombreador **NULL** para evitar a execução de um sombreador.

Ao coletar várias amostras de uma textura, um sombreador de pixel é invocado uma vez quando ocorre um teste de profundidade/estêncil para as várias amostras cobertas. Amostras que passarem no teste de profundidade/estêncil serão atualizadas com a cor de saída do sombreador de pixel.

As funções intrínsecas do sombreador de pixel produzem ou usam derivadas de quantidades em relação ao espaço da tela x e y. O uso mais comum para derivadas é calcular o nível de detalhe para a amostragem de textura e, no caso de uma filtragem anisotrópica, selecionar amostras ao longo do eixo de anisotropia. Normalmente, uma implementação de hardware executa um sombreador de pixel em vários pixels (por exemplo, uma grade 2x2) ao mesmo tempo para que derivadas de quantidades calculadas no sombreador de pixel possam ser razoavelmente aproximadas como deltas dos valores no mesmo ponto de execução em pixels adjacentes.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Entradas


Quando o pipeline é configurado sem um sombreador de geometria, um sombreador de pixel é limitado a 16 entradas de 32 bits e 4 componentes. Caso contrário, um sombreador de pixel pode aceitar até 32 entradas de 32 bits e 4 componentes.

Dados de entrada de sombreador de pixel incluem atributos de vértice (que podem ser interpolados com ou sem correção da perspectiva) ou podem ser tratados como constantes por primitivo. Entradas do sombreador de pixel são interpoladas a partir dos atributos de vértice da primitivo a ser rasterizado com base no modo de interpolação declarado. Se um primitivo for recortado antes da rasterização, o modo de interpolação também será aplicado durante o processo de recorte.

Atributos de vértice são interpolados (ou avaliados) em locais de centro de sombreador de pixel. Modos de interpolação de atributo de sombreador de pixel são declarados em uma declaração de registro de entrada, de acordo com o elemento, em um [argumento](https://msdn.microsoft.com/library/windows/desktop/bb509606) ou um [entrada estrutura](https://msdn.microsoft.com/library/windows/desktop/bb509668). Atributos podem ser interpolados linearmente ou amostragem de centroides. Consulte a seção "Amostragem de centroides de atributos com suavização de várias amostras" em [Regras de rasterização](rasterization-rules.md). A avaliação de centroides é relevante apenas durante a coleta de várias amostras para cobrir casos onde um pixel é coberto por um primitivo, mas um centro de pixel pode não estar; a avaliação de centroides ocorre o mais próximo possível do centro de pixel (não coberto).

Entradas também podem ser declaradas com uma [semântica de valor do sistema](https://msdn.microsoft.com/library/windows/desktop/bb509647), que marca um parâmetro que é consumido pelos outros estágios de pipeline. Por exemplo, uma posição de pixel deve ser marcada com a semântica SV\_Position. O [estágio do assembler de entrada (IA)](input-assembler-stage--ia-.md) pode produzir um escalar para um sombreador de pixel (usando SV\_PrimitiveID); o [estágio do rasterizador (RS)](rasterizer-stage--rs-.md) também pode gerar um escalar para um sombreador de pixel (usando SV\_IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Saídas


Um sombreador de pixel pode gerar até 8 cores de 32 bits e 4 componentes, ou nenhuma cor se o pixel for descartado. Componentes de registro de saída de sombreador de pixel devem ser declarados antes de poderem ser usados; cada registro pode ter uma máscara de gravação de saída distinta.

Use o estado de profundidade de gravação de habilitada (no [estágio de fusão de saída (OM)](output-merger-stage--om-.md)) para controlar se os dados de profundidade são gravados em um buffer de profundidade (ou use a instrução de descarte para descartar dados para o pixel). Um sombreador de pixel também pode gerar uma saída opcional de valor de profundidade de ponto flutuante de 32 bits e 1 componente para teste de profundidade (usando a semântica SV\_Depth). O valor de profundidade tem saída no registro oDepth e substitui o valor de profundidade interpolado para teste de profundidade (supondo que o teste de profundidade está habilitado). Não há como alternar dinamicamente entre usar a profundidade de função fixa ou oDepth do sombreador.

Um sombreador de pixel não pode retornar um valor de estêncil.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 





