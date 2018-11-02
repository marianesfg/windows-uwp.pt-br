---
author: mtoepke
title: Portabilidade de buffers de OpenGL ES 2.0, uniformes, vértices para Direct3D
description: Durante o processo de compatibilização para o Direct3D 11 a partir do OpenGL ES 2.0, você deve mudar a sintaxe e o comportamento de API para passar dados entre o aplicativo e os programas sombreadores.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, opengl, direct3d, buffers, uniformes, atributos de vértice
ms.localizationpriority: medium
ms.openlocfilehash: bc0192eb4b89ef91bc895a96e46cd39524f24c44
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929806"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>Comparar buffers, uniformes e atributos de vértice do OpenGL ES 2.0 com os do Direct3D




**APIs Importantes**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

Durante o processo de compatibilização para o Direct3D 11 a partir do OpenGL ES 2.0, você deve mudar a sintaxe e o comportamento de API para passar dados entre o aplicativo e os programas sombreadores.

No OpenGL ES 2.0, os dados são passados de e para programas de sombreador de três maneiras: como uniformes para dados constantes; como atributos para dados de vértice; como objetos de para outros dados de recursos (como texturas). No Direct3D 11, eles correspondem, em linhas gerais, a buffers constantes, buffers de vértice e sub-recursos. Apesar da aparente semelhança, o uso é bastante diferente.

Este é o mapeamento básico:

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniforme                   | campo (**cbuffer**) de buffer constante.                                                                                                                                                |
| atributo                 | campo do elemento buffer de vértices, designado por um layout de entrada e marcado com uma semântica específica de HLSL.                                                                                |
| objeto de buffer             | buffer; Consulte [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) e [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) e para definições de buffer de uso geral. |
| objeto de buffer de quadro (FBO) | renderizar destino(s); Consulte [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) com [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635).                                       |
| buffer de fundo               | cadeia de troca com superfície de "buffer de fundo"; Consulte [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) com anexo [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343).                       |

 

## <a name="port-buffers"></a>Compatibilizar buffers


No OpenGL ES 2.0, o processo de criação e associação de qualquer tipo de buffer costuma seguir este padrão

-   Chame glGenBuffers para gerar um ou mais buffers e retorne os identificadores para eles.
-   Chame glBindBuffer para definir o layout de um buffer, como GL\_ELEMENT\_ARRAY\_BUFFER.
-   Chame glBufferData para popular o buffer com dados específicos (como estruturas de vértice, dados de índice ou dados de cor) em um layout específico.

O tipo de buffer mais comum é o de vértice, que contém, no mínimo, as posições dos vértices em algum sistema de coordenadas. Em um uso típico, um vértice é representado por uma estrutura que contém as coordenadas de posição, um vetor normal à posição do vértice, um vetor tangente à posição do vértice e coordenadas de pesquisa de textura (uv). O buffer contém uma lista contígua desses vértices, em uma ordem específica (como uma lista, faixa ou leque de triângulos) e que, em conjunto, representam os polígonos visíveis da cena (em Direct3D 11 e OpenGL ES 2.0, não é eficiente ter vários buffers de vértice por chamada de desenho).

Consulte um exemplo de buffer de vértice e buffer de índice criados com OpenGL ES 2.0:

OpenGL ES 2.0: criando e preenchendo um buffer de vértice e um buffer de índice.

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

Outros buffers incluem buffers de pixel e mapas, como texturas. O pipeline do sombreador pode renderizar buffers de textura (pixmaps) ou objetos do buffer, usando esses buffers em passagens futuras do sombreador. No caso mais simples, o fluxo de chamadas é o seguinte:

-   Chame glGenFramebuffers para gerar um objeto de buffer de quadros.
-   Chame glBindFramebuffer para vincular o objeto de buffer de quadros para gravação.
-   Chame glFramebufferTexture2D para desenhar em um mapa de textura específico.

No Direct3D 11, elementos de dados de buffer são considerados "sub-recursos" e podem variar de elementos de dados de vértice individuais a texturas MIP-map.

-   Popule uma estrutura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) com a configuração para um elemento de dados de buffer.
-   Popule uma estrutura [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) com o tamanho dos elementos individuais no buffer e com o tipo de buffer.
-   Chame [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575) com essas duas estruturas.

Direct3D 11: criando e populando um buffer de vértices e um buffer de índice.

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

Buffers de pixel ou mapas graváveis, como um buffer de quadro, podem ser criados como objetos [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635). Eles podem ser ligados como recursos a um [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) ou [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628), que, uma vez desenhado, pode ser exibido com a cadeia de troca associada ou passado para um sombreador, respectivamente.

Direct3D 11: criando um objeto de buffer de quadros.

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>Alterar uniformes e objetos de buffer de uniformes para buffers constantes Direct3D


No Open GL ES 2.0, os uniformes constituem o mecanismo que fornece dados constantes a programas de sombreador individuais. Esses dados não podem ser alterados pelos sombreadores.

Definir um uniforme costuma envolver o fornecimento de um dos métodos glUniform\* com a localização de carregamento na GPU junto com um ponteiro para os dados na memória de aplicativo. Depois que o método ithe glUniform\* é executado, os dados de uniforme ficam na memória da GPU acessíveis aos sombreadores que declararam tal uniforme. Você deve garantir o empacotamento dos dados de modo que o sombreador possa interpretá-los com base na declaração do uniforme no sombreador (usando tipos compatíveis).

OpenGL ES 2.0: criando um uniforme e carregando dados nele

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

