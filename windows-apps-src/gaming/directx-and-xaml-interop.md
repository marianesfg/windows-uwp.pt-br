---
author: mtoepke
title: Interoperabilidade entre DirectX e XAML
description: "Você pode usar a XAML (Extensible Application Markup Language) e o Microsoft DirectX juntos no seu jogo UWP (Plataforma Universal do Windows)."
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, jogos, directx, interoperabilidade com xaml
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6934ac8bfbff487e57d0097cb129faf853a3eb9f
ms.lasthandoff: 02/07/2017

---

# <a name="directx-and-xaml-interop"></a>Interoperabilidade entre DirectX e XAML


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Você pode usar a XAML (Extensible Application Markup Language) e o Microsoft DirectX juntos no seu jogo ou aplicativo UWP (Plataforma Universal do Windows). A combinação da XAML e do DirectX permite que você crie estruturas de interface de usuário flexíveis que interoperem com o seu conteúdo renderizado em DirectX e é particularmente útil para aplicativos com muitos gráficos. Este tópico explica a estrutura de um aplicativo UWP que usa DirectX e identifica os tipos importantes a serem usados na criação do seu aplicativo UWP para funcionar com o DirectX.

Se seu aplicativo se preocupar principalmente com renderização 2D, convém usar biblioteca do Windows Runtime [**Win2D**](https://github.com/microsoft/win2d). Essa biblioteca é mantida pela Microsoft e criada com base nas tecnologias principais de Direct2D. Ela simplifica bastante o padrão de uso para implementar os gráficos 2D e inclui abstrações úteis para algumas das técnicas descritas neste documento. Consulte a página de projeto para obter mais detalhes. Esse documento aborda orientações para desenvolvedores de aplicativos que optam por *não* usar Win2D.

> **Observação**  As APIs do DirectX não são definidas como tipos do Windows Runtime e, portanto, você normalmente usa extensões de componentes Visual C++ (C++/CX) para desenvolver componentes XAML UWP que interoperam com o DirectX. Além disso, você pode criar um aplicativo UWP em C# e XAML que usa DirectX se encapsular as chamadas DirectX em um arquivo de metadados do Windows Runtime separado.

 

## <a name="xaml-and-directx"></a>XAML e DirectX

O DirectX fornece duas bibliotecas poderosas para gráficos 2D e 3D: Direct2D e Microsoft Direct3D. Embora a XAML dê suporte para primitivos e efeitos básicos 2D, muitos aplicativos, como de modelagem e jogos, precisam de suporte gráfico mais complexo. Para esses, você pode usar o Direct2D e o Direct3D para renderizar parte dos gráficos, ou todos eles, e usar a XAML para todo o resto.

Se estiver implementando interoperabilidade personalizada entre XAML e DirectX, você precisa conhecer estes dois conceitos:

-   Superfícies compartilhadas são regiões de tamanho da tela, definidas pela XAML, em que você pode usar o DirectX para desenhar indiretamente, usando tipos de [**Windows::UI::Xaml::Media::ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx). Para superfícies compartilhadas, você não controla o tempo preciso para quando o novo conteúdo é exibido na tela. Em vez disso, as atualizações para a superfície compartilhada são sincronizadas para atualizar a estrutura do XAML.
-   [Cadeias de troca](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx) representam uma coleção de buffers usados para exibir elementos gráficos com latência mínima. Normalmente, as cadeias de troca são atualizadas em 60 quadros por segundo separadamente do thread da interface do usuário. No entanto, as cadeias de troca usam mais memória e recursos da CPU para dar suporte a atualizações rápidas e são mais difíceis de usar, pois é necessário gerenciar vários threads.

Reflita sobre porque você está usando o DirectX. Ele será usado para compor ou animar um único controle que se encaixa dentro das dimensões da janela de exibição? Ela conterá a saída que precisa ser renderizada e controlada em tempo real, como em um jogo? Em caso afirmativo, você provavelmente precisará implementar uma cadeia de troca. Caso contrário, deve ser recomendado usando uma superfície compartilhada.

Depois de determinar como pretende usar o DirectX, você pode usar um destes tipos de Windows Runtime para incorporar renderização do DirectX em seu aplicativo da Windows Store:

-   Se quiser compor uma imagem estática ou desenhar uma imagem nos intervalos acionados por eventos, desenhe em uma superfície compartilhada com [**Windows::UI::Xaml::Media::Imaging::SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Esse tipo lida com uma superfície de desenho dimensionada do DirectX. Normalmente, você usa esse tipo ao compor uma imagem ou textura como um bitmap para exibição em um documento ou elemento da interface do usuário. Ele não funciona bem para interatividade em tempo real, como para um jogo de alto desempenho. Isso ocorre porque as atualizações em um objeto **SurfaceImageSource** são sincronizadas com as atualizações da interface do usuário da XAML, e isso pode introduzir latência no feedback visual que você fornece ao usuário, como uma taxa de quadros flutuante ou uma resposta fraca percebida para entrada em tempo real. Ainda assim, as atualizações são suficientemente rápidas para controles dinâmicos ou simulações de dados!

-   Se a imagem for maior que o estado real fornecido da tela, e puder ser panorâmica ou ampliada pelo usuário, use [**Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050). Esse tipo trata uma superfície de desenho dimensionada do DirectX que é maior que a tela. Como [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041), você usa isso ao compor uma imagem ou controle complexo dinamicamente. E, assim como **SurfaceImageSource**, ele não funciona bem para jogos de alto desempenho. Alguns exemplos de elementos XAML que podem usar um **VirtualSurfaceImageSource** são controles de mapa ou um visualizador de documentos para imagens grandes e densas.

-   Se você estiver usando o DirectX para apresentar gráficos atualizados em tempo real ou em uma situação na qual as atualizações devem vir em intervalos regulares de baixa latência, use a classe [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) para que você possa atualizar os gráficos sem sincronizar com o timer de atualização da estrutura da XAML. Esse tipo permite acessar diretamente a cadeia de troca do dispositivo gráfico ([**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631)) e a camada da XAML sobre o destino da renderização. Esse tipo funciona muito bem para jogos e aplicativos do DirectX em tela inteira que necessitam de uma interface de usuário baseada em XAML. É necessário conhecer bem o DirectX para usar essa abordagem, incluindo as tecnologias Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D e Direct3D. Para saber mais, consulte [Guia de programação para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## <a name="surfaceimagesource"></a>SurfaceImageSource


[**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) fornece superfícies compartilhadas do Microsoft DirectX nas quais desenhar e depois compôs os bits no conteúdo do aplicativo.

Veja a seguir um processo básico para criar e atualizar um objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) no code behind:

1.  Defina o tamanho da superfície compartilhada transmitindo a altura e a largura ao construtor [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041). Você também pode indicar se a superfície precisa de suporte alfa (opacidade).

    Por exemplo:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtenha um ponteiro para [**ISurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848322). Converta o objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) como [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**) e chame **QueryInterface** nele para obter a implementação **ISurfaceImageSourceNative** adjacente. Você pode usar os métodos definidos nessa implementação para definir o dispositivo e executar as operações de desenho.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNative> m_sisNative;
    // ...
    IInspectable* sisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);
    sisInspectable->QueryInterface(__uuidof(ISurfaceImageSourceNative), (void **)&m_sisNative);
    ```

3.  Defina o dispositivo DXGI chamando primeiro [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) e transmitindo o dispositivo e o contexto para [**ISurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Por exemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_sisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Forneça um ponteiro para o objeto [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) para [**ISurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) e desenhe nessa superfície usando o DirectX. Somente a área especificada para atualização no parâmetro *updateRect* é desenhada.

    > **Observação**   Você pode ter somente uma operação [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendente ativa por vez para cada [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

     

    Este método retorna o deslocamento do ponto (x,y) do retângulo de destino atualizado no parâmetro *offset*. Você pode usar esse deslocamento para determinar onde desenhar em [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

    ```cpp
    ComPtr<IDXGISurface> surface;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(updateRect, &surface, &offset);
    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
    {
              // Device changed
    }
    else
    {
        // Draw to IDXGISurface (the surface paramater)
    }
    ```

5.  Chame [**ISurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324) para completar o bitmap. O bitmap pode ser usado como uma fonte para [**Image**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) ou [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) XAML.

    ```cpp
    m_sisNative->EndDraw();
    // ...
    // The SurfaceImageSource object's underlying ISurfaceImageSourceNative object contains the completed bitmap.
    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

