---
description: C++/WinRT fornece funções e classes base que economizam muito tempo e esforço quando você deseja implementar e/ou passar coleções.
title: Coleções com C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, coleção
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270150"
---
# <a name="collections-with-cwinrt"></a>Coleções com C++/WinRT

Internamente, uma coleção do Tempo de Execução do Windows tem muitas partes móveis complicadas. Entretanto, quando você deseja passar um objeto de coleção para uma função do Tempo de Execução do Windows ou implementar seus próprios tipos de coleção e propriedades de coleção, há funções e classes base no [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para ajudá-lo. Esses recursos eliminam a complexidade para você e diminuem a sobrecarga de tempo e esforço.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) é a interface do Tempo de Execução do Windows implementada por qualquer coleção de acesso aleatório de elementos. Se você implementar **IVector** por conta própria, também precisará implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) e [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Mesmo se você *precisar* de um tipo de coleção personalizada, isso envolverá muito trabalho. Porém, se você tiver dados em um **std::vector** (ou em um **std::map** ou **std::unordered_map**) e quiser apenas passá-lo para uma API do Tempo de Execução do Windows, será melhor evitar esse nível de trabalho. E evitar isso *é* possível, porque o C++/WinRT ajuda você a criar coleções de forma eficiente e com pouco esforço.

Confira também [Controles de itens XAML; associar a uma coleção C++/WinRT](binding-collection.md).

## <a name="helper-functions-for-collections"></a>Funções auxiliares para coleções

### <a name="general-purpose-collection-empty"></a>Coleção de uso geral, vazia

Esta seção aborda o cenário em que você deseja criar uma coleção que está inicialmente vazia e, em seguida, preenchê-la *após* a criação.

Para recuperar um novo objeto de um tipo que implementa uma coleção de uso geral, chame o modelo de função [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector). O objeto retorna como um [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), e essa é a interface por meio da qual você chama as funções e as propriedades do objeto retornado.

Se quiser copiar e colar os exemplos de código a seguir diretamente no arquivo de código-fonte principal de um projeto de **Aplicativo de Console do Windows (C++/WinRT)** , primeiro defina **Não usar Cabeçalhos Pré-compilados** nas propriedades do projeto.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

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

Como você pode ver no exemplo de código anterior, após a criação da coleção é possível acrescentar elementos, iterá-los e geralmente tratar o objeto como você faria com qualquer objeto de coleção do Tempo de Execução do Windows, que você pode ter recebido de uma API. Se precisar de uma exibição imutável da coleção, você poderá chamar [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), conforme mostrado. O padrão apresentado anteriormente&mdash;de criar e consumir uma coleção&mdash;é apropriado para cenários simples em que você deseje passar dados para uma API ou extrair dados dela. Você pode passar um **IVector** ou um **IVectorView** em qualquer lugar em que um [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) é esperado.

No exemplo de código anterior, a chamada para **winrt::init_apartment** inicializa o thread no Windows Runtime; por padrão, em um Multi-Threaded Apartment. A chamada também inicializa o COM.

### <a name="general-purpose-collection-primed-from-data"></a>Coleção de uso geral, preenchida com dados

Esta seção aborda o cenário em que você deseja criar uma coleção e preenchê-la ao mesmo tempo.

Você pode evitar a sobrecarga de chamadas para **Append** no exemplo de código anterior. Talvez você já tenha os dados de origem ou prefira preencher os dados de origem antes de criar o objeto de coleção do Tempo de Execução do Windows. Veja a seguir como fazer isso.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Você pode passar um objeto temporário que contenha seus dados para **winrt::single_threaded_vector**, assim como acontece com `coll1`, acima. Ou pode mover um **std::vector** (supondo que você não o acessará novamente) para uma função. Em ambos os casos, você estará passando um *rvalue* para uma função. Isso permite que o compilador seja eficiente e evite a cópia dos dados. Se quiser saber mais sobre *rvalues*, confira [Categorias de valor e as referências a elas](cpp-value-categories.md).

Se quiser associar um controle de itens XAML à sua coleção, você poderá. Mas esteja ciente de que, para definir corretamente a propriedade [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), será necessário defini-la como um valor de tipo **IVector** de **IInspectable** (ou de um tipo de interoperabilidade como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)).

