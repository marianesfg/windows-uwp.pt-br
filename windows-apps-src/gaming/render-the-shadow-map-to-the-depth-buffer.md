---
author: mtoepke
title: Renderizar o mapa de sombra para o buffer de profundidade
description: Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, renderização, mapa de sombra, buffer de profundidade, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: a73754fef6d87505751460ec134d853c6bca0530
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996809"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>Renderizar o mapa de sombra para o buffer de profundidade




Faça a renderização do ponto de vista da luz para criar um mapa de profundidade bidimensional que representa o volume de sombra. O mapa de profundidade mascara o espaço que será renderizado com sombras. Parte 2 do [Guia passo a passo: implementar volumes de sombra usando buffers de profundidade no Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>Limpar o buffer de profundidade


Sempre limpe o buffer de profundidade antes de renderizar com ele.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>Renderizar o mapa de sombra para o buffer de profundidade


Para a passagem de renderização de sombra, especifique um buffer de profundidade, mas não defina um destino de renderização.

Especifique o visor de luz, um sombreador de vértice e defina buffers constantes do espaço de luz. Use o conjunto de face frontal desta passagem para otimizar os valores de profundidade definidos no buffer de sombra.

Não se esqueça de que, na maioria dos dispositivos, você pode especificar nullptr para o sombreador de pixel (ou simplesmente não especificar um sombreador de pixel). Porém, alguns drivers podem emitir uma exceção quando você chama a operação de desenho no dispositivo Direct3D com um sombreador de pixel nulo definido. Para evitar essa exceção, você pode definir um sombreador de pixel mínimo para a passagem de renderização de sombra. A saída desse sombreador é descartada; ele pode chamar [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995) em todos os pixels.

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

**Otimizar o tronco de exibição:** garanta que a implementação calcule um tronco de exibição firme, para obter o máximo de precisão com o buffer de profundidade. Consulte as [técnicas comuns para melhorar mapas de profundidade de sombra](https://msdn.microsoft.com/library/windows/desktop/ee416324) para conhecer mais dicas sobre técnicas de sombreamento.

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

 

 




