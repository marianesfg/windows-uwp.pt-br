---
author: stevewhims
description: Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela.
title: Controles XAML; vincular a uma propriedade C++/WinRT
ms.author: stwhi
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, vínculo, propriedade
ms.localizationpriority: medium
ms.openlocfilehash: b54f0dd60a90cd13e5b3586a956b09e30f6d9755
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832280"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>Controles XAML; vincular a uma propriedade [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Essa ideia é baseada no padrão de design do software conhecido como o *padrão do observador*. Este tópico mostra como implementar propriedades observáveis em C++/WinRT, e como vincular controles de XAML a elas.

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>O que faz *observável* significa para uma propriedade?
Digamos que uma classe de tempo de execução nomeada **BookSku** tem uma propriedade chamada **Título**. Se **BookSku** escolhe acionar o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) sempre que o valor de **Título** muda, então, **Título** é uma propriedade observável. É o comportamento de **BookSku** (acionamento ou não acionamento do evento) que determina qual, se houver alguma, de suas propriedades são observáveis.

Um elemento de texto ou controle de XAML pode se associar a e manipular esses eventos, recuperando os valores atualizados e, em seguida, atualizando ele mesmo para mostrar o novo valor.

> [!NOTE]
> Para obter informações sobre a disponibilidade atual do Visual Studio Extension (VSIX) de C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild de C++/WinRT), veja [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Criar um Aplicativo em branco (Bookstore)
Inicie criando um novo projeto no Microsoft Visual Studio. Crie um projeto do **Aplicativo em branco do Visual C++ (C++/WinRT)** e nomeie-o *Bookstore*.

Vamos criar uma nova classe para representar um livro que tem uma propriedade de título observável. Estamos criando e consumindo a classe dentro da mesma unidade de compilação. Mas queremos poder associar a essa classe do XAML, e por isso ela será uma classe de tempo de execução. E vamos usar C++/WinRT para criar e consumi-lo.

A primeira etapa na criação de uma nova classe de tempo de execução é adicionar um novo item **Midl File (.idl)** ao projeto. Nomeie-o `BookSku.idl`. Exclua o conteúdo padrão do `BookSku.idl` e cole nessa declaração de classe de tempo de execução.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.DependencyObject, Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!IMPORTANT]
> Para um aplicativo passar nos testes do [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) e para ser inserido com êxito na Microsoft Store, a classe de base definitiva de cada classe de tempo de execução *declarada no aplicativo* deve ser de um tipo originando do namespace do Windows*.

Para preencher esse requisito, derive suas classes de modelo de exibição de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject). Como alternativa, declare uma classe de base associável derivada de **DependencyObject**e derive seus modelos de exibição a partir dela. Você pode declarar seus modelos de dados como estruturas C++; eles não precisam ser declarados no MIDL (desde que você os esteja consumindo apenas a partir de seus modelos de exibição e não vinculando XAML diretamente a eles; nesse caso, eles possivelmente seriam modelos de exibição por definição, mesmo assim).

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução **BookSku** que foi declarada em sua IDL. Esses stubs são `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` e `BookSku.cpp`.

Copie os arquivos stub `BookSku.h` e `BookSku.cpp` de `\Bookstore\Bookstore\Generated Files\sources\` na pasta do projetor, que é `\Bookstore\Bookstore\`. No **Explorador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-booksku"></a>Implemente **BookSku**
Agora, vamos abrir `\Bookstore\Bookstore\BookSku.h` e `BookSku.cpp` e implementar nossa classe de tempo de execução. Em `BookSku.h`, adicione um construtor que leva um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), um membro particular para armazenar a cadeia de caracteres do título e outro para o evento que criaremos quando o título mudar. Após a inclusão desses controles, seu `BookSku.h` terá a seguinte aparência.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(hstring const& title);

        hstring Title();
        void Title(hstring const& value);
        event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(event_token const& token);
    
    private:
        hstring title;
        event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> propertyChanged;
    };
}
```

