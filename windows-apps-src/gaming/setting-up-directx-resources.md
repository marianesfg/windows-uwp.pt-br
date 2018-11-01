---
author: mtoepke
title: Configurar recursos DirectX e exibir uma imagem
description: Aqui, mostramos a você como criar um dispositivo Direct3D, a cadeia de troca, a exibição de destino de renderização e como apresentar a imagem renderizada para a exibição.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, recursos, imagens
ms.localizationpriority: medium
ms.openlocfilehash: 24fd038bdd447491da43e5d5803445d00147ba2d
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5881875"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Configurar recursos DirectX e exibir uma imagem



Aqui, mostramos a você como criar um dispositivo Direct3D, a cadeia de troca, a exibição de destino de renderização e como apresentar a imagem renderizada para a exibição.

**Objetivo:** configurar recursos DirectX em um aplicativo UWP (Plataforma Universal do Windows) do Windows em C++ e exibir uma cor sólida.

## <a name="prerequisites"></a>Pré-requisitos


Partimos do princípio de que você conhece C++. Você também precisa ter experiência básica com conceitos de programação de elementos gráficos.

**Tempo para concluir:** 20 minutos.

## <a name="instructions"></a>Instruções

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Declarando as variáveis de interface Direct3D com ComPtr

Declaramos as variáveis da interface Direct3D com o modelo de [ponteiro inteligente](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx) ComPtr da WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows), para que possamos gerenciar o tempo de vida dessas variáveis de maneira protegida contra exceções. Podemos então usar essas variáveis para acessar a [**ComPtr class**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) e seus membros. Por exemplo:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Se você declarar [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) com ComPtr, poderá usar o método **GetAddressOf** do ComPtr para obter o endereço do ponteiro para o **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) a ser transmitido para [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464). **OMSetRenderTargets** associa o destino de renderização ao [estágio de fusão de saída](https://msdn.microsoft.com/library/windows/desktop/bb205120) para especificar o destino de renderização como o destino de saída.

Depois que o aplicativo de exemplo é iniciado, ele é inicializado e carregado, e então está pronto para execução.

### <a name="2-creating-the-direct3d-device"></a>2. Criando o dispositivo Direct3D

Para usar o API Direct3D para renderizar uma cena, primeiro precisamos criar um dispositivo Direct3D que representa o adaptador de exibição. Para criar o serviço Direct3D, chamamos a função [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Especificamos os níveis 9.1 a 11.1 na matriz de valores [**D3D\_FEATURE\_LEVEL**](https://msdn.microsoft.com/library/windows/desktop/ff476329). O Direct3D guia a matriz em ordem e retorna o mais alto nível de funcionalidade com suporte. Portanto, para obter o nível de recurso mais alto disponível, listamos as entradas da matriz de **D3D\_FEATURE\_LEVEL** da mais alta para a mais baixa. Transmitimos o sinalizador [**D3D11\_CREATE\_DEVICE\_BGRA\_SUPPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT) ao parâmetro *Flags* para que os recursos Direct3D possam interoperar com o Direct2D. Se usarmos uma compilação de depuração, também transmitiremos o sinalizador [**D3D11\_CREATE\_DEVICE\_DEBUG**](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG). Para saber mais sobre os aplicativos de depuração, consulte [Usando a camada de depuração para depurar aplicativos](https://msdn.microsoft.com/library/windows/desktop/jj200584).

Obtemos o dispositivo Direct3D 11.1 ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) e o contexto de dispositivo ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) consultando o dispositivo Direct3D 11 e o contexto do dispositivo que são retornados do [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082).

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. Criando a cadeia de troca

Em seguida, criamos uma cadeia de troca que o dispositivo usa para renderização e exibição. Declaramos e inicializamos uma estrutura [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) para descrever a cadeia de troca. Em seguida, configuramos a cadeia de troca como modelo de inversão (isto é, uma cadeia de troca que tem o valor [**DXGI\_SWAP\_EFFECT\_FLIP\_SEQUENTIAL**](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL) definido no membro **SwapEffect**) e definimos o membro **Format** como [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM). Definimos o membro **Count** da estrutura [**DXGI\_SAMPLE\_DESC**](https://msdn.microsoft.com/library/windows/desktop/bb173072) que o membro **SampleDesc** especifica como 1 e o membro **Quality** de **DXGI\_SAMPLE\_DESC** como zero porque flip-model não dá suporte à MSAA (suavização múltipla de amostra). Definimos o membro **BufferCount** como 2 para que a cadeia de troca possa usar um buffer frontal para apresentar para o dispositivo de exibição e um buffer traseiro que serve como o destino de renderização.

Obtemos o dispositivo DXGI subjacente consultando o dispositivo Direct3D 11.1. Para minimizar o consumo de energia, o que é importante em dispositivos alimentados por bateria, como laptops e tablets, chamamos o método [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) com 1 como o número máximo de quadros de buffer traseiro que a DXGI pode consultar. Isso garante que o aplicativo seja renderizado somente após o vazio vertical.

Para finalmente criar a cadeia de troca, precisamos obter o alocador pai do dispositivo DXGI. Chamamos [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) para obter o adaptador para o dispositivo e chamar [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) no adaptador para obter o alocador pai ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)). Para criar a cadeia de troca, chamamos [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) com o descritor swap-chain e a janela principal do aplicativo.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. Criando o modo de exibição de destino de renderização

