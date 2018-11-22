---
author: mtoepke
title: Aplicar texturas a primitivas
description: Aqui, carregamos dados de textura bruta e os aplicamos em um primitivo 3D usando o cubo que criamos em Usando efeitos e profundidade em primitivas.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, texturas, directx
ms.localizationpriority: medium
ms.openlocfilehash: 252613bbea7f4cdb720758d3435cf0920dd93efa
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/22/2018
ms.locfileid: "7577740"
---
# <a name="apply-textures-to-primitives"></a>Aplicar texturas a primitivas



Aqui, carregamos dados de textura bruta e os aplicamos em um primitivo 3D usando o cubo que criamos em [Using depth and effects on primitives](using-depth-and-effects-on-primitives.md). Também introduzimos um modelo de iluminação de produto escalar simples, em que as superfícies do cubo são mais claras ou escuras de acordo com a distância e o ângulo relativo delas para uma fonte de luz.

**Objetivo:** aplicar texturas a primitivas.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

Também partimos do princípio de que você passou pelos artigos [Quickstart: setting up DirectX resources and displaying an image](setting-up-directx-resources.md), [Creating shaders and drawing primitives](creating-shaders-and-drawing-primitives.md) e [Using depth and effects on primitives](using-depth-and-effects-on-primitives.md).

**Tempo para concluir:** 20 minutos.

<a name="instructions"></a>Instruções
------------

### <a name="1-defining-variables-for-a-textured-cube"></a>1. Definindo variáveis para um cubo texturizado

Primeiro, precisamos definir as estruturas **BasicVertex** e **ConstantBuffer** para o cubo texturizado. Essas estruturas especificam as posições, as orientações e as texturas de vértice para o cubo e como ele será exibido. Caso contrário, declaramos as variáveis similarmente ao tutorial anterior, [Using depth and effects on primitives](using-depth-and-effects-on-primitives.md).

