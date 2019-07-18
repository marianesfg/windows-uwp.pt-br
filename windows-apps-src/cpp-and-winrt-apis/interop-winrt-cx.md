---
description: Este tópico mostra duas funções auxiliares que podem ser usadas para realizar a conversão entre objetos de C++/CX e de C++/WinRT.
title: Interoperabilidade entre C++/WinRT e C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, interoperabilidade, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: a6b57627cbf9021732a8a66818250ffc1fca915f
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660124"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidade entre C++/WinRT e C++/CX

As estratégias para a portabilidade gradual do código no seu projeto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) para o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) são discutidas em [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md).

Este tópico mostra duas funções auxiliares que podem ser usadas para realizar a conversão entre objetos de C++/CX e de C++/WinRT no mesmo projeto. Use-as para favorecer a interoperabilidade entre o código que usa as duas projeções de linguagem ou use as funções ao fazer a portabilidade do código de C++/CX para C++/WinRT.

## <a name="fromcx-and-tocx-functions"></a>Funções from_cx e to_cx
A função auxiliar a seguir converte um objeto de C++/CX em um objeto de C++/WinRT equivalente. A função converte um objeto de C++/CX em seu ponteiro de interface [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) subjacente. Em seguida, chama [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) nesse ponteiro para consultar a interface padrão do objeto de C++/WinRT. A **QueryInterface** é o equivalente da interface binária de aplicativo (ABI) do Windows Runtime da extensão safe_cast do C++/CX. A função [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera o endereço do ponteiro de interface **IUnknown** subjacente do objeto de C++/WinRT para que ele possa ser definido com outro valor.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

A função auxiliar a seguir converte um objeto de C++/WinRT em um objeto de C++/CX equivalente. A função [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) recupera um ponteiro para a interface **IUnknown** subjacente de um objeto de C++/WinRT. A função converte esse ponteiro para um objeto de C++/CX antes de usar a extensão safe_cast do C++/CX para consultar o tipo de C++/CX solicitado.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Exemplo de projeto mostrando as duas funções auxiliares em uso

Para reproduzir de forma simples o cenário de portabilidade gradual do código de um projeto C++/CX para C++/WinRT, comece criando um novo projeto no Visual Studio usando um dos modelos de projeto C++/WinRT (confira [Suporte do Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Este exemplo de projeto também ilustra como é possível usar aliases de namespace para usos paralelos diferentes a fim de lidar com outros possíveis conflitos de namespace entre as projeções do C++/WinRT e do C++/CX.

- Crie um projeto **Visual C++** \> **Windows Universal** > **App Core (C++/WinRT)** .
- Nas propriedades do projeto, **C/C++** \> **Geral** \> **Utilizar Extensão do Windows Runtime** \> **Sim (/ZW)** . Isso ativa o suporte a projetos de C++/CX.
- Substitua o conteúdo de `App.cpp` pela listagem de códigos a seguir.

`WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>APIs Importantes
* [Interface IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Função QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Função winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Função winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
