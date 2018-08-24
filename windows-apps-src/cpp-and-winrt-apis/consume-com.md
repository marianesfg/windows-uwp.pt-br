---
author: stevewhims
description: Este tópico usa um exemplo de código completo Direct2D para mostrar como usar C + + / WinRT consumam classes e interfaces COM.
title: Consumir DirectX e outras APIs COM C + + / WinRT
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, COM, componente, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: b87eb90ed5ecf731cc851e81e81ad016956e5fea
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2837088"
---
# <a name="consume-directx-and-other-com-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Consumir DirectX e outras APIs COM [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Você pode usar as instalações do C + + / biblioteca WinRT consumir componentes COM, como os gráficos 2D e 3D de alto desempenho das APIs do DirectX. C + + / WinRT é a maneira mais simples para usar o DirectX sem comprometer o desempenho. Este tópico usa um exemplo de código Direct2D para mostrar como usar C + + / WinRT consumam classes e interfaces COM. Você pode, obviamente, misturar tempo de execução do Windows e COM programação dentro do mesmo C + + / WinRT project.

No final deste tópico, você encontrará uma listagem de código fonte completo de um aplicativo de Direct2D mínimo. Vamos Levante trechos de código e usá-los para ilustrar a consumir componentes COM usando C + + / WinRT usando várias instalações do C + + / biblioteca WinRT.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Ponteiros inteligentes de COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Quando você programa com, trabalhe diretamente com interfaces, e não com objetos (Isso também true nos bastidores para APIs de tempo de execução do Windows, que são uma evolução do COM). Para chamar uma função em uma classe COM, por exemplo, você ativar a classe, obtenha uma interface novamente e, em seguida, chamar funções nessa interface. Para acessar o estado de um objeto, você não acessar seus membros de dados diretamente; em vez disso, você deve chamar funções acessador e modificadores em uma interface.

Para ser mais específico, estamos falando sobre como interagir com os *ponteiros*de interface. E para fazer isso, podemos beneficiar a existência do tipo de ponteiro inteligente COM em C + + / WinRT&mdash;o tipo de [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) .

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

O código acima mostra como declarar um ponteiro inteligente não inicializado à interface COM [**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) . O ponteiro inteligente não foi inicializado, portanto, ela ainda não estiver apontando para uma interface **ID2D1Factory1** que pertencem a qualquer objeto real (ele não está apontando para uma interface nisso). Mas ele tem o potencial de fazê-lo; e (sendo um ponteiro inteligente) tem a capacidade via para gerenciar o tempo de vida do objeto proprietário da interface que ela aponta para e para ser o meio pelo qual você chamar funções na interface de contagem de referência de COM.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Funções de COM que retornam um ponteiro de interface como **void\ * \ ***

É possível chamar a função [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) para gravar um não inicializado ponteiro inteligente subjacente bruto ponteiro.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

O código acima chama a função [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) , que retorna um ponteiro de interface **ID2D1Factory1** via seu último parâmetro, que tem **void\ * \ *** tipo. Muitos COM as funções retornam um **void\ * \ ***. Para tais funções, use [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) , conforme mostrado.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funções de COM que retornam um ponteiro de interface específica

A função [**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retorna um ponteiro de interface [**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596) via seu parâmetro antepenultimate, que tem **ID3D11Device\ * \ *** tipo. Para funções que retornam um ponteiro de interface específica como esse, use [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

O exemplo de código na seção antes deste mostra como chamar a função de **D2D1CreateFactory** bruta. Mas, na verdade, quando o exemplo de código deste tópico chama **D2D1CreateFactory**, ele usa um modelo de função auxiliar que envolve a API bruta e portanto o exemplo de código usa realmente [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Funções de COM que retornam um ponteiro de interface como **IUnknown\ * \ ***

A função [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retorna um ponteiro de interface de fábrica DirectWrite via seu último parâmetro, que tem **IUnknown\ * \ *** tipo. Função, use [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function), mas reinterpret convertida a **IUnknown\ * \ ***.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Reconecte um **winrt::com_ptr**

> [!IMPORTANT]
> Se você tem um [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que já estiver encaixado (seu ponteiro bruto interno já possui um destino) e você deseja reconecte-lo para apontar para um objeto diferente, primeiro você precisa atribuir `nullptr` a ela&mdash;conforme mostrado no exemplo de código abaixo. Se não fizer isso, então um fixada já **com_ptr** será desenha o problema para sua atenção (quando você chamar [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) ou [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) declarando que seu ponteiro interno não for nulo.

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>Lidar com códigos de erro HRESULT

Para verificar se que o valor de um HRESULT retornados de uma função COM e acionar uma exceção que representa um código de erro, chame [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funções de COM que utilizem um ponteiro de interface específica

É possível chamar a função [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) para passar seu **com_ptr** a uma função que usa um ponteiro de interface específica do mesmo tipo.

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Funções de COM que utilizem um ponteiro de interface **IUnknown**

É possível chamar a função de livre [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) para passar seu **com_ptr** a uma função que leva um ponteiro de interface **IUnknown** .

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Passando e retornando COM ponteiros de inteligentes

Uma função aproveitando um ponteiro inteligente de COM em forma de um **winrt::com_ptr** deverá fazê-lo por referência constante, ou referência.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Uma função que retorna um **winrt::com_ptr** deverá fazê-lo por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Consultar um ponteiro inteligente de COM para uma interface diferente

Você pode usar a função [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) para consultar um ponteiro inteligente de COM para uma interface diferente. A função gera uma exceção se a consulta não for bem sucedida.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Como alternativa, use [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function), que retorna um valor que você pode verificar contra `nullptr` para ver se a consulta foi bem-sucedida.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Listagem de código fonte completo de um aplicativo de Direct2D mínimo

Se você deseja criar e executar este primeiro, efetue um exemplo de código fonte no Visual Studio, crie um novo **App Core (C + + / WinRT)**. `Direct2D` é um nome para o projeto de razoável, mas você pode atribuir o nome que desejar. Open `App.cpp`, excluir todo o seu conteúdo e colá-lo na lista abaixo.

```cppwinrt
#include "pch.h"

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App());
}
```

## <a name="important-apis"></a>APIs importantes
* [WinRT::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
