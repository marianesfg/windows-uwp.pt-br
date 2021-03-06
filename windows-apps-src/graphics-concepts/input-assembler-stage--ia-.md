---
title: Estágio do assembler de entrada (IA)
description: O estágio do assembler de entrada fornece dados primitivos e de adjacência ao pipeline, como triângulos, linhas e pontos, incluindo IDs de semântica para ajudar a tornar sombreadores mais eficientes reduzindo o processamento para primitivas que ainda não foram processadas.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Estágio do assembler de entrada (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 70957cbce10da25943b3c6347ccbbc81aafb5739
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370769"
---
# <a name="input-assembler-ia-stage"></a>Estágio do assembler de entrada (IA)


O estágio do assembler de entrada fornece dados primitivos e de adjacência ao pipeline, como triângulos, linhas e pontos, incluindo IDs de semântica para ajudar a tornar sombreadores mais eficientes reduzindo o processamento para primitivas que ainda não foram processadas.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Finalidade e usos


A finalidade do estágio de assembler de entrada (IA) é ler dados primitivos (pontos, linhas e triângulos) de buffers preenchidos pelo usuário e reunir os dados em primitivas que serão usadas por outros estágios de pipeline e anexar [valores gerados pelo sistema](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) para ajudar a tornar os sombreadores mais eficientes. Valores gerados pelo sistema são cadeias de caracteres de texto que também são chamadas de semântica. Os estágios de sombreador programáveis são construídos em um núcleo comum de sombreador que usa valores gerados pelo sistema (como uma ID primitiva, ID de instância ou ID de vértice) para que o estágio do sombreador possa reduzir o processamento a apenas primitivas, instâncias ou vértices que ainda não foram processados.

O estágio de IA pode reunir vértices em vários [tipos primitivos](primitive-topologies.md) diferentes (por exemplo, listas de linha, faixas de triângulos ou primitivas com adjacência). Tipos primitivos, como uma lista de triângulos com adjacência e uma lista de linhas com adjacência, suportam o [estágio do sombreador de geometria (GS)](geometry-shader-stage--gs-.md).

Informações de adjacência são visíveis para um app apenas em um sombreador de geometria. Se um sombreador de geometria for invocado com um triângulo incluindo adjacência, por exemplo, os dados de entrada conterão 3 vértices para cada triângulo e 3 vértices para dados de adjacência por triângulo.

Quando o estágio IA é solicitado para gerar dados de adjacência, os dados de entrada devem incluir dados de adjacência. Isso pode exigir o fornecimento de um vértice fictício (formando um triângulo degenerado), ou talvez sinalização em um dos atributos de vértice, seja o vértice existente ou não. Isso também precisaria ser detectado e manipulado por um sombreador de geometria, embora a seleção da geometria degenerada aconteça no estágio de rasterizador.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrada


O estágio IA lê os dados da memória: dados primitivos (pontos, linhas e/ou triângulos) dos buffers preenchidos pelo usuário.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


O estágio IA reúne os dados em primitivas e anexa valores gerados pelo sistema, gerando saída que, como primitivas, serão usadas pelo [estágio do sombreador de vértice (VS)](vertex-shader-stage--vs-.md) e, em seguida, outros estágios de pipeline.

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
<td align="left"><p><a href="primitive-topologies.md">Topologias primitivas</a></p></td>
<td align="left"><p>O Direct3D dá suporte a várias topologias primitivas, que definem como os vértices são interpretados e renderizados pelo pipeline, como listas de ponto, listas de linha e faixas de triângulos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Usando os valores gerados pelo sistema</a></p></td>
<td align="left"><p>Valores gerados pelo sistema são gerados pelo estágio de assembler de entrada (IA) (com base na <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">semântica</a>) da entrada fornecida pelo usuário para permitir determinadas eficiências em operações do sombreador. Anexando dados, como uma ID de instância (visível para o <a href="vertex-shader-stage--vs-.md">estágio do sombreador de vértice (VS)</a>), uma ID de vértice (visível para VS) ou uma ID primitiva (visível para <a href="geometry-shader-stage--gs-.md">estágio do sombreador de geometria (GS)</a>/<a href="pixel-shader-stage--ps-.md">estágio do sombreador de pixel (PS)</a>), um estágio do sombreador subsequente pode acessar esses valores do sistema para otimizar o processamento nesse estágio.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




