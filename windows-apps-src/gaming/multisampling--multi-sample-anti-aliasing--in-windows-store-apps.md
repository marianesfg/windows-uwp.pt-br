---
title: Multisampling em aplicativos UWP
description: Saiba como utilizar o multisampling em aplicativos da Plataforma Universal do Windows (UWP) desenvolvidos com Direct3D.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, várias amostras, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: f4d3e590f99fdf6ca907fcc8fd5b412c5796f474
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735101"
---
# <a name="span-iddev_gamingmultisampling__multi-sample_anti_aliasing__in_windows_store_appsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span>Multiamostrar em aplicativos Plataforma Universal do Windows (UWP)



Saiba como utilizar o multisampling em aplicativos da Plataforma Universal do Windows (UWP) desenvolvidos com Direct3D. O multisampling, também conhecido como suavização de múltipla amostra, é uma técnica de elementos gráficos usada para reduzir a aparição de bordas suavizadas. Essa técnica desenha mais pixels do que o que realmente existe no destino de renderização e, depois, faz uma média dos valores para manter a aparência de uma borda "parcial" em determinados pixels. Para uma descrição detalhada sobre como o multisampling funciona no Direct3D, consulte [Regras de rasterização para suavização de várias amostras](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage-rules).

## <a name="multisampling-and-the-flip-model-swap-chain"></a>Várias amostras e a cadeia de troca do modelo de virar a página


Os aplicativos UWP em DirectX devem usar cadeias de troca do modelo de inversão. As cadeias de troca do modelo de inversão não dão suporte a várias amostras diretamente, mas esse método ainda pode ser aplicado de uma forma diferente, renderizando a cena para um modo de exibição de destino de renderização com várias amostras e, depois, resolvendo o destino de renderização com várias amostras para o buffer de fundo antes da apresentação. Este artigo explica as etapas necessárias para adicionar várias amostras a seu aplicativo UWP.

### <a name="how-to-use-multisampling"></a>Como usar várias amostras

Os níveis de recursos do Direct3D garantem o suporte a funcionalidades específicas de contagem mínima de amostras e garantem que determinados formatos de buffer que dão suporte a várias amostras estarão disponíveis. Os dispositivos gráficos costumam dar suporte a um número maior do que o mínimo necessário de formatos e contagens de amostras. O suporte ao multisampling pode ser determinado no tempo de execução verificando o suporte ao recurso de multisampling para formatos DXGI específicos e, em seguida, verificando o número de amostras que podem ser usadas com cada formato com suporte.

1.  Chame [**ID3D11Device::CheckFeatureSupport**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) para descobrir quais formatos DXGI podem ser usados com o multisampling. Forneça os formatos de destino de renderização que seu jogo pode usar. Tanto o destino de renderização quanto o destino de resolução devem usar o mesmo formato, portanto, verifique o [**formato de\_D3D11\_suporte\_** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_format_support) formato de\_de RENDERTARGET e **D3D11\_\_suporte\_resolução de\_de multiamostras**.

    **Nível de recurso 9:  ** Embora os dispositivos de nível de recurso 9 [garantam suporte para formatos de destino de renderização com uma amostra](https://docs.microsoft.com/previous-versions/ff471324(v=vs.85)), o suporte não é garantido para destinos de resolução de multiamostrar. Sendo assim, essa verificação é necessária antes de tentar usar a técnica de várias amostras descrita neste tópico.

    O código a seguir verifica o suporte a multiamostrar para todos os valores de formato DXGI\_:

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  Para cada formato com suporte, consulte o suporte a contagem de amostras chamando [**ID3D11Device::CheckMultisampleQualityLevels**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels).

    O código a seguir verifica o suporte a tamanho da amostra para todos os formatos DXGI com suporte:

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **Observação**   use [**ID3D11Device2:: CheckMultisampleQualityLevels1**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-checkmultisamplequalitylevels1) em vez disso, se você precisar verificar o suporte a multiamostrar para buffers de recursos lado a lado.

     

3.  Crie um buffer e renderize a visualização do destino com a contagem de amostras desejada. Use o mesmo formato, largura e altura de\_DXGI que a cadeia de permuta, mas especifique uma contagem de amostra maior que 1 e use uma dimensão de textura de multiamostração (**D3D11\_RTV\_dimension\_TEXTURE2DMS** , por exemplo). Se necessário, você pode recriar a cadeia de troca com novas configurações que sejam ideais para o multisampling.

    O código a seguir cria um destino de renderização com várias amostras:

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  O buffer de profundidade deve ter a mesma largura, altura, contagem de amostras e dimensão de texturas para corresponder ao destino de renderização de várias amostras.

    O código a seguir cria um buffer de profundidade com várias amostras:

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  Agora é uma boa hora para criar o visor, pois a largura e a altura do visor também devem corresponder ao destino de renderização.

    O código a seguir cria um visor:

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  Renderize cada quadro como um destino de renderização de várias amostras. Quando a renderização estiver concluída, chame [**ID3D11DeviceContext::ResolveSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-resolvesubresource) antes de apresentar o quadro. Isso instrui o Direct3D a realizar a operação de multisampling, calculando o valor de cada pixel para exibição e colocando o resultado no buffer de fundo. O buffer de fundo contém então a imagem de suavização final e pode ser apresentado.

    O código a seguir resolve o sub-recurso antes de apresentar o quadro:

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




