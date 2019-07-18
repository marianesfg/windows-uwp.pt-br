---
description: Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela.
title: Controles XAML; associar a uma propriedade de C++/WinRT
ms.date: 06/21/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, XAML, controle, associação, propriedade
ms.localizationpriority: medium
ms.openlocfilehash: 25ce3164ece443c8c1d95bccbc2bfb57e3347a55
ms.sourcegitcommit: a7a1e27b04f0ac51c4622318170af870571069f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717657"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>Controles XAML; associar a uma propriedade de C++/WinRT
Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Essa ideia baseia-se no padrão de design de software conhecido como o *padrão do observador*. Este tópico mostra como implementar propriedades observáveis em [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) e como associar controles de XAML a elas.

> [!IMPORTANT]
> Para ver conceitos e termos essenciais que ajudam a entender como utilizar e criar classes de tempo de execução com C++/WinRT, confira [Utilizar APIs com C++/WinRT](consume-apis.md) e [Criar APIs com C++/WinRT](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>O que significa *observável* para uma propriedade?
Digamos que uma classe de tempo de execução chamada **BookSku** tem uma propriedade chamada **Title**. Se **BookSku** escolhe acionar o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) sempre que o valor de **Title** mudar, **Title** é uma propriedade observável. É o comportamento de **BookSku** (acionamento ou não acionamento do evento) que determina qual de suas propriedades são observáveis, caso haja alguma.

Um controle ou elemento de texto de XAML pode se associar a e manipular esses eventos, recuperando o(s) valor(es) atualizado(s) e, em seguida, atualizando a si próprio para mostrar o novo valor.

> [!NOTE]
> Para saber mais sobre como instalar e usar a VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Criar um Aplicativo em branco (Bookstore)
Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Aplicativo em branco (C++/WinRT)** e nomeie-o como *Bookstore*.

Criaremos uma nova classe para representar um livro que tem uma propriedade de título observável. Criaremos e utilizaremos a classe dentro da mesma unidade de compilação. Entretanto, queremos poder criar associações nessa classe do XAML e, por isso, ela será uma classe de tempo de execução. E usaremos o C++/WinRT para criar e usar.

A primeira etapa na criação de uma nova classe de tempo de execução é adicionar um novo item **Midl File (.idl)** ao projeto. Nomeie-o como `BookSku.idl`. Exclua o conteúdo padrão do `BookSku.idl` e cole esta declaração de classe de tempo de execução.

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
> Suas classes de modelo de exibição (na verdade, qualquer classe de tempo de execução que você declarar em seu aplicativo) não precisam derivar de uma classe base. A classe **BookSku** declarada acima é um exemplo disso. Ela implementa uma interface, mas não deriva de qualquer classe base.
>
> Qualquer classe de tempo de execução declarada em seu aplicativo, que *é* derivada de uma classe base, é conhecida como uma classe *combinável*. E há restrições sobre as classes combináveis. Para um aplicativo ser aprovado nos testes do [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) usados pelo Visual Studio e pela Microsoft Store para validar os envios (e, portanto, para que o aplicativo seja processado com êxito na Microsoft Store), uma classe combinável deve derivar de uma classe base do Windows. Isso significa que a classe na própria raiz da hierarquia de herança deve ser um tipo que origina em um namespace Windows.*. Se for necessário derivar uma classe de tempo de execução de uma classe base (por exemplo, para implementar uma classe **BindableBase** da qual todos os seus modelos de exibição devem derivar), então é possível derivar a partir de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Um modelo de exibição é uma abstração de uma exibição e está vinculado diretamente ao modo de exibição (a marcação XAML). Um modelo de dados é uma abstração de dados, é consumido somente por seus modelos de exibição e não está diretamente associado ao XAML. Portanto, você pode declarar os modelos de dados como estruturas ou classes C++, não como classes de tempo de execução. Eles não precisam ser declarados em MIDL e sinta-se à vontade para usar qualquer hierarquia de herança desejada.