> **Observação**   Chamar [**SurfaceImageSource::SetSource**](https://msdn.microsoft.com/library/windows/apps/br243255) (herdado de **IBitmapSource::SetSource**) lança atualmente uma exceção. Não o chame do seu objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041).

 

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource


[**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) estende a [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) quando o conteúdo é potencialmente maior que o que cabe na tela e, portanto, precisa ser virtualizado para ser renderizado de forma ideal.

[**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) difere de [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041) por usar um retorno de chamada, [**IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), que você implementa para atualizar regiões da superfície à medida que se elas tornam visíveis na tela. Você não precisa limpar as regiões que estão ocultas, pois a estrutura da XAML cuida disso para você.

Veja a seguir um processo básico para criar e atualizar um objeto [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) no codebehind:

1.  Crie uma instância de [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) no tamanho desejado. Por exemplo:

    `VirtualSurfaceImageSource^ virtualSIS = ref new VirtualSurfaceImageSource(2000, 2000);`

2.  Obtenha um ponteiro para [**IVirtualSurfaceImageSourceNative**](https://msdn.microsoft.com/library/windows/desktop/hh848328). Converta o objeto [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) como [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) ou [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) e chame [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) nele para obter a implementação **IVirtualSurfaceImageSourceNative** adjacente. Você pode usar os métodos definidos nessa implementação para definir o dispositivo e executar as operações de desenho.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    // ...
    IInspectable* vsisInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);
    vsisInspectable->QueryInterface(__uuidof(IVirtualSurfaceImageSourceNative), (void **)&m_vsisNative);
    ```

3.  Defina o dispositivo DXGI chamando [**IVirtualSurfaceImageSourceNative::SetDevice**](https://msdn.microsoft.com/library/windows/desktop/hh848325). Por exemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device>              m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext>           m_d3dContext;
    // ...
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext
            )
        );  
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    // ...
    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Chame [**IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334), transmitindo uma referência à sua implementação de [**IVirtualSurfaceUpdatesCallbackNative**](https://msdn.microsoft.com/library/windows/desktop/hh848336).

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }
    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    A estrutura chamará a sua implementação de [**IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848334) quando uma região de [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050) precisar ser atualizada.

    Isso pode acontecer quando a estrutura determinar que a região precisa ser desenhada (como quando o usuário faz panorâmicas ou amplia o modo de exibição da superfície) ou após o aplicativo ter chamado [**IVirtualSurfaceImageSourceNative::Invalidate**](https://msdn.microsoft.com/library/windows/desktop/hh848332) nessa região.

5.  Em [**IVirtualSurfaceImageSourceNative::UpdatesNeeded**](https://msdn.microsoft.com/library/windows/desktop/hh848337), use os métodos [**IVirtualSurfaceImageSourceNative::GetUpdateRectCount**](https://msdn.microsoft.com/library/windows/desktop/hh848329) e [**IVirtualSurfaceImageSourceNative::GetUpdateRects**](https://msdn.microsoft.com/library/windows/desktop/hh848330) para determinar quais regiões da superfície precisam ser desenhadas.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  

                  m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);
            std::unique_ptr<RECT[]> drawingBounds(new RECT[drawingBoundsCount]);
            m_vsisNative->GetUpdateRects(drawingBounds.get(), drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  Por último, para cada região que precisa ser atualizada:

    1.  Forneça um ponteiro para o objeto [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565) para [**IVirtualSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) e desenhe nessa superfície usando o DirectX. Somente a área especificada para atualização no parâmetro *updateRect* será desenhada.

        Como no caso de [**IlSurfaceImageSourceNative::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323), esse método retorna o deslocamento do ponto (x,y) do retângulo-alvo atualizado no parâmetro *offset*. Você pode usar esse deslocamento para determinar onde desenhar em [**IDXGISurface**](https://msdn.microsoft.com/library/windows/desktop/bb174565).

        > **Observação**   Você pode ter somente uma operação [**BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848323) pendente ativa por vez para cada [**IDXGIDevice**](https://msdn.microsoft.com/library/windows/desktop/bb174527).

         

        ```cpp
        ComPtr<IDXGISurface> bigSurface;

        HRESULT beginDrawHR = m_vsisNative->BeginDraw(updateRect, &bigSurface, &offset);
        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED || beginDrawHR == DXGI_ERROR_DEVICE_RESET)
        {
                  // Device changed
        }
        else
        {
            // Draw to IDXGISurface
        }
        ```

    2.  Desenhe o conteúdo específico nessa região, mas restrinja o seu desenho às regiões delimitadas para obter melhor desempenho.

    3.  Chame [**IVirtualSurfaceImageSourceNative::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/hh848324). O resultado será um bitmap.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel e jogos


[**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) é o tipo do Windows Runtime desenvolvido para dar suporte a gráficos de alto desempenho e jogos, no qual você gerencia a cadeia de troca diretamente. Nesse caso, você cria a sua própria cadeia de troca do DirectX e gerencia a apresentação do seu conteúdo renderizado.

Para assegurar um bom desempenho, há algumas limitações para o tipo [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834):

-   Há no máximo 4 instâncias de [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) por aplicativo.
-   Você deve definir a altura e a largura da cadeia de troca do DirectX (em [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como as dimensões atuais do elemento da cadeia de troca. Caso contrário, o conteúdo da exibição será ajustado (usando **DXGI\_SCALING\_STRETCH**) para caber.
-   Você deve definir o modo de dimensionamento da cadeia de troca do DirectX (em [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como **DXGI\_SCALING\_STRETCH**.
-   Não é possível definir o modo alfa da cadeia de troca do DirectX (em [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**.
-   Você deve criar a cadeia de troca do DirectX chamando [**IDXGIFactory2::CreateSwapChainForComposition**](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Atualize o [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) com base nas necessidades do seu aplicativo, e não as atualizações da estrutura da XAML. Se você precisar sincronizar as atualizações de **SwapChainPanel** com as da estrutura da XAML, registre-se para o evento [**Windows::UI::Xaml::Media::CompositionTarget::Rendering**](https://msdn.microsoft.com/library/windows/apps/br228127). Caso contrário, será necessário levar em consideração todos os problemas entre threads se você tentar atualizar os elementos XAML de um thread diferente daquele que atualiza **SwapChainPanel**.

Se você precisar receber a entrada do ponteiro de baixa latência de **SwapChainPanel**, use [**SwapChainPanel::CreateCoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Esse método retorna um objeto [**CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) que pode ser usado para receber eventos de entrada na latência mínima em um thread em segundo plano. Observe que, depois que esse método é chamado, eventos normais de entrada de ponteiro XAML não serão disparados para **SwapChainPanel**, já que todas as entradas serão redirecionadas para o thread em segundo plano.


> **Observação**   Em geral, seus aplicativos DirectX devem criar cadeias de troca na orientação paisagem e se igualar ao tamanho da janela de exibição (que normalmente é a resolução de tela nativa na maioria dos jogos da Windows Store). Isso garante que seu aplicativo use a implementação de cadeia de troca ideal quando não tiver nenhuma sobreposição XAML visível. Se o aplicativo for girado para o modo retrato, ele deverá chamar [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) na cadeia de troca existente, aplicar uma transformação ao conteúdo, se necessário, e depois chamar [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) novamente na mesma cadeia de troca. Da mesma forma, seu aplicativo deverá chamar **SetSwapChain** novamente na mesma cadeia de troca sempre que esta for redimensionada com uma chamada para [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577).


 

Veja a seguir um processo básico para criar e atualizar um objeto [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) no code behind:

1.  Obtenha uma instância individual de um painel de cadeia de troca para o seu aplicativo. As instâncias estão indicadas no XAML com a marca `<SwapChainPanel>`.

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    Essa é uma marca `<SwapChainPanel>` de exemplo.

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  Obtenha um ponteiro para [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143). Converta o objeto [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) como [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**) e chame **QueryInterface** nele para obter a implementação **ISwapChainPanelNative** adjacente.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Crie o dispositivo DXGI e a cadeia de troca e defina esta última como [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143) transmitindo-a para [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144).

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  Desenhe na cadeia de troca do DirectX e apresente-o para exibir o conteúdo.

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    Os elementos XAML são atualizados quando o Tempo de execução Windows organiza/renderiza sinais lógicos de uma atualização.

## <a name="related-topics"></a>Tópicos relacionados

* [**Win2D**](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [**VirtualSurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Guia de programação para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 





