---
author: mtoepke
title: Criar recursos de dispositivo de buffer de profundidade
description: Aprenda a criar recursos de dispositivos Direct3D necessários para dar suporte a testes de profundidade para volumes de sombra.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, direct3d, buffer de profundidade
ms.localizationpriority: medium
ms.openlocfilehash: 091ac04b2a41c54fbce7294bcf6651f4ad4aafd2
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5983839"
---
# <a name="create-depth-buffer-device-resources"></a>Criar recursos de dispositivo de buffer de profundidade




Aprenda a criar recursos de dispositivos Direct3D necessários ao suporte de testes e profundidade para volumes de sombra. Parte 1 do [Guia passo a passo: implementar volumes de sombra usando buffers de profundidade no Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="resources-youll-need"></a>Recursos necessários


Para renderizar um mapa de profundidade para volumes de sombra, são necessários os seguintes recursos, dependentes de dispositivos Direct3D:

-   Um recurso (buffer) para o mapa de profundidade
-   Uma exibição de estêncil de profundidade e uma exibição de recurso de sombreador para o recurso
-   Um objeto de estado da amostra de comparação
-   Buffers constante para matrizes POV de luz
-   Um visor (geralmente quadrado) para renderizar o mapa de sombra
-   Um objeto de estado de renderização para habilitar o conjunto de face frontal
-   Você também precisará de um objeto de estado de renderização para voltar ao conjunto de face traseira, caso ainda não use um.

Observe que a criação desses recursos deve se incluída em uma rotina de criação de recursos dependentes de dispositivos. Desse modo, seu renderizador poderá recriá-los quando um novo driver de dispositivo for instalado ou o usuário mover o aplicativo para um monitor conectado a uma placa gráfica diferente, apenas para citar alguns exemplos.

## <a name="check-feature-support"></a>Verificar o suporte ao recurso


Antes de criar o mapa de profundidade, chame o método [**CheckFeatureSupport**](https://msdn.microsoft.com/library/windows/desktop/ff476497) no dispositivo Direct3D, solicite **D3D11\_FEATURE\_D3D9\_SHADOW\_SUPPORT** e forneça uma estrutura [**D3D11\_FEATURE\_DATA\_D3D9\_SHADOW\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/jj247569).

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

Senão houver suporte para esse recurso, não tente carregar sombreadores compilados para o modelo de sombreador 4 de nível 9\_x que chama exemplos de funções de comparação. Em muitos casos, a falta de suporte a esse recurso significa que a GPU é um dispositivo herdado com um driver que não está atualizado para dar suporte a pelo menos o WDDM 1.2. Quando o dispositivo dá suporte a pelo menos o nível de recurso 10\_0, você pode carregar um exemplo de sombreador de comparação compilado para o modelo de sombreador 4\_0.

## <a name="create-depth-buffer"></a>Criar o buffer de profundidade


Primeiro, tente criar o mapa de profundidade com um formato de profundidade de precisão maior. Inicialmente, configure as propriedades de exibição do recurso de sombreador correspondente. Caso ocorra uma falha na criação do recurso (por exemplo, devido à baixa memória do dispositivo ou à incompatibilidade do formato com o hardware), tente um formato de precisão mais baixa e altere as propriedades para que haja correspondência.

Esta etapa é opcional se você precisa apenas de um formato de profundidade de baixa precisão (por exemplo, ao fazer renderização em dispositivos Direct3D com nível de recursos 9\_1 e resolução média).

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

Em seguida, crie as exibições de recursos. Defina o tamanho de mip como zero na exibição de estêncil de profundidade; defina também os níveis de mip como 1 na exibição de recurso de sombreador. Ambos têm uma dimensão de textura TEXTURE2D, e devem usar um [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059) correspondente.

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>Criar estado de comparação


Agora crie o objeto de estado de amostra da comparação. O nível de recurso 9\_1 só dá suporte a D3D11\_COMPARISON\_LESS\_EQUAL. As opções de filtragem são explicadas com mais detalhes no tópico sobre [suporte a mapas de sombra em diversos hardwares](target-a-range-of-hardware.md), mas você pode simplesmente escolher a filtragem por pontos para obter mapas de sombra mais rápidos.

Não se esqueça de que você pode especificar o modo de endereço D3D11\_TEXTURE\_ADDRESS\_BORDER, pois ele funciona em dispositivos com nível de recurso 9\_1. Isso se aplica a sombreadores de pixel que não testam se o pixel está no tronco de exibição da luz antes da realização do teste de profundidade. Ao especificar 0 ou 1 para cada borda, você pode controlar se os pixels fora do tronco de exibição da luz são aprovados ou não no teste de profundidade (e, consequentemente, se são iluminados ou sombreados).

No nível de recurso 9\_1, defina os seguintes valores obrigatórios: **MinLOD** é definido como zero, **MaxLOD** é definido como **D3D11\_FLOAT32\_MAX** e **MaxAnisotropy** como zero.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>Criar estados de renderização.


Agora crie um estado de renderização que você possa usar para habilitar o conjunto de face frontal. Não se esqueça de que os dispositivos com nível de recursos 9\_1 precisam de **DepthClipEnable** definido como **true**.

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

Crie um estado de renderização que você possa usar para habilitar o conjunto de face traseira. Caso o código de renderização já ative o conjunto de face traseira, ignore esta etapa.

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>Criar buffers constantes


Não se esqueça de criar um buffer constante para realizar a renderização do ponto de vista da luz. Você também pode usar esse buffer constante para especificar a posição da luz ao sombreador. Use uma matriz de perspectiva para pontos iluminados e matriz ortogonal para fachos de luz, como raios de sol.

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

Preencha os dados de buffer constante. Atualize os buffers constantes uma vez durante a inicialização e novamente quando os valores de luz forem alterados do quadro anterior.

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>Criar um visor


Você precisa de um visor separado para renderizar o mapa de sombra. O visor não é um recurso baseado em dispositivo; você pode criá-lo em qualquer parte do código. A criação do visor com o mapa de sombra pode ajudar a tornar a dimensão do visor mais congruente com a dimensão do mapa de sombra.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

Na próxima parte deste guia passo a passo, veremos como criar o mapa de sombra por meio da [renderização do buffer de profundidade](render-the-shadow-map-to-the-depth-buffer.md).

 

 