Salve o arquivo e compile o projeto. Durante o processo de compilação, a ferramenta `midl.exe` é executada para criar um arquivo de metadados do Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`), descrevendo a classe de tempo de execução. Em seguida, a ferramenta `cppwinrt.exe` é executada para gerar arquivos de código fonte para dar suporte à criação e utilização da classe de tempo de execução. Esses arquivos incluem stubs para ajudá-lo a começar a implementar a classe de tempo de execução **BookSku** que foi declarada em seu IDL. Esses stubs são `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` e `BookSku.cpp`.

Clique com o botão direito do mouse no nó do projeto e clique em **Abrir Pasta no Explorador de Arquivos**. A pasta do projeto abre no Explorador de Arquivos. Copie ali os arquivos de stub `BookSku.h` e `BookSku.cpp` da pasta `\Bookstore\Bookstore\Generated Files\sources\` e para a pasta do projeto, que é `\Bookstore\Bookstore\`. No **Explorador de Soluções**, com o nó do projeto selecionado, certifique-se de que a opção **Mostrar todos os arquivos** esteja ativada. Clique com o botão direito do mouse nos arquivos de stub copiados e clique em **Incluir no Projeto**.

## <a name="implement-booksku"></a>Implemente **BookSku**
Agora vamos abrir `\Bookstore\Bookstore\BookSku.h` e `BookSku.cpp`, e implementar nossa classe de tempo de execução. Em `BookSku.h`, adicione um construtor que leva um [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), um membro privado para armazenar a cadeia de caracteres do título, e outro para o evento que criaremos quando o título mudar. Depois de fazer essas alterações, seu `BookSku.h` terá esta aparência.

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
#include "BookSku.g.cpp"

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

Na função modificadora **Title**, verificamos se um valor está sendo definido de forma diferente do valor atual. Em caso afirmativo, atualizamos o título e também geramos o evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) com um argumento igual ao nome da propriedade que mudou. Isso é feito para que a interface do usuário (IU) saiba qual valor de propriedade deve ser consultado novamente.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declare e implemente **BookstoreViewModel**
Nossa página principal de XAML será associada a um modelo de exibição principal. E esse modelo de exibição terá várias propriedades, incluindo uma do tipo **BookSku**. Nesta etapa, vamos declarar e implementar nossa classe de tempo de execução do modelo de exibição principal.

Adicione um novo item **Midl File (.idl)** chamado `BookstoreViewModel.idl`.

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

Salve e compile. Copie `BookstoreViewModel.h` e `BookstoreViewModel.cpp` da pasta `Generated Files\sources` para a pasta do projeto e inclua-os no projeto. Abra os arquivos e implemente a classe de tempo de execução, como mostrado abaixo. Observe como, em `BookstoreViewModel.h`, incluímos `BookSku.h`, que declara o tipo de implementação de **BookSku** (que é **winrt::Bookstore::implementation::BookSku**). E removemos `= default` do construtor padrão.

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
#include "BookstoreViewModel.g.cpp"

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
> O tipo `m_bookSku` é o tipo projetado (**winrt::Bookstore::BookSku**) e o parâmetro de modelo que você usa com [**winrt::make**](/uwp/cpp-ref-for-winrt/make) é o tipo de implementação (**winrt::Bookstore::implementation::BookSku**). Ainda assim, **make** retorna uma instância do tipo projetado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Adicione uma propriedade do tipo **BookstoreViewModel** em **MainPage**
Abra `MainPage.idl`, que declara a classe de tempo de execução que representa nossa página principal da interface do usuário. Adicione uma instrução de importação para importar `BookstoreViewModel.idl` e adicione a propriedade somente leitura MainViewModel do tipo **BookstoreViewModel**. Remova também a propriedade **MyProperty**. Além disso, observe a diretiva `import` na lista abaixo.

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

Salve o arquivo. No momento o projeto não será completamente compilado, mas é útil compilá-lo agora porque ele gera novamente os arquivos de código fonte nos quais a classe de tempo de execução **MainPage** foi implementada (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`). Portanto, vá em frente e compile agora mesmo. O erro de compilação que você pode esperar nesse estágio é **'MainViewModel': não é um membro de 'winrt::Bookstore::implementation::MainPage'** .

