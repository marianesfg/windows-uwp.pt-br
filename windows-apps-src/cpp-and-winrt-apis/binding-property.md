---
description: Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela.
title: Controles XAML; associar a uma propriedade C++/WinRT
ms.date: 08/21/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, vínculo, propriedade
ms.localizationpriority: medium
ms.openlocfilehash: 9bdbfef54b799f8dff23ad739007cec9fef98af8
ms.sourcegitcommit: c315ec3e17489aeee19f5095ec4af613ad2837e1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58921722"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>Controles XAML; associar a uma propriedade C++/WinRT
Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Essa ideia é baseada no padrão de design do software conhecido como o *padrão do observador*. Este tópico mostra como implementar propriedades observable em [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)e como associar controles XAML a eles.

> [!IMPORTANT]
> Para conceitos e termos essenciais que dão suporte ao entendimento de como consumir e criar classes de tempo de execução com C++/WinRT, consulte [Consumir APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>O que faz *observável* significa para uma propriedade?
Digamos que uma classe de tempo de execução nomeada **BookSku** tem uma propriedade chamada **Título**. Se **BookSku** escolhe acionar o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) sempre que o valor de **Título** muda, então, **Título** é uma propriedade observável. É o comportamento de **BookSku** (acionamento ou não acionamento do evento) que determina qual, se houver alguma, de suas propriedades são observáveis.

Um elemento de texto ou controle de XAML pode se associar a e manipular esses eventos, recuperando os valores atualizados e, em seguida, atualizando ele mesmo para mostrar o novo valor.

