---
title: Regras de rasterização
description: Regras de rasterização definem como os dados de vetor são mapeados para dados de rasterização.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Regras de rasterização
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c622c037f878d1ad34cdadf897dde10683532832
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7704467"
---
# <a name="rasterization-rules"></a>Regras de rasterização


As regras de rasterização definem como os dados de vetor são mapeados nos dados de rasterização. Os dados de rasterização são ajustados para locais de inteiro que são, em seguida, selecionados e recortados (para desenhar o número mínimo de pixels) e atributos por pixel são interpolados (de atributos por vértice) antes de serem passados para um sombreador de pixel.

Existem vários tipos de regras, que dependem do tipo de primitivo que está sendo mapeado, bem como se os dados usam ou não várias amostras para reduzir o serrilhado. As ilustrações a seguir demonstram como os casos de canto são manipulados.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Regras de rasterização de triângulo (sem várias amostragens)


Qualquer centro de pixel que cai dentro de um triângulo é desenhado; um pixel é considerado estar dentro se passar pela regra superior esquerdo. A regra superior esquerdo é quando um centro de pixel é definido como estando dentro de um triângulo se ficar na borda superior ou na borda esquerda de um triângulo.

onde:

-   Uma borda superior é uma borda exatamente horizontal e acima das outras bordas.
-   Uma borda esquerda é uma borda não exatamente horizontal e que está no lado esquerdo do triângulo. Um triângulo pode ter uma ou duas bordas esquerdas.

A regra superior esquerdo garante que triângulos adjacentes sejam desenhados uma vez.

Esta ilustração mostra exemplos de pixels desenhados por estarem dentro de um triângulo ou seguindo a regra superior esquerdo.

![exemplos de rasterização de triângulo superior esquerdo](images/d3d10-rasterrulestriangle.png)

A cobertura cinza clara e escura dos pixels os mostra como grupos de pixels para indicar dentro de qual triângulo estão dentro.

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Regras de rasterização de linha (serrilhado, sem várias amostras)


Regras de rasterização de linha usam uma área de teste de diamante para determinar se uma linha abrange um pixel. Para linhas de x grande (linhas com -1 &lt;= inclinação &lt;= + 1), a área de teste de diamante inclui (mostrado sólido) a borda inferior esquerda, a borda inferior direita e o canto inferior; o diamante exclui (mostrado pontilhado) a borda superior esquerda, borda superior direita, canto superior, canto esquerdo e canto direito. Uma linha de y grande é qualquer linha que não seja uma linha de x grande; a área de diamante de teste é a mesma conforme descrito para a linha de x grande, exceto pelo canto direito também estar incluído.

Dada a área de diamante, uma linha abrange um pixel se a linha sai da área de teste de diamante do pixel ao passar pela linha desde o início até o final. Uma faixa de linha se comporta da mesma forma, pois é desenhada como uma sequência de linhas.

A ilustração a seguir mostra alguns exemplos.

