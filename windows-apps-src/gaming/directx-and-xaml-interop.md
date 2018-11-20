---
author: mtoepke
title: Interoperabilidade entre DirectX e XAML
description: Você pode usar a XAML (Extensible Application Markup Language) e o Microsoft DirectX juntos no seu jogo UWP (Plataforma Universal do Windows).
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, interoperabilidade com xaml
ms.localizationpriority: medium
ms.openlocfilehash: 7f3a70be3dd31b0a5e4214621ab9fb4efa72cc54
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7290958"
---
# <a name="directx-and-xaml-interop"></a>Interoperabilidade entre DirectX e XAML



Você pode usar a XAML (Extensible Application Markup Language) e o Microsoft DirectX juntos no seu jogo ou aplicativo UWP (Plataforma Universal do Windows). A combinação da XAML e do DirectX permite que você crie estruturas de interface de usuário flexíveis que interoperem com o seu conteúdo renderizado em DirectX e é particularmente útil para aplicativos com muitos gráficos. Este tópico explica a estrutura de um aplicativo UWP que usa DirectX e identifica os tipos importantes a serem usados na criação do seu aplicativo UWP para funcionar com o DirectX.

Se seu app se concentrar principalmente na renderização 2D, convém usar biblioteca [Win2D](https://github.com/microsoft/win2d) do Windows Runtime. Essa biblioteca é mantida pela Microsoft e criada com base nas tecnologias principais de Direct2D. Ela simplifica bastante o padrão de uso para implementar os gráficos 2D e inclui abstrações úteis para algumas das técnicas descritas neste documento. Consulte a página de projeto para obter mais detalhes. Esse documento aborda orientações para desenvolvedores de aplicativos que optam por *não* usar Win2D.

> **Observação**APIs do DirectX não são definidas como tipos de tempo de execução do Windows, portanto, você normalmente usa extensões de componente VisualC + + (C++ c++ /CX) para desenvolver componentes XAML UWP que interoperam com o DirectX. Além disso, você pode criar um aplicativo UWP em C# e XAML que usa DirectX se encapsular as chamadas DirectX em um arquivo de metadados do Windows Runtime separado.

 

## <a name="xaml-and-directx"></a>XAML e DirectX

O DirectX fornece duas bibliotecas poderosas para gráficos 2D e 3D: Direct2D e Microsoft Direct3D. Embora a XAML dê suporte para primitivos e efeitos básicos 2D, muitos aplicativos, como de modelagem e jogos, precisam de suporte gráfico mais complexo. Para esses, você pode usar o Direct2D e o Direct3D para renderizar parte dos gráficos, ou todos eles, e usar a XAML para todo o resto.

Se estiver implementando interoperabilidade personalizada entre XAML e DirectX, você precisa conhecer estes dois conceitos:

-   As superfícies compartilhadas são regiões do tamanho da tela, definidas por XAML, em que você pode usar o DirectX para desenhar indiretamente, usando os tipos  [Windows::UI::Xaml::Media::ImageSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx). Para superfícies compartilhadas, você não controla o tempo preciso para quando o novo conteúdo é exibido na tela. Em vez disso, as atualizações para a superfície compartilhada são sincronizadas para atualizar a estrutura do XAML.
-   [Cadeias de troca](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx) representam uma coleção de buffers usados para exibir elementos gráficos com latência mínima. Normalmente, as cadeias de troca são atualizadas em 60 quadros por segundo separadamente do thread da interface do usuário. No entanto, as cadeias de troca usam mais memória e recursos da CPU para dar suporte a atualizações rápidas e são mais difíceis de usar, pois é necessário gerenciar vários threads.

Reflita sobre porque você está usando o DirectX. Ele será usado para compor ou animar um único controle que se encaixa dentro das dimensões da janela de exibição? Ela conterá a saída que precisa ser renderizada e controlada em tempo real, como em um jogo? Em caso afirmativo, você provavelmente precisará implementar uma cadeia de troca. Caso contrário, deve ser recomendado usando uma superfície compartilhada.

Depois de determinar como pretende usar o DirectX, você pode usar um destes tipos de Windows Runtime para incorporar renderização do DirectX em seu aplicativo UWP:

-   Se quiser compor uma imagem estática ou desenhar uma imagem nos intervalos acionados por eventos, desenhe em uma superfície compartilhada com [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041). Esse tipo lida com uma superfície de desenho dimensionada do DirectX. Normalmente, você usa esse tipo ao compor uma imagem ou textura como um bitmap para exibição em um documento ou elemento da interface do usuário. Ele não funciona bem para interatividade em tempo real, como para um jogo de alto desempenho. Isso ocorre porque as atualizações em um objeto **SurfaceImageSource** são sincronizadas com as atualizações da interface do usuário da XAML, e isso pode introduzir latência no feedback visual que você fornece ao usuário, como uma taxa de quadros flutuante ou uma resposta fraca percebida para entrada em tempo real. Ainda assim, as atualizações são suficientemente rápidas para controles dinâmicos ou simulações de dados!

-   Se a imagem for maior do que o estado real da tela fornecido, e ser puder ser estendida ou ampliada pelo usuário, use [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050). Esse tipo trata uma superfície de desenho dimensionada do DirectX que é maior que a tela. Como [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041), você usa isso ao compor uma imagem ou um controle complexo dinamicamente. E, assim como **SurfaceImageSource**, ele não funciona bem para jogos de alto desempenho. Alguns exemplos de elementos XAML que podem usar um **VirtualSurfaceImageSource** são controles de mapa ou um visualizador de documentos para imagens grandes e densas.

-   Se você estiver usando o DirectX para apresentar gráficos atualizados em tempo real ou em uma situação na qual as atualizações devem vir em intervalos regulares de baixa latência, use a classe [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) para que você possa atualizar os gráficos sem sincronizar com o timer de atualização da estrutura da XAML. Esse tipo permite acessar diretamente a cadeia de troca do dispositivo gráfico ([IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631)) e a camada XAML sobre o destino da renderização. Esse tipo funciona muito bem para jogos e aplicativos do DirectX em tela inteira que necessitam de uma interface de usuário baseada em XAML. É necessário conhecer bem o DirectX para usar essa abordagem, incluindo as tecnologias Microsoft DirectX Graphics Infrastructure (DXGI), Direct2D e Direct3D. Para saber mais, consulte [Guia de programação para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345).

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) fornece superfícies compartilhadas do Microsoft DirectX para desenhar e depois compõe os bits no conteúdo do aplicativo.