Para renderizar os elementos gráficos para a janela, precisamos criar um modo de exibição de destino de renderização. Chamamos [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) para obter o buffer de retorno da cadeia de troca a ser usado quando criamos o modo de exibição de destino de renderização. Especificamos o buffer traseiro como uma textura 2D ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)). Para criar o modo de exibição de destino de renderização, chamamos [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517) com o buffer de retorno da cadeia de troca. Devemos especificar para desenhar a janela central inteira especificando a porta de visualização ([**D3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) como o tamanho total do buffer de retorno da cadeia de troca. Usamos a porta de visualização em uma chamada a [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) para associar a porta de visualização ao [estágio de rasterizador](https://msdn.microsoft.com/library/windows/desktop/bb205125) do pipeline. O estágio do rasterizador converte as informações de vetor em uma imagem de rasterizador. Nesse caso, não exigimos uma conversão porque estamos apenas exibindo uma cor sólida.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. Apresentando a imagem renderizada

Entramos em um loop infinito para processar e exibir a cena continuamente.

Neste loop, temos:

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) para especificar o destino de renderização como o destino de saída.
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476388) para limpar o destino de renderização para uma cor sólida.
3.  [**IDXGISwapChain::Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576) para apresentar a imagem renderizada na janela.

Como definimos anteriormente a latência máxima de quadros como 1, o Windows geralmente retarda o loop de renderização para a taxa de atualização de tela, geralmente em torno de 60 Hz. O Windows retarda o loop de processamento, fazendo o aplicativo entrar no modo de suspensão quando o aplicativo chama [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576). O Windows faz o aplicativo entrar no modo de suspensão até que a tela seja atualizada.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

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

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Redimensionando a janela do aplicativo e o buffer da cadeia de troca

Se o tamanho da janela do aplicativo for alterado, o aplicativo deverá redimensionar os buffers da cadeia de troca, recriar o modo de exibição de destino de renderização e apresentar o imagem renderizada redimensionada. Para redimensionar os buffers da cadeia de troca, chamamos [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). Nessa chamada, não alteramos o número de buffers e o formato dos buffers (o parâmetro *BufferCount* como dois e o parâmetro *NewFormat* como [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)). Tornamos o tamanho do buffer de retorno da cadeia de troca como o mesmo tamanho da janela redimensionada. Depois redimensionamos os buffers da cadeia de troca, criamos o novo destino de renderização e apresentamos a nova imagem renderizada de forma semelhante à quando iniciamos o aplicativo.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas


Criamos um dispositivo Direct3D, uma cadeia de troca e um modo de exibição de destino de processamento e apresentamos a imagem renderizada para exibição.

Também desenhamos um triângulo na tela.

[Criando sombreadores e desenhando primitivas](creating-shaders-and-drawing-primitives.md)

 

 




