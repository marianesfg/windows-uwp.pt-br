---
author: stevewhims
description: Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Este tópico mostra como implementar e consumir uma coleção observável e como associar um controle de itens XAML a ela.
title: Controles de itens XAML; vincular a uma coleção C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, vínculo, coleção
ms.localizationpriority: medium
ms.openlocfilehash: 9337c0625c68970d9e68df74fa13228369e8bf41
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834125"
---
# <a name="xaml-items-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-collection"></a>Controles de itens XAML; vincular a uma coleção [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Essa ideia é baseada no padrão de design do software conhecido como o *padrão do observador*. Este tópico mostra como implementar coleções observáveis em C++/WinRT, e como vincular controles de itens XAML a elas.

Este passo a passo se baseia no projeto criado em [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md), e ele adiciona aos conceitos explicados nesse tópico.

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>O que faz *observável* significa para uma coleção?
Se uma classe de tempo de execução que representa uma coleção escolhe acionar o evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) sempre que um elemento é adicionado a ele ou removido dele, então a classe de tempo de execução é um coleção observável.

Um controle de itens XAML pode se associar a e manipular esses eventos, recuperando a coleção atualizada e, em seguida, atualizando ele mesmo para mostrar os elementos atuais.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="implement-singlethreadedobservablevectorlttgt"></a>Implementar **single_threaded_observable_vector&lt;T&gt;**
É bom ter um modelo de vetor observável para servir como uma implementação útil e de uso geral de [**IObservableVector&lt;T&gt;**](/uwp/api/windows.foundation.collections.iobservablevector_t_). Veja uma lista de uma classe chamada **single_threaded_observable_vector\<T>**.

> [!NOTE]
> Se você instalou o [Windows 10 SDK Preview Build 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou posterior, em seguida, você pode apenas diretamente use a função de fábrica **winrt::single_threaded_observable_vector\ < T\ >** em vez de listagem abaixo de código. Se você não ainda estiver nessa versão do SDK, será fácil alternem do usando a versão de listagem de código para a função **winrt** quando você estiver. Lembre-se de que, em vez de chamar [**winrt::make**]() com o tipo listado abaixo, você em vez disso chamar a função **winrt::single_threaded_observable_vector\ < T\ >** .

```cppwinrt
// single_threaded_observable_vector.h
#pragma once

namespace winrt::Bookstore::implementation
{
    using namespace Windows::Foundation::Collections;

    template <typename T>
    struct single_threaded_observable_vector : implements<single_threaded_observable_vector<T>,
        IObservableVector<T>,
        IVector<T>,
        IVectorView<T>,
        IIterable<T>>
    {
        event_token VectorChanged(VectorChangedEventHandler<T> const& handler)
        {
            return m_changed.add(handler);
        }

        void VectorChanged(event_token const cookie)
        {
            m_changed.remove(cookie);
        }

        T GetAt(uint32_t const index) const
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            return m_values[index];
        }

        uint32_t Size() const noexcept
        {
            return static_cast<uint32_t>(m_values.size());
        }

        IVectorView<T> GetView()
        {
            return *this;
        }

        bool IndexOf(T const& value, uint32_t& index) const noexcept
        {
            index = static_cast<uint32_t>(std::find(m_values.begin(), m_values.end(), value) - m_values.begin());
            return index < m_values.size();
        }

        void SetAt(uint32_t const index, T const& value)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values[index] = value;
            m_changed(*this, make<args>(CollectionChange::ItemChanged, index));
        }

        void InsertAt(uint32_t const index, T const& value)
        {
            if (index > m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.insert(m_values.begin() + index, value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, index));
        }

        void RemoveAt(uint32_t const index)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.erase(m_values.begin() + index);
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, index));
        }

        void Append(T const& value)
        {
            ++m_version;
            m_values.push_back(value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
        }

        void RemoveAtEnd()
        {
            if (m_values.empty())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.pop_back();
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, Size()));
        }

        void Clear() noexcept
        {
            ++m_version;
            m_values.clear();
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        uint32_t GetMany(uint32_t const startIndex, array_view<T> values) const
        {
            if (startIndex >= m_values.size())
            {
                return 0;
            }

            uint32_t actual = static_cast<uint32_t>(m_values.size() - startIndex);

            if (actual > values.size())
            {
                actual = values.size();
            }

            std::copy_n(m_values.begin() + startIndex, actual, values.begin());
            return actual;
        }

        void ReplaceAll(array_view<T const> value)
        {
            ++m_version;
            m_values.assign(value.begin(), value.end());
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        IIterator<T> First()
        {
            return make<iterator>(this);
        }

    private:

        std::vector<T> m_values;
        event<VectorChangedEventHandler<T>> m_changed;
        uint32_t m_version{};

        struct args : implements<args, IVectorChangedEventArgs>
        {
            args(CollectionChange const change, uint32_t const index) :
                m_change(change),
                m_index(index)
            {
            }

            CollectionChange CollectionChange() const
            {
                return m_change;
            }

            uint32_t Index() const
            {
                return m_index;
            }

        private:

            Windows::Foundation::Collections::CollectionChange const m_change{};
            uint32_t const m_index{};
        };

        struct iterator : implements<iterator, IIterator<T>>
        {
            explicit iterator(single_threaded_observable_vector<T>* owner) noexcept :
            m_version(owner->m_version),
                m_current(owner->m_values.begin()),
                m_end(owner->m_values.end())
            {
                m_owner.copy_from(owner);
            }

            void abi_enter() const
            {
                if (m_version != m_owner->m_version)
                {
                    throw hresult_changed_state();
                }
            }

            T Current() const
            {
                if (m_current == m_end)
                {
                    throw hresult_out_of_bounds();
                }

                return*m_current;
            }

            bool HasCurrent() const noexcept
            {
                return m_current != m_end;
            }

            bool MoveNext() noexcept
            {
                if (m_current != m_end)
                {
                    ++m_current;
                }

                return HasCurrent();
            }

            uint32_t GetMany(array_view<T> values)
            {
                uint32_t actual = static_cast<uint32_t>(std::distance(m_current, m_end));

                if (actual > values.size())
                {
                    actual = values.size();
                }

                std::copy_n(m_current, actual, values.begin());
                std::advance(m_current, actual);
                return actual;
            }

        private:

            com_ptr<single_threaded_observable_vector<T>> m_owner;
            uint32_t const m_version;
            typename std::vector<T>::const_iterator m_current;
            typename std::vector<T>::const_iterator const m_end;
        };
    };
}
```

A função **Anexar** ilustra como disparar o evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged).

