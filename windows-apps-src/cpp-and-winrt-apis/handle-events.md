---
description: Este tópico mostra como registrar e revogar delegados lidando com eventos usando C++/WinRT.
title: Manipular eventos usando delegados em C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, manipulação, evento, delegado
ms.localizationpriority: medium
ms.openlocfilehash: 194fd9041b76acb1ef76288fed21c8098462b406
ms.sourcegitcommit: 8b4c1fdfef21925d372287901ab33441068e1a80
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67844343"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Manipular eventos usando delegados em C++/WinRT

Este tópico mostra como registrar e revogar delegados que manipulam eventos usando [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Você pode manipular um evento usando qualquer objeto de função de C++ padrão.

> [!NOTE]
> Para saber mais sobre como instalar e usar a VSIX (Extensão do Visual Studio) para C++/WinRT e o pacote do NuGet (que juntos fornecem um modelo de projeto e suporte ao build), confira [Suporte ao Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>Usando o Visual Studio 2019 para adicionar um manipulador de eventos

Uma maneira conveniente de adicionar um manipulador de eventos ao seu projeto é usar a IU (interface do usuário) do Designer XAML no Visual Studio 2019. Com sua página XAML aberta no Designer XAML, selecione o controle cujo evento você deseja manipular. Na página de propriedades do controle, clique no ícone de raio para listar todos os eventos originados do controle. Em seguida, clique duas vezes no evento que deseja manipular; por exemplo, *OnClicked*.

O Designer XAML adiciona o protótipo da função do manipulador de eventos apropriado (e uma implementação de stub) aos arquivos de origem, prontos para substituição pela sua implementação.

> [!NOTE]
> Normalmente, seus manipuladores de eventos não precisam ser descritos em seu arquivo Midl (`.idl`). Portanto, o Designer XAML não adiciona protótipos de função do manipulador de eventos ao seu arquivo Midl. Ele apenas adiciona seus arquivos `.h` e `.cpp`.

## <a name="register-a-delegate-to-handle-an-event"></a>Registrar um delegado para manipular um evento

Um exemplo simples está manipulando um evento de clique do botão. É comum usar marcação XAML para registrar uma função de membro para manipular o evento, como este.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

Ao invés de fazer isso declarativamente na marcação, registre imperativamente uma função de membro para manipular um evento. Talvez não seja óbvio pelo exemplo de código abaixo, mas o argumento para a chamada [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) é uma instância do delegado [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). Nesse caso, estamos usando a sobrecarga de construtor **RoutedEventHandler** que usa um objeto e um ponteiro para a função de membro.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Ao registrar o delegado, o exemplo de código acima passa um ponteiro bruto *this* (apontando para o objeto atual). Para saber como estabelecer uma referência fraca ou forte ao objeto atual, consulte [Se você usar uma função de membro como um delegado](weak-references.md#if-you-use-a-member-function-as-a-delegate).

Há outras formas de criar um **RoutedEventHandler**. Apresentamos a seguir o bloco de sintaxe tirado do tópico da documentação de [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (escolha *C++/WinRT* na lista suspensa **Language** no canto superior direito da página). Observe os vários construtores: um deles usa um lambda; outro uma função livre; e outro (aquele que usamos acima) usa um objeto e um ponteiro para função de membro.

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

Também é útil analisar a sintaxe do operador de chamada de função. Ela informa quais precisam ser os parâmetros do delegado. Como se pode ver, neste caso a sintaxe de operador de chamada da função combina com os parâmetros de **MainPage::ClickHandler**.

> [!NOTE]
> Em qualquer evento, para descobrir os detalhes do delegado e os parâmetros desse delegado, vá primeiro para o tópico de documentação do evento propriamente dito. Vamos utilizar o [evento UIElement.KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) como exemplo. Visite esse tópico e, em seguida, escolha  *C++/WinRT* na lista suspensa **Language**. No bloco de sintaxe no início do tópico, você verá isso.
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> Essa informação nos diz que o evento **UIElement.KeyDown** (o tópico em que estamos) tem o tipo de delegado **KeyEventHandler**, pois esse é o tipo que você passa ao registrar um delegado com esse tipo de evento. Então agora clique no link no tópico para esse tipo [delegado KeyEventHandler](/uwp/api/windows.ui.xaml.input.keyeventhandler). Aqui o bloco de sintaxe contém um operador de chamada de função. E, como foi mencionado acima, informa quais precisam ser os parâmetros do delegado.
> 
> ```cppwinrt
> void operator()(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  Como você pode ver, o delegado precisa ser declarado para utilizar um **IInspectable** como o remetente e uma instância da [classe KeyRoutedEventArgs](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) como os argumentos.
>
> Para ver outro exemplo, vamos examinar o [evento Popup.Closed](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed). Seu tipo delegado é [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler). Assim, seu delegado utilizará um **IInspectable** como o remetente e outro **IInspectable** (porque é o parâmetro do tipo **EventHandler**) como os argumentos.

Se você não estiver fazendo muito trabalho no manipulador de eventos, pode usar uma função lambda em vez de uma função de membro. Talvez não seja óbvio a partir do exemplo de código abaixo, mas um delegado **RoutedEventHandler** está sendo construído a partir de uma função lambda que, novamente, precisa corresponder à sintaxe do operador de chamada de função.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

Você pode optar por ser um pouco mais explícito quando construir seu delegado. Por exemplo, se quiser passá-lo ou usá-lo mais de uma vez.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>Revogar um delegado registrado

Ao registrar um delegado, normalmente se recebe um token de retorno. Em seguida, é possível usar esse token para revogar seu delegado; o que significa que o delegado tem o registro do evento cancelado e não será chamado caso o evento inicie novamente. Por questões de simplicidade, nenhum dos exemplos de código acima mostrou como fazer isso. Entretanto, este próximo exemplo de código armazena o token no membro de dados privados do struct e revoga seu manipulador no destruidor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

Em vez de uma referência forte, como no exemplo acima, armazene uma referência fraca no botão (confira [Referências fortes e fracas em C++/WinRT](weak-references.md)).

Como alternativa, ao registrar um delegado, é possível especificar **winrt::auto_revoke** (um valor do tipo [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) para solicitar um revogador de evento (do tipo [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). O revogador do evento mantém uma referência fraca à origem do evento (o objeto que aciona o evento). Você pode revogar manualmente ao chamar a função de membro **event_revoker::revoke**; mas o revogador de evento chama essa função automaticamente quando sai do escopo. A função **revoke** verifica se a fonte do evento ainda existe e, em caso afirmativo, revoga o delegado. Neste exemplo, não há necessidade de armazenar a origem do evento e não há necessidade de um destruidor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

A seguir apresentamos o bloco de sintaxe do tópico de documentação para o evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Ele mostra os três registros diferentes e a revogação de funções. É possível ver exatamente qual tipo de revogador de evento você precisa declarar a partir da terceira sobrecarga.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> No exemplo de código acima, `Button::Click_revoker` é um alias de tipo para `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Um padrão semelhante aplica-se a todos os eventos de C++/WinRT. Cada evento de tempo de execução do Windows tem uma sobrecarga de função de revogação que retorna um revogador de evento, e esse tipo de revogador é um membro da origem do evento. Vejamos outro exemplo, o evento [**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) tem uma sobrecarga de função de registro que retorna um valor do tipo **CoreWindow::SizeChanged_revoker**.

Considere a revogação de manipuladores em um cenário de navegação de página. Se estiver navegando repetidamente em uma página e depois retroceder, é possível revogar quaisquer manipuladores ao navegar para fora da página. Como alternativa, se estiver reutilizando a mesma instância de página, verifique o valor do token e registre somente se ele ainda não tiver sido definido (`if (!m_token){ ... }`). Uma terceira opção é armazenar um revogador de evento na página como um membro de dados. E uma quarta opção, conforme descrito mais adiante neste tópico, é capturar uma referência forte ou fraca para o objeto *this* na sua função lambda.

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>Se o registro de seu delegado de revogação automática falhar

Se você tentar especificar [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) ao registrar um delegado e o resultado for uma exceção [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface), normalmente isso significa que a origem do evento não dará suporte a referências fracas. Essa é uma situação comum no namespace [**Windows.UI.Composition**](/uwp/api/windows.ui.composition), por exemplo. Nessa situação, você não pode usar o recurso de revogação automática. Você precisará fazer fallback para revogar manualmente seus manipuladores de eventos.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Tipos delegados para ações e operações assíncronas

Os exemplos acima usam o tipo delegado **RoutedEventHandler**, mas há muitos outros tipos de delegado, é claro. Por exemplo, ações e operações assíncronas (com e sem progresso) foram concluídas e/ou eventos de progresso que esperam delegados do tipo correspondente. Por exemplo, o evento de progresso de uma operação assíncrona com progresso (que é qualquer coisa que implementa [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requer um delegado do tipo [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler). Eis um exemplo de código de criação de um delegado desse tipo usando uma função lambda. O exemplo também mostra como criar um delegado [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Como sugere o comentário da "corrotina", ao invés de usar um delegado com os eventos concluídos de ações assíncronas e operações, talvez seja mais natural usar corrotinas concomitantes. Para saber mais e obter exemplos de código, confira [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md).

> [!NOTE]
> Não é correto implementar mais de um *manipulador de conclusão* para uma ação ou operação assíncrona. Você pode ter um único delegado para seu evento concluído ou pode usar `co_await`. Se tiver ambos, o segundo falhará.

Se continuar com delegados ao invés de uma corrotina, poderá optar por uma sintaxe mais simples.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>Tipos delegados que retornam um valor

Alguns tipos delegados devem retornar um valor. Um exemplo é [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), que retorna uma cadeia de caracteres. Este é um exemplo de criação de um delegado desse tipo (observe que a função lambda retorna um valor).

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acessar com segurança o ponteiro *this* com um delegado de manipulação de eventos

Se você manipular um evento com uma função de membro de objeto, ou de dentro de uma função lambda dentro da função de membro de um objeto, precisará pensar sobre os ciclos de vida relativos do destinatário do evento (o objeto que está manipulando o evento) e a origem do evento (o objeto que está acionando o evento). Para saber mais e obter exemplos de código, confira [Referências fortes e fracas em C++/WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>APIs Importantes
* [Struct de marcador winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [Função winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [Função winrt::implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar eventos em C++/WinRT](author-events.md)
* [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)
* [Referências fortes e fracas em C++/WinRT](weak-references.md)