Veja a seguir um processo básico para criar e atualizar um objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) no code-behind:

1.  Defina o tamanho da superfície compartilhada transmitindo a altura e a largura ao construtor [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041). Você também pode indicar se a superfície precisa de suporte alfa (opacidade).

    Por exemplo:

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  Obtenha um ponteiro para [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137). Converta o objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) como [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**) e chame **QueryInterface** nele para obter a implementação **ISurfaceImageSourceNativeWithD2D** adjacente. Você pode usar os métodos definidos nessa implementação para definir o dispositivo e executar as operações de desenho.

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  Crie os dispositivos DXGI e D2D ao chamar primeiro [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) e [D2D1CreateDevice](https://msdn.microsoft.com/library/windows/desktop/hh404272(v=vs.85).aspx) e então passe o dispositivo e o contexto para [ISurfaceImageSourceNativeWithD2D::SetDevice](https://msdn.microsoft.com/library/dn302141.aspx). 

    > [!NOTE]
    > Se for desenhar em sua **SurfaceImageSource** de um thread em segundo plano, você também precisará garantir que o dispositivo DXGI tenha habilitado o acesso com multithread. Isso só deverá ser feito se você pretende desenhar de um thread em segundo plano, por motivos de desempenho.

    Por exemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
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
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  Forneça um ponteiro para o objeto [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/bb174565) para [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://msdn.microsoft.com/library/dn302138.aspx)e use o contexto de desenho retornado para desenhar o conteúdo do retângulo desejado dentro da **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D::BeginDraw** e os comandos de desenho podem ser chamados de um thread em segundo plano. Somente a área especificada para atualização no parâmetro *updateRect* é desenhada.

    Este método retorna o deslocamento do ponto (x,y) do retângulo de destino atualizado no parâmetro *offset*. Você pode usar esse deslocamento para determinar onde desenhar o conteúdo atualizado com o **ID2D1DeviceContext**.

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. Chame [ISurfaceImageSourceNativeWithD2D::EndDraw](https://msdn.microsoft.com/library/dn302139.aspx) para concluir o bitmap. O bitmap pode ser usado como uma origem de um XAML [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) ou [ImageBrush](https://msdn.microsoft.com/library/windows/apps/br210101). **ISurfaceImageSourceNativeWithD2D::EndDraw** deve ser chamado apenas do thread da interface do usuário.

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > Atualmente, chamar [SurfaceImageSource::SetSource](https://msdn.microsoft.com/library/windows/apps/br243255) (herdado de **IBitmapSource::SetSource**) lança uma exceção. Não o chame do seu objeto [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041).

    > [!NOTE]
    > Os aplicativos devem evitar desenhar em **SurfaceImageSource** enquanto sua [Janela](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) associada fica oculta, caso contrário, as APIs de **ISurfaceImageSourceNativeWithD2D** falharão. Para fazer isso, registre-se como um ouvinte de eventos para o evento [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para controlar alterações de visibilidade.

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) estende a [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) quando o conteúdo é potencialmente maior que o que cabe na tela e, portanto, precisa ser virtualizado para ser renderizado de forma ideal.

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) difere de [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) por usar um retorno de chamada, [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337), que você implementa para atualizar regiões da superfície à medida que se elas tornam visíveis na tela. Você não precisa limpar as regiões que estão ocultas, pois a estrutura da XAML cuida disso para você.

Veja a seguir o processo básico para criar e atualizar um objeto [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) no code-behind:

1.  Crie uma instância de [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) no tamanho desejado. Por exemplo:

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  Obtenha os ponteiros para [IVirtualSurfaceImageSourceNative](https://msdn.microsoft.com/library/windows/desktop/hh848328) e [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137). Converta o objeto [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) como [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) ou [IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509), e chame [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521) nele para obter as implementações **IVirtualSurfaceImageSourceNative** e **ISurfaceImageSourceNativeWithD2D** subjacentes. Você pode usar os métodos definidos nessas implementações para definir o dispositivo e executar as operações de desenho.

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  Crie os dispositivos DXGI e D2D ao chamar primeiro **D3D11CreateDevice** e **D2D1CreateDevice** e então passe o dispositivo D2D para **ISurfaceImageSourceNativeWithD2D::SetDevice**.

    > [!NOTE]
    > Se for desenhar em sua **VirtualSurfaceImageSource** de um thread em segundo plano, você também precisará garantir que o dispositivo DXGI tenha habilitado o acesso com multithread. Isso só deverá ser feito se você pretende desenhar de um thread em segundo plano, por motivos de desempenho.

    Por exemplo:

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
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
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  Chame [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) passando uma referência para sua implementação de [IVirtualSurfaceUpdatesCallbackNative](https://msdn.microsoft.com/library/windows/desktop/hh848336).

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

    A estrutura chamará a sua implementação de [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) quando uma região da [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) precisar ser atualizada.

    Isso pode acontecer quando a estrutura determinar que a região precisa ser desenhada (como quando o usuário faz panorâmicas ou amplia o modo de exibição da superfície) ou após o aplicativo ter chamado [IVirtualSurfaceImageSourceNative::Invalidate](https://msdn.microsoft.com/library/windows/desktop/hh848332) nessa região.

5.  Em [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337), use os métodos [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://msdn.microsoft.com/library/windows/desktop/hh848329) e [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://msdn.microsoft.com/library/windows/desktop/hh848330) para determinar quais regiões da superfície precisam ser desenhadas.

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
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

    1.  Forneça um ponteiro para o objeto **ID2D1DeviceContext** para **ISurfaceImageSourceNativeWithD2D::BeginDraw**e use o contexto de desenho retornado para desenhar o conteúdo do retângulo desejado dentro da **SurfaceImageSource**. **ISurfaceImageSourceNativeWithD2D::BeginDraw** e os comandos de desenho podem ser chamados de um thread em segundo plano. Somente a área especificada para atualização no parâmetro *updateRect* é desenhada.

        Este método retorna o deslocamento do ponto (x,y) do retângulo de destino atualizado no parâmetro *offset*. Você pode usar esse deslocamento para determinar onde desenhar o conteúdo atualizado com o **ID2D1DeviceContext**.

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  Desenhe o conteúdo específico nessa região, mas restrinja o seu desenho às regiões delimitadas para obter melhor desempenho.

    3.  Chame **ISurfaceImageSourceNativeWithD2D::EndDraw**. O resultado será um bitmap.

> [!NOTE]
> Os aplicativos devem evitar desenhar em **SurfaceImageSource** enquanto sua [Janela](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) associada fica oculta, caso contrário, as APIs de **ISurfaceImageSourceNativeWithD2D** falharão. Para fazer isso, registre-se como um ouvinte de eventos para o evento [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) para controlar alterações de visibilidade.

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel e jogos


[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) é o tipo do Windows Runtime desenvolvido para dar suporte a gráficos de alto desempenho e jogos, no qual você gerencia a cadeia de troca diretamente. Nesse caso, você cria a sua própria cadeia de troca do DirectX e gerencia a apresentação do seu conteúdo renderizado.

Para assegurar um bom desempenho, há algumas limitações para o tipo [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834):

-   Há, no máximo, quatro instâncias de [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) por app.
-   Você deve definir a altura e a largura da cadeia de troca do DirectX (em [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como as dimensões atuais do elemento da cadeia de troca. Caso contrário, o conteúdo da exibição será ajustado (usando **DXGI\_SCALING\_STRETCH**) para caber.
-   Você deve definir o modo de dimensionamento da cadeia de troca do DirectX (em [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como **DXGI\_SCALING\_STRETCH**.
-   Não é possível definir o modo alfa da cadeia de troca do DirectX (em [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528)) como **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**.
-   Você deve criar a cadeia de troca do DirectX chamando [IDXGIFactory2::CreateSwapChainForComposition](https://msdn.microsoft.com/library/windows/desktop/hh404558).

Atualize o [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) com base nas necessidades do seu aplicativo, e não as atualizações da estrutura do XAML. Se você precisar sincronizar as atualizações de **SwapChainPanel** com as da estrutura da XAML, registre-se no evento [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://msdn.microsoft.com/library/windows/apps/br228127). Caso contrário, será necessário levar em consideração todos os problemas entre threads se você tentar atualizar os elementos XAML de um thread diferente daquele que atualiza **SwapChainPanel**.

Se você precisar receber a entrada do ponteiro de baixa latência de **SwapChainPanel**, use [SwapChainPanel::CreateCoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource). Esse método retorna um objeto [CoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) que pode ser usado para receber eventos de entrada na latência mínima em um thread em segundo plano. Observe que, depois que esse método é chamado, eventos normais de entrada de ponteiro XAML não serão disparados para **SwapChainPanel**, já que todas as entradas serão redirecionadas para o thread em segundo plano.


> **Observação**   Em geral, seus aplicativos DirectX devem criar cadeias de troca na orientação paisagem e se igualar ao tamanho da janela de exibição (que normalmente é a resolução de tela nativa na maioria dos jogos da Microsoft Store). Isso garante que seu aplicativo use a implementação de cadeia de troca ideal quando não tiver nenhuma sobreposição XAML visível. Se o aplicativo for girado para o modo retrato, ele deverá chamar [IDXGISwapChain1::SetRotation](https://msdn.microsoft.com/library/windows/desktop/hh446801) na cadeia de troca existente, aplicar uma transformação ao conteúdo, se necessário, e depois chamar [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144) novamente na mesma cadeia de troca. Da mesma forma, seu aplicativo deverá chamar **SetSwapChain** novamente na mesma cadeia de troca sempre que esta for redimensionada com uma chamada para [IDXGISwapChain::ResizeBuffers](https://msdn.microsoft.com/library/windows/desktop/bb174577).


 

Veja a seguir um processo básico para criar e atualizar um objeto [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) no code-behind:

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

2.  Obtenha um ponteiro para [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143). Converta o objeto [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) como [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) (ou **IUnknown**) e chame **QueryInterface** nele para obter a implementação **ISwapChainPanelNative** subjacente.

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  Crie o dispositivo DXGI e a cadeia de troca e defina esta última como [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) passando-a para [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144).

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

* [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Guia de programação para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




