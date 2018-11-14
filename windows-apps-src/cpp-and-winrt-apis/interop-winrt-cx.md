---
author: stevewhims
description: Este tópico mostra duas funções auxiliares que podem ser usada para realizar a conversão entre os objetos C++/CX e C++/WinRT.
title: Interoperabilidade entre C++/WinRT e C++/CX
ms.author: stwhi
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, interoperabilidade, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: ca3cc69065ef2898aebafc832da1639985231d60
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6203992"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidade entre C++/WinRT e C++/CX

Estratégias para a portabilidade gradualmente o código em seu [C++ c++ CX](/cpp/cppcx/visual-c-language-reference-c-cx) quiserem [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) são abordados no [Mover para C++ c++ WinRT do C++ c++ CX](move-to-winrt-from-cx.md).

Este tópico mostra duas funções auxiliares que você pode usar para converter entre C++ c++ /CX e C++ c++ WinRT objetos dentro do mesmo projeto. Você pode usá-los para interoperabilidade entre o código que usa as duas projeções de linguagem, ou você pode usar as funções como portar seu código do C++ c++ /CX para C++ c++ WinRT.

## <a name="fromcx-and-tocx-functions"></a>Funções from_cx e to_cx
A função auxiliar a seguir converte um objeto C++/CX em um objeto C++/WinRT equivalente. A função converte um objeto C++/CX em seu ponteiro de interface [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) subjacente. Em seguida, ele chama a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) nesse ponteiro para consulta a interface padrão do objeto C++/WinRT. A **QueryInterface** é o equivalente da interface binária de aplicativo (ABI) do Windows Runtime da extensão safe_cast do C++/CX. A função [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera o endereço do ponteiro de interface **IUnknown** subjacente do objeto C++/WinRT para que ele possa ser definido para outro valor.

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

A função auxiliar a seguir converte um objeto C++/WinRT em objeto C++/CX equivalente. A função [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) recupera um ponteiro para a interface **IUnknown** subjacente de um objeto C++/WinRT. A função converte esse ponteiro para um objeto C++/CX antes de usar a extensão safe_cast do C++/CX para consultar o tipo C++/CX solicitado.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Projeto de exemplo mostra as duas funções auxiliares em uso

Para reproduzir, de forma simple, o cenário de portabilidade gradualmente o código C++ c++ projeto CX para C++ c++ WinRT, você pode começar criando um novo projeto no Visual Studio usando um dos C++ c++ modelos de projeto do WinRT (consulte [suporte do Visual Studio para C++ c++ WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)).

Este projeto de exemplo também ilustra como você pode usar aliases de namespace para as Ilhas diferentes de código, para lidar com outros possíveis conflitos de namespace entre C++ c++ WinRT projeção e C++ c++ projeção CX.

- Criar um **Visual C++** \> **Universal do Windows** > **aplicativo principal (C++ c++ WinRT)** projeto.
- Nas propriedades do projeto, **C/C++** \> **Geral** \> **Consume Windows Runtime Extension** \> **Sim (/ZW)**. Isso ativa o suporte de projeto para C++ c++ /CX.
- Substitua o conteúdo do `App.cpp` com a listagem de código abaixo.

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
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Função QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Função winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Função winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Mudar de C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
