---
description: Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Este tópico mostra como implementar e consumir uma coleção observável e como associar um controle de itens XAML a ela.
title: Controles de itens XAML; vincular a uma coleção C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, vínculo, coleção
ms.localizationpriority: medium
ms.openlocfilehash: 9df7c96549254ab8318fd9a7c8224c6e87701747
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705682"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Controles de itens XAML; vincular a uma coleção C++/WinRT

Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Essa ideia é baseada no padrão de design do software conhecido como o *padrão do observador*. Este tópico mostra como implementar coleções observáveis em [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), e como vincular XAML itens controles para eles.

Este passo a passo se baseia no projeto criado em [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md), e ele adiciona aos conceitos explicados nesse tópico.

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>O que faz *observável* significa para uma coleção?
Se uma classe de tempo de execução que representa uma coleção escolhe acionar o evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) sempre que um elemento é adicionado a ele ou removido dele, então a classe de tempo de execução é um coleção observável. Um controle de itens XAML pode se associar a e manipular esses eventos, recuperando a coleção atualizada e, em seguida, atualizando ele mesmo para mostrar os elementos atuais.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Adicionar uma coleção **BookSkus** a **BookstoreViewModel**

Em [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md), nós adicionamos um tipo de propriedade **BookSku** a nosso modelo de visualização principal. Nesta etapa, vamos usar o modelo de função de fábrica [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) para nos ajudar a implementar uma coleção observável de **BookSku** no mesmo modelo de exibição.

> [!NOTE]
> Se você ainda não tiver instalado o SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809) ou posterior, em seguida, consulte [se você tiver uma versão mais antiga do SDK do Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) para uma listagem de um modelo de vetor observável que você pode usar em vez de winrt::single_ ** threaded_observable_vector**.

Declarar uma nova propriedade em `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> Na listagem da MIDL 3.0 acima, observe que o tipo da propriedade **BookSkus** é [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Na próxima seção deste tópico, nós vai ser associando a origem de itens de uma [**caixa de listagem**](/uwp/api/windows.ui.xaml.controls.listbox) a **BookSkus**. Uma caixa de listagem é um controle de itens e para configurar a propriedade [**ItemsControl**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) corretamente, você precisa definir para um valor do tipo **IObservableVector** (ou **IVector**) de **IInspectable**ou de um tipo de interoperabilidade como [** IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Salvar e compilar. Copie os stubs de acessador de `BookstoreViewModel.h` e `BookstoreViewModel.cpp` na pasta `Generated Files` e implemente-os.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
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
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
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
