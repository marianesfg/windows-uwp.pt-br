---
title: Fazer a portabilidade de dados e buffers de vértices
description: Nesta etapa, você definirá os buffers de vértices que conterão suas malhas e os buffers de índice que permitem que os sombreadores percorram os vértices em uma ordem específica.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, porta, buffers de vértice, dados, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 4c961a8852fb1e03e4e86209f62bda821b980f8c
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8735226"
---
# <a name="port-the-vertex-buffers-and-data"></a>Fazer a portabilidade de dados e buffers de vértice




**APIs Importantes**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

Nesta etapa, você definirá os buffers de vértices que conterão suas malhas e os buffers de índice que permitem que os sombreadores percorram os vértices em uma ordem específica.

Neste ponto, vamos examinar o modelo com código fixo para a malha de cubo que estamos usando. Ambas as representações têm vértices organizados como uma lista de triângulos (em oposição a uma faixa ou outro layout de triângulos mais eficiente). Todos os vértices das duas representações também têm índices e valores de cor associados. Grande parte do código em Direct3D neste tópico refere-se a variáveis e objetos definidos no projeto Direct3D.

Consulte o cubo para processamento pelo OpenGL ES 2.0. No exemplo de implementação, cada vértice tem sete valores flutuantes: três coordenadas de posição seguidas de quatro valores de cor RGBA.

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

### <a name="step-1-create-an-input-layout"></a>Etapa 1: criar uma camada de entrada

No OpenGL ES 2.0, seus dados de vértice são fornecidos como atributos que serão fornecidos e lidos pelos objetos de sombreador. Geralmente, você deve fornecer uma cadeia que contenha o nome do atributo usado no GLSL do sombreador para o objeto do programa de sombreador; depois, obter um local de memória que possa ser fornecido ao sombreador. Neste exemplo, um objeto de buffer de vértice contém uma lista de estruturas de vértice personalizadas, definidas e formatadas da seguinte maneira:

OpenGL ES 2.0: configurar os atributos que contêm informações por vértice.

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

No OpenGL ES 2.0, camadas de entrada ficam implícitas; você pega um propósito geral GL\_ELEMENT\_ARRAY\_BUFFER e fornece o passo e o deslocamento de forma que o sombreador de vértice possa interpretar os dados após carregá-lo. Você informa ao sombreador antes de renderizar que atributos mapeiam que partes de cada bloco de dados de vértice com **glVertexAttribPointer**.

No Direct3D, você deve fornecer um layout de entrada para descrever a estrutura dos dados de vértice no buffer de vértices quando cria o buffer, em vez de antes de desenhar a geometria. Para isso, se usa um layout de entrada que corresponde ao layout dos dados dos nossos vértices individuais na memória. É muito importante especificar de forma precisa!

Aqui, você cria uma descrição de entrada como uma matriz de estruturas [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180).

Direct3D: defina uma descrição de layout de entrada.

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

Esta descrição de entrada define um vértice como um par de dois vetores com três coordenadas: um vetor 3D para armazenar a posição do vértice em coordenadas de modelo e outro vetor 3D para armazenar o valor de cor RGB associado ao vértice. Neste caso, use o formato de três pontos flutuantes de 32 bits, elementos representados em código como `XMFLOAT3(X.Xf, X.Xf, X.Xf)`. Você deve usar tipos da livraria [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574) sempre que for manipular dados que serão usados por um sombreador, pois isso assegura o compactação e o alinhamento adequados desses dados. (Por exemplo, use [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) ou [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) para dados de vetor e [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) para matrizes).

