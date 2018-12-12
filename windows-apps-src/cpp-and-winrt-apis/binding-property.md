---
description: Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela.
title: Controles XAML; vincular a uma propriedade C++/WinRT
ms.date: 08/21/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, vínculo, propriedade
ms.localizationpriority: medium
ms.openlocfilehash: fc38dfff99e5bef9de686d754444ee93375c7895
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8943701"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>Controles XAML; vincular a uma propriedade C++/WinRT
Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Essa ideia é baseada no padrão de design do software conhecido como o *padrão do observador*. Este tópico mostra como implementar propriedades observáveis em [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)e como vincular controles XAML a elas.

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>O que faz *observável* significa para uma propriedade?
Digamos que uma classe de tempo de execução nomeada **BookSku** tem uma propriedade chamada **Título**. Se **BookSku** escolhe acionar o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) sempre que o valor de **Título** muda, então, **Título** é uma propriedade observável. É o comportamento de **BookSku** (acionamento ou não acionamento do evento) que determina qual, se houver alguma, de suas propriedades são observáveis.

Um elemento de texto ou controle de XAML pode se associar a e manipular esses eventos, recuperando os valores atualizados e, em seguida, atualizando ele mesmo para mostrar o novo valor.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Criar um Aplicativo em branco (Bookstore)
Inicie criando um novo projeto no Microsoft Visual Studio. Criar um **Visual C++** > **Universal do Windows** > **aplicativo em branco (C++ c++ WinRT)** de projeto e nomeie- *Bookstore*.

Vamos criar uma nova classe para representar um livro que tem uma propriedade de título observável. Estamos criando e consumindo a classe dentro da mesma unidade de compilação. Mas queremos poder associar a essa classe do XAML, e por isso ela será uma classe de tempo de execução. E vamos usar C++/WinRT para criar e consumi-lo.

A primeira etapa na criação de uma nova classe de tempo de execução é adicionar um novo item **Midl File (.idl)** ao projeto. Nomeie-o `BookSku.idl`. Exclua o conteúdo padrão do `BookSku.idl` e cole nessa declaração de classe de tempo de execução.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> Suas classes de modelo de exibição&mdash;na verdade, qualquer classe de tempo de execução que você declarar em seu aplicativo&mdash;não precisa derivar de uma classe base. A classe **BookSku** declarada acima é um exemplo disso. Ele implementa uma interface, mas ele não deriva de qualquer classe base.
>
> Qualquer classe de tempo de execução que você declara no aplicativo *faz* derivar de uma base de classe é conhecido como um *composable* classe. E há restrições ao redor composable classes. Para um aplicativo passar nos testes do [Kit de certificação de aplicativo do Windows](../debug-test-perf/windows-app-certification-kit.md) usados pelo Visual Studio e pela Microsoft Store para validar envios (e, portanto, para o aplicativo ser inserido com êxito na Microsoft Store), uma classe composable deve Por fim derive de uma classe base do Windows. Isso significa que a classe na raiz muito da hierarquia de herança deve ser um tipo originando do namespace. Se você precisar derivar uma classe de tempo de execução de uma classe base&mdash;por exemplo, para implementar uma classe **BindableBase** para todos os seus modelos de exibição derivar de&mdash;, em seguida, você pode derivar de [**DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Um modelo de exibição é uma abstração de um modo de exibição e, portanto, ele está vinculado diretamente para o modo de exibição (a marcação XAML). Um modelo de dados é uma abstração de dados, e ele tem consumido somente de seus modelos de exibição e não vinculado diretamente em XAML. Portanto, você pode declarar seus modelos de dados não como classes de tempo de execução, mas como classes ou estruturas de C++. Eles não precisam ser declaradas no MIDL, e você pode usar qualquer hierarquia que desejar.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução **BookSku** que foi declarada em sua IDL. Esses stubs são `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` e `BookSku.cpp`.

Copie os arquivos stub `BookSku.h` e `BookSku.cpp` de `\Bookstore\Bookstore\Generated Files\sources\` na pasta do projetor, que é `\Bookstore\Bookstore\`. No **Explorador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-booksku"></a>Implemente **BookSku**
Agora, vamos abrir `\Bookstore\Bookstore\BookSku.h` e `BookSku.cpp` e implementar nossa classe de tempo de execução. Em `BookSku.h`, adicione um construtor que leva um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), um membro particular para armazenar a cadeia de caracteres do título e outro para o evento que criaremos quando o título mudar. Depois de fazer essas alterações, o `BookSku.h` terá esta aparência.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
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
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

A função de modificador de **título** , verificamos se um valor está sendo definido que que é diferente do valor atual. E, em caso afirmativo, podemos atualizar o título e também disparar o evento [**INotifyPropertyChanged:: PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) com um argumento igual ao nome da propriedade que foi alterada. Isso é feito para que a interface do usuário (IU) saiba qual valor de propriedade consultar novamente.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declare e implemente **BookstoreViewModel**
Nossa página principal de XAML associará a um modelo de exibição principal. E esse modelo de exibição terá várias propriedades, incluindo uma do tipo **BookSku**. Nesta etapa, vamos declarar e implementar nossa classe de tempo de execução do modelo de exibição principal.

Adicione um novo item chamado **Midl File (.idl)** `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

Salvar e compilar. Copie `BookstoreViewModel.h` e `BookstoreViewModel.cpp` da pasta `Generated Files` para a pasta de projeto e os inclua no projeto. Abra os arquivos e implemente a classe de tempo de execução, como mostrado a seguir. Observe como, na `BookstoreViewModel.h`, estamos incluindo `BookSku.h`, que declara o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). E podemos está restaurando o construtor padrão removendo `= delete`.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
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
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> O tipo de `m_bookSku` é o tipo projetado (**winrt::Bookstore::BookSku**) e o parâmetro de modelo que você usar com [**WinRT:: make**](/uwp/cpp-ref-for-winrt/make) é o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). Ainda assim, **fazer** retorna uma instância do tipo projetado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Adicione uma propriedade do tipo **BookstoreViewModel** para **MainPage**
Abra `MainPage.idl`, que declara a classe de tempo de execução que representa nossa página principal da interface do usuário. Adicione uma instrução importante para importar `BookstoreViewModel.idl`, e adicione uma propriedade somente leitura chamada MainViewModel do tipo **BookstoreViewModel**. Também remova a propriedade **MyProperty** . Observe também a `import` diretiva na lista abaixo.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Salve o arquivo. O projeto não será compilado para a conclusão no momento, mas criando agora é algo útil porque ele gera novamente os arquivos de código de origem no qual a classe de tempo de execução **MainPage** é implementada (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`). Portanto, vá em frente e crie agora. O erro de compilação, você pode esperar ver neste estágio é **'MainViewModel': não é um membro de 'winrt::Bookstore::implementation::MainPage'**.

Se você omitir a inclusão de `BookstoreViewModel.idl` (consulte a listagem de `MainPage.idl` acima), em seguida, você verá o erro **esperando \< próximo "MainViewModel"**. Outra dica é certificar-se de que você deixe todos os tipos no mesmo namespace: o namespace que é mostrado nas listagens de código.

Para resolver o erro que esperamos que, agora será necessário copiar os stubs de acessador para a propriedade **MainViewModel** fora dos arquivos gerados (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`) e em `\Bookstore\Bookstore\MainPage.h` e `MainPage.cpp`.