Veja a seguir um exemplo de código que produz uma coleção de um tipo adequado para associação e acrescenta um elemento a ele. Você pode encontrar o contexto para esse exemplo de código em [Controles de itens XAML; associar a uma coleção C++/WinRT](binding-collection.md).

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

É possível criar uma coleção do Tempo de Execução do Windows a partir de dados e obter uma exibição dela pronta para passar para uma API, tudo sem copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

Nos exemplos acima, a coleção que criamos *pode* ser associada a um controle de itens XAML; mas a coleção não é observável.

### <a name="observable-collection"></a>Coleção observável

Para recuperar um novo objeto de um tipo que implementa uma coleção *observável*, chame o modelo de função [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) com qualquer tipo de elemento. Porém, para tornar uma coleção observável adequada para associação a um controle de itens XAML, use **IInspectable** como o tipo de elemento.

O objeto retorna como um [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), e essa é a interface por meio da qual você (ou o controle ao qual ela está associada) chama as funções e as propriedades do objeto retornado.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obter mais detalhes e exemplos de código sobre a associação de controles da interface do usuário (IU) a uma coleção observável, confira [Controles de itens de XAML; associar a uma coleção C++/WinRT](binding-collection.md).

### <a name="associative-collection-map"></a>Coleção associativa (mapa)

Há versões de coleção associativa das duas funções que vimos.

- O modelo de função [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) retorna uma coleção de associação não observável como um [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- O modelo de função [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) retorna uma coleção associativa observável como um [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

É possível preencher essas coleções com dados, passando para a função um *rvalue* do tipo **std::map** ou **std::unordered_map**.

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

O "single-threaded" nos nomes dessas funções indica que elas não fornecem nenhuma simultaneidade&mdash;em outras palavras, elas não são thread-safe. A menção a threads não está relacionada a apartments, porque os objetos retornados dessas funções são todos agile (confira [Objetos agile no C++/WinRT](agile-objects.md)). Os objetos são single-threaded. Caso você queira apenas passar os dados de uma forma ou de outra pela interface binária do aplicativo (ABI), isso é totalmente apropriado.

## <a name="base-classes-for-collections"></a>Classes base para coleções

Se, para total flexibilidade, você quiser implementar sua própria coleção personalizada, evite fazer isso da maneira mais difícil. Por exemplo, esta é a aparência de uma exibição de vetor personalizada *sem a assistência das classes base do C++/WinRT*.

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

Em vez disso, é muito mais fácil derivar a exibição personalizada de vetor do modelo de struct [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) e apenas implementar a função **get_container** para expor o contêiner que mantém seus dados.

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

O contêiner retornado por **get_container** deve fornecer o **início** e o **fim** da interface que **winrt::vector_view_base** espera. Conforme mostrado no exemplo anterior, **std::vector** oferece isso. Porém, é possível retornar qualquer contêiner que atenda ao mesmo contrato, incluindo seu próprio contêiner personalizado.

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

Estas são as classes base que o C++/WinRT fornece para ajudar a implementar as coleções personalizadas.

### <a name="winrtvector_view_baseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Confira os exemplos de código acima.

### <a name="winrtvector_baseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### <a name="winrtobservable_vector_baseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### <a name="winrtmap_view_baseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### <a name="winrtmap_baseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### <a name="winrtobservable_map_baseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [Propriedade ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interface IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interface IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [Modelo de struct winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [Modelo de struct winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [Modelo de struct winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [Modelo de struct winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [Modelo de função winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [Modelo de função winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [Modelo de função winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Modelo de função winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [Modelo de struct winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [Modelo de struct winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Tópicos relacionados
* [Categorias de valor e referências a eles](cpp-value-categories.md)
* [Controles de itens XAML; associar a uma coleção C++/WinRT](binding-collection.md)