```cppwinrt
m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
```

Os argumentos do evento indicam que um elemento foi inserido e também qual é seu índice (o último elemento, neste caso). Estes argumentos permitem que um controle de itens XAML responda ao evento e seja atualizado de forma ideal.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Adicionar uma coleção **BookSkus** a **BookstoreViewModel**
Em [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md), nós adicionamos um tipo de propriedade **BookSku** a nosso modelo de visualização principal. Nesta etapa, usaremos **single_threaded_observable_vector&lt;T&gt;** para nos ajudar a implementar uma coleção observável de **BookSku** no mesmo modelo de exibição.

Declarar uma nova propriedade em `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
        Windows.Foundation.Collections.IVector<IInspectable> BookSkus{ get; };
    }
...
```

Salvar e compilar. Copie os stubs de acessador de `BookstoreViewModel.h` e `BookstoreViewModel.cpp` na pasta `Generated Files` e implemente-os.

```cppwinrt
// BookstoreViewModel.h
...
#include "single_threaded_observable_vector.h"
...
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();
        Bookstore::BookSku BookSku();
        Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookSkus();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
        Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_bookSkus;
    };
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
        m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
        m_bookSkus.Append(m_bookSku);
    }

    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
    {
        return m_bookSkus;
    }
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Vincular uma ListBox à propriedade **BookSkus**
Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Adicione a seguinte marcação dentro do mesmo **StackPanel** como o **Botão**.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

Em `MainPage.cpp`, adicione uma linha de código para o manipulador de eventos de **Clique** para acrescentar um livro à coleção.

```cppwinrt
// MainPage.cpp
...
    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
        MainViewModel().BookSkus().Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
    }
...
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos de **Clique**. Vimos que a implementação de **Anexar** aciona um evento para informar à interface do usuário que a coleção foi alterada; e o **ListBox** consulta novamente a coleção para atualizar seu próprio valor de **Itens**. Exatamente como antes, o título de um dos livros muda; e essa alteração de título será refletida no botão e na caixa de listagem.

## <a name="important-apis"></a>APIs Importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
