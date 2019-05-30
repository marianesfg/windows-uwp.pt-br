---
description: Este tópico usa um exemplo de código completo do Direct2D para mostrar como usar C++/WinRT para consumir classes e interfaces COM.
title: Consumir componentes COM com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, COM, componente, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: dc4acd288496d83d5d91f1bdf206be19fe2fbb06
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361154"
---
# <a name="consume-com-components-with-cwinrt"></a>Consumir componentes COM com C++/WinRT

Você pode usar os recursos do [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) biblioteca para consumir componentes COM, como os gráficos 2D e 3D de alto desempenho das APIs do DirectX. C++/ WinRT é a maneira mais simples de usar o DirectX sem comprometer o desempenho. Este tópico usa um exemplo de código do Direct2D para mostrar como usar o C++/WinRT para consumir classes e interfaces COM. Você pode, claro, misturar programação COM e o tempo de execução do Windows dentro do mesmo C++projeto /WinRT.

No final deste tópico, você encontrará uma listagem de código do código-fonte completo de um aplicativo Direct2D mínimo. Vamos trechos de código de comparação de precisão e usá-los para ilustrar como consumir componentes COM usando o C++/WinRT usando vários recursos do C++biblioteca /WinRT.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Ponteiros inteligentes COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Quando você programa com, você trabalha diretamente com interfaces em vez de usar objetos (que também verdadeiros do segundo plano para APIs de tempo de execução do Windows, que são uma evolução do COM). Para chamar uma função em uma classe COM, por exemplo, ativar a classe, obter de volta uma interface e, em seguida, chamar funções nessa interface. Para acessar o estado de um objeto, você não acessar seus membros de dados diretamente. em vez disso, você deve chamar funções acessadores e modificadores em uma interface.

Para ser mais específico, estamos falando sobre como interagir com a interface *ponteiros*. E para fazer isso, podemos aproveitar a existência do tipo de ponteiro inteligente COM em C++/WinRT&mdash;as [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) tipo.

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

O código acima mostra como declarar um ponteiro inteligente não inicializado para uma [ **ID2D1Factory1** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1) interface COM. O ponteiro inteligente não foi inicializado, portanto, ele ainda não estiver apontando para um **ID2D1Factory1** interface pertencente a qualquer objeto real (ele não estiver apontando para uma interface em todos os). Mas ele tem o potencial para fazer isso; e (um ponteiro inteligente que está sendo) tem a capacidade por meio de referência COM contagem para gerenciar o tempo de vida do objeto proprietário da interface do qual ele aponta e para ser o meio pelo qual você chamar funções nessa interface.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Funções COM que retornam um ponteiro de interface como **void**

Você pode chamar o [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) função para gravar em um ponteiro inteligente não inicializado subjacente do ponteiro bruto.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

O código acima chamadas a [ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) função, que retorna um **ID2D1Factory1** por meio de seu último parâmetro, que tem o ponteiro de interface **void \* \***  tipo. Muitos COM as funções retornam um **void\*\*** . Para essas funções, use [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) conforme mostrado.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funções COM que retornam um ponteiro de interface específica

O [ **D3D11CreateDevice** ](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) retornos de função um [ **ID3D11Device** ](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) por meio de seu parâmetro terceiro da última, que tem o ponteiro de interface **ID3D11Device\* \***  tipo. Para funções que retornam um ponteiro de interface específica como essa, use [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

O exemplo de código na seção antes que este trecho mostra como chamar o raw **D2D1CreateFactory** função. Mas na verdade, quando o exemplo de código para este tópico chama **D2D1CreateFactory**, ele usa um modelo de função auxiliar que encapsula a API bruta e então, o exemplo de código realmente usa [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Funções COM que retornam um ponteiro de interface como **IUnknown**

O [ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) função retorna um ponteiro de interface de fábrica do DirectWrite por meio de seu último parâmetro, que tem [ **IUnknown** ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)tipo. Para função, use [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), mas reinterpretação convertido para **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Re-seat a **winrt::com_ptr**

> [!IMPORTANT]
> Se você tiver um [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) que já está encaixado (seu ponteiro bruto interno já tem um destino) e você deseja encaixá-la para apontar para um objeto diferente de novamente, em seguida, você primeiro precisará atribuir `nullptr` a ele&mdash;conforme mostrado no exemplo de código abaixo. Se você não fizer isso, em seguida, um já fixada **com_ptr** desenhará o problema para a sua atenção (quando você chama [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) ou [ **com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)), confirmando que seu ponteiro interno não é nulo.

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

Para verificar se o valor de um HRESULT retornado de uma função COM e lançar uma exceção no caso em que ele representa um código de erro, chame [ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funções COM que levam um ponteiro de interface específica

Você pode chamar o [ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) função passar seu **com_ptr** para uma função que usa um ponteiro de interface específica do mesmo tipo.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM funções que usam um **IUnknown** ponteiro de interface

Você pode chamar o [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) livre de função para passar seu **com_ptr** para uma função que usa um **IUnknown** interface ponteiro.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Ponteiros inteligentes de passar e retornar COM

Uma função usando um ponteiro inteligente COM na forma de um **winrt::com_ptr** deve fazer isso por referência constante, ou por referência.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Uma função que retorna um **winrt::com_ptr** deve fazer isso por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Um ponteiro inteligente COM uma interface diferente de consulta

Você pode usar o [ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) função para consultar um ponteiro inteligente COM uma interface diferente. A função gera uma exceção se a consulta não tiver êxito.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Como alternativa, use [ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), que retorna um valor que você pode verificar em relação a `nullptr` para ver se a consulta foi bem-sucedida.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Listagem de código do código-fonte completo de um aplicativo Direct2D mínimo

Se você quiser compilar e executar esse exemplo de código-fonte, em seguida, primeiro, no Visual Studio, crie uma nova **aplicativo Core (C++/WinRT)** . `Direct2D` é um nome razoável para o projeto, mas você pode nomeá-que desejar. Abra `App.cpp`, excluir todo o seu conteúdo e cole na lista abaixo.

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

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

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
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
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Trabalhando com tipos de COM, como BSTR e VARIANTE

Como você pode ver, C++/WinRT fornece suporte para implementar e chamar interfaces COM. Para usar tipos COM, como o BSTR e VARIANTE, sempre há a opção de usá-los em sua forma bruta (junto com as APIs apropriadas). Como alternativa, você pode usar wrappers fornecidos por uma estrutura como o [biblioteca ATL (Active Template)](/cpp/atl/active-template-library-atl-concepts), ou pelo compilador do Visual C++ [suporte COM](/cpp/cpp/compiler-com-support), ou até mesmo por seus próprios wrappers.

## <a name="important-apis"></a>APIs Importantes
* [função WinRT::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [WinRT::com_ptr struct modelo](/uwp/cpp-ref-for-winrt/com-ptr)
* [struct WinRT::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