Se você omitir a inclusão de `BookstoreViewModel.idl` (veja a lista de `MainPage.idl` acima), verá o erro **esperando \< próximo a "MainViewModel"** . Outra dica é certificar-se de que você deixou todos os tipos no mesmo namespace que é mostrado nas listagens de código.

Para resolver o erro esperado, agora é necessário copiar os stubs do acessador para a propriedade **MainViewModel** fora dos arquivos gerados (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` e `MainPage.cpp`) e em `\Bookstore\Bookstore\MainPage.h` e `MainPage.cpp`. As etapas para fazer isso são descritas a seguir.

Em `\Bookstore\Bookstore\MainPage.h`, inclua `BookstoreViewModel.h`, que declara o tipo de implementação **BookstoreViewModel** (que é **winrt::Bookstore::implementation::BookstoreViewModel**). Adicione um membro privado para armazenar o modelo de exibição. Observe que a função de acessador de propriedade (e o membro m_mainViewModel) é implementada em termos do tipo projetado para **BookstoreViewModel** (que é **Bookstore::BookstoreViewModel**). O tipo de implementação e o aplicativo estão no mesmo projeto (unidade de compilação), então criamos m_mainViewModel por meio da sobrecarga do construtor que usa **std::nullptr_t**. Remova também a propriedade **MyProperty**.

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

Em `\Bookstore\Bookstore\MainPage.cpp`, chame [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (com o tipo de implementação) para atribuir uma nova instância do tipo projetado para m_mainViewModel. Atribua um valor inicial para o título do livro. Implemente o acessador para a propriedade MainViewModel. E, por fim, atualize o título do livro no manipulador de eventos do botão. Remova também a propriedade **MyProperty**.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

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

## <a name="bind-the-button-to-the-title-property"></a>Associe o botão à propriedade **Title**
Abra `MainPage.xaml`, que contém a marcação XAML para a página principal da interface do usuário. Como mostrado na listagem abaixo, remova o nome do botão e altere seu valor de propriedade **Content** de um literal para uma expressão de associação. Observe a propriedade `Mode=OneWay` na expressão de associação (unidirecional do modelo de exibição na interface do usuário). Sem essa propriedade, a interface do usuário não responderá aos eventos da propriedade alterada.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Agora compile e execute o projeto. Clique no botão para executar o manipulador de eventos **Click**. Esse manipulador chama a função modificadora de título do livro, esse modificador aciona um evento para informar a interface do usuário que a propriedade **Title** foi alterada e o botão consulta novamente o valor dessa propriedade para atualizar seu próprio valor de **Content**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Usar a extensão de marcação {Binding} com C++/WinRT
Na versão atual de C++/WinRT, para que seja possível usar a extensão de marcação {Binding}, é preciso implementar as interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) e [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty).

## <a name="element-to-element-binding"></a>Associação de elemento a elemento

Você pode associar a propriedade de um elemento XAML à propriedade de outro elemento XAML. Veja um exemplo de como isso funciona na marcação.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

Você precisará declarar a entidade XAML nomeada `myTextBox` como uma propriedade somente leitura em seu arquivo Midl (.idl).

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

Isso é necessário pelo seguinte motivo. Todos os tipos que o compilador XAML precisa validar (incluindo aqueles usados em [{X:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) são lidos dos Metadados do Windows (WinMD). Você precisa apenas adicionar a propriedade somente leitura ao arquivo Midl. Não implemente-a, porque o code-behind XAML gerado automaticamente fornece a implementação para você.

## <a name="important-apis"></a>APIs Importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Modelo da função winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Tópicos relacionados
* [Consumir APIs com C++/WinRT](consume-apis.md)
* [Criar APIs com C++/WinRT](author-apis.md)
