---
title: "Pipeline de elementos gráficos"
description: "O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis."
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- "Pipeline de elementos gráficos"
- "Estágios de pipeline"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: af583deb51b93bffc05c4371466fa8412bb0476d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="graphics-pipeline"></a>Pipeline de elementos gráficos


O pipeline de gráficos do Direct3D foi projetado para a geração de elementos gráficos para aplicativos de jogos em tempo real. Os dados fluem da entrada para a saída por meio de cada um dos estágios configuráveis ou programáveis.

Todos os estágios de podem ser configurados usando a API do Direct3D. Estágios que apresentam núcleos comuns de sombreador (os blocos retangulares arredondados) são programáveis usando a linguagem de programação [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Isso torna o pipeline extremamente flexível e adaptável.

Os mais utilizados são o estágio de sombreador de vértice (VS) e o estágio de sombreador de Pixel (PS). Se você não fornecer esses estágios de sombreador, um sombreador de pixel e vértice de passagem no-op padrão será utilizado.

![diagrama do fluxo de dados no pipeline programável do direct3d 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Estágio do Assembler de Entrada

 

| | |
|-|-|
|Finalidade|O [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md) fornece dados de primitivo e adjacência ao pipeline, como triângulos, linhas e pontos, incluindo IDs de semântica para ajudar a tornar os sombreadores mais eficientes, reduzindo o processamento para primitivos que ainda não foram processados.|
|Entrada|Dados primitivos (triângulos, linhas, e/ou pontos), de buffers preenchidos pelo usuário na memória. E possivelmente dados de adjacência. Um triângulo seria 3 vértices para cada triângulo e possivelmente 3 vértices para dados de adjacência por triângulo.|
|Saída|Primitivos com valores anexados gerados pelo sistema (como uma ID de primitivo, uma ID de instância ou uma ID de vértice).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>Estágio do Sombreador de Vértice
 

| | |
|-|-|
|Finalidade|O [estágio do Sombreador de Vértice (VS)](vertex-shader-stage--vs-.md) processa vértices, normalmente realizando operações como transformações, aplicação de capas e iluminação. Um sombreador de vértice usa um único vértice de entrada para produzir um vértice de saída. Operações por vértice individuais, como Transformações, Aplicação de Capa, Metamorfose e Iluminação por Vértice.|
|Entrada|Um único vértice, com valores VertexID e InstanceID gerados pelo sistema. Cada vértice de entrada de sombreador de vértice pode ser composto de até 16 vetores de 32 bits (até 4 componentes cada).|
|Saída|Um único vértice. Cada vértice de saída pode ser composto de até 16 vetores de 4 componentes de 32 bits.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>Estágio de sombreador Hull
 

| | |
|-|-|
|Finalidade|O [estágio de Sombreador Hull (HS)](hull-shader-stage--hs-.md) é um dos estágios de mosaico, que divide uma única superfície de um modelo em vários triângulos com eficiência. Um sombreador hull é chamado uma vez por patch e transforma os pontos de controle de entrada que definem uma superfície de ordem baixa em pontos de controle que formam um patch. Ela também faz alguns cálculos por patch para fornecer dados para o estágio Tessellator (TS) e o estágio do sombreador de domínio (DS).|
|Entrada|Entre 1 e 32 pontos de controle de entrada, que juntos definem uma superfície de ordem baixa.|
|Saída|Entre 1 e 32 pontos de controle de saída, que juntos formam um patch. O sombreador hull declara o estado para o estágio de Mosaico (TS), incluindo o número de pontos de controle, o tipo da face do patch e o tipo de particionamento a ser usado ao criar o mosaico.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>Estágio de mosaico
 

| | |
|-|-|
|Finalidade|O [estágio de Mosaico (TS)](tessellator-stage--ts-.md) cria um padrão de amostragem do domínio que representa o patch de geometria e gera um conjunto de objetos menores (triângulos, os pontos ou linhas) que se conectam a essas amostras.|
|Entrada|O mosaico opera uma vez por patch usando os fatores de mosaico (que especificam o quão delicadamente o domínio será transformado em mosaico) e o tipo de particionamento (que especifica o algoritmo usado para dividir um patch) que são transmitidos do estágio de sombreador hull. |
|Saída|O mosaico gera coordenadas uv (e, opcionalmente, w) e a topologia de superfície para o estágio do sombreador de domínio.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>Estágio de sombreador de domínio
 

| | |
|-|-|
|Finalidade|O [estágio do Sombreador de Domínio (DS)](domain-shader-stage--ds-.md) calcula a posição de vértice de um ponto subdividido no patch saída; ele calcula a posição do vértice que corresponde a cada amostra de domínio. Um sombreador de domínio é executado uma vez por ponto de saída do estágio de mosaico e tem acesso somente leitura às constantes do patch de saída, ao patch de saída do sombreador e às coordenadas UV de saída do estágio de mosaico.|
|Entrada|Um sombreador de domínio consome pontos de controle de saída do [estágio de Sombreador Hull (HS)](hull-shader-stage--hs-.md). As saídas do sombreador hull incluem: pontos de controle, dados da constante de patch, e fatores de mosaico (os fatores de mosaico podem incluir os valores usados pelo gerador de mosaico de função fixa, bem como os valores brutos, antes do arredondamento por mosaico inteiro, o que facilita o geomorfismo, por exemplo). Um sombreador de domínio é chamado uma vez por coordenadas de saída do [estágio de Mosaico (TS)](tessellator-stage--ts-.md).|
|Saída|O estágio de Sombreador de Domínio (DS) gera a posição de vértice de um ponto subdividido no patch de saída.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>Estágio de sombreador de geometria
 

| | |
|-|-|
|Finalidade|O [estágio de Sombreador de Geometria (GS)](geometry-shader-stage--gs-.md) processa triângulos primitivos inteiros, linhas e pontos, juntamente com seus vértices adjacentes. Ele suporta amplificação e cancelamento de amplificação de geometria. Ele é útil para algoritmos incluindo expansão de sprites de ponto, sistemas de partículas dinâmicas, geração de pele/barbatanas, geração de volume de sombra, passagem única de renderização para Cubemap, troca de material por primitivo, a configuração de material por primitivo — incluindo geração de coordenadas baricêntricas como dados primitivos para que um sombreador de pixel possa executar a interpolação de atributo personalizada. |
|Entrada|Ao contrário de sombreadores de vértice que operam em um único vértice, as entradas do sombreador de geometria são os vértices de um primitivo completo (três vértices para triângulos, dois vértices para linhas ou um único vértice para ponto).|
|Saída|O estágio de sombreador de geometria (GS) é capaz de gerar vários vértices formando uma única topologia selecionada. As topologias de saída do sombreador de geometria disponíveis são <strong>tristrip</strong>, <strong>linestrip</strong>, e <strong>pointlist</strong>. O número de primitivos emitidos pode variar livremente dentro de qualquer chamada do sombreador de geometria, embora o número máximo de vértices que podem ser emitidos deva ser declarado estaticamente. Os tamanhos de faixa emitidos a partir de uma chamada de sombreador de geometria podem ser arbitrários, e novas faixas podem ser criadas por meio da função HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>Estágio de Saída de Fluxo
 

| | |
|-|-|
|Finalidade|O [estágio de Saída de Fluxo (SO)](stream-output-stage--so-.md) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.|
|Entrada|Dados de vértice de um estágio de pipeline anterior.|
|Saída|O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior, como o estágio de Sombreador de Geometria (GS), para um ou mais buffers na memória. Se o estágio de Sombreador de Geometria (GS) estiver inativo, e o estágio de Saída de Fluxo (SO) estiver ativo, ele gera continuamente dados de vértice do estágio de Sombreador de Domínio (DS) para buffers na memória (ou se o DS também estiver inativo, do estágio do Sombreador de Vértice (VS)).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>Estágio do rasterizador
 

| | |
|-|-|
|Finalidade|O [estágio do Rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivos que não estão em exibição, prepara-os para o estágio de Sombreador de Pixel (PS) e determina como chamar os sombreadores de pixel. Converte informações de vetor (compostas de formas ou primitivos) em uma imagem de rasterização (composta de pixels) para fins de exibição de gráficos 3D em tempo real.|
|Entrada|Presume-se que os vértices (x, y, z, w) que estão entrando no estagio de rasterizador estejam o espaço de recorte homogêneo. Nesse espaço de coordenadas, o eixo X aponta para a direita, Y aponta para cima, Z aponta para longe da câmera.|
|Saída|Os pixels reais que precisam ser renderizados. Inclui alguns atributos de vértice para uso em interpolação pelo sombreador de pixel.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>Estágio do sombreador de pixel
 

| | |
|-|-|
|Finalidade|O [estágio de Sombreador de Pixel (PS)](pixel-shader-stage--ps-.md) recebe dados interpolados para uma primitiva e gera dados por pixel, como cor.|
|Entrada|Quando o pipeline é configurado sem um sombreador de geometria, um sombreador de pixel é limitado a 16 entradas de 32 bits de 4 componentes. Caso contrário, um sombreador de pixel pode levar até 32 entradas de 32 bits de 4 componentes. Os dados de entrada de sombreador de pixel incluem atributos de vértice (que podem ser interpolados com ou sem correção de perspectiva) ou podem ser tratados como constantes por primitivo. As entradas do sombreador de pixel são interpoladas dos atributos de vértice do primitivo que está sendo rasterizado, com base no modo de interpolação declarado. Se um primitivo for recortado antes de rasterização, o modo de interpolação também será aplicado durante o processo de recorte. |
|Saída|Um sombreador de pixel pode gerar até 8 cores de 4 componentes de 32 bits, ou nenhuma cor se o pixel for descartado. Os componentes de registro de saída de sombreador de pixel devem ser declarados antes que possam ser usados; uma máscara de gravação de saída distinta é permitida para cada registro.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>Estágio de fusão de saída
 

| | |
|-|-|
|Finalidade|O [estágio de Fusão de Saída (OM)](output-merger-stage--om-.md) combina vários tipos de dados de saída (valores de sombreador de pixel, informações de profundidade e estêncil) com o conteúdo do destino de renderização e buffers de profundidade/estêncil para gerar o resultado do pipeline final.|
|Entrada|As entradas de fusão de saída são o estado do pipeline, os dados de pixel gerados pelos sombreadores de pixel, o conteúdo dos destinos de renderização e o conteúdo dos buffers de profundidade/estêncil.|
|Saída|A cor do pixel renderizado final.|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

[Calcular o pipeline](compute-pipeline.md)

 

 
