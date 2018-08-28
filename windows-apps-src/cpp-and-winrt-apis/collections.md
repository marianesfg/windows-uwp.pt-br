---
author: stevewhims
description: C + + / WinRT fornece funções e classes base que poupá-lo de tempo e esforço quando quiser implementar e/ou passe conjuntos.
title: Coleções com C + + / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, coleção
ms.localizationpriority: medium
ms.openlocfilehash: dacfe4135402b85bac68b63c06f99f97001fa5b9
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2889323"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Coleções com [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Internamente, uma coleção de tempo de execução do Windows tem várias partes móveis complicado. Mas quando você deseja passar um objeto de coleção a uma função de tempo de execução do Windows ou para implementar seus próprios tipos de coleção e propriedades da coleção, há funções e classes base em C + + / WinRT para dar suporte a você. Esses recursos eliminam a complexidade do suas mãos e salvar muita sobrecarga no tempo e esforço.

> [!IMPORTANT]
> Os recursos descritos neste tópico estão disponíveis se você tiver instalado o [Windows 10 SDK Preview Build 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou posterior.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) é implementado por qualquer coleção de acesso aleatório de elementos de interface do tempo de execução do Windows. Se você estava implementar a **IVector** , você também precisaria implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)e [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Mesmo que você *precisa de* uma coleção adaptada digitar, que é muito trabalho. Mas, se você tiver dados em um **std:: Vector** (ou um **std::map**ou um **std::unordered_map**) e tudo o que você deseja fazer é passá-lo para uma API de tempo de execução do Windows, em seguida, seria deseja evitar fazer o respectivo nível de trabalho, se possível. E evitando *é* possível, pois C + + / WinRT ajuda você a criar conjuntos de forma eficiente e com pouco esforço.

## <a name="helper-functions-for-collections"></a>Funções auxiliares para coleções

### <a name="general-purpose-collection-empty"></a>Conjunto de finalidade geral, vazio

Para recuperar um novo objeto do tipo que implementa um conjunto de finalidade geral, é possível chamar o modelo de função [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . O objeto é retornado como um [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)e que é a interface através da qual você chamar funções e propriedades do objeto retornado.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

Como você pode ver no exemplo de código acima, após a criação da coleção você pode acrescentar elementos, iterá-los e geralmente tratam do objeto como faria com qualquer objeto da coleção de tempo de execução do Windows que você pode ter recebido a partir de uma API. Se você precisar de um modo de exibição imutável sobre a coleção, você pode chamar [IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview), conforme mostrado. O padrão mostrado acima&mdash;de criação e consumindo uma coleção&mdash;é apropriada para cenários simples onde deseja passar dados em ou obter dados para fora de uma API.

### <a name="general-purpose-collection-primed-from-data"></a>Conjunto de finalidade geral, preparado a partir de dados

Você também pode evitar a sobrecarga de chamadas para **Append** que você pode ver no exemplo de código acima. Talvez você já tenha os dados de origem, ou talvez você prefira popula antes de começar a criar o objeto da coleção de tempo de execução do Windows. Aqui está o que fazer.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Você pode passar um objeto temporário que contém seus dados para **winrt::single_threaded_vector**, assim como a `coll1`, acima. Ou você pode mover um **std:: Vector** (supondo que você não ser acessá-lo novamente) para a função. Em ambos os casos, você está passando uma *rvalue* para a função. Isso permite que o compilador para serem eficientes e evitar a cópia dos dados. Se você deseja saber mais sobre *rvalues*, consulte [categorias de valor e referências a eles](cpp-value-categories.md).

Se você deseja ligar um controle de itens XAML ao seu conjunto, em seguida, você pode. Mas, lembre-se de que, para definir corretamente a propriedade [**ItemsControl**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , você precisará defini-la como um valor de tipo **IVector** de **IInspectable** (ou de um tipo de interoperabilidade como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Aqui está um exemplo de código que produz uma coleção de um tipo adequado para vinculação e anexa um elemento a ela.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

A coleção acima *pode* ser vinculado a um controle de itens XAML; mas a coleção não é observáveis.

### <a name="observable-collection"></a>Coleção observável

Para recuperar um novo objeto do tipo que implementa a uma coleção *observável* , chame o modelo de função [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) com qualquer tipo de elemento. Mas, para tornar uma coleção observável adequado para associação a um controle de itens XAML, usar **IInspectable** como o tipo de elemento.

O objeto é retornado como um [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)e que é a interface através da qual você (ou o controle ao qual ele está acoplado) chamar funções e propriedades do objeto retornado.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para mais detalhes e exemplos de código, sobre associando seu usuário, interface do controles a uma coleção observável, consulte [controles de itens de XAML; vincular a C + + / WinRT coleção](binding-collection.md).

### <a name="associative-collection-map"></a>Coleção de associação (map)

Há versões da coleção associação das duas funções que vimos.

- O modelo de função [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) retorna uma coleção de associação de não-observáveis como um [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- O modelo de função [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) retorna uma coleção de associação observável como um [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente, você pode primo essas coleções com dados passando para a função uma *rvalue* do tipo **std::map** ou **std::unordered_map**.

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

### <a name="single-threaded"></a>Com um único segmento

O "single-threaded" nos nomes dessas funções indica que eles não fornecem nenhum simultaneidade&mdash;em outras palavras, eles não estejam thread-safe. O mencionam de threads está relacionado ao apartments, como os objetos retornados dessas funções são todos ágil (consulte [ágil e objetos em C + + / WinRT](agile-objects.md)). É apenas que os objetos são com um único segmento. E isso é totalmente apropriado, se você quiser apenas passar dados de uma forma ou outro através da interface de binários de aplicativo (ABI).

## <a name="base-classes-for-collections"></a>Classes base para coleções

Se, para obter flexibilidade completa, você deseja implementar sua própria coleção personalizada, você desejará evitar fazer isso da maneira mais difícil. Por exemplo, esta é a aparência um modo de exibição personalizado vetor *sem a Ajuda do C + + / classes de base do WinRT*.

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

Em vez disso, é muito mais fácil derivar sua opinião vetoriais personalizados o modelo de struct [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) e implementar apenas a função **get_container** para expor o contêiner mantendo os dados.

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

O contêiner retornado pela **get_container** deve fornecer a interface **begin** e **end** que **winrt::vector_view_base** espera. Conforme mostrado no exemplo acima, **std:: Vector** fornece que. Mas você pode retornar qualquer contêiner que atenda o mesmo contrato, incluindo seu próprio contêiner personalizado.

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

Essas são a base de classes que C + + / WinRT oferece para ajudá-lo a implementar coleções personalizadas.

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

## <a name="important-apis"></a>APIs importantes
* [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Tópicos relacionados
* [Categorias de valor e referências a eles](cpp-value-categories.md)
* [Controles de itens XAML; vincular a uma coleção C++/WinRT](binding-collection.md)
