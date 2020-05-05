---
description: Um valor escalar precisa ser encapsulado em um objeto de classe de referência antes de ser passado para uma função que espera **IInspectable**. Esse processo de encapsulamento é conhecido como *conversão* do valor.
title: Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, conversão, boxing, escalar, valor
ms.localizationpriority: medium
ms.openlocfilehash: 29263260217de154f1a942d37d1e18fece15e3d0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79038089"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT
 
A [**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) é a interface raiz de cada classe de tempo de execução no Windows Runtime (WinRT). Isso é uma ideia análoga a [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) estando na raiz de cada interface e classe COM, e **System.Object** estando na raiz de cada classe [Common Type System](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

Em outras palavras, uma função que espera **IInspectable** pode ser passada como uma instância de qualquer classe de runtime. Porém, não é possível passar diretamente um valor escalar, como um valor numérico ou de texto, a tal função. Em vez disso, um valor escalar precisa ser encapsulado dentro de um objeto de classe de referência. Esse processo de encapsulamento é conhecido como *conversão* do valor.

> [!IMPORTANT]
> Você pode converter e desconverter qualquer tipo que você possa passar para uma API Windows Runtime. Em outras palavras, um tipo do Windows Runtime. Valores numéricos e de texto (cadeias de caracteres) são os exemplos fornecidos acima. Outro exemplo é um `struct` que você define em IDL. Se você tentar usar um `struct` C++ comum (um que não esteja definido em IDL), o compilador lembrará você de que é possível converter apenas um tipo de Windows Runtime. Uma classe de runtime é um tipo de Windows Runtime, mas é claro que você pode passar classes de runtime para APIs do Windows Runtime sem convertê-las.

O [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) fornece a função [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value), que pega um valor escalar e retorna o valor convertido em **IInspectable**. Para converter um **IInspectable** via unboxing de volta ao valor escalar, há as funções [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) e [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Exemplos de conversão boxing de um valor
A função de acessador [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) retorna uma [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), que é um valor escalar. Podemos converter o valor **hstring** e passá-lo para uma função que espera **IInspectable** da seguinte maneira.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Para definir a propriedade de conteúdo de um [**Button**](/uwp/api/windows.ui.xaml.controls.button) XAML, chame a função modificadora [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Para definir a propriedade de conteúdo como um valor de cadeia de caracteres, use esse código.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Primeiro, o construtor de conversão [**hstring**](/uwp/cpp-ref-for-winrt/hstring) converte a cadeia de caracteres literal em uma **hstring**. Em seguida, chama-se a sobrecarga de **winrt::box_value** que usa uma **hstring**.

## <a name="examples-of-unboxing-an-iinspectable"></a>Exemplos de conversão unboxing de um IInspectable
Nas funções que esperam **IInspectable**, use [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) para fazer a conversão unboxing, e [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) para fazer a conversão unboxing com um valor padrão.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Determinar o tipo de um valor de conversão boxing
Se você receber um valor de conversão boxing e não tiver certeza de qual tipo ele contém (é preciso saber o tipo para convertê-lo), confira o valor de conversão boxing para a interface [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) e chame **Type**. Aqui está um exemplo de código.

`WINRT_ASSERT` é uma definição de macro e se expande para [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>APIs importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Modelo de função winrt::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Modelo de função winrt::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [Modelo de função winrt::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)