Para uma lista de todos os tipos de formatos possíveis, consulte [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

Como o layout de entrada por vértice definido, você cria o objeto de layout. No código a seguir, você escreve isso para **m\_inputLayout**, uma variável de tipo **ComPtr** (que aponta para um objeto de tipo [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)). **fileData** contém o objeto de sombreador de vértice compilado da etapa anterior, [Compatibilizar os sombreadores](port-the-shader-config.md).

Direct3D: criar o layout de entrada usado pelo buffer de vértices.

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

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Etapa 2: criar e carregar o(s) buffer(s) de vértices

No OpenGL ES 2.0, você cria um par de buffers, um para os dados de posição e um para os dados de cor. (Você também poderia criar uma estrutura que contém ambos e um único buffer.) Você associa cada buffer e gravar dados de cor e de posição neles. Depois, durante a função de renderização, vincule os buffers novamente e forneça ao sombreador o formato dos dados no buffer, para que ele possa interpretá-los corretamente.

OpenGL ES 2.0: vincular os buffers de vértice

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

No Direct3D, buffers acessíveis a sombreadores são representados como estruturas [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220). Para associar a localização desse buffer ao objeto de sombreador, você precisa criar uma estrutura CD3D11\_BUFFER\_DESC para cada buffer com [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)e, em seguida, definir o buffer do contexto de dispositivo do Direct3D chamando um método definido específico para o tipo de buffer, como [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456).

Quando você definir o buffer, precisará definir o passo (o tamanho do elemento de dados de um vértice individual) e o deslocamento (onde a matriz de dados de vértice de fato começa) do começo do buffer.

Perceba que atribuímos o ponteiro da matriz **vertexIndices** para o campo **pSysMem** da estrutura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220). Se isso não estiver certo, sua malha estará corrompida ou vazia!

Direct3D: criar e definir o buffer de vértices

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

### <a name="step-3-create-and-load-the-index-buffer"></a>Etapa 3: criar e carregar o buffer de índice

Buffers de índice são uma forma eficiente de permitir que o sombreador de vértice procure vértices individuais. Embora não sejam necessários, nós os usamos no exemplo de renderizador. Assim como os buffers de vértice no OpenGL ES 2.0, um buffer de índice é criado e vinculado como buffer para fins gerais; além disso, os índices de vértice criados anteriormente são acoplados a ele.

Quando você estiver pronto para desenhar, associe tanto o vértice quando o buffer de índice novamente e chame **glDrawElements**.

Abra o OpenGL ES 2.0: enviar a ordem de índice para a chamada de desenho.

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

Com o Direct3D, é um processo bem parecido, embora um pouco mais didático. Forneça o buffer de índice como um sub-recurso Direct3D ao [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) que você criou ao configurar o Direct3D. Você faz isso chamando [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588) com o sub-recurso configurado para a matriz de índice, como a seguir. (Novamente, observe que você atribui o ponteiro da matriz **cubeIndices** para o campo **pSysMem** da estrutura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220).)

Direct3D: criar e o buffer de índice.

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

Depois, você desenhará triângulos com uma chamada para [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (ou [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) para vértices não indexados), como a seguir. (Para saber mais, vá direto para [Desenhar na tela](draw-to-the-screen.md).)

Direct3D: desenhar os vértices indexados.

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

## <a name="next-step"></a>Próxima etapa

[Fazer a portabilidade do GLSL](port-the-glsl.md)

## <a name="remarks"></a>Comentários

Ao estruturar o seu Direct3D, separe o código que chama métodos em [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) em um método que é chamado sempre os recursos do dispositivo precisam ser recriados. (No modelo de projeto do Direct3D, esse código está nos métodos **CreateDeviceResource** do objeto de renderização. O código que atualiza o contexto de dispositivo ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)), por outro lado, é colocado no método **Render**, já que é onde se cria, de fato, os estágios de sombreador e se associa os dados.

## <a name="related-topics"></a>Tópicos relacionados


* [Como: compatibilizar um renderizador simples do OpenGL ES 2.0 ao Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Fazer a portabilidade de objetos de sombreador](port-the-shader-config.md)
* [Fazer a portabilidade de dados e buffers de vértices](port-the-vertex-buffers-and-data-config.md)
* [Fazer a portabilidade do GLSL](port-the-glsl.md)

 

 




