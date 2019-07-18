---
description: Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.
title: Simultaneidade e operações assíncronas com C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: cbabf38f41ae940f5c92944154638eae7016e043
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660092"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Simultaneidade e operações assíncronas com C++/WinRT

Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operações assíncronas e funções "Async" do Windows Runtime

Qualquer API do Windows Runtime que tem o potencial de demorar mais de 50 milissegundos para concluir é implementada como uma função assíncrona (com um nome terminado em "Async"). A implementação de uma função assíncrona inicia o trabalho em outro thread e retorna imediatamente com um objeto que representa a operação assíncrona. Quando a operação assíncrona for concluída, aquele objeto retornado contém qualquer valor que resultou do trabalho. O namespace do Windows Runtime **Windows::Foundation** contém quatro tipos de objeto de operação assíncrona.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) e
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada um desses tipos de operação assíncrona é projetado para um tipo correspondente no namespace C++/WinRT do **winrt::Windows::Foundation**. C++/WinRT também contém um struct de adaptador de espera interno. Você não o usa diretamente, mas, graças a esse struct, você pode escrever uma instrução `co_await` para aguardar de maneira cooperativa o resultado de qualquer função que retorna um desses tipos de operação assíncrona. Você pode criar suas próprias rotinas concomitantes que retornam esses tipos.

Um exemplo de uma função do Windows assíncrona é [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que retorna um objeto de operação assíncrono do tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Vejamos algumas maneiras de&mdash;primeiro bloquear, depois desbloquear&mdash;usando o C++/WinRT para chamar uma API assim.

## <a name="block-the-calling-thread"></a>Bloquear a conversa de chamada

O exemplo de código abaixo recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear o thread de chamada até os resultados da operação assíncrona estarem disponíveis.

Se você quiser copiar e colar esse exemplo diretamente no arquivo de código-fonte principal de um projeto de **Aplicativo de Console do Windows (C++/WinRT)** , primeiro defina **Não usando cabeçalhos pré-compilados** nas Propriedades do projeto.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

Chamar **get** é conveniente para a codificação e é ideal para aplicativos de console ou threads de segundo plano nos quais você não deseja usar uma corrotina. Mas ele não é simultâneo nem assíncrono, portanto, não é adequado para um thread de interface do usuário (e uma declaração será acionada em builds não otimizados caso tente usá-lo em um build desse tipo). Para evitar que threads do sistema operacional sejam impedidos de realizar outros trabalhos úteis, precisamos de uma técnica diferente.

## <a name="write-a-coroutine"></a>Escrever uma corrotina

C++/WinRT integra corrotinas de C++ no modelo de programação para fornecer uma maneira natural de esperar cooperativamente por um resultado. Você pode produzir sua própria operação assíncrona do Windows Runtime criando uma corrotina. No exemplo de código abaixo, **ProcessFeedAsync** é a corrotina.

> [!NOTE]
> A função **get** existe no tipo de projeção C++/WinRT **winrt::Windows::Foundation::IAsyncAction**, portanto, você pode chamar a função de dentro de um projeto C++/WinRT. Você não encontrará a função listada como um membro da interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction), pois **get** não faz parte da superfície da ABI (interface binária do aplicativo) do tipo de Windows Runtime **IAsyncAction** propriamente dito.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

Um corrotina é uma função que pode ser suspensa e retomada. Na corrotina **ProcessFeedAsync** acima, quando a instrução `co_await` é alcançada, a corrotina inicia assincronamente a chamada de **RetrieveFeedAsync** e, em seguida, ela imediatamente se suspende e retorna o controle de volta ao autor da chamada (que é **main** no exemplo acima). **main** pode então continuar a trabalhar enquanto o feed está sendo recuperado e impresso. Quando isso é feito (quando a chamada de **RetrieveFeedAsync** é concluída), a corrotina de **ProcessFeedAsync** é retomada na próxima instrução.

Você pode agregar uma corrotina em outras. Ou você pode chamar **get** para bloquear e aguardar a conclusão (e obter o resultado, se houver algum). Ou você pode passá-la para outra linguagem de programação compatível com o Windows Runtime.

