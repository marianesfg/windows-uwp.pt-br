---
author: stevewhims
description: Um valor escalar precisa ser encapsulado em um objeto de classe de referência antes de ser passado para uma função que espera **IInspectable**. Esse processo de encapsulamento é conhecido como *conversão* do valor.
title: Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, boxing, escalar, valor
ms.localizationpriority: medium
ms.openlocfilehash: f4b99f587fbd517b677d85b50abb26fdf072b359
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7301328"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Valores de conversão de boxing e unboxing para IInspectable com C++/WinRT
 
A [**interface IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) é a interface raiz de cada classe de tempo de execução no Windows Runtime (WinRT). Isso é uma ideia análoga [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) estando na raiz de cada interface e classe COM; e **System.Object** estando na raiz de cada classe [Sistema de tipo comum](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

Em outras palavras, uma função que espera **IInspectable** pode ser passada como uma instância de qualquer classe de tempo de execução. Mas você não pode passar diretamente um valor escalar, como um valor numérico ou de texto, para uma função. Em vez disso, um valor escalar precisa ser encapsulado dentro de um objeto de classe de referência. Esse processo de encapsulamento é conhecido como *conversão* do valor.

[C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) fornece a função [**WinRT:: box_value**](/uwp/cpp-ref-for-winrt/box-value) , que leva um valor escalar e retorna o valor contido em um **IInspectable**. Para deixar um **IInspectable** de volta ao valor escalar, há as funções [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) e [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Exemplos de contensão de um valor
A função de acessador [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) retorna um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), que é um valor escalar. Podemos conter o valor **hstring** e passá-lo para uma função que espera **IInspectable** da seguinte maneira.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Para definir a propriedade de conteúdo de um [**Botão**](/uwp/api/windows.ui.xaml.controls.button) XAML, você chama o modificador de função [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Para definir a propriedade de conteúdo como um valor de sequência, você pode usar esse código.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Primeiro, o construtor de conversão [**hstring**](/uwp/cpp-ref-for-winrt/hstring) converte a cadeia de caracteres literal em um **hstring**. Em seguida, a sobrecarga de **winrt::box_value** que leva um **hstring** é invocada.

## <a name="examples-of-unboxing-an-iinspectable"></a>Exemplos de unboxing um IInspectable
Em suas próprias funções que esperam **IInspectable**, você pode usar [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) para retirar, e você pode usar [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) para retirar com um valor padrão.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Determinar o tipo de um valor boxed
Se você receber um valor boxes e não tiver certeza de qual tipo contém (é preciso saber o tipo para convertê-lo), é possível consultar o valor boxed para a interface [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) e, em seguida, chamar **Type**. A seguir está um exemplo de código.

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>APIs Importantes
* [Interface IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Modelo de função winrt::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [Struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Modelo de função winrt::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [Modelo de função winrt::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)
