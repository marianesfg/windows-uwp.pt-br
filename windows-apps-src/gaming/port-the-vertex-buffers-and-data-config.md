---
title: Realizar a portabilidade de dados e buffers de vértice
description: Nesta etapa, você definirá os buffers de vértices que conterão suas malhas e os buffers de índice que permitem que os sombreadores percorram os vértices em uma ordem específica.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, porta, buffers de vértice, dados, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 8445339d442fb740e9e2aba5e9d1cb0388c746ef
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368238"
---
# <a name="port-the-vertex-buffers-and-data"></a>Realizar a portabilidade de dados e buffers de vértice




**APIs importantes**

-   [**ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer)

Nesta etapa, você definirá os buffers de vértices que conterão suas malhas e os buffers de índice que permitem que os sombreadores percorram os vértices em uma ordem específica.

Neste ponto, vamos examinar o modelo com código fixo para a malha de cubo que estamos usando. Ambas as representações têm vértices organizados como uma lista de triângulos (em oposição a uma faixa ou outro layout de triângulos mais eficiente). Todos os vértices das duas representações também têm índices e valores de cor associados. Grande parte do código em Direct3D neste tópico refere-se a variáveis e objetos definidos no projeto Direct3D.

Consulte o cubo para processamento pelo OpenGL ES 2.0. Na implementação de exemplo, cada vértice é 7 valores de float: coordenadas de posição 3 seguidas de 4 valores de cor RGBA.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

E aqui está o mesmo cubo para processamento pelo Direct3D 11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Revisando este código, você perceberá que o cubo no código OpenGL ES 2.0 é representado em um sistema de coordenadas de mão direita, enquanto o cubo no código específico ao Direct3D é representado em um sistema de coordenadas de mão esquerda. Ao importar seus próprios dados de malha, você deverá reverter as coordenadas do eixo z do modelo e alterar os índices de cada malha do modo adequado para percorres os triângulos de acordo com a alteração no sistema de coordenadas.

Supondo que tenhamos movido a malha de cubo do sistema de coordenadas destro do OpenGL ES 2.0 para o canhoto do Direct3D de forma bem-sucedida, vejamos como carregar os dados de cubo para o processamento nos dois modelos.

## <a name="instructions"></a>Instruções

### <a name="step-1-create-an-input-layout"></a>Etapa 1: Criar um layout de entrada

No OpenGL ES 2.0, seus dados de vértice são fornecidos como atributos que serão fornecidos e lidos pelos objetos de sombreador. Geralmente, você deve fornecer uma cadeia que contenha o nome do atributo usado no GLSL do sombreador para o objeto do programa de sombreador; depois, obter um local de memória que possa ser fornecido ao sombreador. Neste exemplo, um objeto de buffer de vértice contém uma lista de estruturas de vértice personalizadas, definidas e formatadas da seguinte maneira:

OpenGL ES 2.0: Configure os atributos que contêm as informações por vértice.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

Em OpenGL ES 2.0, layouts de entrada são implícitos; você levar a uma finalidade geral GL\_elemento\_matriz\_BUFFER e fornecer o stride e deslocamento, de modo que o sombreador de vértices pode interpretar os dados depois de carregá-lo. Você informa ao sombreador antes de renderizar que atributos mapeiam que partes de cada bloco de dados de vértice com **glVertexAttribPointer**.

No Direct3D, você deve fornecer um layout de entrada para descrever a estrutura dos dados de vértice no buffer de vértices quando cria o buffer, em vez de antes de desenhar a geometria. Para isso, se usa um layout de entrada que corresponde ao layout dos dados dos nossos vértices individuais na memória. É muito importante especificar de forma precisa!

Aqui, você pode criar uma descrição de entrada como uma matriz de [ **D3D11\_entrada\_elemento\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) estruturas.

