---
description: Com C++/WinRT, é possível chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longa de C++ padrão ou o tipo winrt::hstring.
title: Processamento da cadeia de caracteres em C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, cadeia de caracteres
ms.localizationpriority: medium
ms.openlocfilehash: 1771c3754e8e9580514f646ae8589b1982911fc7
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79448566"
---
# <a name="string-handling-in-cwinrt"></a>Processamento da cadeia de caracteres em C++/WinRT

Com o [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) é possível chamar APIs do Windows Runtime usando tipos de cadeia de caracteres longa da Biblioteca padrão de C++, como **std::wstring** (observação: mas não com tipos de cadeia de caracteres curta, como **std::string**). O C++/WinRT tem um tipo de cadeia de caracteres personalizado chamado [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (definido na biblioteca de C++/WinRT, que é `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Esse é o tipo de cadeia de caracteres que construtores, funções e propriedades do Windows Runtime podem usar e retornar. Porém, em muitos casos, graças aos construtores e operadores de conversão de **hstring**, é possível optar se deseja ou não estar ciente de **hstring** no código cliente. Se você estiver *criando* APIs, provavelmente precisará saber sobre a existência de **hstring**.

Existem muitos tipos de cadeia de caracteres em C++. Existem variantes em muitas bibliotecas além de **std::basic_string** na Biblioteca padrão de C++. O C++17 tem utilitários de conversão de cadeia de caracteres e **std::basic_string_view** para preencher a lacuna entre todos os tipos de cadeia de caracteres.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) fornece conversibilidade com **std::wstring_view** para fornecer a interoperabilidade para a qual **std::basic_string_view** foi projetado.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Usar **std::wstring** (e, opcionalmente, **winrt::hstring**) com **Uri**
[**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) é construído a partir de um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Entretanto, **hstring** tem [construtores de conversão](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor) que permitem usá-lo sem precisar estar ciente dele. Este é um exemplo de código que mostra como criar um **Uri** a partir de um literal de cadeia de caracteres longa, de um modo de exibição de cadeia de caracteres longa e de um **std::wstring**.

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

Lembrando que estar ciente desse detalhe é opcional, graças ao [operador de conversão de **hstring** para **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view).

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Da mesma forma, [**IStringable::ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) retorna hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**Uri** implementa a interface [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Use a função [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) para obter uma cadeia de caracteres longa padrão a partir de um **hstring** (assim como se faz em um **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Se houver um **hstring**,será possível criar um **Uri** a partir dele.

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

**hstring** tem um operador de conversão **std::wstring_view** de membro; a conversão é realizada sem custo adicional.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Operadores e funções **winrt::hstring**
Uma série de construtores, operadores, funções e iteradores são implementados para [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

Um **hstring** é um intervalo, portanto, use-o com `for` baseado em intervalo ou com `std::for_each`. Ele também fornece operadores de comparação para fazer uma comparação com naturalidade e eficiência de seus equivalentes na Biblioteca padrão do C++. Ele contém tudo o que você precisa para usar o **hstring** como uma chave para contêineres associativos.

Reconhecemos que várias bibliotecas do C++ usam **std::string** e trabalham exclusivamente com textos em UTF-8. Como uma conveniência, fornecemos auxiliares, como [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) e [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), para converter em ambas as direções.

`WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Para obter mais exemplos e saber mais sobre funções e operadores de **hstring**, confira o tópico de referência de API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>A lógica de **winrt::hstring** e **winrt::param::hstring**
O Windows Runtime é implementado em termos de caracteres **wchar_t**, mas a Interface Binária de Aplicativo (ABI) do Windows Runtime não é um subconjunto do que **std::wstring** ou **std::wstring_view** fornece. O uso deles resultaria em uma ineficiência significativa. Ao invés disso, o C++/WinRT fornece **winrt::hstring**, que representa uma cadeia de caracteres imutável consistente com o [HSTRING](https://docs.microsoft.com/windows/desktop/WinRT/hstring) subjacente e implementado por trás de uma interface semelhante à de **std::wstring**. 

Você pode perceber que os parâmetros de entrada do C++/WinRT que logicamente devem aceitar **winrt::hstring** esperam, na verdade, **winrt::param::hstring**. O namespace **param** contém um conjunto de tipos usado exclusivamente para otimizar os parâmetros de entrada a fim de fazer a associação natural a tipos da Biblioteca padrão do C++ e evitar cópias e outras ineficiências. Não se deve usar esses tipos diretamente. Se você quiser usar uma otimização para suas próprias funções, use **std::wstring_view**. Confira também [Passagem de parâmetros para o limite do ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

O resultado é que, na maioria das vezes, é possível ignorar as especificidades do gerenciamento de cadeias de caracteres do Windows Runtime e trabalhar com eficiência com o que você sabe. E isso é importante, considerando até que ponto as cadeias de caracteres são usadas no Windows Runtime.

## <a name="formatting-strings"></a>Formatar cadeias de caracteres
Uma opção para a formatação de cadeias de caracteres é **std::wostringstream**. Este é um exemplo que formata e exibe uma mensagem simples de rastreamento de depuração.

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>A maneira correta de definir uma propriedade

Uma propriedade é definida passando um valor para uma função setter. Aqui está um exemplo.

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

O código a seguir está incorreto. Ele é compilado, mas tudo o que faz é modificar a **winrt::hstring** temporária retornada pela função de acessador **Text()** e, em seguida, descartar o resultado.

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>APIs importantes
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Função winrt::to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [Função winrt::to_string](/uwp/cpp-ref-for-winrt/to-string)
