---
author: stevewhims
description: Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.
title: Simultaneidade e operações assíncronas com C++/WinRT
ms.author: stwhi
ms.date: 10/21/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: b1a45ba0bd362c07c27516ef18c11c326d747b1f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556919"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Simultaneidade e operações assíncronas com C++/WinRT

Este tópico mostra as maneiras nas quais você pode criar e consumam objetos assíncronos do Windows Runtime com [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operações assíncronas e funções "Async" do Windows Runtime

Qualquer API do Windows Runtime que tem o potencial de demorar mais de 50 milissegundos para concluir é implementado como uma função assíncrona (com um nome terminado em "Async"). A implementação de uma função assíncrona inicia o trabalho em outra conversa e retorna imediatamente com um objeto que representa a operação assíncrona. Quando a operação assíncrona for concluída, aquele objeto retornado contém qualquer valor que resultou do trabalho. O namespace do Windows Runtime **Windows::Foundation** contém quatro tipos de objeto de operação assíncrona.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_), and
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada um desses tipos de operação assíncrona é projetado para um tipo correspondente no namespace C++/WinRT do **winrt::Windows::Foundation**. C++/WinRT também contém uma estrutura de adaptador de espera interno. Você não o usa diretamente, mas, graças a essa estrutura, você pode escrever um `co_await` instrução para cooperativa o resultado de qualquer função que retorna um desses tipos de operação assíncrona. Você pode criar suas próprias rotinas concomitantes que retornam esses tipos.

