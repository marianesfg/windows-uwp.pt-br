---
description: Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Este tópico mostra como implementar e consumir uma coleção observável e como associar um controle de itens XAML a ela.
title: Controles de itens XAML; associar a uma coleção C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, associação, coleção
ms.localizationpriority: medium
ms.openlocfilehash: a98056190d035910a8ed83d2f37799a98b685ce6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "70304526"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Controles de itens XAML; associar a uma coleção C++/WinRT

Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Essa ideia baseia-se no padrão de design de software conhecido como o *padrão do observador*. Este tópico mostra como implementar coleções observáveis em [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) e como associar controles de itens XAML a elas (para obter informações em segundo plano, confira [Vinculação de dados](/windows/uwp/data-binding)).

Se você deseja acompanhar este tópico, recomendamos que crie primeiro o projeto descrito em [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md). Este tópico adiciona mais código e expande os conceitos explicados naquele tópico.

> [!IMPORTANT]
> Para ver conceitos e termos essenciais que ajudam a entender como utilizar e criar classes de runtime com C++/WinRT, confira [Utilizar APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>O que significa *observável* para uma coleção?
Se uma classe de runtime que representa uma coleção escolhe acionar o evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) sempre que um elemento é adicionado ou removido dele, então a classe de runtime é uma coleção observável. Um controle de itens XAML pode se associar e manipular esses eventos, recuperando a coleção atualizada e, em seguida, atualizando a si mesmo para mostrar os elementos atuais.

> [!NOTE]
> Para saber mais sobre como instalar e usar a VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Adicionar uma coleção **BookSkus** a **BookstoreViewModel**

Em [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md), nós adicionamos um tipo de propriedade **BookSku** a nosso modelo de visualização principal. Nesta etapa, usaremos o modelo de função de fábrica [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) para ajudar a implementar uma coleção observável de **BookSku** no mesmo modelo de exibição.

Declare uma nova propriedade em `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> Na listagem MIDL 3.0 acima, observe que o tipo da propriedade **BookSkus** é [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de **BookSku**. Na próxima seção deste tópico, associaremos a fonte de itens de uma [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) a **BookSkus**. Uma caixa de listagem é um controle de itens e, para definir corretamente a propriedade [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), é necessário defini-la como um valor do tipo **IObservableVector** ou **IVector**, ou de um tipo de interoperabilidade como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

> [!WARNING]
> O código mostrado neste tópico se aplica ao C++/WinRT versão 2.0.190530.8 e superiores. Se estiver usando uma versão anterior, você precisará fazer alguns pequenos ajustes no código mostrado. Na listagem MIDL 3.0 acima, altere a propriedade **BookSkus** para [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). E, em seguida, use **IInspectable** (em vez de **BookSku**) em sua implementação, também.

Salve e compile. Copie os stubs de acessador de `BookstoreViewModel.h` e `BookstoreViewModel.cpp` na pasta `\Bookstore\Bookstore\Generated Files\sources` (para saber mais, confira o tópico anterior, [Controles XAML; associar a uma propriedade de C++/WinRT](binding-property.md)). Implemente os stubs de acessador dessa forma.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Associar uma ListBox à propriedade **BookSkus**
Abra `MainPage.xaml`, que contém a marcação XAML para a página principal da interface do usuário. Adicione a seguinte marcação dentro do mesmo **StackPanel** que **Button**.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

Em `MainPage.cpp`, adicione uma linha de código ao manipulador de eventos **Click** para acrescentar um livro à coleção.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos **Click**. Vimos que a implementação de **Append** aciona um evento para informar à interface do usuário que a coleção foi alterada; a **ListBox** consulta novamente a coleção para atualizar seu próprio valor de **Items**. Exatamente como antes, o título de um dos livros muda e essa alteração de título é refletida no botão e na caixa de listagem.

## <a name="important-apis"></a>APIs importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [modelo da função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
