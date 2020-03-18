---
description: Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT.
title: Simultaneidade e operações assíncronas com C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: 949f8c407e0a49c87cbb45c01117a7e2e1525010
ms.sourcegitcommit: 5f22e596443ff4645ebf68626d8a4d275d8a865f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083182"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Simultaneidade e operações assíncronas com C++/WinRT

> [!IMPORTANT]
> Este tópico apresenta os conceitos de *corrotinas* e `co_await`, que recomendamos que você use na interface do usuário *e* em seus aplicativos que não sejam da interface do usuário. Para simplificar, a maioria dos exemplos de código neste tópico introdutório mostra projetos **Aplicativo de Console do Windows (C++/WinRT)** . Os exemplos de código posteriores neste tópico usam corrotinas, mas, para conveniência, os exemplos do aplicativo de console também continuam usando a chamada de função **get** de bloqueio logo antes de sair, de modo que o aplicativo não saia antes de concluir a impressão da saída. Você não fará isso (chame a função **get** de bloqueio) em um thread da IU. Em vez disso, você usará a instrução `co_await`. As técnicas que você usará em seus aplicativos de interface do usuário são descritas no tópico [Simultaneidade e assincronia mais avançadas](concurrency-2.md).

Este tópico introdutório mostra algumas das maneiras como você pode criar e consumir objetos assíncronos do Windows Runtime com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Depois de ler este tópico, especialmente para as técnicas que você usará em seus aplicativos de interface do usuário, confira também [Simultaneidade e assincronia mais avançadas](concurrency-2.md).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operações assíncronas e funções "Async" do Windows Runtime

Qualquer API do Windows Runtime que tem o potencial de demorar mais de 50 milissegundos para concluir é implementada como uma função assíncrona (com um nome terminado em "Async"). A implementação de uma função assíncrona inicia o trabalho em outro thread e é retornada imediatamente com um objeto que representa a operação assíncrona. Quando a operação assíncrona for concluída, aquele objeto retornado contém qualquer valor que resultou do trabalho. O namespace do Windows Runtime **Windows::Foundation** contém quatro tipos de objeto de operação assíncrona.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) e
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada um desses tipos de operação assíncrona é projetado para um tipo correspondente no namespace C++/WinRT do **winrt::Windows::Foundation**. C++/WinRT também contém um struct de adaptador de espera interno. Você não o usa diretamente, mas, graças a esse struct, é possível escrever uma instrução `co_await` para aguardar de maneira cooperativa o resultado de qualquer função que retorne um desses tipos de operação assíncrona. Você pode criar suas próprias rotinas concomitantes que retornam esses tipos.

Um exemplo de uma função do Windows assíncrona é [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que retorna um objeto de operação assíncrono do tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Vejamos algumas maneiras de&mdash;primeiro bloquear, depois desbloquear&mdash;usando o C++/WinRT para chamar uma API assim. Apenas para ilustrar as ideias básicas, usaremos um projeto de **Aplicativo de Console do Windows (C++/WinRT)** nos próximos exemplos de código. As técnicas mais apropriadas para um aplicativo de interface do usuário são discutidas [Simultaneidade e assincronia mais avançadas](concurrency-2.md).

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

Como você pode ver, no exemplo de código acima, continuamos usando a chamada de função **get** de bloqueio imediatamente antes do **main** de saída. Mas isso é apenas para que o aplicativo não saia antes de concluir a impressão da saída.

## <a name="asynchronously-return-a-windows-runtime-type"></a>O modo assíncrono retorna um tipo do Windows Runtime

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

Se estiver retornando assincronamente um tipo Windows Runtime, será possível retornar um [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) ou [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Qualquer classe de tempo de execução primária ou de terceiros está qualificada, ou qualquer tipo que possa ser passado de ou para uma função do Windows Runtime (por exemplo, `int` ou **winrt::hstring**). O compilador ajudará com um erro "*precisa ser tipo WinRT*" se você tentar usar um desses tipos de operação assíncrona com um tipo que não seja do Windows Runtime.

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

## <a name="asynchronously-return-a-non-windows-runtime-type"></a>O modo assíncrono não retorna um tipo do Windows Runtime

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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

Em uma corrotina, a execução é síncrona até o primeiro ponto de suspensão, no qual o controle é retornado para o autor da chamada e o quadro de chamada sai do escopo. Quando a corrotina é retomada, qualquer coisa pode ter acontecido com o valor de origem que faz referência a um parâmetro de referência. Da perspectiva da corrotina, um parâmetro de referência tem um tempo de vida não controlado. Por isso, no exemplo acima, podemos acessar o *valor* até o `co_await`, mas não depois. Caso o *valor* seja destruído pelo chamador, tentar acessá-lo dentro da corrotina depois disso resultará em uma corrupção de memória. Nem poderemos passar com segurança o *valor* para **DoOtherWorkAsync** se houver qualquer risco de que a função seja suspensa e, em seguida, tente usar o *valor* após ser retomada.

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

## <a name="important-apis"></a>APIs importantes
* [concurrency::task class](/cpp/parallel/concrt/reference/task-class)
* [Interface IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interface IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [Interface IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [Interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Classe SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>Tópicos relacionados
* [Simultaneidade e assincronia mais avançadas](concurrency-2.md)
* [Manipular eventos usando delegados em C++/WinRT](handle-events.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)