Direct3D: Defina uma descrição de layout de entrada.

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Esta descrição de entrada define um vértice como um par de dois vetores com três coordenadas: um vetor 3D para armazenar a posição do vértice em coordenadas de modelo e outro vetor 3D para armazenar o valor de cor RGB associado ao vértice. Neste caso, use o formato de três pontos flutuantes de 32 bits, elementos representados em código como `XMFLOAT3(X.Xf, X.Xf, X.Xf)`. Você deve usar tipos da livraria [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-reference) sempre que for manipular dados que serão usados por um sombreador, pois isso assegura o compactação e o alinhamento adequados desses dados. (Por exemplo, use [**XMFLOAT3**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat3) ou [**XMFLOAT4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4) para dados de vetor e [**XMFLOAT4X4**](https://docs.microsoft.com/windows/desktop/api/directxmath/ns-directxmath-xmfloat4x4) para matrizes).

Para obter uma lista de todos os tipos possíveis de formato, consulte [ **DXGI\_formato**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format).

Como o layout de entrada por vértice definido, você cria o objeto de layout. No código a seguir, você escrevê-lo para **m\_inputLayout**, uma variável do tipo **ComPtr** (que aponta para um objeto do tipo [ **ID3D11InputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11inputlayout)). **fileData** contém o objeto de sombreador de vértice compilado da etapa anterior, [Compatibilizar os sombreadores](port-the-shader-config.md).

Direct3D: Crie o layout de entrada usado pelo buffer de vértice.

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Definimos o layout de entrada. Agora, vamos criar um buffer que usa esse layout e o carrega com os dados de malha de cubo.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Etapa 2: Criar e carregar o buffer de vértice (s)

No OpenGL ES 2.0, você cria um par de buffers, um para os dados de posição e um para os dados de cor. (Você também pode criar uma estrutura que contém ambos e um único buffer.) Associar cada buffer e gravar dados de cor e posição nelas. Depois, durante a função de renderização, vincule os buffers novamente e forneça ao sombreador o formato dos dados no buffer, para que ele possa interpretá-los corretamente.

OpenGL ES 2.0: Associar os buffers de vértice

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

No Direct3D, buffers de sombreador acessíveis são representadas como [ **D3D11\_SUBRESOURCE\_DATA** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) estruturas. Para associar o local desse buffer ao objeto de sombreador, você precisará criar um CD3D11\_BUFFER\_estrutura DESC para cada buffer com [ **ID3DDevice::CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer)e, em seguida, defina o buffer de contexto do dispositivo Direct3D chamando um método set específico para o tipo de buffer, como [ **ID3DDeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers).

Quando você definir o buffer, precisará definir o passo (o tamanho do elemento de dados de um vértice individual) e o deslocamento (onde a matriz de dados de vértice de fato começa) do começo do buffer.

Observe que podemos atribuir o ponteiro para o **vertexIndices** de matriz para o **pSysMem** campo dos [ **D3D11\_SUBRESOURCE\_dados** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) estrutura. Se isso não estiver certo, sua malha estará corrompida ou vazia!

Direct3D: Crie e defina o buffer de vértice

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>Etapa 3: Criar e carregar o buffer de índices

Buffers de índice são uma forma eficiente de permitir que o sombreador de vértice procure vértices individuais. Embora não sejam necessários, nós os usamos no exemplo de renderizador. Assim como os buffers de vértice no OpenGL ES 2.0, um buffer de índice é criado e vinculado como buffer para fins gerais; além disso, os índices de vértice criados anteriormente são acoplados a ele.

Quando você estiver pronto para desenhar, associe tanto o vértice quando o buffer de índice novamente e chame **glDrawElements**.

OpenGL ES 2.0: Envie a ordem de índice para a chamada de desenho.

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Com o Direct3D, é um processo bem parecido, embora um pouco mais didático. Forneça o buffer de índice como um sub-recurso Direct3D ao [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) que você criou ao configurar o Direct3D. Você faz isso chamando [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d10/nf-d3d10-id3d10device-iasetindexbuffer) com o sub-recurso configurado para a matriz de índice, como a seguir. (Novamente, observe que você atribua o ponteiro para o **cubeIndices** de matriz para o **pSysMem** campo dos [ **D3D11\_SUBRESOURCE\_dados** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) estrutura.)

Direct3D: Crie o buffer de índices.

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

Depois, você desenhará triângulos com uma chamada para [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (ou [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) para vértices não indexados), como a seguir. (Para saber mais, vá direto para [Desenhar na tela](draw-to-the-screen.md).)

Direct3D: Desenhe os vértices indexados.

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>Etapa anterior


[Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)

## <a name="next-step"></a>Próximas etapas

[Fazer a portabilidade do GLSL](port-the-glsl.md)

## <a name="remarks"></a>Comentários

Ao estruturar o seu Direct3D, separe o código que chama métodos em [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) em um método que é chamado sempre os recursos do dispositivo precisam ser recriados. (No modelo de projeto do Direct3D, esse código está nos métodos **CreateDeviceResource** do objeto de renderização. O código que atualiza o contexto de dispositivo ([**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)), por outro lado, é colocado no método **Render**, já que é onde se cria, de fato, os estágios de sombreador e se associa os dados.

## <a name="related-topics"></a>Tópicos relacionados


* [Como: um renderizador simple do OpenGL ES 2.0 ao Direct3D 11 da porta](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)
* [Realizar a portabilidade de dados e buffers de vértice](port-the-vertex-buffers-and-data-config.md)
* [Fazer a portabilidade do GLSL](port-the-glsl.md)

 

 




