---
author: stevewhims
description: Este tópico mostra como registrar e revogar delegados lidando com eventos usando C++/WinRT.
title: Manejar eventos usando delegados em C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projetado, projeção, manejar, evento, delegado
ms.localizationpriority: medium
ms.openlocfilehash: 96655c14f9c21f804ef5ebfdfe73cee0b04edfe3
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483596"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Manejar eventos usando delegados em C++/WinRT

Este tópico mostra como registrar e revogar representantes de manipulação de eventos usando [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Você pode manipular um evento usando qualquer objeto de função de C++ padrão.

> [!NOTE]
> Para obter informações sobre como instalar e usar a Extensão do Visual Studio (VSIX) C++/WinRT (que oferece suporte ao modelo de projeto, bem como propriedades e destinos de MSBuild para C++/WinRT), consulte [Suporte do Visual Studio para C++/WinRT e o VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

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

Em vez de fazer isso declarativamente na marcação, você pode registrar imperativamente uma função de membro para manipular um evento. Talvez não seja óbvio a partir do código de exemplo abaixo, mas o argumento para a chamada [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) é uma instância do delegado [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). Nesse caso, estamos usando a sobrecarga de construtor **RoutedEventHandler** que leva um objeto e um ponteiro para função de membro.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Quando registrado o representante, o exemplo de código acima passa um bruto *esse* ponteiro (apontando para o objeto atual). Para saber como estabelecer uma referência forte ou fraca ao objeto atual, consulte a seção subpropriedade **se você usar uma função de membro como um delegado** na seção [com segurança acessando o ponteiro *this* com um representante do manipulador de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

Há outras formas para construir um **RoutedEventHandler**. A seguir está o bloco de sintaxe tirado do tópico documentação para [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (escolha *C++/WinRT* a partir da lista suspensa **Idioma** na página). Observe os vários construtores: um deles leva um lambda; outro uma função livre; e outro (aquele que usamos acima) usa um objeto e um ponteiro para função de membro.

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

A sintaxe do operador de chamada de função também é útil para ver. Ela informa quais precisam ser os parâmetros do delegado. Como você pode ver, neste caso a sintaxe de operador de chamada da função combina com os parâmetros de nosso **MainPage::ClickHandler**.

Se você não estiver fazendo muito trabalho no manipulador de eventos, você pode usar uma função lambda em vez de uma função de membro. Novamente, talvez não seja óbvio a partir do exemplo de código abaixo, mas um delegado **RoutedEventHandler** está sendo construído a partir de uma função lambda que, novamente, precisa corresponder a sintaxe do operador de chamada de função.

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

Você pode optar por ser um pouco mais explícito quando construir seu delegado. Por exemplo, se você quiser passá-lo ou usá-lo mais de uma vez.

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

Quando você registra um delegado, normalmente um token é retornado para você. Depois você pode usar esse token para revogar seu delegado; que significa que o delegado tem o registro do evento cancelado, e não será chamado caso o evento inicie novamente. Por questões de simplicidade, nenhum dos exemplos de código acima mostrou como fazer isso. Mas este próximo exemplo de código armazena o token no membro de dados privados da estrutura e revoga seu manipulador no destruidor.

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

Em vez de uma referência forte, como no exemplo acima, você pode armazenar uma referência fraca para o botão (consulte [referências fortes e fracas em C++ c++ WinRT](weak-references.md)).

Como alternativa, quando você registra um delegado, você pode especificar **WinRT:: auto_revoke** (que é um valor de tipo [**WinRT:: auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) para solicitar um revogador de evento (do tipo [**WinRT:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). O revocador do evento mantém uma referência fraca à origem do evento (o objeto que gera o evento) para você. Você pode revogar manualmente ao chamar a função membro **event_revoker::revoke**; mas o revogador de evento chama essa função automaticamente quando sai do escopo. A função **revoke** verifica se o fonte do evento ainda existe e, caso afirmativo, revoga o delegado. Neste exemplo, não há necessidade de armazenar a origem do evento e não há necessidade de um destruidor.

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

A seguir está o bloco de sintaxe tirado do tópico de documentação para o evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Ele mostra os três registros diferentes e revogação de funções. Você pode ver exatamente qual tipo de revogador de evento você precisa declarar a partir da terceira sobrecarga.

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
> No exemplo de código acima, `Button::Click_revoker` é um alias de tipo para `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Um padrão semelhante se aplica a todos os eventos de C++/WinRT. Cada evento de tempo de execução do Windows tem uma sobrecarga de função revoke que retorna um revogador de evento, e que tipo de revogador é um membro de origem do evento. Portanto, para entrar em outro exemplo, o evento [**corewindow:: SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) tem uma sobrecarga de função de registro que retorna um valor do tipo **CoreWindow::SizeChanged_revoker**.


Você pode considerar a revogação de manipuladores em um cenário de navegação de página. Se você estiver navegando repetidamente em uma página e depois retroceder, você pode revogar quaisquer manipuladores quando você navega para fora da página. Como alternativa, se você estiver reutilizando a mesma instância de página, então verifique o valor do seu token e registre somente se ele ainda não estiver sido definido (`if (!m_token){ ... }`). Uma terceira opção é armazenar um revogador de evento na página como um membro de dados. E uma quarta opção, conforme descrito mais adiante neste tópico, é capturar uma referência forte ou uma fraca para o objeto *isso* na sua função lambda.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Delegar os tipos de ações e operações assíncronas

Os exemplos acima usam o tipo delegado **RoutedEventHandler**, mas é claro, há muitos outros tipos de delegado. Por exemplo, ações e operações (com e sem progresso) assíncronas foram concluídas e/ou eventos de progresso que esperam delegados do tipo correspondente. Por exemplo, o evento de progresso de uma operação assíncrona com progresso (que é qualquer coisa que implementa [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requer um delegado do tipo [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler). Aqui está um exemplo de código de criação de um delegado desse tipo usando uma função lambda. O exemplo também mostra como criar um delegado [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

```cppwinrt
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

Como sugere o comentário da "corrotina", em vez de usar um delegado com os eventos concluídos de ações assíncronas e operações, você provavelmente achará mais natural para usar rotinas concomitantes. Para obter detalhes e exemplos de código, consulte [Operações de concorrência e assíncrona com C++/WinRT](concurrency.md).

> [!NOTE]
> Ele não estiver correto implementar mais de um *manipulador de conclusão* para uma ação assíncrona ou operação. Você pode ter um único delegate para seu evento concluído, ou você pode `co_await` -lo. Se você tiver ambos, o segundo falhará.

Se você com delegados em vez de uma corrotina, em seguida, você pode optar por uma sintaxe mais simples.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>Tipos de delegado que retornam um valor

Alguns tipos de delegado devem retornar eles mesmos um valor. Um exemplo é [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), que retorna uma sequência. Aqui está um exemplo de criação de um delegado desse tipo (observe que a função lambda retorna um valor).

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acessando com segurança o *esse* ponteiro com um representante do manipulador de eventos

Se você manipula um evento com função de membro de um objeto ou de dentro de uma função lambda dentro da função de membro de um objeto, em seguida, você precisa pensar sobre os tempos de vida relativos do destinatário do evento (o objeto manipulando o evento) e a origem do evento (o objeto acionamento do evento). Para obter mais informações e exemplos de código, consulte [referências fortes e fracas em C++ c++ WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>APIs Importantes
* [struct de marcador WinRT:: auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [Função winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Função winrt::implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>Tópicos relacionados
* [Criar eventos com C++/WinRT](author-events.md)
* [Simultaneidade e operações assíncronas com C++/WinRT](concurrency.md)
* [Referências fortes e fracas em C++ c++ WinRT](weak-references.md)
