---
author: mtoepke
title: Usar efeitos e profundidade em primitivas
description: Aqui mostramos a você como usar profundidade, perspectiva, cor e outros efeitos em primitivas.
ms.assetid: 71ef34c5-b4a3-adae-5266-f86ba257482a
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, profundidade, efeitos, primitivos, directx
ms.localizationpriority: medium
ms.openlocfilehash: f81c441910cd0d0205641a119c243cb22d0b695e
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7558315"
---
# <a name="use-depth-and-effects-on-primitives"></a>Usar efeitos e profundidade em primitivas



Aqui mostramos a você como usar profundidade, perspectiva, cor e outros efeitos em primitivas.

**Objetivo:** criar um objeto 3D e aplicar iluminação de vértice básica e coloração a ela.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

Também supomos que você leu [Guia de início rápido: configurando recursos DirectX e exibindo uma imagem](setting-up-directx-resources.md) e [Criando sombreadores e desenhando primitivas](creating-shaders-and-drawing-primitives.md).

**Tempo para concluir:** 20 minutos.

<a name="instructions"></a>Instruções
------------

### <a name="1-defining-cube-variables"></a>1. Definindo variáveis de cubo

Primeiro, precisamos definir as estruturas **SimpleCubeVertex** e **ConstantBuffer** para o cubo. Essas estruturas especificam a posição de vértice e as cores do cubo e como o cubo será exibido. Declaramos [**ID3D11DepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476377) e [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) com [**ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) e declaramos uma instância do **ConstantBuffer**.

```cpp
struct SimpleCubeVertex
{
    DirectX::XMFLOAT3 pos;   // Position
    DirectX::XMFLOAT3 color; // Color
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

### <a name="2-creating-a-depth-stencil-view"></a>2. Criando um modo de exibição de estêncil de profundidade

Além de criar o modo de exibição de destino de renderização, também criamos um modo de exibição de estêncil de profundidade. O modo de exibição de estêncil de profundidade permite que o Direct3D renderiza de forma eficiente os objetos mais próximos à câmera na frente dos objetos mais longe da câmera. Antes de podemos criar um modo de exibição para um buffer de estêncil de profundidade, devemos criar o buffer de estêncil de profundidade. Populamos um [**D3D11\_TEXTURE2D\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476253) para descrever o buffer de estêncil de profundidade e chamamos [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) para criar o buffer de estêncil de profundidade. Para criar o modo de exibição de estêncil de profundidade, populamos uma [**D3D11\_DEPTH\_STENCIL\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476112) para descrever o modo de exibição de estêncil de profundidade e passar a descrição do modo de exibição de estêncil de profundidade e a textura para o [**ID3D11Device::CreateDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476507).

```cpp
        // Once the render target view is created, create a depth stencil view.  This
        // allows Direct3D to efficiently render objects closer to the camera in front
        // of objects further from the camera.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_TEXTURE2D_DESC depthStencilDesc;
        depthStencilDesc.Width = backBufferDesc.Width;
        depthStencilDesc.Height = backBufferDesc.Height;
        depthStencilDesc.MipLevels = 1;
        depthStencilDesc.ArraySize = 1;
        depthStencilDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
        depthStencilDesc.SampleDesc.Count = 1;
        depthStencilDesc.SampleDesc.Quality = 0;
        depthStencilDesc.Usage = D3D11_USAGE_DEFAULT;
        depthStencilDesc.BindFlags = D3D11_BIND_DEPTH_STENCIL;
        depthStencilDesc.CPUAccessFlags = 0;
        depthStencilDesc.MiscFlags = 0;
        ComPtr<ID3D11Texture2D> depthStencil;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateTexture2D(
                &depthStencilDesc,
                nullptr,
                &depthStencil
                )
            );

        D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
        depthStencilViewDesc.Format = depthStencilDesc.Format;
        depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
        depthStencilViewDesc.Flags = 0;
        depthStencilViewDesc.Texture2D.MipSlice = 0;
        DX::ThrowIfFailed(
            m_d3dDevice->CreateDepthStencilView(
                depthStencil.Get(),
                &depthStencilViewDesc,
                &m_depthStencilView
                )
            );
```

### <a name="3-updating-perspective-with-the-window"></a>3. Atualizando perspectiva com a janela

Atualizamos os parâmetros de projeção de perspectiva para o buffer constante, dependendo das dimensões da janela. Podemos corrigir os parâmetros para um campo de visão de 70 graus, com um intervalo de profundidade de 0,01 a 100.

```cpp
        // Finally, update the constant buffer perspective projection parameters
        // to account for the size of the application window.  In this sample,
        // the parameters are fixed to a 70-degree field of view, with a depth
        // range of 0.01 to 100.  For a generalized camera class, see Lesson 5.

        float xScale = 1.42814801f;
        float yScale = 1.42814801f;
        if (backBufferDesc.Width > backBufferDesc.Height)
        {
            xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
        }
        else
        {
            yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
        }

        m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

### <a name="4-creating-vertex-and-pixel-shaders-with-color-elements"></a>4. Criando sombreadores de vértice e pixel com elementos de cor

Neste aplicativo, criamos sombreadores de vértice e pixel mais complexos do que o que descrevemos no tutorial anterior, [Criando sombreadores e desenhando primitivas](creating-shaders-and-drawing-primitives.md). O sombreador de vértice deste aplicativo transforma cada posição de vértice no espaço de projeção e passa a cor de vértice para o sombreador de pixel.

A matriz do aplicativo de estruturas [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) que descrevem o layout do código sombreador do vértice tem dois elementos de layout: um elemento define a posição do vértice e o outro elemento define a cor.

