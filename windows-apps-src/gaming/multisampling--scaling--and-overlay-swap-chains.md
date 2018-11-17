---
author: mtoepke
title: Dimensionamento e sobreposições de cadeia de troca
description: Saiba como criar cadeias de troca dimensionadas para permitir renderização mais rápida em dispositivos móveis e usar cadeias de troca sobrepostas (quando disponíveis) para aumentar a qualidade visual.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, escala de sobreposições, sobreposições, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9d159a78412bea528c1a12428288daebe31d1fe1
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173506"
---
# <a name="swap-chain-scaling-and-overlays"></a>Dimensionamento e sobreposições de cadeia de troca



Saiba como criar cadeias de troca dimensionadas para permitir renderização mais rápida em dispositivos móveis e usar cadeias de troca sobrepostas (quando disponíveis) para aumentar a qualidade visual.

## <a name="swap-chains-in-directx-112"></a>Cadeias de troca no DirectX 11.2


O Direct3D 11.2 permite criar aplicativos UWP (Plataforma Universal do Windows) com cadeias de troca que são ampliadas a partir de resoluções não nativas (reduzidas), permitindo taxas de preenchimento mais rápidas. O Direct3D 11.2 também inclui APIs para renderização com sobreposições de hardware para que você possa apresentar uma interface do usuário em outra cadeia de troca com resolução nativa. Isso permite que o seu jogo desenhe a interface do usuário em resolução nativa total, ao mesmo tempo que mantém uma alta taxa de quadros, fazendo o melhor uso dos dispositivos móveis e de monitores com alto DPI (como 3840 x 2160). Este artigo explica como usar cadeias de troca sobrepostas.

O Direct3D 11.2 também introduz um novo recurso para permitir latência reduzida com cadeias de troca de modelo invertido. Consulte [Reduzir latência com cadeias de troca DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md)

## <a name="use-swap-chain-scaling"></a>Usar redimensionamento das cadeias de troca


Quando seu jogo está sendo executado em hardware de nível inferior, ou hardware otimizado para economizar energia, pode ser vantajoso renderizar conteúdo de jogo em tempo real em resolução mais baixa do que a capacidade nativa do vídeo. Para fazer isso, a cadeia de troca que é usada para renderizar conteúdo de jogo precisa ser menor do que a resolução nativa, ou então uma sub-região da cadeia de troca precisa ser usada.

1.  Primeiro, crie uma cadeia de troca com resolução nativa total.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
    ComPtr<IDXGIDevice3> dxgiDevice;
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );

    ComPtr<IDXGIFactory2> dxgiFactory;
    DX::ThrowIfFailed(
        dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
        );

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  Depois, escolha uma sub-região da cadeia de troca a ser ampliada definindo o tamanho de origem com uma resolução reduzida.

    O exemplo de cadeias de troca de primeiro plano do DirectX calcula um tamanho reduzido com base em uma porcentagem:

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  Crie um visor para corresponder à sub-região da cadeia de troca.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Se o Direct2D estiver sendo usado, a transformação de rotação precisará ser ajustada para compensar a região de origem.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>Criar uma cadeia de troca de sobreposição de hardware para elementos da interface do usuário


Ao usar redimensionamento da cadeia de troca, há uma desvantagem inerente no sentido de que a interface do usuário também é reduzida, possivelmente tornando-a desfocada e mais difícil de usar. Em dispositivos com suporte de hardware a cadeia de troca de sobreposição, esse problema é totalmente solucionado pela renderização da interface do usuário em resolução nativa em uma cadeia de troca que esteja separada do conteúdo do jogo em tempo real. Observe que essa técnica aplica-se somente a cadeias de troca [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), e não pode ser usada com interoperabilidade XAML.

Use as etapas a seguir para criar uma cadeia de troca de primeiro plano que use o recurso de sobreposição de hardware. Estas etapas são executadas depois da criação de uma cadeia de troca para conteúdo de jogo em tempo real conforme descrito anteriormente.

1.  Primeiro, determine se o adaptador DXGI dá suporte a sobreposições. Obtenha o adaptador de saída DXGI na cadeia de troca:

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    O adaptador DXGI dá suporte a sobreposições se o adaptador de saída retornar True para [**SupportsOverlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411).

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Observação**  se o adaptador DXGI dá suporte a sobreposições, continue para a próxima etapa. Se o dispositivo não ser suporte a sobreposições, a renderização com várias cadeias de troca não será eficiente. Em vez disso, renderize a interface do usuário em uma resolução reduzida na mesma cadeia de troca que o conteúdo do jogo em tempo real.

     

2.  Crie uma cadeia de troca de primeiro plano com [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). As seguintes opções devem ser definidas no [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) fornecido ao parâmetro *pDesc*:

    -   Especifique o sinalizador de cadeia de troca [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) para indicar uma cadeia de troca de primeiro plano.
    -   Use o sinalizador de modo alfa [**DXGI\_ALPHA\_MODE\_PREMULTIPLIED**](https://msdn.microsoft.com/library/windows/desktop/hh404496). As cadeias de troca de primeiro plano são sempre pré-multiplicadas.
    -   Defina o sinalizador [**DXGI\_SCALING\_NONE**](https://msdn.microsoft.com/library/windows/desktop/hh404526). As cadeias de troca de primeiro plano são sempre executadas em resolução nativa.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Observação**  definir o [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) novamente sempre que a cadeia de troca for redimensionada.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  Quando duas cadeias de troca estão em uso, aumente a latência máxima de quadros para 2 para que o adaptador DXGI tenha tempo de apresentar as duas cadeias de troca simultaneamente (dentro do mesmo intervalo de VSync).

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  As cadeias de troca de primeiro plano sempre usam alfa premultiplicado. Espera-se que os valores de cor de cada pixel já estejam multiplicados pelo valor alfa antes de o quadro ser apresentado. Por exemplo, um pixel BGRA de 100% branco em alfa de 50% é definido como (0,5 - 0,5 - 0,5 - 0,5).

    A etapa de pré-multiplicação de alfa pode ser realizada no estágio de fusão de saída aplicando um estado de mesclagem de aplicativo (consulte [**ID3D11BlendState**](https://msdn.microsoft.com/library/windows/desktop/ff476349)) com o campo **SrcBlend** da estrutura [**D3D11\_RENDER\_TARGET\_BLEND\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476200) definido como **D3D11\_SRC\_ALPHA**. Os ativos com valores de alfa pré-multiplicado também podem ser usados.

    Se a etapa de premultiplicação de alfa não for executada, as cores na cadeia de troca de primeiro plano estarão mais brilhantes do que o esperado.

5.  Dependendo de a cadeia de troca de primeiro plano ter ou não ter sido criada, a superfície de desenho do Direct2D para elementos de interface do usuário poderá estar associada à cadeia de troca de primeiro plano.

    Criando modos de exibição de destino de renderização:

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Criando a superfície de desenho do Direct2D:

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  Apresente a cadeia de troca de primeiro plano junto com a cadeia de troca redimensionada usada para conteúdo de jogo em tempo real. Como a latência de quadro foi definida como 2 para as duas cadeias de troca, o DXGI pode apresentá-las no mesmo intervalo de VSync.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 




