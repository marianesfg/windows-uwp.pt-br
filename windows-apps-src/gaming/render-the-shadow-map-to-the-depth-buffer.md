---
title: Renderizar o mapa de sombra para o buffer de profundidade
description: Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização, mapa de sombra, buffer de profundidade, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: a8ae67df457d4abafc8fb689a747139f62ca0e0e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368071"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>Renderizar o mapa de sombra para o buffer de profundidade




Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra. O mapa de profundidade mascara o espaço que será renderizado com sombras. Parte 2 de [passo a passo: Implementar os volumes de sombra usando buffers de profundidade em Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>Limpar o buffer de profundidade


Sempre limpe o buffer de profundidade antes de renderizar com ele.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>Renderizar o mapa de sombra para o buffer de profundidade


Para a passagem de renderização de sombra, especifique um buffer de profundidade, mas não defina um destino de renderização.

Especifique o visor de luz, um sombreador de vértice e defina buffers constantes do espaço de luz. Use o conjunto de face frontal desta passagem para otimizar os valores de profundidade definidos no buffer de sombra.

Não se esqueça de que, na maioria dos dispositivos, você pode especificar nullptr para o sombreador de pixel (ou simplesmente não especificar um sombreador de pixel). Porém, alguns drivers podem emitir uma exceção quando você chama a operação de desenho no dispositivo Direct3D com um sombreador de pixel nulo definido. Para evitar essa exceção, você pode definir um sombreador de pixel mínimo para a passagem de renderização de sombra. A saída desse sombreador é descartada; ele pode chamar [**discard**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-discard) em todos os pixels.

Renderize os objetos que podem projetar sombras, mas não se preocupe em renderizar geometrias que não possam fazer isso (como o chão de uma sala ou objetos removidos da passagem de sombra para fins de otimização).

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**Otimize o frustum do modo de exibição:**  Certifique-se de que sua implementação calcula um frustum estreito do modo de exibição para que você obtenha mais precisão de seu buffer de profundidade. Consulte as [técnicas comuns para melhorar mapas de profundidade de sombra](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps) para conhecer mais dicas sobre técnicas de sombreamento.

## <a name="vertex-shader-for-shadow-pass"></a>Sombreador de vértice para passagem de sombra


Use uma versão simplificada do sombreador de vértice para renderizar somente a posição do vértice no espaço de luz. Não inclua normais de iluminação, transformações secundárias, entre outros.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

Na próxima parte deste guia passo a passo, veremos como adicionar sombras pela [renderização com teste de profundidade](render-the-scene-with-depth-testing.md).

 

 




