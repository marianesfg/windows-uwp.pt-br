---
author: stevewhims
description: Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.
title: Simultaneidade e operações assíncronas com C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: 85071fb28cb87c991e2f5ba7f64b681c6850c819
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4083383"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Simultaneidade e operações assíncronas com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operações assíncronas e funções "Async" do Windows Runtime
Qualquer API do Windows Runtime que tem o potencial de demorar mais de 50 milissegundos para concluir é implementado como uma função assíncrona (com um nome terminado em "Async"). A implementação de uma função assíncrona inicia o trabalho em outra conversa e retorna imediatamente com um objeto que representa a operação assíncrona. Quando a operação assíncrona for concluída, aquele objeto retornado contém qualquer valor que resultou do trabalho. O namespace do Windows Runtime **Windows::Foundation** contém quatro tipos de objeto de operação assíncrona.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_), and
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada um desses tipos de operação assíncrona é projetado para um tipo correspondente no namespace C++/WinRT do **winrt::Windows::Foundation**. C++/WinRT também contém uma estrutura de adaptador de espera interno. Você não o usa diretamente, mas, graças a essa estrutura, você pode escrever uma instrução "co_await" para aguardar de maneira cooperativa o resultado de qualquer função que retorna um desses tipos de operação assíncrona. Você pode criar suas próprias rotinas concomitantes que retornam esses tipos.

Um exemplo de uma função do Windows assíncrona [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que retorna um objeto de operação assíncrono do tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Vejamos algumas maneiras de&mdash;bloqueio e desbloqueio&mdash;de usar C++/WinRT para chamar uma API como essas.

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
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
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
> A função de **obter** existe em C++ c++ WinRT projeção digite **winrt::Windows::Foundation::IAsyncAction**, portanto, você pode chamar a função de dentro de qualquer C + c++ projeto WinRT. A função listada como um membro da interface [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) , não será possível encontrar porque não é parte da superfície da interface binária (ABI) do aplicativo do tipo real do Windows Runtime **IAsyncAction** **obter** .

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

    auto processOp = ProcessFeedAsync();
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

    auto feedOp = RetrieveBlogFeedAsync();
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
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
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
Antes de executar tarefas associadas à computação em uma corrotina, é necessário retornar a execução para o autor da chamada para que não seja bloqueado (ou seja, introduzir um ponto de suspensão). Se ainda não estiver fazendo isso por `co-await` de alguma outra operação, é possível fazer o `co-await` da função **winrt::resume_background**. Isso retorna o controle para o autor da chamada e continua a execução imediatamente em um thread do pool de threads.

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
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

O código acima gera uma exceção [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread), pois um **TextBlock** deve ser atualizado a partir do thread que o gerou, ou seja, um thread da interface do usuário. Uma solução é capturar o contexto do thread no qual a corrotina foi chamada originalmente. Instancie um objeto **winrt::apartment_context** e `co_await`.

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

Enquanto a corrotina acima for chamada do thread da interface do usuário que criou o **TextBlock**, essa técnica funciona. Há muitos casos em seu aplicativo em que você tem certeza disso.

> [!NOTE]
> **O exemplo de código a seguir está relacionado ao produto de pré-lançamento, o qual poderá ser consideravelmente modificado antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.**

Há uma solução mais geral para atualizar a interface do usuário, que abrange casos nos quais não há certeza sobre o thread de chamada. Você pode instalar a [Versão Prévia 17661 do SDK do Windows 10](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) ou posterior. Em seguida, você pode `co-await` a função **winrt::resume_foreground** a fim de alternar para um thread específico em primeiro plano. No exemplo de código abaixo, especificamos o thread de primeiro plano passando o objeto dispatcher associado a **TextBlock** (ao acessar a propriedade [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). A implementação de **winrt::resume_foreground** chama [**Coredispatcher**](/uwp/api/windows.ui.core.coredispatcher.runasync) nesse objeto dispatcher para executar o trabalho que vem depois na corrotina.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="important-apis"></a>APIs Importantes
* [classe Concurrency:: Task](/cpp/parallel/concrt/reference/task-class)
* [Interface IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interface](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método syndicationclient:: Retrievefeedasync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>Tópicos relacionados
* [Processar eventos usando delegados em C++/WinRT](handle-events.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)