---
description: Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longa C++ padrão ou usar o tipo winrt::hstring.
title: Tratamento de cadeia de caracteres em C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, cadeia de caracteres
ms.localizationpriority: medium
ms.openlocfilehash: 9572d9ba8b96d245b783535e159acbae9043ea3e
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8879303"
---
# <a name="string-handling-in-cwinrt"></a>Processamento de cadeia de caracteres em C++/WinRT

Com [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), você pode chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longa da biblioteca padrão C++, como **std:: wstring** (Observação: não com tipos de cadeia de caracteres estreita como **std:: string**). O C++/WinRT tem um tipo de cadeia de caracteres personalizado chamado [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (definido na biblioteca C++/WinRT, que é `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Esse é o tipo de cadeia de caracteres que construtores, funções e propriedades do Windows Runtime podem retornar. Mas, em muitos casos, graças aos construtores e operadores de conversão **hstring**, você pode optar se deseja ou não estar ciente do **hstring** no código cliente. Se você estiver *criando* APIs, provavelmente precisará saber sobre a existência de **hstring**.

Existem muitos tipos de cadeia de caracteres em C++. Existem variantes em muitas bibliotecas além de **std::basic_string** na Biblioteca Padrão C++. C++17 tem utilitários de conversão de cadeia de caracteres e **std::basic_string_view** para preencher a lacuna entre todos os tipos de cadeia de caracteres.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) fornece conversibilidade com **std::wstring_view** para fornecer a interoperabilidade para a qual **std::basic_string_view** foi projetado.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Usando **std::wstring** (e, opcionalmente, **winrt::hstring**) com **Uri**
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) é construído a partir de um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Mas **hstring** tem [construtores de conversão](/uwp/api/windows.foundation.uri#hstringhstring-constructor) que permitem que você trabalhe com ele sem precisar estar ciente dele. Este é um exemplo de código que mostra como criar um **Uri** a partir de um literal de cadeia de caracteres longa, de um modo de exibição de cadeia de caracteres larga e de um **std::wstring**.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

O acessador de propriedade [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) é do tipo **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Lembrando que estar ciente desse detalhe é opcional graças ao [operador de conversão em **std::wstring_view**](/uwp/api/hstring#hstringoperator-stdwstringview) do **hstring**.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Da mesma forma, [**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136) retorna hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implementa a interface [**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Você pode usar a função [**hstring::c_str function**](/uwp/api/windows.foundation.uri#hstringcstr-function) para obter uma cadeia de caracteres larga padrão a partir de um **hstring** (assim como pode fazer em um **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Se você tiver um **hstring**, poderá criar um **Uri** a partir dele.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Considere um método que use um **hstring**.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Todas as opções que você acabou de ver também se aplicam a esses casos.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** tem um operador de conversão **std::wstring_view** membro; a conversão é realizada sem custo adicional.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Operadores e funções **winrt::hstring**
Uma série de construtores, operadores, funções e iteradores são implementados para [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

Um **hstring** é um intervalo, para que você possa usá-lo com `for` baseado em intervalo ou `std::for_each`. Ele também fornece operadores de comparação para fazer uma comparação com naturalidade e eficiência de seus equivalentes na Biblioteca Padrão C++. Ele contém tudo o que você precisa para usar o **hstring** como uma chave para contêineres associativos.

Reconhecemos que várias bibliotecas C++ usam **std::string** e trabalham exclusivamente com textos UTF-8. Como uma conveniência, fornecemos auxiliares, como [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) e [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), para converter em ambas as direções.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Para obter mais exemplos e informações sobre as funções e operadores de **hstring**, consulte o tópico de referência de API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>A lógica de **winrt::hstring** e **winrt::param::hstring**
O Windows Runtime é implementado em termos de caracteres **wchar_t**, mas a ABI (Interface Binária de Aplicativo) do Windows Runtime não é um subconjunto do que **std::wstring** ou **std::wstring_view** fornece. O uso deles resultaria em uma ineficiência significativa. Em vez disso, C++/WinRT fornece **winrt::hstring**, que representa uma cadeia de caracteres imutável consistente com o [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775) subjacente e implementado por trás de uma interface semelhante à de **std::wstring**. 

Você pode perceber que os parâmetros de entrada do C++/WinRT que logicamente devem aceitar **winrt::hstring** esperam, na verdade, **winrt::param::hstring**. O namespace **param** contém um conjunto de tipos usado exclusivamente para otimizar os parâmetros de entrada a fim de fazer a associação naturalmente a tipos da Biblioteca Padrão C++ e evitar cópias e outras ineficiências. Você não deve usar esses tipos diretamente. Se você quiser usar uma otimização para suas próprias funções, use **std::wstring_view**.

O resultado é que você pode ignorar amplamente as especificidades do gerenciamento de cadeias de caracteres do Windows Runtime e funciona apenas com eficiência com o que você sabe. E isso é importante, considerando até que ponto as cadeias de caracteres são usadas no Windows Runtime.

# <a name="formatting-strings"></a>Formatando cadeias de caracteres
Uma opção para a formatação de cadeias de caracteres é **std::wstringstream**. Este é um exemplo que formata e exibe uma mensagem simples de rastreamento de depuração.

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>APIs Importantes
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [função to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [função WinRT:: to_string](/uwp/cpp-ref-for-winrt/to-string)
