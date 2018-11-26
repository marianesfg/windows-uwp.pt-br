---
title: Referência de GLSL para HLSL
description: Porte seu código GLSL (OpenGL Shader Language) para código Microsoft HLSL (High Level Shader Language) ao portar sua arquitetura gráfica do OpenGL ES 2.0 para o Direct3D 11 para criar um jogo para a UWP (Plataforma Universal do Windows).
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, glsl, hlsl, opengl, directx, sombreadores
ms.localizationpriority: medium
ms.openlocfilehash: 8f468584d995de40ff14df1527ab1df8275c36a8
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7691950"
---
# <a name="glsl-to-hlsl-reference"></a>Referência de GLSL para HLSL



Porte seu código GLSL (OpenGL Shader Language) para código Microsoft HLSL (High Level Shader Language) ao [portar sua arquitetura gráfica do OpenGL ES 2.0 para o Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md) para criar um jogo para a UWP (Plataforma Universal do Windows). O GLSL citado neste documento é compatível com OpenGL ES 2.0; o HLSL é compatível com Direct3D 11. Para saber mais sobre as diferenças entre o Direct3D 11 e versões anteriores do Direct3D, consulte [Mapeamento de recursos](feature-mapping.md).

-   [Comparando o OpenGL ES 2.0 com o Direct3D 11](#comparing-opengl-es-20-with-direct3d-11)
-   [Portando variáveis do GLSL para o HLSL](#porting-glsl-variables-to-hlsl)
-   [Portando tipos do GLSL para o HLSL](#porting-glsl-types-to-hlsl)
-   [Portando variáveis globais predefinidas do GLSL para o HLSL](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [Exemplos de portabilidade de variáveis do GLSL para o HLSL](#examples-of-porting-glsl-variables-to-hlsl)
    -   [Uniforme, atributo e variação no GLSL](#uniform-attribute-and-varying-in-glsl)
    -   [Buffers constantes e transferências de dados no HLSL](#constant-buffers-and-data-transfers-in-hlsl)
-   [Exemplos de portabilidade do código de renderização do OpenGL para o Direct3D](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [Tópicos relacionados](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>Comparando o OpenGL ES 2.0 com o Direct3D 11


O OpenGL ES 2.0 e o Direct3D 11 têm muitas semelhanças. Ambos têm pipelines de renderização e recursos gráficos parecidos. Mas o Direct3D 11 é uma API e implementação de renderização, não uma especificação; já o OpenGL ES 2.0 é uma API e especificação de renderização, mas não uma implementação. Geralmente, o Direct3D 11 e o OpenGL ES 2.0 têm as seguintes diferenças:

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Especificação independente de hardware e sistema operacional, com implementações oferecidas pelo fornecedor             | Implementação da Microsoft de abstração e certificação de hardware em plataformas Windows                                |
| Abstraído para proporcionar diversidade de hardware; o tempo de execução gerencia a maioria dos recursos                                     | Acesso direto ao layout de hardware; o aplicativo pode gerenciar recursos e processamento                                              |
| Fornece módulos mais gerais por meio de bibliotecas de terceiros, como SDL (Simple DirectMedia Layer) | Módulos mais gerais, como Direct2D, são criados com base em módulos mais específicos para simplificar o desenvolvimento de aplicativos do Windows             |
| Diferenciação de fornecedores de hardware por extensões                                                         | A Microsoft adiciona recursos opcionais para a API de modo genérico, para que não sejam específicos a qualquer fornecedor de hardware em particular |

 

Geralmente, o GLSL e o HLSL diferem das seguintes maneiras:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Voltado a procedimentos, centrado em etapas (semelhante ao C)</td>
<td align="left">Orientado a objetos, centrado em dados (semelhante ao C++)</td>
</tr>
<tr class="even">
<td align="left">Compilação de sombreador integrada à API gráfica</td>
<td align="left">O compilador HLSL <a href="https://msdn.microsoft.com/library/windows/desktop/bb509633">compila o sombreador</a> em uma representação binária intermediária antes de o Direct3D passá-lo ao driver.
<div class="alert">
<strong>Observação</strong>essa representação binária é independente de hardware. Normalmente, ela é compilada no momento da compilação do aplicativo, e não no tempo de execução dele.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Modificadores de armazenamento <a href="#porting-glsl-variables-to-hlsl">variáveis</a></td>
<td align="left">Buffers constantes e transferências de dados via declarações de layout de entrada</td>
</tr>
<tr class="even">
<td align="left"><p><a href="#porting-glsl-types-to-hlsl">Tipos</a></p>
<p>Tipo comum de vetor: vec2/3/4</p>
<p>lowp, mediump, highp</p></td>
<td align="left"><p>Tipo comum de vetor: float2/3/4</p>
<p>min10float, min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509695">texture.Sample</a> [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a> [datatype]</td>
</tr>
<tr class="odd">
<td align="left">Matrizes da linha principal (padrão)</td>
<td align="left">Matrizes de coluna principal (padrão)
<div class="alert">
<strong>Observação</strong>  Use o modificador de tipo <strong>row_major</strong> para alterar o layout de uma variável. Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509706">Sintaxe de variáveis</a>. Você também pode especificar um pragma ou um sinalizador de compilador para alterar o padrão global.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Sombreador de fragmento</td>
<td align="left">Sombreador de pixel</td>
</tr>
</tbody>
</table>

 

> **Observação**HLSL tem texturas e amostras como dois objetos separados. No GLSL, assim como no Direct3D 9, a associação de textura faz parte do estado da amostra.

 

No GLSL, grande parte do estado do OpenGL é apresentada como variáveis globais predefinidas. Por exemplo, com o GLSL, você usa a variável **gl\_Position** para definir a posição do vértice e a variável **gl\_FragColor** para especificar a cor do fragmento. No HLSL, você passa o estado do Direct3D explicitamente do código do aplicativo para o sombreador. Por exemplo, com Direct3D e HLSL, a entrada para o sombreador de vértice deve corresponder ao formato de dados no buffer de vértice, e a estrutura de um buffer constante no código do aplicativo deve corresponder à estrutura de um buffer constante ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) no código do sombreador.

## <a name="porting-glsl-variables-to-hlsl"></a>Portando variáveis do GLSL para HLSL


No GLSL, você deve aplicar modificadores (qualificadores) a uma declaração de variável de sombreador global, fornecendo à variável um comportamento específico nos sombreadores. No HLSL, não é necessário usar esses modificadores, pois o fluxo do sombreador é definido com os argumentos passados para o sombreador e retornados dele.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Comportamento de variável do GLSL</th>
<th align="left">Equivalente no HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniforme</strong></p>
<p>Você passa uma variável uniforme do código do aplicativo para o sombreador de vértice, para o sombreador de fragmento ou para ambos. Você deve definir os valores de todos os uniformes antes de desenhar triângulos com os sombreadores, de modo que os valores sejam os mesmos em todo o desenho de uma malha de triângulos. Esses valores são uniformes. Alguns uniformes são definidos para todo e quadro, e outros são exclusivos de um par específico de sombreadores de vértice-pixel.</p>
<p>As variáveis uniformes são específicas dos polígonos.</p></td>
<td align="left"><p>Use buffers constantes.</p>
<p>Consulte <a href="https://msdn.microsoft.com/library/windows/desktop/ff476896">Como: criar um buffer constante</a> e <a href="https://msdn.microsoft.com/library/windows/desktop/bb509581">Constantes de sombreador</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>variação</strong></p>
<p>Inicialize uma variável de variação dentro do sombreador de vértice e passe-a para uma variável de variação com nome idêntico no sombreador de fragmento. Como o sombreador de vértice define apenas o valor das variáveis diversas em cada vértice, o rasterizador interpola esses valores (de modo a corrigir a perspectiva) para gerar valores por fragmento, que serão passados no sombreador de fragmento. Essas variáveis variam em cada triângulo.</p></td>
<td align="left">Use a estrutura retornada do sombreador de vértice como entrada do sombreador de pixel. Verifique se valores semânticos correspondem.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>atributo</strong></p>
<p>Um atributo é uma parte da descrição de um vértice que você passa do código do aplicativo para o sombreador de vértice. Ao contrário de um uniforme, você pode definir o valor do atributo de cada vértice, que, por sua vez, permite que cada vértice tenha um valor diferente. As variáveis de atributo são específicas dos vértices.</p></td>
<td align="left"><p>Defina um buffer de vértice no código do aplicativo em Direct3D e corresponda-o à entrada de vértice definida no sombreador de vértice. Você também pode definir um buffer de índice. Veja <a href="https://msdn.microsoft.com/library/windows/desktop/ff476899">Como: criar um buffer de vértice</a> e <a href="https://msdn.microsoft.com/library/windows/desktop/ff476897">Como: criar um buffer de índice</a>.</p>
<p>Crie um layout de entrada no código de seu aplicativo Direct3D e corresponda os valores semânticos aos da entrada do vértice. Consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout">Criar o layout de entrada</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>Constantes que são compiladas no sombreador e nunca mudam.</p></td>
<td align="left">Use uma <strong>static const</strong>. <strong>static</strong> indica que o valor não é exposto a buffers constantes; <strong>const</strong> indica que o sombreador não pode alterar o valor. Por isso, o valor é conhecido no momento da compilação com base em seu inicializador.</td>
</tr>
</tbody>
</table>

 

No GLSL, as variáveis sem modificadores são apenas variáveis globais comuns particulares a cada sombreador.

Quando você passa dados para texturas ([Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) no HLSL) e suas amostras associadas ([SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644) no HLSL), normalmente você as declara como variáveis globais no sombreador de pixel.

## <a name="porting-glsl-types-to-hlsl"></a>Portando tipos do GLSL para o HLSL


Use esta tabela para fazer a portabilidade de tipos do GLSL para o HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo do GLSL</th>
<th align="left">Tipo do HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">tipos escalares: float, int, bool</td>
<td align="left"><p>tipos escalares: float, int, bool</p>
<p>também uint, double</p>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">Tipos escalares</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>tipo de vetor</p>
<ul>
<li>vetor de ponto flutuante: vec2, vec3, vec4</li>
<li>vetor booliano: bvec2, bvec3, bvec4</li>
<li>vetor de inteiro assinado: ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>tipo de vetor</p>
<ul>
<li>float2, float3, float4 e float1</li>
<li>bool2, bool3, bool4 e bool1</li>
<li>int2, int3, int4 e int1</li>
<li><p>Esses tipos também têm expansões de vetor semelhantes a float, bool e int:</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509707">Tipo de vetor</a> e <a href="https://msdn.microsoft.com/library/windows/desktop/bb509568">Palavras-chave</a>.</p>
<p>o vetor também é um tipo definido como float4 (typedef vector &lt;float, 4&gt; vector;). Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">Tipo definido pelo usuário</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>tipo de matriz</p>
<ul>
<li>mat2: matriz float 2 x 2</li>
<li>mat3: matriz float 3 x 3</li>
<li>mat4: matriz float 4 x 4</li>
</ul></td>
<td align="left"><p>tipo de matriz</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>também float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>Esses tipos também têm expansões de matriz semelhantes a float:</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Você também pode usar o <a href="https://msdn.microsoft.com/library/windows/desktop/bb509623">tipo de matriz</a> para definir uma matriz.</p>
<p>Por exemplo: matrix &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>a matriz também é um tipo definido como float4x4 (typedef matrix &lt;float, 4, 4&gt; matrix;). Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509702">Tipo definido pelo usuário</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>qualificadores de precisão para float, int, sampler</p>
<ul>
<li><p>highp</p>
<p>Este qualificador atende aos requisitos mínimos de precisão, maiores que os fornecidos por min16float e menores que um float de 32 bits completo. O equivalente no HLSL é:</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>Este qualificador aplicado a float, e int é equivalente a min16float e a min12int no HLSL. Mínimo de 10 bits de mantissa, diferente de min10float.</p></li>
<li><p>lowp</p>
<p>Esse qualificador aplicado a float oferece um intervalo de ponto flutuante de -2 a 2. Equivalente a min10float no HLSL.</p></li>
</ul></td>
<td align="left"><p>tipos de precisão</p>
<ul>
<li>min16float: valor mínimo de ponto flutuante de 16 bits</li>
<li><p>min10float</p>
<p>Valor mínimo de ponto fixo mínimo assinado de 2,8 bits (2 bits de números inteiros e componente fracionário de 8 bits). O componente fracionário de 8 bits pode incluir 1 em vez de excluir, proporcionando o intervalo totalmente inclusivo de -2 a 2.</p></li>
<li>min16int: inteiro mínimo assinado de 16 bits</li>
<li><p>min12int: inteiro mínimo assinado de 12 bits</p>
<p>Esse tipo serve para 10Level9 (<a href="https://msdn.microsoft.com/library/windows/desktop/ff476876">níveis de recursos 9_x</a>), em que os inteiros são representados por números de ponto flutuante. Essa é a precisão que você pode conseguir ao emular um inteiro com um número de ponto flutuante de 16 bits.</p></li>
<li>min16uint: inteiro mínimo não assinado de 16 bits</li>
</ul>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/bb509646">Tipos escalares</a> e o tópico sobre <a href="https://msdn.microsoft.com/library/windows/desktop/hh968108">uso da precisão mínima do HLSL</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471525">Texture2D</a></td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/bb509700">TextureCube</a></td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>Fazendo a portabilidade de variáveis globais predefinidas do GLSL para HLSL


Use esta tabela para portar variáveis globais predefinidas do GLSL para o HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Variável global predefinida do GLSL</th>
<th align="left">Semântica do HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>Esta variável é do tipo <strong>vec4</strong>.</p>
<p>Posição do vértice</p>
<p>por exemplo - gl_Position = posição;</p></td>
<td align="left"><p>SV_Position</p>
<p>POSITION no Direct3D 9</p>
<p>Esta semântica é do tipo <strong>float4</strong>.</p>
<p>Saída do sombreador de vértice</p>
<p>Posição do vértice</p>
<p>por exemplo - float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>Esta variável é do tipo <strong>float</strong>.</p>
<p>Tamanho do ponto</p></td>
<td align="left"><p>PSIZE</p>
<p>Sem significado a não ser que você use o Direct3D 9</p>
<p>Esta semântica é do tipo <strong>float</strong>.</p>
<p>Saída do sombreador de vértice</p>
<p>Tamanho do ponto</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>Esta variável é do tipo <strong>vec4</strong>.</p>
<p>Cor do fragmento</p>
<p>por exemplo - gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>COLOR no Direct3D 9</p>
<p>Esta semântica é do tipo <strong>float4</strong>.</p>
<p>Saída do sombreador de pixel</p>
<p>Cor do pixel</p>
<p>por exemplo - float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>Esta variável é do tipo <strong>vec4</strong>.</p>
<p>Cor do fragmento para o anexo de cor n</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>Esta semântica é do tipo <strong>float4</strong>.</p>
<p>Valor de saída do sombreador de pixel armazenado no destino de renderização 0 &lt;= n &lt;= 7.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>Esta variável é do tipo <strong>vec4</strong>.</p>
<p>Posição do fragmento no buffer de quadros</p></td>
<td align="left"><p>SV_Position</p>
<p>Indisponível no Direct3D 9</p>
<p>Esta semântica é do tipo <strong>float4</strong>.</p>
<p>Entrada do sombreador de pixel</p>
<p>Coordenadas de espaço na tela</p>
<p>por exemplo - float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>Esta variável é do tipo <strong>bool</strong>.</p>
<p>Determina se o fragmento pertence a uma primitiva frontal.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>VFACE no Direct3D 9</p>
<p>SV_IsFrontFace é do tipo <strong>bool</strong>.</p>
<p>VFACE é do tipo <strong>float</strong>.</p>
<p>Entrada do sombreador de pixel</p>
<p>Voltado à primitiva</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>Esta variável é do tipo <strong>vec2</strong>.</p>
<p>Posição do fragmento em um ponto (somente rasterização de pontos)</p></td>
<td align="left"><p>SV_Position</p>
<p>VPOS no Direct3D 9</p>
<p>SV_Position é do tipo <strong>float4</strong>.</p>
<p>VPOS é do tipo <strong>float2</strong>.</p>
<p>Entrada do sombreador de pixel</p>
<p>Posição do pixel ou da amostra no espaço da tela</p>
<p>por exemplo - float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>Esta variável é do tipo <strong>float</strong>.</p>
<p>Dados do buffer de profundidade</p></td>
<td align="left"><p>SV_Depth</p>
<p>DEPTH no Direct3D 9</p>
<p>SV_Depth é do tipo <strong>float</strong>.</p>
<p>Saída do sombreador de pixel</p>
<p>Dados do buffer de profundidade</p></td>
</tr>
</tbody>
</table>

 

Use a semântica para especificar a posição, a cor e outros aspectos das entradas dos sombreadores de vértice e de pixel. Você deve corresponder os valores semânticos no layout de entrada à entrada do sombreador de vértice. Para obter mais informações, consulte os [exemplos de portabilidade de variáveis do GLSL para HLSL](#examples-of-porting-glsl-variables-to-hlsl). Para saber mais sobre a semântica HLSL, veja [Semântica](https://msdn.microsoft.com/library/windows/desktop/bb509647).

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>Exemplos de portabilidade de variáveis do GLSL para o HLSL


Consulte exemplos de uso de variáveis do GLSL em código OpenGL/GLSL e o exemplo equivalente em código Direct3D/HLSL.

### <a name="uniform-attribute-and-varying-in-glsl"></a>Uniforme, atributo e variação no GLSL

Código do aplicativo OpenGL

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

Código do sombreador de vértice GLSL

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

Código do sombreador de fragmento GLSL

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>Buffers constantes e transferências de dados no HLSL

Consulte um exemplo de como passar, ao sombreador de vértice HLSL, dados que depois fluem pelo sombreador de pixel. No código do aplicativo, defina um vértice e um buffer constante. Em seguida, no código do sombreador de vértice, defina o buffer constante como [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581) e armazene os dados de vértice e os dados de entrada do sombreador de pixel. Aqui, usamos estruturas chamadas **VertexShaderInput** e **PixelShaderInput**.

Código do aplicativo Direct3D

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

Código do sombreador de vértice do HLSL

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

Código do sombreador de pixel do HLSL

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>Exemplos de portabilidade do código de renderização do OpenGL para o Direct3D


Aqui, mostramos um exemplo de renderização com código do OpenGL ES 2.0 e o exemplo equivalente no código do Direct3D 11.

Código de renderização do OpenGL

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Código de renderização do Direct3D

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>Tópicos relacionados


* [Portar do OpenGL ES 2.0 para o Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




