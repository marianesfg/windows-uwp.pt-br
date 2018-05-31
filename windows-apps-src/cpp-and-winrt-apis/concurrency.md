---
author: stevewhims
description: Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.
title: Simultaneidade e operações assíncronas com C++/WinRT
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: 3af9125abc3abf41327f5b49e6a05d81e214f89f
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831830"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Simultaneidade e operações assíncronas com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operações assíncronas e funções "Async" do Windows Runtime
Qualquer API do Windows Runtime que tem o potencial de demorar mais de 50 milissegundos para concluir é implementado como uma função assíncrona (com um nome terminado em "Async"). A implementação de uma função assíncrona inicia o trabalho em outra conversa e retorna imediatamente com um objeto que representa a operação assíncrona. Quando a operação assíncrona for concluída, aquele objeto retornado contém qualquer valor que resultou do trabalho. O namespace do Windows Runtime **Windows::Foundation** contém quatro tipos de objeto de operação assíncrona, e eles são

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_), and
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada um desses tipos de operação assíncrona é projetado para um tipo correspondente no namespace C++/WinRT do **winrt::Windows::Foundation**. C++/WinRT também contém uma estrutura de adaptador de espera interno. Você não o usa diretamente, mas, graças a essa estrutura, você pode escrever uma instrução **co_await** para aguardar de maneira cooperativa o resultado de qualquer função que retorna um desses tipos de operação assíncrona. Você pode criar suas próprias rotinas concomitantes que retornam esses tipos.

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

Chamar **get** funciona para codificação conveniente, mas não é o que você chamaria de cooperativo. Não é simultânea nem assíncrona. Para evitar atraso de conversas do sistema operacional de realizar outros trabalhos úteis, precisamos de uma técnica diferente.

## <a name="write-a-coroutine"></a>Escrever uma rotina concomitante
C++/WinRT integra rotinas concomitantes de C++ no modelo de programação para fornecer uma maneira natural de esperar cooperativamente por um resultado. Você pode produzir sua própria operação assíncrona do Windows Runtime criando uma rotina concomitante. No exemplo de código abaixo, **ProcessFeedAsync** é a rotina concomitante.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = co_await syndicationClient.RetrieveFeedAsync(rssFeedUri);
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

Um rotina concomitante é uma função que pode ser suspensa e retomada. Na rotina concomitante **ProcessFeedAsync** acima, quando a instrução **co_await** é alcançada, a rotina concomitante assincronamente inicia a chamada de **RetrieveFeedAsync** e, em seguida, ele imediatamente se suspende e retorna o controle de volta ao chamador (que é **main** no exemplo acima). **main** pode então continuar a trabalhar enquanto o feed está sendo recuperado e impresso. Quando isso é feito (quando a chamada de **RetrieveFeedAsync** é concluída), a rotina concomitante de **ProcessFeedAsync** é retomada na próxima instrução.

Você pode agregar uma rotina concomitante em outras rotinas. Ou você pode chamar **get** para bloquear e aguardar a conclusão (e obter o resultado, se houver algum). Ou você pode passá-la para outra linguagem de programação compatível com o Windows Runtime.

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

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
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

Se você estiver assincronamente retornando um tipo de Windows Runtime (seja do tipo primário ou de terceiros), então você deve retornar um [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou um [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

O compilador ajudará você com um erro "*deve ser tipo WinRT*" se tentar usar um desses tipos de operação assíncrona com um tipo que não for do Windows Runtime.

## <a name="asychronously-return-a-non-windows-runtime-type"></a>O modo assíncrono não retorna um tipo do Windows Runtime
Se você estiver assincronamente retornando um tipo que *não* é um tipo do Windows Runtime, em seguida, você deve retornar uma biblioteca de padrões paralelos (PPL) [**tarefa**](https://msdn.microsoft.com/library/hh750113). Recomendamos **tarefa** porque ela oferece melhor desempenho (e melhor compatibilidade no futuro) que o **std::future**.

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
    // do other work.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="important-apis"></a>APIs Importantes
* [concurrency::task](https://msdn.microsoft.com/library/hh750113)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
