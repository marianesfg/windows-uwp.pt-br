---
author: stevewhims
description: Este tópico mostra como realizar a conversão entre interface binária do aplicativo (ABI) e objetos do C++/WinRT.
title: Interoperabilidade entre C++/WinRT e ABI
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, interoperabilidade, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 098d182b9cc4cc51bda0a7959702e53accf2699f
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4755537"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilidade entre C++/WinRT e ABI

Este tópico mostra como realizar a conversão entre interface binária do aplicativo do SDK (ABI) e [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) objetos. Você pode usar essas técnicas para favorecer a interoperabilidade entre o código que usa essas duas maneiras de programação com o Windows Runtime ou pode usá-las à medida que migra gradativamente o código da ABI para o C++/WinRT.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>Qual é a ABI do Windows Runtime e quais são os tipos ABI?
Uma classe do Windows Runtime (classe do tempo de execução) é uma abstração. Essa abstração define uma interface binária (a Interface binária do aplicativo ou ABI), que permite a interação entre várias linguagens de programação com um objeto. Independentemente da linguagem de programação, a interação do código de cliente com um objeto do Windows Runtime acontece no nível mais baixo, com construções de linguagem do cliente convertidas em chamadas na ABI do objeto.

Os cabeçalhos do SDK do Windows na pasta "%WindowsSdkDir%Include\10.0.17134.0\winrt" (ajuste o número de versão do SDK para a sua ocorrência, se necessário) são os arquivos de cabeçalho ABI do Windows Runtime. Eles foram gerados pelo compilador MIDL. Este é um exemplo da inclusão de um desses cabeçalhos.

```
#include <windows.foundation.h>
```

Veja um exemplo simplificado de um dos tipos de ABI que você encontrará nesse cabeçalho de SDK específico. Observe o namespace do **ABI**; **Windows::Foundation**, e os demais namespaces do Windows são declarados pelos cabeçalhos do SDK no namespace do **ABI**.

```
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** é uma interface COM. Porém, mais do que isso, considerando que sua base é **IInspectable**, a **IUriRuntimeClass** é uma interface do Windows Runtime. Observe o tipo de retorno **HRESULT**, em vez da geração de exceções. E o uso de artefatos como o identificador **HSTRING** (essa é uma boa prática para definir esse identificador novamente como `nullptr` quando você tiver terminado de trabalhar com ele). Isso dá uma ideia da aparência do Windows Runtime no nível binário do aplicativo. Em outras palavras, no nível de programação COM.

O Windows Runtime baseia-se nas COM (Component Object Model) APIs. Você pode acessar o Windows Runtime dessa forma ou pode acessá-lo por meio de *projeções de linguagem*. Uma projeção oculta os detalhes do COM e oferece uma experiência de programação mais natural para uma linguagem específica.

Por exemplo, se você analisar a pasta "%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt" (novamente, ajuste o número de versão do SDK para a sua ocorrência, se necessário), encontrará os cabeçalhos de projeção da linguagem C++/WinRT. Há um cabeçalho para cada namespace do Windows, assim como há um cabeçalho ABI por namespace do Windows. Este é um exemplo da inclusão de um dos cabeçalhos do C++/WinRT.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

A partir desse cabeçalho, obtemos o equivalente (simplificado) do C++/WinRT desse tipo de ABI que acabamos de ver.

```
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

A interface aqui é o C++ padrão moderno. Ele dispensa os **HRESULT**s (o C++/WinRT gera exceções, se necessário). A função do acessor retorna um objeto de cadeia de caracteres simples, que é removido no final do seu escopo.

Este tópico serve nos casos em que você desejar fazer a portabilidade ou interoperabilidade com um código que funciona na camada da Interface binária do aplicativo (ABI).

## <a name="converting-to-and-from-abi-types-in-code"></a>Realizando a conversão dos tipos de ABI no código
Para fins de segurança e praticidade, nas conversões em ambas as direções, você pode simplesmente usar [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) e [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). Veja um exemplo de código (com base no modelo de projeto do **Aplicativo de console**), que também ilustra como você pode usar aliases de namespace para as ilhas diferentes a fim de solucionar possíveis conflitos de namespace entre as projeção do C++/WinRT e o ABI.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr = uri.as<abi::IStringable>();

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable = ptr.as<winrt::IStringable>();
}
```

As implementações das funções **as** chamam a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Se você precisar de conversões de nível inferior que chamem apenas [**AddRef**](https://msdn.microsoft.com/library/windows/desktop/ms691379), use as funções auxiliares [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) e [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). O exemplo de código a seguir adiciona essas conversões de nível inferior ao exemplo de código anterior.

```cppwinrt
int main()
{
    ...

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

Estas são outras técnicas de conversão de nível inferior similares, mas que usam ponteiros brutos para tipos de interface ABI (aqueles definidos pelos cabeçalhos do SDK do Windows) no momento.

```cppwinrt
    ...

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning = nullptr;
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Nas conversões de nível inferior, que apenas copiam endereços, você pode usar as funções auxiliares [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) e [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

```cppwinrt
    ...

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning = static_cast<abi::IStringable*>(winrt::get_abi(uri));
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>Função convert_from_abi
Essa função auxiliar converte um ponteiro bruto de interface ABI em um objeto equivalente do C++/WinRT, com o mínimo de sobrecarga.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

A função simplesmente chama [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) para consultar a interface padrão do tipo de C++/WinRT solicitado.

Como vimos, não é necessário ter uma função auxiliar para converter um objeto C++/WinRT em um ponteiro de interface ABI equivalente. Basta usar a função de membro [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) para consultar a interface solicitada. As funções **as** e **try_as** retornam um objeto [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que encapsula o tipo de ABI solicitado.

## <a name="code-example-using-convertfromabi"></a>Exemplo de código que usa convert_from_abi
Este é um exemplo de código que mostra essa função auxiliar na prática.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>APIs importantes
* [Função AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379)
* [Função QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [função WinRT:: attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Modelo de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [função WinRT:: copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [função WinRT:: copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [função WinRT:: detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Função winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Função membro winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Função membro winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)