![exemplos de rasterização de linha serrilhada](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Regras de rasterização de linha (com suavização, sem várias amostras)


Uma linha suavizada é rasterizada como se fosse um retângulo (com largura = 1). O retângulo cruza com um destino de renderização produzindo valores de cobertura por pixel, que são multiplicados em componentes alfa de saída de sombreador de pixel. Não há nenhuma suavização executada ao desenhar linhas em um destino de renderização com várias amostras.

Será considerado que não há uma "melhor" e única maneira para realizar uma renderização de linha com suavização. Direct3D adota como diretriz o método mostrado na ilustração a seguir. Esse método foi derivado empiricamente, apresentando um número de propriedades visuais considerado desejável.

O hardware não precisa corresponder exatamente a esse algoritmo; testes em relação a essa referência deverão ter tolerâncias "razoáveis" orientadas por alguns dos princípios listados mais abaixo, permitindo várias implementações de hardware e tamanhos de kernel do filtro. Essa flexibilidade permitida na implementação de hardware, no entanto, não pode ser comunicada pelo Direct3D para apps, além de simplesmente desenhar linhas e observar/medir sua aparência.

![exemplos de rasterização de linha suavizada](images/d3d10-rasterruleslineaa.png)

Esse algoritmo gera linhas relativamente suaves, com intensidade uniforme, com mínimo de bordas com dentes ou trançados. O padrão moiré para linhas próximas é minimizado. Há uma boa cobertura de junções entre os segmentos de linha colocados de ponta a ponta.

O kernel do filtro é um equilíbrio razoável entre a quantidade de desfoque de borda e as alterações na intensidade causadas por correções de gama. O valor de cobertura é multiplicado em o0.a de sombreador de pixel (srcAlpha) pela seguinte fórmula pelo [estágio de fusão de saída (OM)](output-merger-stage--om-.md):

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Regras de rasterização de pontos (sem várias amostragens)


Um ponto é interpretado como se fosse composto de dois triângulos em um padrão Z, que usa regras de rasterização de triângulo. A coordenada identifica o centro de um quadrado na largura de um pixel. Não há seleção para pontos.

A ilustração a seguir mostra alguns exemplos.

![exemplos de rasterização de pontos](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Regras de rasterização de suavização de várias amostras


A suavização de várias amostras (MSAA) reduz o serrilhado de geometria usando testes de cobertura de pixel e estêncil de profundidade em vários locais de subamostra. Para melhorar o desempenho, cálculos por pixel são executados uma vez para cada pixel coberto, compartilhando saídas de sombreador por subpixels cobertos. A suavização de várias amostras não reduz o serrilhado de superfície. Locais de amostra e funções de reconstrução dependem da implementação de hardware.

A ilustração a seguir mostra alguns exemplos.

![exemplos de rasterização de suavização de várias amostras](images/d3d10-rasterrulesmsaa.png)

O número de locais de amostra depende do modo de várias amostras. Atributos de vértice são interpolados em centros de pixel, pois é onde o sombreador de pixel é invocado (isso se torna extrapolação se o centro não está coberto). Atributos podem ser sinalizados no sombreador de pixel para serem amostras centroides, o que faz com que pixels não cobertos interpolem o atributo na interseção entre a área do pixel e o primitivo.

Um sombreador de pixel é executado para cada área de pixel de 2x2 dar suporte a cálculos derivados (que usam deltas de x e y). Isso significa que as invocações de sombreador ocorrerem mais do que é mostrado para preencher o quanta de 2x2 mínimo (que é independente de várias amostras). O resultado do sombreador é gravado para cada amostra coberta que passa no teste de estêncil de profundidade por amostra.

Regras de rasterização para primitivos são, em geral, inalteradas por suavização de várias amostras, exceto:

-   Para um triângulo, um teste de cobertura é realizado para cada local de amostra (não para um centro de pixel). Se mais de um local de amostra for coberto, um sombreador de pixel será executado uma vez com atributos interpolados no centro de pixel. O resultado é armazenado (replicado) para cada local de amostra coberto no pixel passa no teste de profundidade/estêncil.

    Uma linha é tratada como um retângulo composto de dois triângulos, com uma largura de linha de 1,4.

-   Para um ponto, um teste de cobertura é realizado para cada local de amostra (não para um centro de pixel).

Formatos de várias amostras podem ser usados em destinos de renderização que podem ser lidas novamente em sombreadores usando [carregar](https://msdn.microsoft.com/library/windows/desktop/bb509694), já que nenhuma resolução é necessária para amostras individuais acessadas pelo sombreador. Não há suporte para formatos de profundidade para recurso de várias amostras. Portanto, os formatos de profundidade são restritos a somente destinos de renderização.

Formatos sem tipo dão suporte a várias amostras para permitir que um modo de exibição de recurso interprete dados de diferentes maneiras. Por exemplo, você poderia criar um recurso de várias amostras usando R8G8B8A8\_TYPELESS, renderizar usando um recurso exibição de destino de renderização com um formato R8G8B8A8\_UINT e resolver o conteúdo para outro recurso com um formato de dados R8G8B8A8\_UNORM.

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Suporte a hardware

A API relata suporte de hardware para várias amostras por meio do número de níveis de qualidade. Por exemplo, um nível de qualidade 0 significa que o hardware não oferece suporte a várias amostras (em um determinado formato e nível de qualidade). Um 3 para níveis de qualidade significa que o hardware oferecer suporte a três layouts diferentes de amostra e/ou algoritmos de resolução. Também é possível presumir o seguinte:

-   Qualquer formato que dá suporte a várias amostras é compatível com o mesmo número de níveis de qualidade para cada formato nessa família.
-   Cada formato que dá suporte a várias amostras e tem os formatos \_UNORM, \_SRGB, \_SNORM ou \_FLOAT também dá suporte à resolução.

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Amostragem de centroides de atributos com suavização de várias amostras

Por padrão, os atributos de vértice são interpolados para um centro de pixel durante a suavização de várias amostras; se o centro de pixel não estiver coberto, atributos serão extrapolados para um centro de pixel. Se uma entrada de sombreador de pixel contendo a semântica de centroide (supondo que o pixel não está totalmente coberto) for amostrada em algum lugar dentro da área de cobertura do pixel, será possivelmente em um dos locais de amostra cobertos. Uma máscara de amostra (especificada pelo estado de rasterizador) é aplicada antes do cálculo de centroides. Portanto, uma amostra fora da máscara não será usada como um local de centroides.

O rasterizador de referência escolhe um local de amostra para a amostragem de centroides semelhante a este:

-   A máscara de amostra permite todas as amostras. Use um centro de pixel se o pixel estiver coberto ou se nenhuma das amostras estiver coberta. Caso contrário, a primeira amostra coberta será escolhida, começando pelo centro de pixel e movendo para fora.
-   A máscara de amostra desativa todas as amostras menos uma (um cenário comum). Um app pode implementar superamostragem de passagem múltipla passando valores de máscara de amostra de bit único e renderizando novamente a cena para cada amostra usando amostragem de centroide. Isso exige que um app ajuste derivadas para selecionar adequadamente mips de textura mais detalhada para a mais alta densidade de amostragem de textura.

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Cálculos derivados com várias amostras

Os sombreadores de pixel são sempre executados usando uma área mínima de pixel de 2x2 para suporte a cálculos derivados, que são calculados utilizando deltas entre dados de pixels adjacentes (considerando que os dados em cada pixel tenham sido amostrados com espaçamento de unidade horizontal ou vertical). Isso não é afetado por várias amostras.

Se derivadas forem solicitadas em um atributo com amostragem de centroides, o cálculo de hardware não será ajustado, o que pode causar derivadas imprecisas. Um sombreador irá esperar um vetor de unidade no espaço de destino de renderização, mas poderá receber um vetor de não unidade em relação a algum outro espaço de vetor. Portanto, é responsabilidade do app tomar cuidado ao solicitar derivadas de atributos com amostragem de centroides.

Na verdade, é recomendável que você não combine derivadas e amostragem de centroides. A amostragem de centroides pode ser útil para situações em que é fundamental que atributos interpolados de primitivo não sejam extrapolados, mas isso faz com que atributos pareçam saltar onde uma borda de primitivo cruza um pixel (em vez de alterar continuamente) ou derivadas que não podem ser usadas em operações de amostragem de textura que derivam LOD.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Apêndices](appendix.md)

[Estágio do rasterizador (RS)](rasterizer-stage--rs-.md)

 

 