> [!NOTE]
> Para obter informações sobre como instalar e usar o C++WinRT Visual Studio VSIX (extensão) e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), consulte [suporte para Visual Studio C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Criar um Aplicativo em branco (Bookstore)
Comece criando um novo projeto no Microsoft Visual Studio. Criar uma **Visual C++**   >  **Windows Universal** > **aplicativo em branco (C++/WinRT)** do projeto e nomeie-  *Livraria*.

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
> Suas classes de modelo de exibição&mdash;na verdade, qualquer classe de tempo de execução que você declara em seu aplicativo&mdash;não precisam derivar de uma classe base. O **BookSku** classe declarado acima é um exemplo disso. Ele implementa uma interface, mas ele não deriva de qualquer classe base.
>
> Qualquer classe de tempo de execução que você declare no aplicativo que *faz* derivam de uma base de dados de classe é conhecida como uma *Combinável* classe. E há restrições sobre classes combináveis. Para um aplicativo passar o [Kit de certificação de aplicativos do Windows](../debug-test-perf/windows-app-certification-kit.md) testes usados pelo Visual Studio e por que a Microsoft Store para validar os envios (e, portanto, para o aplicativo a ser processado com êxito para a Microsoft Store), um Por fim, a classe Combinável deve derivar de uma classe base do Windows. Isso significa que a classe na própria raiz da hierarquia de herança deve ser um tipo de origem em um namespace do Windows. Se você precisar derivar uma classe de tempo de execução de uma classe base&mdash;por exemplo, para implementar uma **BindableBase** classe para todos os seus modelos de exibição derivar&mdash;e em seguida, você pode derivar de [ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Um modelo de exibição é uma abstração de uma exibição, e então, ele está vinculado diretamente para o modo de exibição (a marcação XAML). Um modelo de dados é uma abstração de dados, e ele tem consumido somente de seus modelos de exibição e não associado diretamente a XAML. Dessa forma, você pode declarar os modelos de dados não como classes de tempo de execução, mas como classes ou structs de C++. Eles não precisam ser declarados em MIDL e fique à vontade para usar qualquer hierarquia de herança que você deseja.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do componente do Tempo de Execução do Windows (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte a você na criação e no consumo da classe de tempo de execução. Esses arquivos incluem stubs para ajudar você a começar a implementar a classe de tempo de execução **BookSku** que foi declarada em sua IDL. Esses stubs são `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` e `BookSku.cpp`.

Clique com botão direito no nó do projeto e clique em **Abrir pasta no Explorador de arquivos**. Isso abre a pasta do projeto no Explorador de arquivos. Lá, copie os arquivos de stub `BookSku.h` e `BookSku.cpp` da `\Bookstore\Bookstore\Generated Files\sources\` pasta e na pasta do projeto, que é `\Bookstore\Bookstore\`. Na **Gerenciador de soluções**, com o nó do projeto selecionado, certifique-se **Mostrar todos os arquivos** é ativado. Clique nos arquivos stub com o botão direito do mouse e clique em **Incluir no projeto**.

## <a name="implement-booksku"></a>Implemente **BookSku**
Agora, vamos abrir `\Bookstore\Bookstore\BookSku.h` e `BookSku.cpp` e implementar nossa classe de tempo de execução. Em `BookSku.h`, adicione um construtor que leva um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), um membro particular para armazenar a cadeia de caracteres do título e outro para o evento que criaremos quando o título mudar. Depois de fazer essas alterações, seu `BookSku.h` terá esta aparência.

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

No **título** função modificador, verificamos se um valor está sendo definido que diferente do valor atual. E, se assim, podemos atualizar o título e também geram a [ **INotifyPropertyChanged::PropertyChanged** ](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) eventos com um argumento igual ao nome da propriedade que foi alterado. Isso é feito para que a interface do usuário (IU) saiba qual valor de propriedade consultar novamente.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declare e implemente **BookstoreViewModel**
Nossa página principal de XAML associará a um modelo de exibição principal. E esse modelo de exibição terá várias propriedades, incluindo uma do tipo **BookSku**. Nesta etapa, vamos declarar e implementar nossa classe de tempo de execução do modelo de exibição principal.

Adicione um novo item chamado **Midl File (.idl)**`BookstoreViewModel.idl`.

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

Salvar e compilar. Copie `BookstoreViewModel.h` e `BookstoreViewModel.cpp` da pasta `Generated Files` para a pasta de projeto e os inclua no projeto. Abrir esses arquivos e implementar a classe de tempo de execução, conforme mostrado abaixo. Observe como, no `BookstoreViewModel.h`, estamos incluindo `BookSku.h`, que declara o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). E podemos estiver restaurando o construtor padrão, removendo `= delete`.

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
> O tipo de `m_bookSku` é o tipo projetado (**winrt::Bookstore::BookSku**) e o parâmetro de modelo que você usa com [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) é o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). Ainda assim, **fazer** retorna uma instância do tipo projetado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Adicione uma propriedade do tipo **BookstoreViewModel** para **MainPage**
Abra `MainPage.idl`, que declara a classe de tempo de execução que representa nossa página principal da interface do usuário. Adicione uma instrução importante para importar `BookstoreViewModel.idl`, e adicione uma propriedade somente leitura chamada MainViewModel do tipo **BookstoreViewModel**. Também remover os **MyProperty** propriedade. Além disso, observe o `import` diretiva na lista abaixo.

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

Salve o arquivo. O projeto não será compilado até a conclusão no momento, mas criar agora é uma coisa útil porque ele gera novamente os arquivos de código fonte no qual o **MainPage** classe de tempo de execução é implementado (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`). Portanto, vá em frente e crie agora mesmo. É o erro de compilação, você pode esperar ver neste estágio **'MainViewModel': não é um membro de 'winrt::Bookstore::implementation::MainPage'**.

Se você omitir a inclusão de `BookstoreViewModel.idl` (consulte a listagem de `MainPage.idl` acima), em seguida, você verá o erro **esperando \< quase "MainViewModel"**. Outra dica é certificar-se de que você deixe todos os tipos no mesmo namespace: o namespace que é mostrado nas listagens de código.

Para resolver o erro que esperamos ver, agora será necessário copiar os stubs de acessador para o **MainViewModel** propriedade fora os arquivos gerados (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`) e em `\Bookstore\Bookstore\MainPage.h` e `MainPage.cpp`.

Na `\Bookstore\Bookstore\MainPage.h`, inclua `BookstoreViewModel.h`, que declara o tipo de implementação (**winrt::Bookstore::implementation::BookstoreViewModel**). Adicione um membro particular para armazenar o modelo de exibição. Observe que a função de acessador de propriedade (e o membro m_mainViewModel) são implementados em termos de **Bookstore::BookstoreViewModel**, que é o tipo projetado. O tipo de implementação está no mesmo projeto (unidade de compilação) que o aplicativo, portanto, podemos construir m_mainViewModel via a sobrecarga de construtor que utiliza `nullptr_t`. Também remover os **MyProperty** propriedade.

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

Na `\Bookstore\Bookstore\MainPage.cpp`, chame [ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) (com o tipo de implementação) para atribuir uma nova instância do tipo projetada para m_mainViewModel. Atribua um valor inicial para o título do livro. Implemente o acessador para a propriedade MainViewModel. E por fim, atualize o título do livro no manipulador de eventos do botão. Também remover os **MyProperty** propriedade.

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
Abra `MainPage.xaml`, que contém a marcação XAML para nossa página da interface do usuário principal. Conforme mostrado na lista abaixo, remover o nome do botão e altere seu **conteúdo** valor de propriedade de um literal a uma expressão de associação. Observe a propriedade `Mode=OneWay` na expressão de associação (unidirecional do modelo de exibição na interface do usuário). Sem essa propriedade, a interface do usuário não responde a eventos da propriedade alterada.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos de **Clique**. Esse manipulador chama a função de modificador de título do livro; Esse modificador aciona um evento para informar a interface do usuário que a propriedade de **Título** foi alterada; e o botão consulta novamente o valor dessa propriedade para atualizar seu próprio valor de **Conteúdo**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Usando a extensão de marcação {Binding} com o C++/WinRT
Para a versão lançada atualmente do C++/WinRT, para que seja possível usar a extensão de marcação {Binding} você precisará implementar o [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) interfaces.

## <a name="important-apis"></a>APIs Importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modelo de função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
