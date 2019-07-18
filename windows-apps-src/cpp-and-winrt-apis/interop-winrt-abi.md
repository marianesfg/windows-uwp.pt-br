---
description: Este tópico mostra como realizar a conversão entre interface binária do aplicativo (ABI) e objetos do C++/WinRT.
title: Interoperabilidade entre C++/WinRT e ABI
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, portabilidade, migrar, interoperabilidade, ABI
ms.localizationpriority: medium
ms.openlocfilehash: d1def649772f94a03d5a1f352dcec1d32c7b0868
ms.sourcegitcommit: 5d71c97b6129a4267fd8334ba2bfe9ac736394cd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800571"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilidade entre C++/WinRT e ABI

Este tópico mostra como realizar a conversão entre a interface binária do aplicativo (ABI) do SDK e objetos do [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Utilize essas técnicas para favorecer a interoperabilidade entre o código que usa essas duas maneiras de programação com o Windows Runtime ou à medida que migra gradativamente o código da ABI para o C++/WinRT.

De modo geral, C++/WinRT expõe tipos de ABI como **void\*** , de modo que você não precisa incluir os arquivos de cabeçalho da plataforma.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>O que é a ABI do Windows Runtime e o que são tipos da ABI?
Uma classe do Windows Runtime (classe de tempo de execução) é uma abstração. Essa abstração define uma interface binária (a interface binária do aplicativo, ou ABI), que permite a interação entre várias linguagens de programação com um objeto. Independentemente da linguagem de programação, a interação do código de cliente com um objeto do Windows Runtime acontece no nível mais baixo, com construções de linguagem do cliente convertidas em chamadas na ABI do objeto.

Os cabeçalhos do SDK do Windows na pasta "%DiretorioSDKWindows%Include\10.0.17134.0\winrt" (ajuste o número de versão do SDK para o seu caso, se necessário) são os arquivos de cabeçalho da ABI do Windows Runtime. Eles foram gerados pelo compilador MIDL. Este é um exemplo da inclusão de um desses cabeçalhos.

```cpp
#include <windows.foundation.h>
```

Veja um exemplo simplificado de um dos tipos de ABI que você encontrará nesse cabeçalho de SDK específico. Observe o namespace **ABI**; **Windows::Foundation** e os demais namespaces do Windows são declarados pelos cabeçalhos do SDK no namespace **ABI**.

```cpp
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

**IUriRuntimeClass** é uma interface COM. Porém, mais do que isso, &mdash;considerando que sua base é **IInspectable**&mdash;**IUriRuntimeClass**, é uma interface do Windows Runtime. Observe o tipo de retorno **HRESULT**, em vez da geração de exceções. E o uso de artefatos como o identificador **HSTRING** (essa é uma boa prática para definir esse identificador novamente como `nullptr` quando tiver terminado de trabalhar com ele). Isso dá uma ideia da aparência do Windows Runtime no nível binário do aplicativo. Em outras palavras, no nível de programação COM.

O Windows Runtime baseia-se nas APIs de COM (Component Object Model). Acesse o Windows Runtime dessa forma ou por meio de *projeções de linguagem*. Uma projeção oculta os detalhes de COM e oferece uma experiência de programação mais natural para determinada linguagem.

Por exemplo, se você analisar a pasta "%DiretorioSDKWindows%Include\10.0.17134.0\cppwinrt\winrt" (novamente, ajuste o número de versão do SDK para o seu caso, se necessário), encontrará os cabeçalhos de projeção da linguagem C++/WinRT. Há um cabeçalho para cada namespace do Windows, assim como há um cabeçalho ABI por namespace do Windows. Este é um exemplo da inclusão de um dos cabeçalhos de C++/WinRT.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

A partir desse cabeçalho, obtemos o equivalente (simplificado) do C++/WinRT desse tipo de ABI que acabamos de ver.

```cppwinrt
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

A interface aqui é o C++ padrão moderno. Ele dispensa os **HRESULT**s (o C++/WinRT gera exceções, se necessário). A função do acessador retorna um objeto de cadeia de caracteres simples, que é removido no final do seu escopo.

Este tópico serve nos casos em que se deseja fazer a portabilidade ou interoperabilidade com um código que funciona na camada da Interface binária do aplicativo (ABI).

## <a name="converting-to-and-from-abi-types-in-code"></a>Convertendo de e para tipos de ABI no código
Para fins de segurança e praticidade, nas conversões em ambas as direções, é possível simplesmente usar [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) e [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). Veja um exemplo de código (com base no modelo de projeto **Aplicativo de console**), que também ilustra como usar aliases de namespace para as diferentes ilhas a fim de solucionar possíveis conflitos de namespace entre a projeção do C++/WinRT e a ABI.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

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
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

As implementações das funções **as** chamam a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Se precisar de conversões de nível inferior que chamem apenas [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref), use as funções auxiliares [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) e [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). O exemplo de código a seguir adiciona essas conversões de nível inferior ao exemplo de código anterior.

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

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

Apresentamos a seguir outras técnicas de conversão de nível inferior similares, mas que usam ponteiros brutos para tipos de interface ABI (aqueles definidos pelos cabeçalhos do SDK do Windows) no momento.

```cppwinrt
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Nas conversões de nível inferior, que apenas copiam endereços, use as funções auxiliares [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) e [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

`WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>Função convert_from_abi
Essa função auxiliar converte um ponteiro de interface ABI bruto em um objeto equivalente do C++/WinRT, com o mínimo de sobrecarga.

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

A função simplesmente chama [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) para consultar a interface padrão do tipo C++/WinRT solicitado.

Como vimos, não é necessário ter uma função auxiliar para converter um objeto C++/WinRT em um ponteiro de interface ABI equivalente. Basta usar a função de membro [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (ou [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) para consultar a interface solicitada. As funções **as** e **try_as** retornam um objeto [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que encapsula o tipo de ABI solicitada.

## <a name="code-example-using-convertfromabi"></a>Exemplo de código que usa convert_from_abi
Este é um exemplo de código que mostra essa função auxiliar na prática.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
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

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr };

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>Interoperação com ponteiros de interface COM da ABI

O modelo de função auxiliar a seguir ilustra como copiar um ponteiro de interface COM da ABI de um determinado tipo no tipo de ponteiro inteligente equivalente projetado em C++/WinRT.

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

Esse próximo modelo de função auxiliar é equivalente, exceto pelo fato de copiar do tipo de ponteiro inteligente da [Biblioteca de Implementação do Windows (WIL)](https://github.com/Microsoft/wil).

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

Confira também [Consumir componentes COM com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-com).

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>Interoperação não segura com ponteiros de interface COM da ABI

A tabela a seguir mostra (além de outras operações) conversões não seguras entre um ponteiro de interface COM da ABI de um determinado tipo no tipo de ponteiro inteligente equivalente projetado em C++/WinRT. Para o código na tabela, suponha essas declarações.

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

Além disso, suponha que **ISample** é a interface padrão para **Sample**.

Você pode declarar isso em tempo de compilação com esse código.

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| Operação | Como fazer | Observações |
|-|-|-|
| Extrair **ISample\*** de **winrt::Sample** | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* ainda tem o objeto. |
| Desanexar **ISample\*** de **winrt::Sample** | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* não tem mais o objeto. |
| Transferir **ISample\*** para um novo **winrt::Sample** | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* assume a propriedade do objeto. |
| Definir **ISample\*** em **winrt::Sample** | `*put_abi(s) = p;` | *s* assume a propriedade do objeto. Qualquer objeto anteriormente pertencente a *s* é vazado (será declarado na depuração). |
| Receber **ISample\*** em **winrt::Sample** | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* assume a propriedade do objeto. Qualquer objeto anteriormente pertencente a *s* é vazado (será declarado na depuração). |
| Substituir **ISample\*** em **winrt::Sample** | `attach_abi(s, p);` | *s* assume a propriedade do objeto. O objeto anteriormente pertencente a *s* é liberado. |
| Copiar **ISample\*** para **winrt::Sample** | `copy_from_abi(s, p);` | *s* faz uma nova referência ao objeto. O objeto anteriormente pertencente a *s* é liberado. |
| Copiar **winrt::Sample** para **ISample\*** | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p* recebe uma cópia do objeto. Qualquer objeto anteriormente pertencente a *s* é vazado. |

## <a name="interoperating-with-the-abis-guid-struct"></a>Interoperação com o struct de GUID da ABI

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) agora é projetado como **winrt::guid**. Para as APIs que você implementa, você deve usar **winrt::guid** para parâmetros de GUID. Caso contrário, há conversões automáticas entre **winrt::guid** e **GUID**, desde que você inclua `unknwn.h` (incluído implicitamente por <windows.h> e por muitos outros arquivos de cabeçalho) antes de incluir qualquer cabeçalho C++/WinRT.

Se não fizer isso, você poderá embutir `reinterpret_cast` entre eles. Para a tabela a seguir, suponha essas declarações.

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| Conversão | Com `#include <unknwn.h>` | Sem `#include <unknwn.h>` |
|-|-|-|
| De **winrt::guid** em **GUID** | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| De **GUID** em **winrt::guid** | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

## <a name="interoperating-with-the-abis-hstring"></a>Interoperação com a HSTRING da ABI

A tabela a seguir mostra conversões entre **winrt::hstring** e [**HSTRING**](/windows/win32/winrt/hstring), bem como outras operações. Para o código na tabela, suponha essas declarações.

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| Operação | Como fazer | Observações |
|-|-|-|
| Extrair **HSTRING** de **hstring** | `h = static_cast<HSTRING>(get_abi(s));` | *s* ainda tem a cadeia de caracteres. |
| Desanexar **HSTRING** de **hstring** | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* não tem mais a cadeia de caracteres. |
| Definir **HSTRING** em **hstring** | `*put_abi(s) = h;` | *s* assume a propriedade da cadeia de caracteres. Qualquer cadeia de caracteres anteriormente pertencente a *s* é vazada (será declarada na depuração). |
| Receber **HSTRING** em **hstring** | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* assume a propriedade da cadeia de caracteres. Qualquer cadeia de caracteres anteriormente pertencente a *s* é vazada (será declarada na depuração). |
| Substituir **HSTRING** em **hstring** | `attach_abi(s, h);` | *s* assume a propriedade da cadeia de caracteres. A cadeia de caracteres anteriormente pertencente a *s* é liberada. |
| Copiar **HSTRING** para **hstring** | `copy_from_abi(s, h);` | *s* faz uma cópia particular da cadeia de caracteres. A cadeia de caracteres anteriormente pertencente a *s* é liberada. |
| Copiar **hstring** para **HSTRING** | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* recebe uma cópia da cadeia de caracteres. Qualquer cadeia de caracteres anteriormente pertencente a *h* é vazada. |

## <a name="important-apis"></a>APIs Importantes
* [Função AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [Função QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Função winrt::attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Modelo de struct winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Função winrt::copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [Função winrt::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [Função winrt::detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Função winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Função de membro winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Função de membro winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
