---
description: Este tópico usa um exemplo de código completo do Direct2D para mostrar como usar C++/WinRT para consumir classes e interfaces COM.
title: Consumir componentes COM com C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, COM, componente, classe, interface
ms.localizationpriority: medium
ms.openlocfilehash: 6a286056fc0c44d01482e23e52df0fa80eca0515
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218516"
---
# <a name="consume-com-components-with-cwinrt"></a>Consumir componentes COM com C++/WinRT

Você pode usar as instalações da biblioteca do [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para consumir componentes COM, como gráficos 2D e 3D de alto desempenho das APIs do DirectX. O C++/ WinRT é a maneira mais simples de usar o DirectX sem comprometer o desempenho. Este tópico usa um exemplo de código do Direct2D para mostrar como usar C++/WinRT para consumir classes e interfaces COM. Obviamente, você pode misturar a programação do Windows Runtime e COM no mesmo projeto de C++/WinRT.

No final deste tópico, você encontrará uma listagem completa de código-fonte de um aplicativo mínimo do Direct2D. Vamos ressaltar extratos desse código e usá-los para ilustrar como consumir componentes COM usando C++/WinRT com várias instalações da biblioteca do C++/WinRT.

## <a name="com-smart-pointers-winrtcom_ptr"></a>Ponteiros inteligentes COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Ao programar usando COM, você trabalha diretamente com interfaces, em vez de projetos (isso também se aplica aos bastidores para APIs do Windows Runtime, que são uma evolução do COM). Para chamar uma função em uma classe COM, por exemplo, ative a classe, obtenha de volta uma interface e, em seguida, chame funções nessa interface. Para acessar o estado de um objeto, não acesse seus membros de dados diretamente; em vez disso, chame as funções de acessador e modificador em uma interface.

Para ser mais específico, estamos falando sobre como interagir com os *ponteiros* da interface. E para fazer isso, aproveitamos a existência do tipo de ponteiro inteligente COM em C++/WinRT&mdash;o tipo [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr).

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

O código acima mostra como declarar um ponteiro inteligente não inicializado para uma interface COM [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1). O ponteiro inteligente não foi inicializado, portanto, ele ainda não está apontando para uma interface **ID2D1Factory1** pertencendo a qualquer objeto real (não está apontando para uma interface). Mas ele tem potencial para isso; e (sendo um ponteiro inteligente) tem a capacidade por meio da contagem de referência COM de gerenciar o tempo de vida do objeto proprietário da interface para o qual ele aponta, assim como para ser o meio pelo qual você chama funções nessa interface.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Funções COM que retornam um ponteiro de interface como **void**

Você pode chamar a função [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) para gravar em um ponteiro bruto subjacente do ponteiro inteligente não inicializado.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

O código acima chama a função [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory), que retorna um ponteiro de interface **ID2D1Factory1** por meio de seu último parâmetro, que tem o tipo **void\*\*** . Muitas funções COM retornam um **void\*\*** . Para essas funções, use [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function), conforme mostrado.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funções COM que retornam um ponteiro de interface específico

A função [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) retorna um ponteiro de interface [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) por meio de seu terceiro parâmetro, a contar do fim, que tem o tipo **ID3D11Device\*\*** . Para funções que retornam um ponteiro de interface específico como esse, use [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

O exemplo de código na seção anterior mostra como chamar a função **D2D1CreateFactory** bruta. Mas, na verdade, quando o exemplo de código para este tópico chama **D2D1CreateFactory**, ele usa um modelo de função auxiliar que encapsula a API bruta, assim, o exemplo de código, de fato, usa [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Funções COM que retornam um ponteiro de interface como **IUnknown**

A função [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) retorna um ponteiro de interface de fábrica DirectWrite por meio de seu último parâmetro, que tem o tipo [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Para essa função, use [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), mas reinterprete a transmissão disso para **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcom_ptr"></a>Restabelecer um **winrt::com_ptr**

> [!IMPORTANT]
> Se você tiver um [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que já esteja estabelecido (seu ponteiro bruto interno já tem um destino) e deseja restabelecê-lo para apontar para outro objeto, primeiramente, é preciso atribuir `nullptr` a ele&mdash; conforme mostrado no exemplo de código abaixo. Se não o fizer, um **com_ptr** já estabelecido chamará sua atenção para o problema (quando você chamar [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) ou [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) afirmando que seu ponteiro interno não é nulo.

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

## <a name="handle-hresult-error-codes"></a>Tratar códigos de erro HRESULT

Para verificar o valor de um HRESULT retornado de uma função COM e lançar uma exceção no caso em que ele representar um código de erro, chame [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funções COM que adotam um ponteiro de interface específico

Você pode chamar a função [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) para passar seu **com_ptr** para uma função que usa um ponteiro de interface específico do mesmo tipo.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Funções COM que adotam um ponteiro de interface **IUnknown**

Você pode chamar a função [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown) para passar seu **com_ptr** para uma função que usa um ponteiro de interface **IUnknown**. Confira este tópico para obter um exemplo de código.

## <a name="passing-and-returning-com-smart-pointers"></a>Passando e retornando ponteiros inteligentes COM

Uma função usando um ponteiro inteligente COM na forma de um **winrt::com_ptr** deve fazer isso por referência de constante, ou por referência.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Uma função que retorna um **winrt::com_ptr** deve fazer isso por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Consultar um ponteiro inteligente COM para uma interface diferente

Você pode usar a função [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) para consultar um ponteiro inteligente COM para uma interface diferente. A função vai gerar uma exceção se a consulta não tiver êxito.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Como alternativa, use [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), que retorna um valor que você pode verificar em relação a `nullptr` para saber se a consulta foi bem-sucedida.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Listagem completa de código-fonte de um aplicativo Direct2D mínimo

Se você quiser compilar e executar esse exemplo de código-fonte, primeiramente, no Visual Studio, crie um novo **aplicativo de núcleo (C++/WinRT)** . `Direct2D` é um nome razoável para o projeto, mas você pode nomeá-lo como desejar.

Abra `pch.h` e adicione `#include <unknwn.h>` imediatamente após a inclusão de `windows.h`. Isso ocorre porque estamos usando [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/get-unknown). É uma boa ideia explicitar `#include <unknwn.h>` sempre que você usar **winrt ::get_unknown**, mesmo que esse cabeçalho tenha sido incluído por outro cabeçalho.

Abra `App.cpp`, exclua todo o seu conteúdo e cole na lista abaixo.

O código a seguir usa a função [winrt::com_ptr::capture](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function) sempre que possível. `WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

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
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

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

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Trabalhando com os tipos de COM, como BSTR e VARIANT

Como você pode ver, C++/WinRT fornece suporte para implementar e chamar interfaces COM. Para usar tipos de COM, como BSTR e VARIANT, é recomendável usar wrappers fornecidos pelas [WILs (Bibliotecas de Implementação do Windows)](https://github.com/Microsoft/wil), como **wil::unique_bstr** e **wil::unique_variant** (que gerenciam os tempos de vida dos recursos).

A [WIL](https://github.com/Microsoft/wil) substitui estruturas como a ATL (Active Template Library) e o suporte COM do compilador do Visual C++. E é recomendável substituir seus próprios wrappers ou usar tipos de COM como BSTR e VARIANT na respectiva forma bruta (com as APIs apropriadas).

## <a name="avoiding-namespace-collisions"></a>Evitar conflitos de namespace

É prática comum em C++/WinRT, &mdash;como a listagem de código neste tópico demonstra&mdash;, usar diretivas de uso livremente. Em alguns casos, no entanto, isso pode levar ao problema de importar nomes conflitantes para o namespace global. Aqui está um exemplo.

C++/WinRT contém um tipo denominado [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown); enquanto COM define um tipo denominado [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Sendo assim, considere o código a seguir, em um projeto de C++/WinRT que consome cabeçalhos COM.

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

O nome não qualificado *IUnknown* entra em conflito com o namespace global, daí o erro do compilador de *símbolo ambíguo*. Em vez disso, você pode isolar a versão C++/WinRT do nome no namespace **winrt**, dessa forma.

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Ou, se quiser a conveniência de `using namespace winrt`, você pode. Você só precisa qualificar a versão global de *IUnknown*, dessa forma.

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Naturalmente, isso funciona com qualquer namespace de C++/WinRT.

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

Você pode, em seguida, fazer referência a **winrt::Windows::Storage::StorageFile**, por exemplo, apenas como **winrt::StorageFile**.

## <a name="important-apis"></a>APIs importantes
* [função winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Modelo de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