Podemos criar buffers de vértice, índice e constante para definir um cubo em órbita.

**Para definir um cubo em órbita**

1.  Primeiro, definimos o cubo. Atribuímos a cada vértice uma cor, além de uma posição. Isso permite que o sombreador de pixels colora cada face de forma diferente, para que a face possa ser distinguida.
2.  Em seguida, descrevemos os buffers de vértice e índice ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) e [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)) usando a definição do cubo. Chamamos [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) uma vez para cada buffer.
3.  Em seguida, criamos um buffer constante ([**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092)) para passar matrizes de modelo, exibição e projeção para o sombreador de vértice. Mais tarde, podemos usar o buffer constante para girar o cubo e aplicar uma projeção de perspectiva a ele. Chamamos [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) para criar o buffer constante.
4.  Em seguida, podemos especificar a transformação da exibição que corresponde a uma posição de câmera de X = 0, Y = 1, Z = 2.
5.  Finalmente, declaramos uma variável de *grau* que usaremos para animar o cubo, girando cada quadro dele.

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
          // For this lesson, this is simply a DirectX::XMFLOAT3 vector defining the vertex position, and
          // a DirectX::XMFLOAT3 vector defining the vertex color.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
              { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
          // each vertex is assigned a color in addition to a position.  This will allow
          // the pixel shader to color each face differently, enabling them to be distinguished.
          SimpleCubeVertex cubeVertices[] =
          {
              { float3(-0.5f, 0.5f, -0.5f), float3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
              { float3( 0.5f, 0.5f, -0.5f), float3(1.0f, 1.0f, 0.0f) },
              { float3( 0.5f, 0.5f,  0.5f), float3(1.0f, 1.0f, 1.0f) },
              { float3(-0.5f, 0.5f,  0.5f), float3(0.0f, 1.0f, 1.0f) },

              { float3(-0.5f, -0.5f,  0.5f), float3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
              { float3( 0.5f, -0.5f,  0.5f), float3(1.0f, 0.0f, 1.0f) },
              { float3( 0.5f, -0.5f, -0.5f), float3(1.0f, 0.0f, 0.0f) },
              { float3(-0.5f, -0.5f, -0.5f), float3(0.0f, 0.0f, 0.0f) },
          };

          unsigned short cubeIndices[] =
          {
              0, 1, 2,
              0, 2, 3,

              4, 5, 6,
              4, 6, 7,

              3, 2, 5,
              3, 5, 4,

              2, 1, 6,
              2, 6, 5,

              1, 7, 6,
              1, 0, 7,

              0, 3, 4,
              0, 4, 7
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
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
        
        // This value will be used to animate the cube by rotating it every frame.
        float degree = 0.0f;
        
```

### <a name="5-rotating-and-drawing-the-cube-and-presenting-the-rendered-image"></a>5. Girando e desenhando o cubo e apresentando a imagem renderizada

Entramos em um loop infinito para processar e exibir a cena continuamente. Chamamos a função em linha (BasicMath.h) **rotationY** com uma quantidade de rotação para definir valores que vão girar a matriz de modelo do cubo em torno do eixo Y. Em seguida, chamamos [**ID3D11DeviceContext::UpdateSubresource**](https://msdn.microsoft.com/library/windows/desktop/ff476486) para atualizar o buffer constante e girar o modelo de cubo. Chamamos [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) para especificar o destino de renderização como o destino de saída. Nesta chamada a **OMSetRenderTargets**, passamos o modo de exibição de estêncil de profundidade. Chamamos [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) para limpar o destino de renderização para uma cor azul sólida e chamamos [**ID3D11DeviceContext::ClearDepthStencilView**](https://msdn.microsoft.com/library/windows/desktop/ff476387) para limpar o buffer de profundidade.

No loop sem fim, também desenhamos o cubo na superfície azul.

**Para desenhar o cubo**

1.  Primeiro, chamamos [**ID3D11DeviceContext::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) para descrever como os dados de buffer de vértices são transmitidos para o estágio de assembler de entrada.
2.  Em seguida, chamamos [**ID3D11DeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456) e [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476453) para associar os buffers de vértice e índice para o estágio de assembler de entrada.
3.  Depois, chamamos [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455) com o valor [**D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLESTRIP**](https://msdn.microsoft.com/library/windows/desktop/ff476189#D3D11_PRIMITIVE_TOPOLOGY_TRIANGLESTRIP) especificando para o estágio de assembler de entrada interpretar os dados de vértice como uma faixa de triângulos.
4.  Chamamos [**ID3D11DeviceContext::VSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476493) para iniciar o estágio de sombreador de vértice com o código de sombreador de vértice e [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472) para iniciar o estágio de sombreador de pixel com o código de sombreador de pixel.
5.  Chamamos [**ID3D11DeviceContext::VSSetConstantBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476491) para definir o buffer constante que é usado pelo estágio de pipeline do sombreador de vértice.
6.  Finalmente, chamamos [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) para desenhar o cubo e enviá-lo para a pipeline de renderização.

Como nos tutoriais anteriores, chamamos [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) para apresentar a imagem renderizada para a janela.

```cpp
            // Update the constant buffer to rotate the cube model.
            m_constantBufferData.model = XMMatrixRotationY(-degree);
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
            UINT stride = sizeof(SimpleCubeVertex);
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

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas


Usamos profundidade, perspectiva, cor e outros efeitos em primitivas.

Em seguida, aplicamos texturas a primitivas.

[Aplicando texturas a primitivas](applying-textures-to-primitives.md)

 

 




