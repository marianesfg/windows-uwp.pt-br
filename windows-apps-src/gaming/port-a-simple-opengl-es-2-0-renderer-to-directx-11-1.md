---
title: Portar um renderizador simples do OpenGL ES 2.0 para o Direct3D 11
description: Para o primeiro exercício de portabilidade, vamos começar com noções básicas - trazer um renderizador simples para um cubo giratório com sombreamento de vértice do OpenGL ES 2.0 para Direct3D, que corresponda ao modelo de aplicativo do DirectX 11 (Windows Universal) do Visual Studio 2015.
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d 11, portabilidade
ms.localizationpriority: medium
ms.openlocfilehash: 3c17e0b8ceb5938b7ca224f4a67198929a37a7f4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368367"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>Portar um renderizador simples do OpenGL ES 2.0 para o Direct3D 11



Para este exercício de portabilidade, vamos começar com noções básicas: trazer um renderizador simples para um cubo giratório com sombreamento de vértice do OpenGL ES 2.0 para Direct3D, que corresponda ao modelo de aplicativo do DirectX 11 (Windows Universal) do Visual Studio 2015. Conforme seguimos por este processo de compatibilização, você aprenderá o seguinte:

-   Como fazer a portabilidade de um conjunto simples de buffers de vértice para buffers de entrada Direct3D
-   Como fazer a portabilidade de uniformes e atributos para buffers constantes
-   Como configurar objetos de sombreador Direct3D
-   Como a semântica HLSL básica é usada no desenvolvimento de sombreador Direct3D
-   Como fazer a portabilidade de um GLSL muito simples para HLSL

Este tópico começa após a criação de um novo projeto do DirectX 11. Para aprender a criar um novo projeto DirectX 11, leia [Criar um novo projeto DirectX 11 para a Plataforma Universal do Windows (UWP)](user-interface.md).

O projeto criado de algum desses links tem todos os códigos para a infraestrutura [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/dx-graphics-overviews) preparada e você pode iniciar imediatamente o processo de portabilidade do seu renderizador Open GL ES 2.0 para o Direct3D 11.

Este tópico mostra dois caminhos de código que realizam a mesma tarefa básica relativa a gráficos: exibir um cubo giratório sombreado por vértices em uma janela. Nos dois casos, o código abrange o seguinte processo:

1.  Criação de uma malha de cubo a partir de dados com código fixo. Essa malha é representada como uma lista de vértices, sendo que cada vértice apresenta uma posição, um vetor normal e um vetor de cor. Esse malha é colocada em um buffer de vértice para processamento pelo pipeline de sombreamento.
2.  Criação de objetos de sombreador para processamento da malha de cubo. Há dois sombreadores: um sombreador de vértice que processa vértices para rasterização e um sombreador de fragmento (pixel) que colore os pixels individuais do cubo após a rasterização. Esses pixels são escritos em um destino de renderização para exibição.
3.  Formando a linguagem de sombreamento que é usada para o processamento de vértice e pixel nos sombreadores de vértice e fragmento, respectivamente.
4.  Exibindo o cubo renderizado na tela.

![cubo opengl simples](images/simple-opengl-cube.png)

Ao concluir esse passo a passo, você deve estar familiarizado com as seguintes diferenças básicas entre o Open GL ES 2.0 e o Direct3D 11:

-   A representação de buffers e dados de vértice.
-   O processo de criação e configuração de sombreadores.
-   Linguagens de sombreamento e entradas/saídas de objetos de sombreador.
-   Comportamentos de desenho da tela.

Neste guia passo a passo, mencionamos uma estrutura de renderizador do OpenGL simples e genérica, definida da seguinte forma:

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

Essa estrutura tem uma instância e contém todos os componentes necessários para renderizar uma malha de vértice sombreado bem simples.

> **Observação**  Any OpenGL ES 2.0 código deste tópico baseia-se a implementação da API do Windows fornecida pelo grupo de Khronos e usa a sintaxe de programação de C de Windows.

 

## <a name="what-you-need-to-know"></a>O que você precisa saber


### <a name="technologies"></a>Tecnologias

-   [Microsoft Visual C++](https://docs.microsoft.com/previous-versions/60k1461a(v=vs.140))
-   OpenGL ES 2.0

### <a name="prerequisites"></a>Pré-requisitos

-   Opcional. Consulte [Compatibilizar código EGL com DXGI e Direct3D](moving-from-egl-to-dxgi.md). Leia este tópico para entender melhor a interface de elementos gráficos oferecida pelo DirectX.

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>Etapas


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
<td align="left"><p><a href="port-the-shader-config.md">Fazer a portabilidade de objetos de sombreador</a></p></td>
<td align="left"><p>Ao fazer a portabilidade do renderizador simples do OpenGL ES 2.0, a primeira etapa é definir o vértice e os objetos de sombreadores equivalentes no Direct3D 11 e certificar que o programa principal consegue se comunicar com os objetos de sombreador depois de eles serem compilados.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">Realizar a portabilidade de dados e buffers de vértice</a></p></td>
<td align="left"><p>Nesta etapa, você definirá os buffers de vértices que conterão suas malhas e os buffers de índice que permitem que os sombreadores percorram os vértices em uma ordem específica.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">Fazer a portabilidade do GLSL</a></p></td>
<td align="left"><p>Quando você tiver movido o código que cria e configura os seus buffers e objetos de sombreador, será o momento de fazer a portabilidade do código dentro dos sombreadores da linguagem de sombreadores GL do OpenGL ES 2.0 (GLSL) para a linguagem de sombreadores de alto nível do Direct3D 11 (HLSL).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">Desenhar na tela</a></p></td>
<td align="left"><p>Por fim, compatibilizamos o código que desenha o cubo giratório na tela.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>Recursos adicionais


-   [Preparar o ambiente de desenvolvimento para desenvolvimento de jogos do DirectX de UWP](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [Criar um novo projeto do DirectX 11 para UWP](user-interface.md)
-   [Mapear o OpenGL ES 2.0 conceitos e a infraestrutura para o Direct3D 11](map-concepts-and-infrastructure.md)

 

 




