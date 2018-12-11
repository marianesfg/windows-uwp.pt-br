---
title: Estágio de fusão de saída (OM)
description: Estágio de fusão de saída – combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo do destino de renderização e buffers de profundidade/estêncil para gerar o resultado do pipeline final.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Estágio de fusão de saída (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 63a77048bed3ad27f2040a672d93380d0250f9aa
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8825276"
---
# <a name="output-merger-om-stage"></a>Estágio de fusão de saída (OM)


Estágio de fusão de saída – combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo do destino de renderização e buffers de profundidade/estêncil para gerar o resultado do pipeline final.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Finalidade e usos


O estágio de fusão de saída (OM) é a etapa final para determinar quais pixels são visíveis (com testes de estêncil de profundidade) e mesclando as cores do pixel final.

O estágio de OM gera a cor de pixel renderizado final usando uma combinação do seguinte:

-   Estado do pipeline
-   Os dados de pixel gerados pelos sombreadores de pixel
-   O conteúdo dos destinos de renderização
-   O conteúdo dos buffers de profundidade/estêncil.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Visão geral de mesclagem

A mesclagem combina um ou mais valores de pixel para criar uma cor de pixel final. O diagrama a seguir mostra o processo envolvido na mesclagem de dados de pixel.

![diagrama de como funciona a mesclagem de dados](images/d3d10-blend-state.png)

Conceitualmente, você pode visualizar esse gráfico de fluxo implementado duas vezes no estágio de fusão de saída: o primeiro mescla dados RGB, e, em paralelo, um outro mescla dados alfa. Para ver como usar a API para criar e definir o estado de mesclagem, consulte [Configurando a funcionalidade de mesclagem](https://msdn.microsoft.com/library/windows/desktop/bb205072).

A mesclagem de função fixa pode ser habilitada independentemente para cada destino de renderização. No entanto, há apenas um conjunto de controles de mesclagem para que a mesma mesclagem seja aplicada a todos os RenderTargets com mesclagem habilitada. Valores de mesclagem (incluindo BlendFactor) sempre são vinculados ao intervalo do formato de destino de renderização antes da mesclagem. A vinculação é feita por destino de renderização, respeitando o tipo de destino de renderização. A única exceção é para os formatos float16, float11 ou float10, que não são vinculados para que as operações de mesclagem nesses formatos possam ser feitas com precisão/intervalo, no mínimo, igual ao formato de saída. NaNs e zeros assinados são propagados para todos os casos (incluindo pesos de mesclagem de 0,0).

Quando você usa destinos de renderização sRGB, o tempo de execução converte a cor do destino de renderização em espaço linear antes de realizar a mesclagem. O tempo de execução converte o valor mesclado final novamente em espaço de sRGB antes de salvar o valor de volta para o destino de renderização.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Mescla de cor de fonte de dupla

Esse recurso habilita o estágio de fusão de saída para usar simultaneamente ambas as saídas de sombreador de pixel (o0 e o1) como entradas para uma operação de mesclagem com o destino de renderização único no slot 0. As operações de mesclagem válidas incluem: adicionar, subtrair e revsubtract. A equação de mesclagem e a máscara de gravação de saída especificam quais componentes são saída do sombreador de pixel. Componentes adicionais são ignorados.

A gravação para outras saídas de sombreador de pixel (o2, o3, etc.) são indefinidas; você não pode gravar em um destino de renderização não vinculado ao slot 0. Gravar oDepth é válido durante a mesclagem de cor de fonte dupla.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Visão geral de testes de estêncil de profundidade

Um buffer de estêncil de profundidade, que é criado como um recurso de textura, pode conter dados de profundidade e dados de estêncil. Os dados de profundidade são usados para determinar quais pixels ficam mais próximos da câmera e os dados de estêncil são usados para mascarar quais pixels podem ser atualizados. Por fim, tanto os dados de valores de profundidade e estêncil são usados pelo estágio de fusão de saída para determinar se um pixel deve ser desenhado ou não. O diagrama a seguir mostra conceitualmente como os testes de estêncil de profundidade são feitos.

![diagrama de como funciona o teste de estêncil de profundidade](images/d3d10-depth-stencil-test.png)

Para configurar o teste de estêncil de profundidade, consulte [Configuração da funcionalidade de estêncil de profundidade](configuring-depth-stencil-functionality.md). Um objeto de estêncil de profundidade encapsula o estado de estêncil de profundidade. Um app pode especificar o estado de estêncil de profundidade, ou o estágio de OM usará valores padrão. Operações de mesclagem serão executadas de acordo com pixels se as várias amostras estiverem desabilitadas. Se as várias amostras estiverem habilitadas, a mesclagem ocorrerá de acordo com as várias amostragens.

O processo de usar o buffer de profundidade para determinar quais pixels devem ser desenhados é chamado de buffering de profundidade, também chamados de buffering z.

Depois que os valores de profundidade atingem o estágio de fusão de saída (seja vindo da interpolação ou de um sombreador de pixel), estarão sempre vinculados: z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z)) de acordo com a formato/precisão do buffer de profundidade, usando as regras de ponto flutuante. Depois da vinculação, o valor de profundidade é comparado (usando DepthFunc) contra o valor existente do buffer de profundidade. Se nenhum buffer de profundidade estiver vinculado, o teste de profundidade sempre será aprovado.

