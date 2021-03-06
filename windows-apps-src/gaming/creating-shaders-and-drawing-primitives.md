---
title: Criar sombreadores e desenhando primitivas
description: Aqui, mostramos a você como usar arquivos de origem HLSL para compilar e criar sombreadores que você pode usar para desenhar primitivas no monitor.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, sombreadores, primitivas, directx
ms.localizationpriority: medium
ms.openlocfilehash: fecce6237d08f9ffa89bc7503412a357b17c641d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368952"
---
# <a name="create-shaders-and-drawing-primitives"></a>Criar sombreadores e desenhando primitivas



Aqui, mostramos a você como usar arquivos de origem HLSL para compilar e criar sombreadores que você pode usar para desenhar primitivas no monitor.

Podemos criar e desenhar um triângulo amarelo usando sombreadores de vértice e pixel. Depois de criarmos o dispositivo Direct3D, a cadeia de troca e o modo de exibição de destino de processamento, podemos ler dados dos arquivos de objeto de sombreador binário no disco.

**Objetivo:** Para criar sombreadores e desenhar primitivas.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

Também supomos que você leu [Guia de início rápido: configurando recursos DirectX e exibindo uma imagem](setting-up-directx-resources.md).

**Tempo para conclusão:** 20 minutos.

## <a name="instructions"></a>Instruções

### <a name="1-compiling-hlsl-source-files"></a>1. Compilando arquivos de origem HLSL

O Microsoft Visual Studio usa o compilador de código HLSL [fxc.exe](https://docs.microsoft.com/windows/desktop/direct3dtools/fxc) para compilar os arquivos-fonte .hlsl (SimpleVertexShader.hlsl e SimplePixelShader.hlsl) nos arquivos de objeto de sombreador binário .cso (SimpleVertexShader.cso e SimplePixelShader.cso). Para saber mais sobre o compilador de código HLSL, consulte a Ferramenta Compilador de Efeitos. Para saber mais sobre o código de sombreador de compilação, consulte [Compilando sombreadores](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1).

Este é o código no SimpleVertexShader.hlsl:

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

Este é o código no SimplePixelShader.hlsl:

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. Ler dados no disco

Usamos a função DX::ReadDataAsync do DirectXHelper.h no modelo de aplicativo do DirectX 11 (Windows Universal) para ler dados de forma assíncrona em um arquivo no disco.

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. Criação de sombreadores de pixel e vértice

Lemos os dados do arquivo SimpleVertexShader.cso e atribuímos os dados à matriz de bytes *vertexShaderBytecode*. Chamamos [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) com a matriz de bytes para criar o sombreador de vértice ([**ID3D11VertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader)). Definimos o valor de profundidade de vértice como 0,5 na origem SimpleVertexShader.hlsl para garantir que nosso triângulo seja desenhado. Podemos preencher uma matriz de [ **D3D11\_entrada\_elemento\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) estruturas para descrever o layout do código de sombreador de vértice e, em seguida, chamar [ **ID3D11Device::CreateInputLayout** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) para criar o layout. A matriz tem um elemento de layout que define a posição do vértice. Lemos os dados do arquivo SimplePixelShader.cso e atribuímos os dados à matriz de bytes *pixelShaderBytecode*. Chamamos [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) com a matriz de bytes para criar o sombreador de vértice ([**ID3D11PixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader)). Definimos o valor de pixel como (1,1,1,1) na origem SimplePixelShader.hlsl para fazer nosso triângulo ficar amarelo. Você pode alterar a cor alterando este valor.

Podemos criar buffers de vértice e de índice que definem um triângulo simples. Para fazer isso, primeiro definimos o triângulo, em seguida, descreva os buffers de índice e vértice ([**D3D11\_BUFFER\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) e [ **D3D11\_ SUBRESOURCE\_dados**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)) usando a definição do triângulo e, finalmente, chame [ **ID3D11Device::CreateBuffer** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) uma vez para cada buffer.

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
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
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
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
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
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
        });
```

Usamos os sombreadores de vértice e de índice, o layout de sombreador de vértice e os buffers de vértice e de índice para desenhar um triângulo amarelo.

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. O triângulo de desenho e apresentar a imagem renderizada

Entramos em um loop infinito para processar e exibir a cena continuamente. Chamamos [**ID3D11DeviceContext::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para especificar o destino de renderização como o destino de saída. Chamamos [**ID3D11DeviceContext::ClearRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) com { 0.071f, 0.04f, 0.561f, 1.0f } para limpar o destino de renderização para uma cor azul sólida.

No loop sem fim, desenhamos um triângulo amarelo na superfície azul.

**Para desenhar um triângulo amarelo**

1.  Primeiro, chamamos [**ID3D11DeviceContext::IASetInputLayout**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) para descrever como os dados de buffer de vértices são transmitidos para o estágio de assembler de entrada.
2.  Em seguida, chamamos [**ID3D11DeviceContext::IASetVertexBuffers**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) e [**ID3D11DeviceContext::IASetIndexBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) para associar os buffers de vértice e índice para o estágio de assembler de entrada.
3.  Em seguida, chamamos [ **ID3D11DeviceContext::IASetPrimitiveTopology** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) com o [ **D3D11\_PRIMITIVO\_topologia\_ TRIANGLESTRIP** ](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) valor a ser especificado para o estágio do assembler de entrada interpretar os dados de vértice como uma faixa de triângulo.
4.  Chamamos [**ID3D11DeviceContext::VSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) para iniciar o estágio de sombreador de vértice com o código de sombreador de vértice e [**ID3D11DeviceContext::PSSetShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) para iniciar o estágio de sombreador de pixel com o código de sombreador de pixel.
5.  Finalmente, chamamos [**ID3D11DeviceContext::DrawIndexed**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) para desenhar o triângulo e enviá-lo ao pipeline de renderização.

Como nos tutoriais anteriores, chamamos [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) para apresentar a imagem renderizada para a janela.

```cpp
            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
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

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
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


Criamos e desenhamos um triângulo amarelo usando sombreadores de vértice e de pixel.

Em seguida, criamos um cubo 3D em órbita e aplicamos os efeitos de iluminação nele.

[Usando a profundidade e efeitos em primitivos](using-depth-and-effects-on-primitives.md)

 

 