```cpp
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. Criando sombreadores de vértice e pixel com elementos de superfície e textura

Aqui, criamos sombreadores de vértice e pixel mais complexos do que no tutorial anterior, [Using depth and effects on primitives](using-depth-and-effects-on-primitives.md). O sombreador de vértice desse aplicativo transforma cada posição de vértice em espaço de projeção e as coordenadas de textura de vértice para o sombreador de pixel.

A matriz de estruturas [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) do aplicativo que descrevem o layout do código do sombreador de vértice tem três elementos: um define a posição de vértice, outro define o vetor normal de superfície (a direção para que a superfície fica normalmente voltada) e o terceiro elemento define as coordenadas de textura.

Criamos índice de vértice e buffers constantes que definem um cubo texturizado em órbita.

**Para definir um cubo texturizado em órbita**

1.  Primeiro, definimos o cubo. Para cada vértice é atribuída uma posição, um vetor normal de superfície e coordenas de textura. Usamos vários vértices para cada canto, para permitir diferentes vetores normais e coordenadas de textura para serem definidas para cada face.
2.  Em seguida, descrevemos os buffers de vértice e índice ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) e [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) usando a definição do cubo. Chamamos [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) uma vez para cada buffer.
3.  Em seguida, criamos um buffer constante ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) para passar matrizes de modelo, exibição e projeção para o sombreador de vértice. Mais tarde, podemos usar o buffer constante para girar o cubo e aplicar uma projeção de perspectiva a ele. Chamamos [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) para criar o buffer constante.
4.  Finalmente, especificamos a transformação de exibição que corresponde à posição de câmera de X = 0, Y = 1, Z = 2.

```cpp
        
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
          
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {  

          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // These correspond to the elements of the BasicVertex struct defined above.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {        

          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });
        
        
        // Create vertex and index buffers that define a simple unit cube.
        auto createCubeTask = (createPSTask && createVSTask).then([this] () {
        
          // In the array below, which will be used to initialize the cube vertex buffers,
          // multiple vertices are used for each corner to allow different normal vectors and
          // texture coordinates to be defined for each face.
          BasicVertex cubeVertices[] =
          {
              { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
              { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
              { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
              { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
              { DirectX::XMFLOAT3( 0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

              { DirectX::XMFLOAT3( 0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
              { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
              { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
              { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              8, 9, 10,
              8, 10, 11,

              12, 13, 14,
              12, 14, 15,

              16, 17, 18,
              16, 18, 19,

              20, 21, 22,
              20, 22, 23
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = cubeVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = cubeIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );


          // Create a constant buffer for passing model, view, and projection matrices
          // to the vertex shader.  This will allow us to rotate the cube and apply
          // a perspective projection to it.

          D3D11_BUFFER_DESC constantBufferDesc = {0};
          constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
          constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
          constantBufferDesc.CPUAccessFlags = 0;
          constantBufferDesc.MiscFlags = 0;
          constantBufferDesc.StructureByteStride = 0;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &constantBufferDesc,
                  nullptr,
                  &m_constantBuffer
                  )
              );

          // Specify the view transform corresponding to a camera position of
          // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

          m_constantBufferData.view = DirectX::XMFLOAT4X4(
              -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
               0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
               0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
               0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f
              );
       });
```

### <a name="3-creating-textures-and-samplers"></a>3. Criando texturas e amostras

Aqui, aplicamos dados de textura a um cubo, em vez de aplicar cores como no tutorial anterior, [Using depth and effects on primitives](using-depth-and-effects-on-primitives.md).

Usamos dados de textura brutos para criar texturas.

**Para criar texturas e amostras**

1.  Primeiro, lemos dados de textura brutos do arquivo exturedata.bin no disco.
2.  Em seguida, construímos uma estrutura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) que faz referência a esses dados de textura brutos.
3.  Depois, populamos uma estrutura [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) para descrever a textura. Passamos, então, as estruturas [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) e **D3D11\_TEXTURE2D\_DESC** em uma chamada para [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521), para criar a textura.
4.  Em seguida, criamos uma exibição de recurso de sombreador da textura, para que sombreadores possam usar a textura. Para criar a exibição de recurso de sombreador, populamos um [**D3D11\_SHADER\_RESOURCE\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476211) para descrever a exibição de recurso de sombreador e passar essa descrição e a textura para [**ID3D11Device::CreateShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476519). Em geral, a descrição de exibição corresponde com a descrição de textura.
5.  Em seguida, criamos o estado de amostra para a textura. Esse estado de amostra usa os dados de textura relevantes para definir como a cor para uma determinada coordenada de textura é determinada. Populamos uma estrutura [**D3D11\_SAMPLER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476207) para descrever o estado de amostra. Passamos, então, a estrutura **D3D11\_SAMPLER\_DESC** e uma chamada para [**ID3D11Device::CreateSamplerState**](https://msdn.microsoft.com/library/windows/desktop/ff476518), para criar o estado de amostra.
6.  Finalmente, declaramos uma variável de *grau* que usaremos para animar o cubo, girando cada quadro dele.

```cpp
        
        // Load the raw texture data from disk and construct a subresource description that references it.
        auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");
          
        auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& vertexShaderBytecode) {  
        
          D3D11_SUBRESOURCE_DATA textureSubresourceData = {0};
          textureSubresourceData.pSysMem = textureData->Data;

          // Specify the size of a row in bytes, known as a priori about the texture data.
          textureSubresourceData.SysMemPitch = 1024;

          // As this is not a texture array or 3D texture, this parameter is ignored.
          textureSubresourceData.SysMemSlicePitch = 0;

          // Create a texture description from information known as a priori about the data.
          // Generalized texture loading code can be found in the Resource Loading sample.
          D3D11_TEXTURE2D_DESC textureDesc = {0};
          textureDesc.Width = 256;
          textureDesc.Height = 256;
          textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
          textureDesc.Usage = D3D11_USAGE_DEFAULT;
          textureDesc.CPUAccessFlags = 0;
          textureDesc.MiscFlags = 0;

          // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
          textureDesc.MipLevels = 1;

          // As this will not be a texture array, this parameter is ignored.
          textureDesc.ArraySize = 1;

          // Don't use multi-sampling.
          textureDesc.SampleDesc.Count = 1;
          textureDesc.SampleDesc.Quality = 0;

          // Allow the texture to be bound as a shader resource.
          textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

          ComPtr<ID3D11Texture2D> texture;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateTexture2D(
                  &textureDesc,
                  &textureSubresourceData,
                  &texture
                  )
              );

          // Once the texture is created, we must create a shader resource view of it
          // so that shaders may use it.  In general, the view description will match
          // the texture description.
          D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
          ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
          textureViewDesc.Format = textureDesc.Format;
          textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
          textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
          textureViewDesc.Texture2D.MostDetailedMip = 0;

          ComPtr<ID3D11ShaderResourceView> textureView;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateShaderResourceView(
                  texture.Get(),
                  &textureViewDesc,
                  &textureView
                  )
              );

          // Once the texture view is created, create a sampler.  This defines how the color
          // for a particular texture coordinate is determined using the relevant texture data.
          D3D11_SAMPLER_DESC samplerDesc;
          ZeroMemory(&samplerDesc, sizeof(samplerDesc));

          samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

          // The sampler does not use anisotropic filtering, so this parameter is ignored.
          samplerDesc.MaxAnisotropy = 0;

          // Specify how texture coordinates outside of the range 0..1 are resolved.
          samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
          samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

          // Use no special MIP clamping or bias.
          samplerDesc.MipLODBias = 0.0f;
          samplerDesc.MinLOD = 0;
          samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

          // Don't use a comparison function.
          samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

          // Border address mode is not used, so this parameter is ignored.
          samplerDesc.BorderColor[0] = 0.0f;
          samplerDesc.BorderColor[1] = 0.0f;
          samplerDesc.BorderColor[2] = 0.0f;
          samplerDesc.BorderColor[3] = 0.0f;

          ComPtr<ID3D11SamplerState> sampler;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateSamplerState(
                  &samplerDesc,
                  &sampler
                  )
              );
        });

        // This value will be used to animate the cube by rotating it every frame;
        float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. Girando e desenhando o cubo texturizado e apresentando a imagem renderizada

Como nos tutoriais anteriores, inserimos um loop infinito para renderizar e exibir continuamente a cena. Chamamos a função em linha (BasicMath.h) **rotationY** com uma quantidade de rotação para definir valores que vão girar a matriz de modelo do cubo em torno do eixo Y. Em seguida, chamamos [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) para atualizar o buffer constante e girar o modelo de cubo. Depois, chamamos [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) para especificar o destino de renderização e a exibição de profundidade e estêncil. Chamamos [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) para limpar o destino de renderização para uma cor azul sólida e chamamos [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) para limpar o buffer de profundidade.

No loop infinito, também desenhamos o cubo texturizado na superfície azul.

**Para desenhar o cubo texturizado**

1.  Primeiro, chamamos [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) para descrever como os dados de buffer de vértices são transmitidos para o estágio de assembler de entrada.
2.  Em seguida, chamamos [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) e [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) para associar os buffers de vértice e índice para o estágio de assembler de entrada.
3.  Depois, chamamos [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) com o valor [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) especificando para o estágio de assembler de entrada interpretar os dados de vértice como uma faixa de triângulos.
4.  Chamamos [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) para iniciar o estágio de sombreador de vértice com o código de sombreador de vértice e [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para iniciar o estágio de sombreador de pixel com o código de sombreador de pixel.
5.  Chamamos [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) para definir o buffer constante que é usado pelo estágio de pipeline do sombreador de vértice.
6.  Chamamos [**PSSetShaderResources**](https://msdn.microsoft.com/library/windows/desktop/ff476473) para associar a exibição de recurso de sombreador da textura ao estágio de pipeline do sombreador de pixel.
7.  Chamamos [**PSSetSamplers**](https://msdn.microsoft.com/library/windows/desktop/ff476471) para definir o estágio de amostra para o estágio de pipeline do sombreador de pixel.
8.  Finalmente, chamamos [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) para desenhar o cubo e enviá-lo para a pipeline de renderização.

Como nos tutoriais anteriores, chamamos [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) para apresentar a imagem renderizada à janela.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
            degree += 1.0f;

            m_d3dDeviceContext->UpdateSubresource(
                m_constantBuffer.Get(),
                0,
                nullptr,
                &m_constantBufferData,
                0,
                0
                );

            // Specify the render target and depth stencil we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                m_depthStencilView.Get()
                );

            // Clear the render target to a solid color, and reset the depth stencil.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->ClearDepthStencilView(
                m_depthStencilView.Get(),
                D3D11_CLEAR_DEPTH,
                1.0f,
                0
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(BasicVertex);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShaderResources(
                0,
                1,
                textureView.GetAddressOf()
                );

            m_d3dDeviceContext->PSSetSamplers(
                0,
                1,
                sampler.GetAddressOf()
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(cubeIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary"></a>Resumo


Carregamos dados de textura bruta e os aplicamos a um primitivo 3D.

 

 




