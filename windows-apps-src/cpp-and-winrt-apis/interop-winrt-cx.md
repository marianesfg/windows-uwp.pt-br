---
description: Este tópico mostra duas funções auxiliares que podem ser usada para realizar a conversão entre os objetos C++/CX e C++/WinRT.
title: Interoperabilidade entre C++/WinRT e C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, interoperabilidade, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 558f3fa75e7dd599927a9d2ace256bf1feb98e77
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621641"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidade entre C++/WinRT e C++/CX

Estratégias para portabilidade gradualmente o código no seu [C + + c++ /CLI CX](/cpp/cppcx/visual-c-language-reference-c-cx) projeto ao [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) são discutidos na [mover para C + + c++ /CLI WinRT da C + + c++ /CLI CX](move-to-winrt-from-cx.md).

Este tópico mostra duas funções auxiliares que você pode usar para converter entre C + + c++ /CLI CX e C + + c++ /CLI objetos WinRT dentro do mesmo projeto. Você pode usá-los para fornecer interoperabilidade entre o código que usa as projeções de dois linguagem, ou você pode usar as funções de portar seu código do C + + c++ /CX para C + + c++ /CLI WinRT.

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

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Projeto de exemplo que mostra as duas funções auxiliares em uso

Para reproduzir, de forma simples, o cenário de portabilidade gradualmente o código em C + c++ /CLI projeto CX C + c++ /CLI WinRT, você pode começar criando um novo projeto no Visual Studio usando um dos C + c++ /CLI WinRT modelos de projeto (consulte [suporte do Visual Studio para C + + c++ /CLI WinRT ](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Este projeto de exemplo também ilustra como você pode usar o alias de namespace para as Ilhas diferentes de código, para lidar com possíveis caso contrário, conflitos de namespace entre o C + + c++ /CLI WinRT projeção e C + c++ /CLI projeção do CX.

- Criar uma **Visual C++** \> **Windows Universal** > **aplicativo Core (C + + c++ /CLI WinRT)** projeto.
- Nas propriedades do projeto **C/C++** \> **geral** \> **consumir extensão de tempo de execução do Windows** \> **Sim (/ZW)** . Isso ativa o suporte de projeto para C + + c++ /CLI CX.
- Substitua o conteúdo do `App.cpp` com a listagem de códigos abaixo.

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
* [função WinRT::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [função WinRT::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
