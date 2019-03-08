---
description: C + + c++ /CLI WinRT fornece funções e classes base que economizar muito tempo e esforço quando você deseja implementar e/ou passar coleções.
title: Coleções com C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, padrão e c++, cpp, winrt, projeção, coleção
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635431"
---
# <a name="collections-with-cwinrt"></a>Coleções com C++/WinRT

Internamente, uma coleção de tempo de execução do Windows tem muitas partes móveis complicada. Mas quando você deseja passar um objeto de coleção para uma função de tempo de execução do Windows, ou para implementar seus próprios tipos de coleção e de propriedades de coleção, existem funções e classes base na [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para dar suporte a você. Esses recursos levar a complexidade de suas mãos e salvar muita sobrecarga no tempo e esforço.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) é a interface do Windows Runtime implementada por qualquer coleção de acesso aleatório de elementos. Se você implementar **IVector** por conta própria, você também precisaria implementar [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_), e [ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Mesmo se você *precisa* um tipo de coleção personalizada, que é muito trabalho. Mas se você tiver dados em uma **std:: Vector** (ou um **std:: Map**, ou uma **std:: unordered_map**) e tudo o que você deseja fazer é passá-lo para uma API de tempo de execução do Windows e, em seguida, você gostaria de evitar fazer Esse nível de trabalho, se possível. E evitá-lo *é* possível, porque C + + c++ /CLI WinRT ajuda você a criar coleções de forma eficiente e com pouco esforço.

Consulte também [controles de itens de XAML; associar a um C + + c++ /CLI WinRT coleção](binding-collection.md).

> [!NOTE]
> Se você ainda não instalou o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, em seguida, você não terá acesso às funções e classes base que estão documentadas neste tópico. Em vez disso, consulte [se você tiver uma versão mais antiga do SDK do Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) para obter uma listagem de um modelo de vetor observável que você pode usar em vez disso.

## <a name="helper-functions-for-collections"></a>Funções auxiliares para coleções

### <a name="general-purpose-collection-empty"></a>Coleção de uso geral, vazia

Esta seção aborda o cenário em que você deseja criar uma coleção que é inicialmente vazia. e, em seguida, populá-lo *após* criação.

Para recuperar um novo objeto de um tipo que implementa uma coleção de uso geral, você pode chamar o [ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector) modelo de função. O objeto é retornado como um [ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_), e essa é a interface por meio do qual você chamar o objeto retornado funções e propriedades.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

Como você pode ver no exemplo de código acima, após a criação da coleção você pode acrescentar elementos, iterar sobre eles e geralmente tratam o objeto, como você faria com qualquer objeto de coleção de tempo de execução do Windows que você talvez tenha recebido de uma API. Se precisar de uma exibição imutável sobre a coleção, você pode chamar [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), conforme mostrado. O padrão mostrado acima&mdash;de criação e uso de uma coleção&mdash;é adequado para cenários simples em que você deseja passar dados em, ou obter dados para fora de uma API. Você pode passar uma **IVector**, ou uma **IVectorView**, em qualquer lugar uma [ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_) é esperado.

No exemplo de código acima, a chamada para **winrt::init_apartment** inicializa COM; por padrão, em um apartamento de vários threads.

### <a name="general-purpose-collection-primed-from-data"></a>Coleção de uso geral, preparada de dados

Esta seção aborda o cenário em que você deseja criar uma coleção e preenchê-lo ao mesmo tempo.

Você pode evitar a sobrecarga das chamadas para **Append** no exemplo de código anterior. Talvez você já tenha os dados de origem, ou talvez você prefira popular os dados de origem antes de criar o objeto de coleção de tempo de execução do Windows. Aqui está como fazer isso.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Você pode passar um objeto temporário que contém seus dados para **winrt::single_threaded_vector**, assim como acontece com `coll1`, acima. Ou você pode mover uma **std:: Vector** (supondo que você não será possível acessá-lo novamente) em uma função. Em ambos os casos, você está passando um *rvalue* em uma função. Isso permite que o compilador para ser eficiente e para evitar a cópia dos dados. Se você quiser saber mais sobre *rvalues*, consulte [categorias de valor e as referências a eles](cpp-value-categories.md).

Se você quiser associar um controle de itens XAML para sua coleção, em seguida, você pode. Mas lembre-se que definir corretamente as [ **ItemsControl** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propriedade, você precisa defini-lo como um valor do tipo **IVector** de **IInspectable** (ou de um tipo de interoperabilidade, como [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Aqui está um exemplo de código que produz uma coleção de um tipo adequado para associação e acrescenta um elemento a ele.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Você pode criar uma coleção de tempo de execução do Windows a partir de dados e obter uma exibição nele pronto para passar para uma API, tudo sem copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Nos exemplos acima, a coleção criamos *pode* ser associado a um XAML itens de controle; mas a coleção não é observável.

### <a name="observable-collection"></a>Coleção observável

Para recuperar um novo objeto de um tipo que implementa um *observável* coleção, chame o [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) modelo de função com qualquer tipo de elemento. Mas, para tornar uma coleção observable adequado para associação a um controle de itens XAML, use **IInspectable** como o tipo de elemento.

O objeto é retornado como um [ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), e essa é a interface por meio do qual você (ou o controle ao qual ele está associado) chama o objeto retornado funções e propriedades.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obter mais detalhes e exemplos de código, sobre a associação do usuário (IU) controles de interface para uma coleção observable, consulte [controles de itens de XAML; associar a um C + + c++ /CLI WinRT coleção](binding-collection.md).

### <a name="associative-collection-map"></a>Coleção associativa (map)

Há versões de coleção associativa das duas funções que vimos.

- O [ **winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) template de função retorna uma coleção de associação não observables como um [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- O [ **winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) template de função retorna uma coleção associativa observável como um [ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Você pode, opcionalmente, primo essas coleções com os dados, passando para a função de um *rvalue* do tipo **std:: Map** ou **std:: unordered_map**.

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>Single-Thread

O "single-threaded" nos nomes de uma dessas funções indica que eles não fornecem nenhuma simultaneidade&mdash;em outras palavras, eles não são thread-safe. A menção de threads está relacionada à apartments, porque os objetos retornados dessas funções são todos agile (consulte [Agile objetos no C + + c++ /CLI WinRT](agile-objects.md)). É assim que os objetos são de thread único. E isso é totalmente apropriado se você quiser apenas passar dados de uma forma ou outra pela interface binária de aplicativo (ABI).

## <a name="base-classes-for-collections"></a>Classes base para coleções

Se, para total flexibilidade, você deseja implementar sua própria coleção personalizada, em seguida, você vai querer evitar fazer isso da maneira mais difícil. Por exemplo, isso é como uma exibição personalizada de vetor ficaria *sem a assistência do C + + c++ /CLI classes de base do WinRT*.

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

Em vez disso, é muito mais fácil derivar sua exibição personalizada de vetor do [ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base) struct de modelo e apenas implementar a **get_container** função expor o contêiner que contém seus dados.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

O contêiner retornado por **get_container** deve fornecer o **começar** e **final** interface que **winrt::vector_view_base** espera. Conforme mostrado no exemplo anterior, **std:: Vector** fornece que. Mas você pode retornar qualquer contêiner que atenda o mesmo contrato, incluindo seu próprio contêiner personalizado.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

Esses são a base de classes que C + + c++ /CLI WinRT fornece para ajudar a implementar coleções personalizadas.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Consulte os exemplos de código acima.

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>APIs Importantes
* [Propriedade ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interface IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [WinRT::map_base struct modelo](/uwp/cpp-ref-for-winrt/map-base)
* [WinRT::map_view_base struct modelo](/uwp/cpp-ref-for-winrt/map-view-base)
* [WinRT::observable_map_base struct modelo](/uwp/cpp-ref-for-winrt/observable-map-base)
* [WinRT::observable_vector_base struct modelo](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [modelo de função WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [modelo de função WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [modelo de função WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [modelo de função WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [WinRT::vector_base struct modelo](/uwp/cpp-ref-for-winrt/vector-base)
* [WinRT::vector_view_base struct modelo](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Tópicos relacionados
* [Categorias de valor e as referências a eles](cpp-value-categories.md)
* [XAML itens controles; associar a um C + + c++ /CLI coleção WinRT](binding-collection.md)
