---
author: joannaleecy
title: Estender o exemplo de jogo
description: Saiba como implementar uma sobreposição XAML para um jogo UWP DirectX.
keywords: DirectX, XAML
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cb837965746eb1c2c7deab613eec239a83cac294
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986683"
---
# <a name="extend-the-game-sample"></a>Estender o exemplo de jogo

A esta altura, já abordamos os componentes principais de um jogo UWP (Plataforma Universal do Windows) DirectX 3D básico. Você pode configurar a estrutura de um jogo, inclusive o provedor de exibição e o pipeline de renderização, e implementar um loop básico de jogo. Você também pode criar uma sobreposição de interface do usuário básica, incorporar sons e implementar controles. Você está no caminho certo para criar seu próprio jogo, mas, se houver necessidade de mais ajuda e informações, confira esses recursos.

-   [Elementos gráficos e jogos do DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Visão geral do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Referência do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>Usando o XAML para a sobreposição


Uma alternativa sobre a qual não falamos com detalhes é o uso de XAML em vez do [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) para a sobreposição. O XAML tem muitos benefícios sobre o Direct2D no desenho dos elementos da interface do usuário. O benefício mais importante é que ele faz a incorporação da aparência do Windows 10 ao seu jogo do DirectX mais conveniente. Muitos dos elementos, estilos e comportamentos comuns que definem um aplicativo UWP são integrados de forma muito próxima ao modelo XAML, fazendo com que o desenvolvedor do jogo tenha bem menos trabalho na implementação. Se o design do seu jogo tiver uma interface do usuário complexa, considere o uso de XAML em vez de Direct2D.

Com o XAML, podemos fazer com que a interface do jogo fique parecida com a do Direct2D feita antes.

### <a name="xaml"></a>XAML
![Sobreposição do XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Sobreposição do D2D](./images/simple-dx-game-extend-d2d.PNG)

Embora apresentem resultados finais semelhantes, há várias diferenças entre a implementação das interfaces de Direct2D e XAML.

Recurso | XAML| Direct2D
:----------|:----------- | :-----------
Definindo a sobreposição | Definido em um arquivo XAML, `\*.xaml`. Após entender o XAML, a criação e a configuração de sobreposições mais complicadas tornam-se mais simples em comparação ao Direct2D.| Definido como um conjunto de primitivas Direct2D e cadeias [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) posicionadas manualmente e escritas em um buffer de destino Direct2D. 
Elementos da interface do usuário | Os elementos de interface do usuário do XAML vêm de elementos padronizados que são parte das APIs XAML do Windows Runtime, incluindo [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) e [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) O código que manipula o comportamento dos elementos da interface do usuário em XAML é definido em um arquivo codebehind, Main.xaml.cpp. | Formas simples podem ser desenhadas, como retângulos e elipses.
Redimensionamento da janela | Manipula naturalmente os eventos de mudança de estado de redimensionamento e visualização, transformando a sobreposição adequadamente | Necessidade de especificar manualmente como redesenhar os componentes da sobreposição.


Outra grande diferença diz respeito à [cadeia de troca](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains). Não é preciso anexar a cadeia de troca a um objeto [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Em vez disso, um aplicativo em DirectX que incorpora linguagem XAML associa uma cadeia de troca quando um novo objeto [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) é construído. 

O seguinte trecho de código mostra como declarar o XAML para o **SwapChainPanel** no arquivo [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml).
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

O objeto **SwapChainPanel** é definido como a propriedade [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) do objeto de janela atual, criado no momento da [at inicialização](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) pelo singleton do aplicativo.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


Para associar a cadeia de troca configurada à instância [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) definida por XAML, você deve obter um ponteiro para a implementação da interface nativa [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) subjacente e chamar [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) nela, passando a cadeia de troca configurada. 

O seguinte trecho de código de [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) detalha isso para a interoperabilidade DirectX/XAML:

```cpp
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

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Para obter mais informações sobre esse processo, consulte [Interoperabilidade entre DirectX e XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Exemplo

Para baixar a versão desse jogo de exemplo que usa XAML para sobreposição, vá para o [exemplo de jogo de tiro Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).


Diferentemente da versão do exemplo de jogo abordada no restante destes tópicos, a versão XAML define sua estrutura nos arquivos [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) e [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp), em vez de [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) e [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp), respectivamente.