No GLSL de um sombreador, a declaração de uniforme correspondente é semelhante à seguinte:

Open GL ES 2.0: declaração de uniforme GLSL

``` syntax
uniform mat4 u_mvpMatrix;
```

O Direct3D designa dados de uniforme como "buffers constantes", que, como uniformes, contêm dados constantes fornecidos a sombreadores individuais. Assim como em buffers de uniforme, é importante empacotar os dados de buffer constante na memória da mesma forma que o sombreador espera interpretá-los. Usar tipos DirectXMath (como [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)) em vez de tipos de plataforma (como **float\*** ou **float\[4\]**) garante alinhamento de elementos de dados adequado.

Buffers constantes devem ter um registro de GPU associado usado para referenciar aqueles dados à GPU. Os dados são empacotados no local do Registro, conforme indicado pelo layout do buffer.

Direct3D 11: criando um buffer constante e carregando dados nele

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

No HLSL de um sombreador, a declaração de buffer constante correspondente é semelhante à seguinte:

Direct3D 11: declaração HLSL de buffer constante

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Lembre-se de que deve haver a declaração de um Registro para cada buffer constante. Níveis diferentes de recursos Direct3D têm um número máximo de Registros disponíveis distinto. Por isso, não exceda o número do nível de recursos mais baixo usado.

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>Fazer a portabilidade de atributos de vértice para layouts de uma entrada Direct3D e semântica HLSL


Como os dados de vértice podem ser modificados pelo pipeline de sombreador, o OpenGL ES 2.0 requer a especificação deles como "atributos" em vez de "uniformes" (Isso mudou em versões mais recentes do OpenGL e GLSL.) Dados específicos de vértice, como a posição do vértice, normais, tangentes e valores de cor, são fornecidos aos sombreadores como valores de atributo. Esses valores correspondem a deslocamentos específicos para cada elemento nos dados de vértice; por exemplo, o primeiro atributo pode apontar para o componente de posição de um vértice individual, o segundo para a normal, e assim por diante.

O processo básico de transferência de dados de buffer de vértice da memória principal para a GPU é mais ou menos assim:

-   Carregue os dados de vértice com glBindBuffer.
-   Obtenha a localização dos atributos na GPU com glGetAttribLocation. Chame-o para cada atributo no elemento de dados de vértice.
-   Chame glVertexAttribPointer para definir o tamanho e o deslocamento corretos do atributo dentro de um elemento de dados de vértice individual. Faça isso para todos os atributos.
-   Habilite as informações de layout de dados de vértice com glEnableVertexAttribArray.

OpenGL ES 2.0: carregando dados de buffer de vértice no atributo do sombreador

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

Agora, no sombreador de vértice, declare atributos com os mesmos nomes definidos na chamada a glGetAttribLocation.

OpenGL ES 2.0: declarando um atributo no GLSL

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

Em certos aspectos, o mesmo processo é válido para o Direct3D. Em vez de atributos, os dados de vértice são fornecidos em buffers de entrada, que incluem buffers de vértice e os buffers de índice correspondentes. Mas como o Direct3D não tem declaração de "atributos", você deve especificar um layout de entrada que declare o componente individual dos elementos de dados no buffer de vértice e a semântica HLSL que indica onde e como esses componentes devem ser interpretados pelo sombreador de vértice. A semântica HLSL requer a definição de uso de cada componente com uma cadeia específica, que informa sua finalidade ao mecanismo do sombreador. Por exemplo, os dados de posição do vértice são marcados como POSITION; os dados normais são marcados como NORMAL; e os dados de cores do vértice são marcados como COLOR (Outros estágios de sombreador também exigem semântica específica, e essa semântica tem interpretações diferentes com base no estágio de sombreador.) Para saber mais sobre a semântica HLSL, leia [Compatibilizar seu pipeline de sombreador](change-your-shader-loading-code.md) e [HLSL Semantics](https://msdn.microsoft.com/library/windows/desktop/bb205574).

De forma conjunta, o processo de definir os buffers de vértice e índice e o layout de entrada é chamado de estágio IA (assembly de entrada) do pipeline gráfico do Direct3D.

Direct3D 11: configurando o estágio de assembly de entrada.

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Um layout de entrada é declarado e associado a um sombreador de vértice declarando o formato do elemento dos dados de vértice e a semântica usada para cada componente. O layout dos dados de elemento de vértice descrito no D3D11\_INPUT\_ELEMENT\_DESC que você criar deve corresponder ao layout da estrutura correspondente. Neste ponto, você deve criar um layout para dados de vértice com dois componentes:

-   Uma coordenada de posição do vértice, representada na memória principal por XMFLOAT3, que é uma matriz alinhada de três valores de ponto flutuante de 32 bits para as coordenadas (x, y, z).
-   Um valor de cor do vértice, representado por XMFLOAT4, que é uma matriz alinhada de quatro valores de ponto flutuante de 32 bits para a cor (RGBA).

Você atribui uma semântica para cada um, assim como um tipo de formato. Em seguida, você passa a descrição para [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512). O layout de entrada é usado quando chamamos [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) quando se define o assembly de saída durante nosso método de renderização.

Direct3D 11: descrevendo um layout de entrada com semântica específica

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Por fim, confira se o sombreador pode interpretar os dados de entrada, declarando-a. A semântica atribuída no layout é usada para selecionar os locais corretos na memória da GPU.

Direct3D 11: declarando dados de entrada do sombreador com semântica HLSL

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




