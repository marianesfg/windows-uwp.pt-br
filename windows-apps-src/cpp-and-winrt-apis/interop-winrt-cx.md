---
author: stevewhims
description: Este tópico mostra duas funções auxiliares que podem ser usada para realizar a conversão entre os objetos C++/CX e C++/WinRT.
title: Interoperabilidade entre C++/WinRT e C++/CX
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, interoperabilidade, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: d265189c338d95a8c8f206fd196e99d5b0a1e068
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4053372"
---
# <a name="interop-between-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-and-ccx"></a>Interoperabilidade entre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) e C++/CX
Este tópico mostra duas funções auxiliares que podem ser usadas para realizar a conversão entre os objetos [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) e C++/WinRT. Você pode usá-los para a interoperabilidade entre o código que usa as duas projeções de linguagem, ou você pode usar as funções gradativamente o código do C++ c++ /CX para C++ c++ WinRT (consulte [Mover para C++ c++ WinRT do C++ c++ /CX](move-to-winrt-from-cx.md)).

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

## <a name="code-example"></a>Exemplo de código
Este é um exemplo de código (baseado no modelo de projeto **Aplicativo em Branco** do C++/CX) que mostra as duas funções auxiliares em uso. Também ilustra como você pode usar aliases de namespace para as ilhas diferentes a fim de lidar com outros possíveis conflitos de namespace entre as projeções do C++/WinRT e do C++/CX.

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
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

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

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
}
```

## <a name="important-apis"></a>APIs Importantes
* [Interface IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Função QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Função winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Função winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