Em `\Bookstore\Bookstore\MainPage.h`, inclua `BookstoreViewModel.h`, que declara o tipo de implementação (**winrt::Bookstore::implementation::BookstoreViewModel**). Adicione um membro particular para armazenar o modelo de exibição. Observe que a função de acessador de propriedade (e o membro m_mainViewModel) são implementados em termos de **Bookstore::BookstoreViewModel**, que é o tipo projetado. O tipo de implementação está no mesmo projeto (unidade de compilação) como o aplicativo, portanto, construímos m_mainViewModel por meio da sobrecarga de construtor que leva `nullptr_t`. Também remova a propriedade **MyProperty** .

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

Em `\Bookstore\Bookstore\MainPage.cpp`, chame [**WinRT:: make**](/uwp/cpp-ref-for-winrt/make) (com o tipo de implementação) para atribuir uma nova instância do tipo projetado para m_mainViewModel. Atribua um valor inicial para o título do livro. Implemente o acessador para a propriedade MainViewModel. E por fim, atualize o título do livro no manipulador de eventos do botão. Também remova a propriedade **MyProperty** .

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Associar o botão para a propriedade **Título**
Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Conforme mostrado na lista a seguir, remova o nome do botão e altere o valor de propriedade de **conteúdo** de um literal para uma expressão de associação. Observe a propriedade `Mode=OneWay` na expressão de associação (unidirecional do modelo de exibição na interface do usuário). Sem essa propriedade, a interface do usuário não responde a eventos da propriedade alterada.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos de **Clique**. Esse manipulador chama a função de modificador de título do livro; Esse modificador aciona um evento para informar a interface do usuário que a propriedade de **Título** foi alterada; e o botão consulta novamente o valor dessa propriedade para atualizar seu próprio valor de **Conteúdo**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Usando a extensão de marcação {Binding} com C++ c++ WinRT
Para a versão lançada no momento do C++ c++ WinRT, para ser capaz de usar a extensão de marcação {Binding} você precisará implementar as interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) .

## <a name="important-apis"></a>APIs Importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