Se não houver nenhum componente de estêncil em formato de buffer de profundidade, ou nenhum buffer de profundidade vinculado, então o teste de estêncil sempre será aprovado.

Somente um buffer de profundidade/estêncil pode estar ativo por vez; qualquer modo de exibição de recurso vinculado deve corresponder (mesmos tamanho e dimensões) ao modo de exibição de profundidade/estêncil. Isso não significa que o tamanho do recurso deve corresponder, basta que o tamanho da exibição corresponda.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Visão geral de máscara de amostra

Uma máscara de amostra é uma máscara de cobertura de várias amostras de 32 bits que determina quais amostras são atualizadas em destinos de renderização ativos. Apenas uma máscara de amostra é permitida. O mapeamento de bits em uma máscara de amostra para as amostras em um recurso é definido pelo usuário. Para renderização de n amostras, os primeiros n bits (de LSB) da máscara de amostra são usados (32 bits é o número máximo de bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


O estágio de fusão de saída (OM) gera a cor de pixel renderizado final usando uma combinação do seguinte:

-   Estado do pipeline
-   Os dados de pixel gerados pelos sombreadores de pixel
-   O conteúdo dos destinos de renderização
-   O conteúdo dos buffers de profundidade/estêncil.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Visão geral de máscara de gravação de saída

Use uma máscara de gravação de saída para controlar (por componente) quais dados podem ser gravados em um destino de renderização.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Visão geral de vários destinos de renderização

Um sombreador de pixel pode ser usado para, no mínimo, 8 destinos de renderização diferentes, e todos devem ser do mesmo tipo (buffer, Texture1D, Texture1DArray e assim por diante). Além disso, todos os destinos de renderização devem ter o mesmo tamanho em todas as dimensões (largura, altura, profundidade, tamanho da matriz, contagens de amostra). Cada destino de renderização pode ter um formato de dados diferente.

Você pode usar qualquer combinação dos slots de destinos de renderização (até 8). No entanto, um modo de exibição de recurso não pode estar vinculado a vários slots de destino de renderização simultaneamente. Um modo de exibição poderá ser reutilizado, desde que os recursos não sejam usados simultaneamente.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Configuração da funcionalidade de estêncil de profundidade</a></p></td>
<td align="left"><p>Esta seção abrange as etapas para configurar o buffer de estêncil de profundidade e o estado de estêncil de profundidade para o estágio de fusão de saída.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