Um exemplo de uma função do Windows assíncrona [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que retorna um objeto de operação assíncrono do tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Vejamos algumas maneiras&mdash;bloqueio primeiro e, em seguida, sem bloqueio&mdash;de usar C++ c++ /WinRT para chamar uma API como essas.

## <a name="block-the-calling-thread"></a>Bloquear a conversa de chamada

O exemplo de código abaixo recebe um objeto de operação assíncrona do **RetrieveFeedAsync** e chama **get** nesse objeto para bloquear a conversa de chamada até os resultados da operação assíncrona estiver disponível.

```cppwinrt
// main.cpp

#include "pch.h"
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

Chamar **get** é conveniente para a codificação e é ideal para aplicativos de console ou threads de segundo plano nos quais você não deseja usar uma corrotina. Mas não é simultâneo nem assíncrono, portanto, não é adequado para um thread de interface do usuário (e uma declaração será acionada nas compilações não otimizadas caso tente usá-lo). Para evitar atraso de conversas do sistema operacional de realizar outros trabalhos úteis, precisamos de uma técnica diferente.

## <a name="write-a-coroutine"></a>Escrever uma rotina concomitante

C++/WinRT integra rotinas concomitantes de C++ no modelo de programação para fornecer uma maneira natural de esperar cooperativamente por um resultado. Você pode produzir sua própria operação assíncrona do Windows Runtime criando uma rotina concomitante. No exemplo de código abaixo, **ProcessFeedAsync** é a rotina concomitante.

> [!NOTE]
> A função de **obter** existe em C++ c++ WinRT projeção digite **winrt::Windows::Foundation::IAsyncAction**, portanto, você pode chamar a função de dentro de qualquer C + c++ projeto WinRT. Você não encontrará a função listada como um membro da interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) , porque não é parte da superfície da interface binária (ABI) do aplicativo do tipo real do Windows Runtime **IAsyncAction** **obter** .

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

Um rotina concomitante é uma função que pode ser suspensa e retomada. Na rotina concomitante **ProcessFeedAsync** acima, quando a instrução `co_await` é alcançada, a rotina concomitante assincronamente inicia a chamada de **RetrieveFeedAsync** e, em seguida, ele imediatamente se suspende e retorna o controle de volta ao chamador (que é **main** no exemplo acima). **main** pode então continuar a trabalhar enquanto o feed está sendo recuperado e impresso. Quando isso é feito (quando a chamada de **RetrieveFeedAsync** é concluída), a rotina concomitante de **ProcessFeedAsync** é retomada na próxima instrução.

Você pode agregar uma corrotina em outras. Ou você pode chamar **get** para bloquear e aguardar a conclusão (e obter o resultado, se houver algum). Ou você pode passá-la para outra linguagem de programação compatível com o Windows Runtime.

Também é possível manipular o os eventos de progresso e/ou concluídos de ações assíncronas e operações usando delegados. Para obter detalhes e exemplos de código, consulte [Tipos de delegado para ações assíncronas e operações](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>O modo assíncrono retorna um tipo do Windows Runtime

Neste exemplo, nós encapsulamos uma chamada para **RetrieveFeedAsync**, para um URI específico, para nos dar uma função **RetrieveBlogFeedAsync** que retorna de forma assíncrona um [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

No exemplo acima, **RetrieveBlogFeedAsync** retorna um **IAsyncOperationWithProgress**, que tem progresso e valor de retorno. Podemos realizar outros trabalhos enquanto **RetrieveBlogFeedAsync** está fazendo sua parte e recuperando o feed. Em seguida, chamamos **get** nesse objeto de operação assíncrona para bloquear, aguardamos sua conclusão e, em seguida, obtemos os resultados da operação.

Se estiver retornando assincronamente um tipo Windows Runtime, é possível retornar um [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Qualquer classe de tempo de execução primária ou de terceiros está qualificada, ou qualquer tipo que possa ser passado de ou para uma função do Windows Runtime (por exemplo, `int` ou **winrt::hstring**). O compilador ajudará você com um erro "*deve ser tipo WinRT*" se tentar usar um desses tipos de operação assíncrona com um tipo que não for do Windows Runtime.

Se não tiver uma corrotina não tem pelo menos uma declaração `co_await`, para se qualificar como corrotina, ela deve ter pelo menos uma declaração `co_return` ou `co_yield`. Há casos em que a corrotina pode retornar um valor sem apresentar qualquer assincronia e, portanto, sem bloquear nem alternar o contexto. Veja um exemplo disso (os horários secundários e subsequentes são chamados) ao armazenar um valor em cache.

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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>O modo assíncrono não retorna um tipo do Windows Runtime

Se estiver retornando assincronamente um tipo que *não* é um tipo do Windows Runtime, você deve retornar uma Biblioteca de padrões paralelos (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Recomendamos **concurrency::task** porque ela oferece o melhor desempenho (e melhor compatibilidade no futuro) que **std::future**.

> [!TIP]
> Se você incluir `<pplawait.h>`, é possível usar **concurrency::task** como um tipo de corrotina.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

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

Para funções síncronas, você deve usar os parâmetros `const&` por padrão. Isso evitará a sobrecarga de cópias (que envolve a contagem de referência e isso significa aumentos e reduções conectados).

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

Em uma corrotina, a execução é síncrona até o primeiro ponto de suspensão, no qual o controle é retornado para o autor da chamada. Quando a corrotina é retomada, tudo pode ter acontecido com o valor de origem que faz referência a um parâmetro de referência. Da perspectiva da corrotina, um parâmetro de referência tem um tempo de vida não controlado. Por isso, no exemplo acima, podemos acessar o *valor* até `co_await`, mas não depois. Nem podemos passar com segurança o *valor* para **DoOtherWorkAsync** se houver qualquer risco de suspensão da função e, em seguida, tentamos usar o *valor* após ser retomado. Para tornar os parâmetros seguros para uso após a suspensão e retomada, as corrotinas devem usar passar pelo valor por padrão a fim de garantir que capturam pelo valor e evitar problemas de tempo de vida. São raros os casos em que você pode desviar-se das diretrizes, pois você tem certeza de que é seguro fazer isso.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

Também é possível afirmar que (exceto quando quiser mover o valor) passar o valor const é uma prática recomendada. Isso não afeta o valor de origem do qual você está fazendo uma cópia, mas torna o intenção clara e ajuda caso você modifique a cópia inadvertidamente.
    
```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Consulte também [Matrizes e vetores padrão](std-cpp-data-types.md#standard-arrays-and-vectors), que aborda como passar um vetor padrão para um comprador chamado assíncrono.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Descarregar o trabalho no pool de threads do Windows

Uma rotina concomitante é uma função como qualquer outro em que um chamador é bloqueado até que uma função retorna a execução para ele. E, na primeira oportunidade para uma rotina concomitante retornar é o primeiro `co_await`, `co_return`, ou `co_yield`.

Portanto, antes de fazer associadas à computação em uma corrotina, é necessário retornar a execução ao chamador (em outras palavras, introduzir um ponto de suspensão) para que o chamador não seja bloqueado. Se você não estiver fazendo isso `co-await`- esboços alguma outra operação, é possível `co-await` a função [**WinRT:: resume_background**](/uwp/cpp-ref-for-winrt/resume-background) . Isso retorna o controle para o autor da chamada e continua a execução imediatamente em um thread do pool de threads.

O pool de threads usado na implementação é o [pool de threads do Windows](https://msdn.microsoft.com/library/windows/desktop/ms686766) de nível inferior, portanto, é eficiente.

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

## <a name="programming-with-thread-affinity-in-mind"></a>Programação com afinidade de threads

Esse cenário expande o anterior. Você descarrega o trabalho em um pool de threads, mas, em seguida, deseja exibir o progresso na interface do usuário (IU).

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

O código acima gera uma exceção [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread), pois um **TextBlock** deve ser atualizado a partir do thread que o gerou, ou seja, um thread da interface do usuário. Uma solução é capturar o contexto do thread no qual a corrotina foi chamada originalmente. Para fazer isso, instanciar um objeto [**WinRT:: apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) , de trabalho, em segundo plano e, em seguida, `co_await` o **apartment_context** para voltar ao contexto de chamada.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Enquanto a corrotina acima for chamada do thread da interface do usuário que criou o **TextBlock**, essa técnica funciona. Há muitos casos em seu aplicativo em que você tem certeza disso.

Para uma solução mais geral para atualizar a interface do usuário, que abrange casos em que você tem certeza sobre o thread de chamada, você pode `co-await` a função [**WinRT:: resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) para alternar para um thread específico em primeiro plano. No exemplo de código abaixo, especificamos o thread de primeiro plano passando o objeto dispatcher associado a **TextBlock** (ao acessar a propriedade [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). A implementação de **winrt::resume_foreground** chama [**Coredispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) nesse objeto dispatcher para executar o trabalho que vem depois na corrotina.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextos de execução, retomar e alterná-los em uma corrotina

Em termos gerais, após um ponto de suspensão em uma corrotina, o segmento original de execução pode desaparecer e retomada pode ocorrer em qualquer thread (em outras palavras, qualquer thread pode chamar o método de **Completed** para a operação assíncrona).

Mas se você `co-await` qualquer um dos quatro do Windows Runtime operação assíncrona tipos (**IAsyncXxx**), em seguida, C + c++ WinRT captura o contexto de chamada no ponto você `co-await`. E garante que você esteja ainda esse contexto quando retoma a continuação. C++ c++ WinRT faz isso verificando se você já está no contexto de chamada e, se não, alternando para ele. Se você estava em um thread de single-threaded apartment (STA) antes de `co-await`, em seguida, você vai estar usando o mesmo posteriormente; Se você estava em um thread apartment de vários threads (MTA) antes de `co-await`, em seguida, você estará em um posteriormente.

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

O motivo pelo qual você pode contar com esse comportamento ocorre porque C++ c++ WinRT fornece o código para adaptar esses tipos de operação assíncrona do tempo de execução do Windows para o suporte de linguagem C++ concomitante (essas partes de código são chamados de adaptadores de espera). Os tipos que pode ser aguardados restantes em C++ c++ WinRT são simplesmente wrappers de pool de threads e/ou auxiliares; Portanto, ele concluir o pool de threads.

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

Se você `co_await` algum outro tipo&mdash;até mesmo em C++ c++ implementação de rotina concomitante WinRT&mdash;outra biblioteca fornece os adaptadores, e você precisará entender que esses adaptadores fazer em termos de retomada e contextos.

Para manter as alternâncias de contexto para mínimo, você pode usar algumas das técnicas que já vimos neste tópico. Vejamos algumas ilustrações de fazer isso. No exemplo a seguir pseudocódigo, mostramos o contorno de um manipulador de eventos que chama uma API de tempo de execução do Windows para carregar uma imagem, cai em um thread em segundo plano para processar essa imagem e, em seguida, retorna ao thread de interface do usuário para exibir a imagem da interface do usuário.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

Para esse cenário, há um pouco de ineffiency em torno da chamada para **StorageFile::OpenAsync**. Há uma atualização de contexto necessário para um plano de fundo thread (para que o manipulador pode retornar a execução para o chamador), na continuação após a qual C + c++ WinRT restaura o contexto do thread da interface do usuário. Mas, nesse caso, não é necessário ser no thread da interface do usuário até que estamos prestes a atualizar a interface do usuário. As APIs do tempo de execução do Windows mais que chamamos *antes de* nossa chamada para **WinRT:: resume_background**, os switches back bidirecional contexto mais desnecessários tivermos. A solução não é chamar *quaisquer* APIs do Windows Runtime antes disso. Movê-los depois do **WinRT:: resume_background**.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

Se você deseja fazer algo mais avançado, em seguida, você pode escrever seus próprios await adaptadores. Por exemplo, se você quiser um `co_await` para continuar no mesmo thread que a ação assíncrona seja concluída em (portanto, não há nenhum comutador de contexto), em seguida, você pode começar escrevendo await adaptadores semelhantes àquelas mostrado abaixo.

> [!NOTE]
> O exemplo de código abaixo é fornecido apenas para fins educacionais; é para você começar Noções básicas sobre como await adaptadores de trabalho. Se você deseja usar essa técnica em seu próprio código de base, em seguida, é recomendável que você desenvolver e testar seus próprios await struct(s) adaptador. Por exemplo, você pode escrever **complete_on_any**, **complete_on_current**e **complete_on(dispatcher)**. Além disso, considere torná-los modelos que usam o tipo de **IAsyncXxx** como um parâmetro de modelo.

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

Para entender como usar o **sem_mudança** await adaptadores, primeiro você precisará saber que, quando o compilador C++ encontra um `co_await` chamado de expressão, ele procura funções **await_ready**, **await_suspend**e **await_resume**. C++ c++ WinRT biblioteca fornece essas funções para que você obter o comportamento razoável por padrão, desta forma.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Usar o **sem_mudança** await adaptadores, apenas mude o tipo de que `co_await` expressão de **IAsyncXxx** a **sem_mudança**, desta forma.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

Em seguida, em vez de procurando as três funções **await_xxx** que correspondem ao **IAsyncXxx**, o compilador C++ procura por funções que correspondem ao **sem_mudança**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Cancelar uma operação assíncrona e retornos de chamada de cancelamento

Recursos do Windows Runtime para programação assíncrona permitem que você cancelar uma operação ou ação assíncrona em trânsito. Vamos começar com um exemplo simples.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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

Se você executar o exemplo acima, e em seguida, você verá **ImplicitCancellationAsync** impressão uma mensagem por segundo por três segundos após o qual vez em que ele automaticamente encerra como resultado do cancelamento. Isso funciona porque, encontrando em um `co_await` expressão, uma rotina concomitante verifica se foi cancelada. Caso afirmativo, em seguida, ele reduz e, caso contrário, em seguida, suspende como normal.

Cancelamento pode, obviamente, acontecer enquanto a rotina concomitante estiver suspenso. Somente quando a corrotina é retomada, ou bate em outro `co_await`, ele verificará cancelamento. O problema é uma das potencialmente muito grande-mais refinado latência responder ao cancelamento.

Portanto, outra opção é explicitamente sondar cancelamento de dentro de sua rotina concomitante. Atualize o exemplo acima com o código na lista abaixo. Neste exemplo de novo, **ExplicitCancellationAsync** recupera o objeto retornado pela função [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) e usa-o para verificar periodicamente se a rotina concomitante foi cancelada. Desde que não seja cancelado, a rotina concomitante permanece em loop indefinidamente; Depois que ele for cancelado, o loop e a função saia normalmente. O resultado é o mesmo, como o exemplo anterior, mas aqui saindo acontece explicitamente e sob o controle.

```cppwinrt
...
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

Aguardando **winrt::get_cancellation_token** recupera um token de cancelamento conhecimento das **IAsyncAction** que está produzindo a rotina concomitante em seu nome. Você pode usar o operador de chamada de função nesse token para consultar o estado de cancelamento&mdash;essencialmente sondando cancelamento. Se você estiver realizando algumas operações associadas à computação ou Iterando através de uma coleção grande, em seguida, isso é uma técnica razoável.

### <a name="register-a-cancellation-callback"></a>Registrar um retorno de chamada de cancelamento

Cancelamento do Windows Runtime automaticamente não flui para outros objetos assíncronos. Mas&mdash;introduzido na versão 10.0.17763.0 (Windows 10, versão 1809) do SDK do Windows&mdash;você pode registrar um retorno de chamada de cancelamento. Este é um gancho preventivo pelo qual o cancelamento pode ser propagado e torna possível integrar bibliotecas de simultaneidade existentes.

Neste próximo exemplo de código, **NestedCoroutineAsync** faz o trabalho, mas ele tem nenhuma lógica especial cancelamento nele. **CancellationPropagatorAsync** é essencialmente um wrapper na rotina concomitante aninhado; o wrapper encaminha preventivamente cancelamento.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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

    cancellation_token.callback([&]
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

**CancellationPropagatorAsync** registra uma função lambda para seu próprio retorno de chamada de cancelamento e, em seguida, ele aguarda (ele suspende) até que o trabalho aninhado seja concluída. Quando ou se **CancellationPropagatorAsync** for cancelado, ele propaga o cancelamento para a rotina concomitante aninhado. Não é necessário para pesquisar cancelamento; nem é cancelamento bloqueado indefinidamente. Esse mecanismo é flexível o suficiente para que você possa usá-lo para interoperabilidade com uma rotina concomitante ou simultaneidade biblioteca que sabe nada do C++ c++ WinRT.

## <a name="reporting-progress"></a>Relatório de progresso

Se a corrotina retorna [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)ou [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), em seguida, você pode recuperar o objeto retornado pela função [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) e usá-lo para relatar o progresso volta para um progresso manipulador. A seguir está um exemplo de código.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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
> Ele não estiver correto implementar mais de um *manipulador de conclusão* para uma ação assíncrona ou operação. Você pode ter um único delegate para seu evento concluído, ou você pode `co_await` -lo. Se você tiver ambos, o segundo falhará. Qualquer um dos seguintes dois tipos de manipuladores de conclusão é apropriado; Não, ambos para o mesmo objeto assíncrono.

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

Para obter mais informações sobre os manipuladores de conclusão, consulte [tipos de delegado para ações e operações assíncronas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Disparar e esquecer

Às vezes, você tem uma tarefa que pode ser feita simultaneamente com outros trabalhos, e você não precisará aguardar a conclusão da tarefa (nenhum outro trabalho depende dela), nem tampouco é necessário para retornar um valor. Nesse caso, você pode disparar a tarefa e esquecer. Você pode fazer isso criando uma rotina concomitante cujo tipo de retorno é [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (em vez de um dos tipos de operação assíncrona do Windows Runtime, ou **Concurrency:: Task**).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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

## <a name="important-apis"></a>APIs Importantes
* [classe Concurrency:: Task](/cpp/parallel/concrt/reference/task-class)
* [Interface de IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método syndicationclient:: Retrievefeedasync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [WinRT::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [WinRT::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [WinRT::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Tópicos relacionados
* [Processar eventos usando delegados em C++/WinRT](handle-events.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)