Em `BookSku.cpp`, implemente as funções desta forma.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(hstring const& title)
    {
        Title(title);
    }

    hstring BookSku::Title()
    {
        return title;
    }

    void BookSku::Title(hstring const& value)
    {
        if (title != value)
        {
            title = value;
            propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(event_token const& token)
    {
        propertyChanged.remove(token);
    }
}
```

Na função modificadora **Título**, verificamos se um valor diferente está sendo definido e, em caso afirmativo, podemos atualizar o título e também disparar o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) com um argumento igual ao nome da propriedade que foi alterada. Isso é feito para que a interface do usuário (IU) saiba qual valor de propriedade consultar novamente.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declare e implemente **BookstoreViewModel**
Nossa página principal de XAML associará a um modelo de exibição principal. E esse modelo de exibição terá várias propriedades, incluindo uma do tipo **BookSku**. Nesta etapa, vamos declarar e implementar nossa classe de tempo de execução do modelo de exibição principal.

Adicione um novo item chamado **Midl File (.idl)** `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel : Windows.UI.Xaml.DependencyObject
    {
        BookSku BookSku{ get; };
    }
}
```

Salvar e compilar. Copie `BookstoreViewModel.h` e `BookstoreViewModel.cpp` da pasta `Generated Files` para a pasta de projeto e os inclua no projeto. Abra os arquivos e implemente a classe de tempo de execução desta forma.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();
        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> O tipo `m_bookSku` é o tipo projetado (**winrt::Bookstore::BookSku**) e o parâmetro de modelo que você usa com **make** é o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). Ainda assim, **fazer** retorna uma instância do tipo projetado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Adicione uma propriedade do tipo **BookstoreViewModel** para **MainPage**
Abra `MainPage.idl`, que declara a classe de tempo de execução que representa nossa página principal da interface do usuário. Adicione uma instrução importante para importar `BookstoreViewModel.idl`, e adicione uma propriedade somente leitura chamada MainViewModel do tipo **BookstoreViewModel**.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace BookstoreCPPWinRT
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Recompile o projeto para gerar os arquivos de código de origem onde a classe de tempo de execução **MainPage** é implementada (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`). Copie os stubs de acessador para a propriedade ViewModel fora dos arquivos gerados e em `\Bookstore\Bookstore\MainPage.h` e `MainPage.cpp`.

Para `\Bookstore\Bookstore\MainPage.h`, adicione um membro particular para armazenar o modelo de exibição. Observe que a função de acessador de propriedade (e o membro m_mainViewModel) são implementados em termos de **Bookstore::BookstoreViewModel**, que é o tipo projetado. O tipo de implementação está no mesmo projeto (unidade de compilação), portanto, construímos m_mainViewModel por meio da sobrecarga de construtor que leva `nullptr`.

```cppwinrt
// MainPage.h
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

    private:
        void ClickHandler(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args);

        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };

        friend class MainPageT<MainPage>;
    };
}
...
```

Em `\Bookstore\Bookstore\MainPage.cpp`, inclua `BookstoreViewModel.h`, que declara o tipo de implementação. Chame [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (com o tipo de implementação) para atribuir uma nova instância do tipo projetado para m_mainViewModel. Atribua um valor inicial para o título do livro. Implemente o acessador para a propriedade MainViewModel. E por fim, atualize o título do livro no manipulador de eventos do botão.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "BookstoreViewModel.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }

    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Associar o botão para a propriedade **Título**
Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Remova o nome do botão e altere seu valor de propriedade de **Conteúdo** de um literal a uma expressão de associação. Observe a propriedade `Mode=OneWay` na expressão de associação (unidirecional do modelo de exibição na interface do usuário). Sem essa propriedade, a interface do usuário não responde a eventos da propriedade alterada.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos de **Clique**. Esse manipulador chama a função de modificador de título do livro; Esse modificador aciona um evento para informar a interface do usuário que a propriedade de **Título** foi alterada; e o botão consulta novamente o valor dessa propriedade para atualizar seu próprio valor de **Conteúdo**.

## <a name="important-apis"></a>APIs Importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
