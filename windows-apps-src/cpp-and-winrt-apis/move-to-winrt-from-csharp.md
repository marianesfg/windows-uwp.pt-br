---
description: Este tópico mostra como fazer a portabilidade do código C# para o equivalente no C++/WinRT.
title: Mover do C# para C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, compatibilizar, migrar, C#
ms.localizationpriority: medium
ms.openlocfilehash: 17900829388bfe0b3cc325e27d0807b139ccaa27
ms.sourcegitcommit: 2c6aac8a0cc02580df0987f0b7dba5924e3472d6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74958956"
---
# <a name="move-to-cwinrt-from-c"></a>Mover do C# para C++/WinRT

Este tópico mostra como compatibilizar o código em um projeto [C#](/visualstudio/get-started/csharp) para seu equivalente no [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="register-an-event-handler"></a>Registrar um manipulador de eventos

Você pode registrar um manipulador de eventos na marcação XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

No C#, o método **OpenButton_Click** pode ser privado e o XAML ainda poderá conectá-lo ao evento [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) gerado por *OpenButton*.

No C++/WinRT, o método **OpenButton_Click** precisa ser público no [tipo de implementação](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Como alternativa, você pode tornar a classe de registro um amigo da classe de implementação e **OpenButton_Click** privado.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>Como disponibilizar uma classe para a extensão de marcação {Binding}

Se você pretende usar a extensão de marcação {Binding} para associar dados ao tipo de dados, confira [Objeto de associação declarado usando {Binding}](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

## <a name="making-a-data-source-available-to-xaml-markup"></a>Como disponibilizar uma fonte de dados para marcação XAML

No C++/WinRT versão 2.0.190530.8 e superior, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) cria um vetor observável que dá suporte a **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** e **IObservableVector\<IInspectable\>** .

Você pode criar o **arquivo MIDL (.idl)** desta forma (confira também [Como fatorar classes de runtime em arquivos MIDL (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Depois, implemente-o desta forma.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Para obter mais informações, confira [Controles de itens XAML; associação a uma coleção do C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection).

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Como disponibilizar uma fonte de dados para marcação XAML (antes do C++/WinRT 2.0.190530.8)

A vinculação de dados XAML exige que uma origem de itens implemente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , bem como uma das combinações de interfaces a seguir.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** e **INotifyCollectionChanged**
- **IBindableVector** e **IBindableObservableVector**
- **IBindableVector** por si só (não responderá a alterações)
- **IVector\<IInspectable\>**
- **IBindableIterable** (iterará e salvará elementos em uma coleção particular)

Uma interface genérica, como **IVector\<T\>** , não pode ser detectada em runtime. Cada **IVector\<T\>** tem um IID (identificador de interface) diferente, que é uma função de **T**. Qualquer desenvolvedor pode expandir o conjunto de **T** de forma arbitrária e, portanto, é evidente que o código de associação XAML nunca pode saber o conjunto completo a ser consultado. Essa restrição não é um problema para o C#, porque cada objeto CLR que implementa **IEnumerable\<T\>** implementa **IEnumerable** automaticamente. No nível do ABI, isso significa que cada objeto que implementa **IObservableVector\<T\>** implementa **IObservableVector\<IInspectable\>** automaticamente.

O C++/WinRT não oferece essa garantia. Se uma classe de runtime do C++/WinRT implementar **IObservableVector\<T\>** , não poderemos pressupor que uma implementação de **IObservableVector\<IInspectable\>** também seja fornecida de alguma forma.

Consequentemente, veja abaixo como o exemplo anterior precisará ser examinado.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Veja também a implementação.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Caso você precise acessar objetos em *m_bookSkus*, precisará executar o QI novamente para **Bookstore::BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

Os tipos C# fornecem o método [Object.ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

O C++/ WinRT não fornece diretamente esse recurso, mas você poderá usar alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

O C++/WinRT também dá suporte a [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) em um número limitado de tipos. Você precisará adicionar sobrecargas para os tipos adicionais que deseja converter em cadeia de caracteres.

| Idioma | Converter int em cadeia de caracteres | Converter uma enumeração em cadeia de caracteres |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

No caso da conversão de uma enumeração em cadeia de caracteres, você precisará fornecer a implementação de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Em geral, essas conversões em cadeia de caracteres são consumidas implicitamente pela vinculação de dados.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Essas associações executarão **winrt::to_hstring** da propriedade associada. No caso do segundo exemplo (o **StatusEnum**), você precisará fornecer sua própria sobrecarga de **winrt::to_hstring**; caso contrário, obterá um erro do compilador.

## <a name="string-building"></a>Construção da cadeia de caracteres

Para a construção de cadeia de caracteres, o C# tem um tipo [**StringBuilder**](/dotnet/api/system.text.stringbuilder) interno.

| | C# | C++/WinRT |
|-|-|-|
| Classe do construtor | `StringBuilder builder;` | `std::wstringstream stream;` |
| Acrescentar cadeia de caracteres, preservando nulos | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| Extrair resultado | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>Conversão boxing e unboxing

O C# faz a conversão boxing automática de escalares em objetos. O C++/WinRT exige que você chame explicitamente a função [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value). Ambas as linguagens exigem que você faça a conversão unboxing explicitamente. Confira [Conversão boxing e unboxing com o C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

Nas tabelas a seguir, usaremos estas definições.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Operação | C# | C++/WinRT|
|-|-|-|
| Conversão boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Conversão unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

O C++/CX e o C# gerarão exceções se você tentar fazer a conversão unboxing de um ponteiro nulo em um tipo de valor. O C++/WinRT considera isso um erro de programação e falha. No C++/WinRT, use a função [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) se desejar lidar com o caso em que o objeto não é do tipo que você pensou que fosse.

| Cenário | C# | C++/WinRT|
|-|-|-|
| Fazer a conversão unboxing de um inteiro conhecido |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Se o for nulo | `System.NullReferenceException` | Falha |
| Se o não for um int convertido | `System.InvalidCastException` | Falha |
| Fazer a conversão unboxing de int, se for nulo, usar fallback; falhar, em qualquer outra situação | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Fazer a conversão unboxing de int, se possível; usar fallback para qualquer outra situação | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversão boxing e unboxing de uma cadeia de caracteres

Uma cadeia de caracteres é, de algumas maneiras, um tipo de valor e, de outras, um tipo de referência. O C# e o C++/WinRT tratam as cadeias de caracteres de maneira diferente.

O tipo do ABI [**HSTRING**](/windows/win32/winrt/hstring) é um ponteiro para uma cadeia de caracteres de contagem de referências. Mas ele não é derivado de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), portanto, não é tecnicamente um *objeto*. Além disso, um **HSTRING** nulo representa a cadeia de caracteres vazia. A conversão boxing de itens não derivados de **IInspectable** é feita encapsulando-os dentro de um [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) e o Windows Runtime fornece uma implementação padrão na forma do objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (os tipos personalizados são relatados como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

O C# representa uma cadeia de caracteres do Windows Runtime como um tipo de referência, enquanto o C++/WinRT projeta uma cadeia de caracteres como um tipo de valor. Isso significa que uma cadeia de caracteres nula convertida pode ter diferentes representações, dependendo do procedimento adotado.

| Comportamento | C# | C++/WinRT|
|-|-|-|
| Declarações | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| Categoria de tipo de cadeia de caracteres | Tipo de referência | Tipo de valor |
| projetos **HSTRING** nulos como | `""` | `hstring{}` |
| São nulos e idênticos `""`? | Não | Sim |
| Validade de nulo | `s = null;`<br>`s.Length` gera NullReferenceException | `s = hstring{};`<br>`s.size() == 0` (válido) |
| Se você atribuir uma cadeia de caracteres nula ao objeto | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Se você atribuir `""` ao objeto | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Conversões boxing e unboxing básicas.

| Operação | C# | C++/WinRT|
|-|-|-|
| Fazer a conversão boxing de uma cadeia de caracteres | `o = s;`<br>A cadeia de caracteres vazia se torna um objeto não nulo. | `o = box_value(s);`<br>A cadeia de caracteres vazia se torna um objeto não nulo. |
| Fazer a conversão unboxing de uma cadeia de caracteres conhecida | `s = (string)o;`<br>O objeto nulo torna-se uma cadeia de caracteres nula.<br>InvalidCastException se não for uma cadeia de caracteres. | `s = unbox_value<hstring>(o);`<br>O objeto nulo falha.<br>Falha se não for uma cadeia de caracteres. |
| Fazer a conversão unboxing de uma possível cadeia de caracteres | `s = o as string;`<br>Objeto nulo ou não cadeia de caracteres se torna uma cadeia de caracteres nula.<br><br>OU<br><br>`s = o as string ?? fallback;`<br>Nulo ou não cadeia de caracteres se torna fallback.<br>Cadeia de caracteres vazia preservada. | `s = unbox_value_or<hstring>(o, fallback);`<br>Nulo ou não cadeia de caracteres se torna fallback.<br>Cadeia de caracteres vazia preservada. |

## <a name="derived-classes"></a>Classes derivadas

Para derivação de uma classe de runtime, a classe base precisa ser *combinável*. O C# não exige que você execute nenhuma etapa especial para tornar suas classes combináveis, ao contrário do C++/WinRT. Use a [palavra-chave não selada](/uwp/midl-3/intro#base-classes) para indicar que deseja que a classe seja utilizável como uma classe base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

Na classe do cabeçalho de implementação, você precisa incluir o arquivo de cabeçalho da classe base antes de incluir o cabeçalho gerado automaticamente da classe derivada. Caso contrário, você receberá erros como "uso ilícito deste tipo como uma expressão".

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>Como consumir objetos por meio da marcação XAML

Em um projeto C#, você pode consumir membros privados e elementos nomeados por meio da marcação XAML. Mas no C++/WinRT, todas as entidades consumidas com o uso da [**extensão de marcação XAML {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) precisam ser expostas publicamente em IDL.

Além disso, a associação a um booliano exibe `true` ou `false` no C#, mas mostra **Windows.Foundation.IReference`1\<Boolean\>** no C++/WinRT.

Para obter mais informações e exemplos de código, confira [Como consumir objetos para marcação](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="important-apis"></a>APIs Importantes
* [Modelo de função winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [Tutoriais do C#](/visualstudio/get-started/csharp)
* [Criar APIs com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Vinculação de dados em detalhes](/windows/uwp/data-binding/data-binding-in-depth)
* [Controles de itens XAML; associar a uma coleção C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection)