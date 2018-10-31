---
author: stevewhims
description: C++ c++ WinRT fornece funções e classes base que economizar muito tempo e esforço quando você deseja implementar e/ou passe coleções.
title: Coleções com C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, coleção
ms.localizationpriority: medium
ms.openlocfilehash: 93b486021813abf320645888d4f19971dc2c80ab
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861891"
---
# <a name="collections-with-cwinrt"></a>Coleções com C++/WinRT

Internamente, uma coleção do Windows Runtime tem muitas das partes móveis complicados. Mas quando você deseja passar um objeto de coleção para uma função de tempo de execução do Windows, ou para implementar seus próprios tipos de coleção e propriedades de coleção, há funções e classes base na [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para dar suporte a você. Esses recursos eliminam a complexidade do seu laboratório e salvar muita sobrecarga no tempo e esforço.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) é a interface de tempo de execução do Windows implementada por qualquer coleção de acesso aleatório de elementos. Se você implementar **IVector** por conta própria, você também precisará implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)e [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Mesmo que você *precisa de* uma coleção personalizada de tipos, que é uma grande quantidade de trabalho. Mas se você tiver dados em um **std:: Vector** (ou um **std:: Map**ou um **std::unordered_map**) e tudo o que você deseja fazer é passá-lo para uma API de tempo de execução do Windows, em seguida, você desejaria evitar fazer esse nível de trabalho, se possível. E evitando *é* possível, pois C++ c++ WinRT ajuda você a criar coleções com eficiência e com pouco esforço.

Consulte também [controles de itens XAML; vincular a C++ c++ WinRT coleção](binding-collection.md).

> [!NOTE]
> Se você ainda não tiver instalado o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, em seguida, você não terá acesso às funções e às classes base que estão documentadas neste tópico. Em vez disso, veja [se você tiver uma versão mais antiga do SDK do Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) para uma listagem de um modelo de vetor observável que você pode usar em vez disso.

## <a name="helper-functions-for-collections"></a>Funções auxiliares para coleções

### <a name="general-purpose-collection-empty"></a>Coleção de uso geral, vazia

Esta seção aborda o cenário em que você deseja criar uma coleção que é inicialmente vazia; e, em seguida, preenchê-la *após* a criação.

Para recuperar um novo objeto de um tipo que implemente uma coleção de finalidade geral, você pode chamar o modelo de função [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . O objeto é retornado como um [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), e que é a interface por meio do qual você chamar funções e propriedades do objeto retornado.

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

Como você pode ver no exemplo de código acima, depois de criar a coleção você pode acrescentar elementos, iterar sobre eles e geralmente tratarem o objeto como faria com qualquer objeto de coleção do Windows Runtime que você pode ter recebido de uma API. Se você precisar de uma exibição imutável sobre a coleção, você pode chamar [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), conforme mostrado. O padrão mostrado acima&mdash;de criar e consumir uma coleção&mdash;é adequado para cenários simples onde você deseja passar dados em ou obter dados fora, uma API. Você pode passar um **IVector**ou um **IVectorView**, em qualquer lugar um [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) é esperado.

No exemplo de código acima, a chamada para o **WinRT:: init_apartment** inicializa COM; Por padrão, em um multithreaded apartment.

### <a name="general-purpose-collection-primed-from-data"></a>Coleção de uso geral, preparada de dados

Esta seção aborda o cenário em que você deseja criar uma coleção e preenchê-la ao mesmo tempo.

Você pode evitar a sobrecarga das chamadas para **Acrescentar** no exemplo de código anterior. Talvez você já tenha os dados de origem, ou talvez você prefira preencher os dados de origem antes de criar o objeto de coleção do Windows Runtime. Veja como fazer isso.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Você pode passar um objeto temporário que contém os dados para **winrt::single_threaded_vector**, como com `coll1`, acima. Ou você pode passar um **std:: Vector** (supondo que você não ser acessá-la novamente) para a função. Em ambos os casos, você está passando um *rvalue* para a função. Isso permite que o compilador para ser eficiente e evitar copiar os dados. Se você quiser saber mais sobre *rvalues*, consulte [categorias de valor e referências a elas](cpp-value-categories.md).

Se você deseja associar um controle de itens XAML a sua coleção, em seguida, você pode. Mas lembre-se de que, para definir corretamente a propriedade [**ItemsControl**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , você precisa defini-lo como um valor do tipo **IVector** de **IInspectable** (ou de um tipo de interoperabilidade como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Aqui está um exemplo de código que produz uma coleção de um tipo adequado para associação e acrescenta um elemento a ela.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Você pode criar uma coleção de tempo de execução do Windows de dados e obter uma visão nele pronto para passar para uma API, tudo sem copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Nos exemplos acima, a coleção criamos *pode* ser vinculado a um controle de itens XAML; mas a coleção não é observável.

### <a name="observable-collection"></a>Coleção observável

Para recuperar um novo objeto de um tipo que implemente uma coleção *observável* , chame o modelo de função [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) com qualquer tipo de elemento. Mas, para tornar uma coleção observável adequados para a associação a um controle de itens XAML, use **IInspectable** como o tipo de elemento.

O objeto é retornado como um [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), e que é a interface por meio do qual você (ou o controle ao qual ele está vinculado) chama funções e propriedades do objeto retornado.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obter mais detalhes e exemplos de código, sobre o usuário de associação, interface (IU) controla a uma coleção observável, consulte [controles de itens XAML; vincular a C++ c++ WinRT coleção](binding-collection.md).

### <a name="associative-collection-map"></a>Coleção associativa (mapear)

Existem versões de coleção associativos das duas funções que vimos.

- O modelo de função [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) retorna uma coleção de associação não observáveis como um [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- O modelo de função [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) retorna uma coleção observável associativa como um [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente, você pode preparar essas coleções com dados passando para a função de um *rvalue* de tipo **abrigar** ou **std::unordered_map**.

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

### <a name="single-threaded"></a>Thread único

O "single-threaded" nos nomes dessas funções indica que eles não forneçam qualquer simultaneidade&mdash;em outras palavras, não elas seguras. A menção de threads está relacionada ao apartments, porque os objetos retornados essas funções são ágeis tudo (consulte [objetos ágeis em C++ c++ WinRT](agile-objects.md)). É assim que os objetos são de thread único. E é totalmente apropriado se você só quiser passar dados de uma maneira ou outro entre a interface binária do aplicativo (ABI).

## <a name="base-classes-for-collections"></a>Classes base para coleções

Se, para flexibilidade completa, você deseja implementar sua própria coleção personalizada, você vai querer evitar fazer isso da maneira mais difícil. Por exemplo, esse é um modo de exibição personalizado de vetor aparência *sem a Ajuda do C++ c++ classes base do WinRT*.

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

Em vez disso, é muito mais fácil derivar o modo de exibição personalizado de vetor de modelo de struct [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) e implementar apenas a função **get_container** para expor o contêiner mantém seus dados.

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

O contêiner retornado por **get_container** deve fornecer a interface de **início** e **término** que **winrt::vector_view_base** espera. Conforme mostrado no exemplo acima, **std:: Vector** fornece que. Mas você pode retornar qualquer contêiner que atenda o mesmo contrato, incluindo seu próprio contêiner personalizado.

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

Esses são a base de classes que C + c++ WinRT fornece para ajudar a implementar coleções personalizadas.

### [<a name="winrtvectorviewbase"></a>WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Consulte os exemplos de código acima.

### [<a name="winrtvectorbase"></a>WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [Interface de IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface de IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [modelo de estrutura WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [modelo de estrutura WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [modelo de estrutura WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [modelo de estrutura WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [modelo de função WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [modelo de função WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [modelo de função WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [modelo de função WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [modelo de estrutura WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [modelo de estrutura WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Tópicos relacionados
* [Categorias de valor e referências a eles](cpp-value-categories.md)
* [Controles de itens XAML; associar a uma coleção C++/WinRT](binding-collection.md)