Pelo uso de delegados, também é possível manipular os eventos de progresso e/ou concluídos de ações assíncronas e operações. Para obter detalhes e exemplos de código, consulte [Tipos de delegado para ações assíncronas e operações](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>O modo assíncrono retorna um tipo do Windows Runtime

Neste exemplo, nós encapsulamos uma chamada para **RetrieveFeedAsync**, para um URI específico, para nos dar uma função **RetrieveBlogFeedAsync** que retorna de forma assíncrona um [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

No exemplo acima, **RetrieveBlogFeedAsync** retorna um **IAsyncOperationWithProgress**, que tem progresso e valor retornado. Podemos realizar outros trabalhos enquanto **RetrieveBlogFeedAsync** está fazendo sua parte e recuperando o feed. Em seguida, chamamos **get** nesse objeto de operação assíncrona para bloquear, aguardamos sua conclusão e, em seguida, obtemos os resultados da operação.

Se estiver retornando assincronamente um tipo Windows Runtime, será possível retornar um [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Qualquer classe de tempo de execução primária ou de terceiros está qualificada, ou qualquer tipo que possa ser passado de ou para uma função do Windows Runtime (por exemplo, `int` ou **winrt::hstring**). O compilador ajudará com um erro "*precisa ser tipo WinRT*" se você tentar usar um desses tipos de operação assíncrona com um tipo que não for do Windows Runtime.

Se uma corrotina não tiver então pelo menos uma declaração `co_await`, para se qualificar como corrotina, ela precisará ter pelo menos uma declaração `co_return` ou `co_yield`. Há casos em que a corrotina pode retornar um valor sem apresentar nenhuma assincronia e, portanto, sem bloquear nem alternar o contexto. Aqui está um exemplo que faz isso (a segunda vez e as vezes subsequentes em que é chamado) armazenando um valor em cache.

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Retorna um tipo não Windows Runtime assincronamente

Se estiver retornando assincronamente um tipo que *não* é um tipo do Windows Runtime, você deverá retornar uma PPL (biblioteca de padrões paralelos) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Recomendamos **concurrency::task** porque ela oferece melhor desempenho (e melhor compatibilidade no futuro) que **std::future**.

> [!TIP]
> Se você incluir `<pplawait.h>`, será possível usar **concurrency::task** como um tipo de corrotina.

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>Passagem de parâmetro

Para funções síncronas, você deve usar os parâmetros `const&` por padrão. Isso evitará a sobrecarga de cópias (o que envolve a contagem de referências, que por sua vez, significa aumentos e reduções interconectados).

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

Mas você pode ter problemas ao passar um parâmetro de referência para uma corrotina.

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

Em uma corrotina, a execução é síncrona até o primeiro ponto de suspensão, no qual o controle é retornado para o autor da chamada. Quando a corrotina é retomada, qualquer coisa pode ter acontecido com o valor de origem que faz referência a um parâmetro de referência. Da perspectiva da corrotina, um parâmetro de referência tem um tempo de vida não controlado. Por isso, no exemplo acima, podemos acessar o *valor* até o `co_await`, mas não depois. Caso o *valor* seja destruído pelo chamador, tentar acessá-lo dentro da corrotina depois disso resultará em uma corrupção de memória. Nem poderemos passar com segurança o *valor* para **DoOtherWorkAsync** se houver qualquer risco de que a função seja suspensa e, em seguida, tente usar o *valor* após ser retomada.

Para tornar os parâmetros seguros para uso após a suspensão e retomada, as corrotinas devem usar passagem por valor por padrão a fim de assegurar que elas capturem pelo valor e evitar problemas de tempo de vida. São raros os casos em que é possível ignorar essa diretriz tendo certeza de que é seguro fazê-lo.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

A passagem por valor exige que a movimentação ou cópia do argumento tenha baixo custo; e esse normalmente é o caso de um ponteiro inteligente.

Também é possível afirmar que (exceto quando você quiser mover o valor) passar pelo valor const é uma boa prática. Isso não afeta o valor de origem do qual você está fazendo uma cópia, mas torna a intenção clara e ajuda caso você modifique a cópia inadvertidamente.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Consulte também [Matrizes e vetores padrão](std-cpp-data-types.md#standard-arrays-and-vectors), que aborda como passar um vetor padrão para um computador chamado assíncrono.

Se não puder alterar a assinatura de sua corrotina, mas puder alterar a implementação, você poderá fazer uma cópia local antes do primeiro `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

Se for caro copiar `Param`, basta extrair apenas as partes necessárias antes do primeiro `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acessar com segurança o ponteiro *this* em uma corrotina de membro de classe

Confira [Referências fortes e fracas em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Descarregar o trabalho no pool de threads do Windows

Uma corrotina é uma função como qualquer outra, em que um chamador é bloqueado até que uma função retorna a execução para ele. E a primeira oportunidade para uma corrotina retornar é o primeiro `co_await`, `co_return` ou `co_yield`.

Assim, antes de executar tarefas associadas à computação em uma corrotina, é necessário retornar a execução para o autor da chamada para que ele não seja bloqueado (ou seja, introduzir um ponto de suspensão). Se ainda não estiver fazendo isso por `co_await` de alguma outra operação, será possível fazer o `co_await` da função [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background). Isso retorna o controle para o autor da chamada e continua a execução imediatamente em um thread do pool de threads.

O pool de threads usado na implementação é o [pool de threads do Windows](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api) de nível inferior, portanto, é idealmente eficiente.

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>Programação pensada para afinidade de threads

Esse cenário expande o anterior. Você descarrega o trabalho em um pool de threads, mas, em seguida, deseja exibir o progresso na IU (interface do usuário).

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

O código acima gera uma exceção [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread), pois um **TextBlock** precisa ser atualizado por meio do thread que o gerou, ou seja, um thread da interface do usuário. Uma solução é capturar o contexto do thread no qual a corrotina foi chamada originalmente. Para fazer isso, crie uma instância de um objeto [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context), faça trabalho em segundo plano e, em seguida, `co_await` o **apartment_context** para alternar novamente para o contexto da chamada.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Desde que a corrotina acima seja chamada do thread da interface do usuário que criou o **TextBlock**, essa técnica funciona. Há muitos casos em seu aplicativo em que você tem certeza disso.

Para obter uma solução mais geral para atualizar a interface do usuário, que abrange os casos em que você não tem certeza sobre o thread que faz a chamada, você pode `co_await` a função [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) função para alternar para um thread de primeiro plano específico. No exemplo de código abaixo, especificamos o thread de primeiro plano passando o objeto dispatcher associado a **TextBlock** (ao acessar a propriedade [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). A implementação de **winrt::resume_foreground** chama [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) nesse objeto dispatcher para executar o trabalho que vem depois dele na corrotina.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextos de execução, retomada e alternância em uma corrotina

Falando genericamente, após um ponto de suspensão em uma corrotina, o thread original de execução pode desaparecer e a retomada pode ocorrer em qualquer thread (em outras palavras, qualquer thread pode chamar o método **Completed** para a operação assíncrona).

Porém, se você `co_await` qualquer um dos quatro tipos de operação assíncrona do Windows Runtime (**IAsyncXxx**), então C++/WinRT vai capturar o contexto de chamada no ponto que você `co_await`. E ele garante que você ainda esteja nesse contexto quando a continuação é retomada. O C++/ WinRT fará isso verificando se você já estiver no contexto da chamada e, se não estiver, alternando para ele. Se você estivesse em um thread STA (single-threaded apartment) antes de `co_await`, você estaria no mesmo posteriormente; se você estivesse em um thread MTA (multi-threaded apartment) antes de `co_await`, você estaria em um posteriormente.

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

O motivo pelo qual você pode contar com esse comportamento é porque o C++/WinRT fornece código para adaptar esses tipos de operação assíncrona do Windows Runtime ao suporte à linguagem de corrotina C++ (esses trechos de código são chamados de adaptadores de espera). Os tipos awaitable restantes em C++/WinRT são simplesmente auxiliares e/ou wrappers de pool de thread, então eles completam o pool de threads.

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Se você `co_await` algum outro tipo &mdash; até mesmo em uma implementação de corrotina C++/WinRT&mdash; outra biblioteca fornecerá os adaptadores e, em seguida, você precisará entender o que esses adaptadores fazem em termos de retomada e contextos.

Para manter o mínimo possível de alternâncias de contexto, você pode usar algumas das técnicas que já vimos neste tópico. Vejamos algumas ilustrações de como fazer isso. Neste próximo exemplo de pseudocódigo, mostramos o esboço de um manipulador de eventos que chama uma API do Windows Runtime para carregar uma imagem, cai em um thread em segundo plano para processar essa imagem e, em seguida, retorna para o thread de interface do usuário para exibir a imagem na interface do usuário.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Para este cenário, há um pouco de ineficiência em torno da chamada para **StorageFile::OpenAsync**. Há uma alternância de contexto necessária para um thread de segundo plano (de modo que o manipulador pode retornar a execução para o chamador), na continuação após a qual o C++/WinRT restaura o contexto do thread da interface do usuário. Mas, nesse caso, não é necessário estar no thread da interface do usuário até que estejamos prestes a atualizar a interface do usuário. Quanto mais APIs do Windows Runtime chamamos *antes* de nossa chamada para **winrt::resume_background**, mais ficamos sujeitos a alternâncias de contexto de ida e volta desnecessárias. A solução não é chamar *nenhuma* API do Windows Runtime antes disso. Mova todas elas após o **winrt::resume_background**.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Se quiser fazer algo mais avançado, você poderá escrever seus próprios adaptadores await. Por exemplo, se quiser que um `co_await` continue no mesmo thread em que a ação assíncrona é concluída (portanto, não há nenhuma opção de contexto), você poderá começar escrevendo await adaptadores semelhantes aos mostrados abaixo.

> [!NOTE]
> O exemplo de código a seguir é fornecido para fins educacionais; é para você começar a entender como os adaptadores await funcionam. Se você quiser usar essa técnica em sua própria base de código, é recomendável que você desenvolva e teste seus próprios structs do adaptador await. Por exemplo, você poderia escrever **complete_on_any**, **complete_on_current** e **complete_on(dispatcher)** . Também considere transformá-los em modelos que usam o tipo **IAsyncXxx** como um parâmetro de modelo.

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

Para entender como usar os adaptadores await **no_switch**, primeiro você precisa saber que, quando o compilador C++ encontra uma expressão `co_await`, ele procura por funções chamadas **await_ready**, **await_suspend** e **await_resume**. A biblioteca C++/WinRT fornece essas funções para que você obtenha um comportamento razoável por padrão, como este.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Para usar os adaptadores await **no_switch**, basta alterar o tipo dessa expressão `co_await` de **IAsyncXxx** para **no_switch**, deste modo.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Em seguida, em vez de olhar para as três funções **await_xxx** que correspondem a **IAsyncXxx**, o compilador C++ procura funções que correspondem a **no_switch**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Cancelar uma operação assíncrona e retornos de chamada de cancelamento

Recursos do Windows Runtime para a programação assíncrona permitem que você cancele uma operação ou ação assíncrona em andamento. Aqui está um exemplo que chama [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) para recuperar uma coleção potencialmente grande de arquivos e armazena o objeto da operação assíncrona resultante em um membro de dados. O usuário tem a opção de cancelar a operação.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

Para o lado da implementação do cancelamento, começaremos com um exemplo simples.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

Se executar o exemplo acima, você verá **ImplicitCancellationAsync** imprimir uma mensagem por segundo durante três segundos, tempo após o qual ela encerrará automaticamente por ser cancelada. Isso funciona porque, ao encontrar uma expressão `co_await`, uma corrotina verifica se ela foi cancelada. Em caso positivo ela deixa de funcionar e, caso contrário, ela suspende normalmente.

É claro que o cancelamento pode ocorrer enquanto a corrotina está suspensa. A corrotina só verifica se há cancelamento quando ela é retomada ou quando acessa outro `co_await`. Trata-se de um problema de latência com granulometria potencialmente muito grosseira ao responder ao cancelamento.

Portanto, outra opção é sondar explicitamente o cancelamento de dentro de sua corrotina. Atualize o exemplo acima com o código na listagem abaixo. Neste novo exemplo, **ExplicitCancellationAsync** recupera o objeto retornado pela função [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) e o utiliza para verificar periodicamente se a corrotina foi cancelada. Desde que ela não tenha sido cancelada, a corrotina fará um loop indefinidamente; depois que ela tiver sido cancelada, o loop e a função se encerrarão normalmente. O resultado é o mesmo do exemplo anterior, mas neste caso o encerramento ocorre explicitamente e sob controle.

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

Aguardar **winrt::get_cancellation_token** recupera um token de cancelamento com conhecimento sobre a **IAsyncAction** que a corrotina está produzindo em seu nome. Você pode usar o operador de chamada de função nesse token para consultar o estado de cancelamento &mdash; essencialmente sondando para cancelamento. Se você estiver executando alguma operação ligada à computação ou iterando por de uma coleção grande, essa será uma técnica razoável.

### <a name="register-a-cancellation-callback"></a>Registrar um retorno de chamada de cancelamento

O cancelamento do Windows Runtime não flui automaticamente para outros objetos assíncronos. Mas &mdash;introduzido na versão 10.0.17763.0 (Windows 10, versão 1809) do SDK do Windows&mdash; você pode registrar um retorno de chamada de cancelamento. Este é um gancho preemptivo pelo qual o cancelamento pode ser propagado e possibilita a integração com as bibliotecas existentes em simultaneidade.

Neste próximo exemplo de código, **NestedCoroutineAsync** faz o trabalho, mas não contém nenhuma lógica de cancelamento especial. **CancellationPropagatorAsync** é essencialmente um wrapper na corrotina aninhada; o wrapper encaminha o cancelamento preventivamente.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** registra uma função lambda para seu próprio retorno de chamada de cancelamento e, em seguida, ele aguarda (suspende) até que o trabalho aninhado seja concluído. Quando ou se **CancellationPropagatorAsync** for cancelado, ele propagará o cancelamento para a corrotina aninhada. Não é necessário sondar para cancelamento. O cancelamento não é bloqueado indefinidamente. Esse mecanismo é flexível o suficiente para que você possa usá-lo para fornecer interoperabilidade com uma biblioteca de corrotina ou simultaneidade sem conhecimento de C++/WinRT.

## <a name="reporting-progress"></a>Relatório de progresso

Se a sua corrotina retornar um [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) ou [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), você poderá recuperar o objeto retornado pela função [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) e usá-lo para relatar o progresso para um manipulador de progresso. Aqui está um exemplo de código.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> Não é correto implementar mais de um *manipulador de conclusão* para uma ação ou operação assíncrona. Você pode ter um único delegado para seu evento concluído ou pode usar `co_await`. Se você tiver ambos, o segundo falhará. Qualquer um dos seguintes dois tipos de manipuladores de conclusão é apropriado, mas não ambos para o mesmo objeto assíncrono.

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

Para obter mais informações sobre manipuladores de conclusão, consulte [Tipos de delegados para ações e operações assíncronas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Disparar e esquecer

Às vezes, você tem uma tarefa que pode ser executada simultaneamente com outro trabalho e não precisa aguardar a conclusão da tarefa (nenhum outro trabalho depende dela), também não é necessário que ela retorne um valor. Nesse caso, você pode disparar a tarefa e esquecê-la. Você pode fazer isso escrevendo uma corrotina cujo tipo de retorno é [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (em vez de um dos tipos de operação assíncrona do Windows Runtime ou **concurrency::task**).

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

**winrt::fire_and_forget** também é útil como tipo retornado de seu manipulador de eventos quando você precisa realizar operações assíncronas nele. Veja um exemplo (confira também [Referências fortes e fracas em C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)).

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

O primeiro argumento (o *remetente*) fica sem nome, porque nunca o usamos. Por esse motivo, é seguro deixá-lo como referência. Mas observe que *args* é passado por valor. Consulte a seção [Passagem de parâmetros](#parameter-passing) acima.

## <a name="important-apis"></a>APIs Importantes
* [concurrency::task class](/cpp/parallel/concrt/reference/task-class)
* [Interface IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interface IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [Interface IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [Interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Tópicos relacionados
* [Manipular eventos usando delegados em C++/WinRT](handle-events.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)
