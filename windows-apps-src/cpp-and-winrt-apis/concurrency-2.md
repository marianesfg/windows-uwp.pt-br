---
description: Cenários mais avançados com simultaneidade e assincronia no C++/WinRT.
title: Simultaneidade e assincronia mais avançadas com o C++/WinRT
ms.date: 07/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, concorrência, async, assíncrono, assincronia
ms.localizationpriority: medium
ms.openlocfilehash: 4a671a319be49e07d3a8fcdacb569c4ae76e299b
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853363"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>Simultaneidade e assincronia mais avançadas com o C++/WinRT

Este tópico descreve cenários mais avançados com simultaneidade e assincronia no C++/WinRT.

Para obter uma introdução sobre esse assunto, primeiro [leia simultaneidade e operações assíncronas](concurrency.md).

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>Um aprofundamento no **winrt::resume_foreground**

Do [C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20) em diante, a função [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) será suspensa mesmo se for chamada do thread do dispatcher (em versões anteriores, ela poderia introduzir deadlocks em alguns cenários porque ela só era suspensa se ainda não estivesse no thread do dispatcher).

O comportamento atual significa que você pode depender do desenrolamento e do re-enfileiramento da pilha, e isso é importante para a estabilidade do sistema, principalmente em código de sistemas de baixo nível. A última listagem de código na seção [Programação pensada para afinidade de threads](#programming-with-thread-affinity-in-mind), acima, ilustra a realização de um pouco de cálculo complexo em um thread em segundo plano e a alternância para o thread de interface do usuário adequado a fim de atualizar a interface do usuário.

Veja a aparência do **winrt::resume_foreground** internamente.

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

Este comportamento atual, em relação ao anterior, é semelhante à diferença entre [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) e [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) no desenvolvimento de aplicativos Win32. **PostMessage** enfileira o trabalho e, em seguida, desenrola a pilha sem aguardar a conclusão do trabalho. O desenrolamento de pilha pode ser essencial.

A função **winrt::resume_foreground** também dá suporte inicialmente apenas ao [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) (vinculado a um [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)), que foi introduzido antes do Windows 10. Introduzimos um dispatcher mais flexível e eficiente: o [**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Você pode criar um **DispatcherQueue** para suas próprias finalidades. Considere este aplicativo de console simples.

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

O exemplo acima cria uma fila (contida em um controlador) em um thread privado e, em seguida, passa o controlador para a corrotina. A corrotina pode usar a fila para aguardar (suspender e retomar) o thread privado. Outro uso comum de **DispatcherQueue** é criar uma fila no thread da interface do usuário atual para um aplicativo tradicional de desktop ou Win32.

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

Isso ilustra como você pode chamar e incorporar funções do Win32 em seus projetos C++/WinRT, simplesmente chamando a função [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) no estilo Win32 para criar o controlador e, em seguida, transferir a propriedade do controlador de fila resultante para o chamador como um objeto WinRT. Isso também é precisamente como você pode dar suporte ao enfileiramento eficiente e contínuo em seu aplicativo de desktop Win32 no estilo Petzold existente.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

Acima, a função **principal** simples é iniciada criando uma janela. Você pode imaginar que isso registra uma classe de janela e chama [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) para criar a janela da área de trabalho de nível superior. A função **CreateDispatcherQueueController** é, então, chamada para criar o controlador de fila antes de chamar alguma corrotina com a fila do dispatcher pertencente a esse controlador. Uma bomba de mensagem tradicional é inserida, na qual a retomada da corrotina ocorre naturalmente nesse thread. Tendo feito isso, você pode retornar ao mundo elegante de corrotinas para seu fluxo de trabalho baseado em mensagem ou assíncrono em seu aplicativo.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

A chamada para **winrt::resume_foreground** sempre *enfileirará* e desenrolará a pilha. Também é possível definir a prioridade de retomada.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

Ou usar a ordem de enfileiramento padrão.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

Ou, nesse caso, detectar o desligamento da fila e manipulá-la normalmente.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

A expressão `co_await` retorna `true`, indicando que a retomada ocorrerá no thread do dispatcher. Em outras palavras, esse enfileiramento foi bem-sucedido. Por outro lado, ele retorna `false` para indicar que a execução permanece no thread de chamada porque o controlador da fila está sendo desligado e não está mais atendendo solicitações de fila.

Portanto, você tem muita potência a seu alcance quando combina o C++/WinRT com corrotinas e, principalmente, ao realizar um desenvolvimento de aplicativo de desktop no estilo Petzold tradicional.

## <a name="canceling-an-asynchronous-operation-and-cancellation-callbacks"></a>Retornos de chamada de cancelamento e cancelar uma operação assíncrona

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

O primeiro argumento (o *remetente*) fica sem nome, porque nunca o usamos. Por esse motivo, é seguro deixá-lo como referência. Mas observe que *args* é passado por valor. Consulte a seção [Passagem de parâmetros](concurrency.md#parameter-passing) acima.

## <a name="awaiting-a-kernel-handle"></a>Como aguardar um identificador de kernel

O C++/WinRT fornece uma classe **resume_on_signal**, que pode ser usada para suspensão até que um evento de kernel seja sinalizado. Você é responsável por garantir que o identificador permaneça válido até o retorno de `co_await resume_on_signal(h)`. **resume_on_signal** por si só não pode fazer isso por você, porque você pode ter perdido o identificador mesmo antes de **resume_on_signal** ser iniciado, como neste primeiro exemplo.

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

O **HANDLE** de entrada é válido somente até a função retornar e essa função (que é uma corrotina) é retornada no primeiro ponto de suspensão (o primeiro `co_await`, nesse caso). Enquanto aguardava **DoWorkAsync**, o controle foi retornado ao autor da chamada, o quadro de chamada saiu do escopo e você não sabe mais se o identificador será válido quando a corrotina for retomada.

Tecnicamente, nossa corrotina está recebendo seus parâmetros por valor, como deveria (confira [Passagem de parâmetro](concurrency.md#parameter-passing) acima). Mas, nesse caso, precisamos avançar um pouco para que estejamos seguindo o *espírito* dessas diretrizes (em vez de apenas a letra). Precisamos passar uma referência forte (em outras palavras, propriedade) junto com o identificador. Veja aqui como fazer isso.

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

Passar um [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) por valor fornece uma semântica de propriedade, o que garante que o identificador do kernel permaneça válido durante o tempo de vida da corrotina.

Veja como você pode chamar essa corrotina.

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="asynchronous-timeouts-made-easy"></a>Tempos limite assíncronos simplificados

O C++/WinRT é investido pesadamente em corrotinas do C++. Seu efeito na gravação de código de simultaneidade é transformativo. Esta seção aborda casos em que os detalhes de assincronia não são importantes, e tudo o que você deseja é obter o resultado rapidamente. Por esse motivo, a implementação do C++/WinRT da interface da operação assíncrona [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) do Windows Runtime tem uma função **get**, semelhante àquela fornecida por **std::function**.

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

A função **get** é bloqueada indefinidamente, enquanto o objeto async é concluído. Objetos async tendem a ter uma vida curta; portanto, geralmente isso é tudo de que você precisa.

Mas há casos em que isso não é suficiente e você precisará abandonar a espera depois que algum tempo tiver decorrido. Escrever esse código sempre foi possível graças aos blocos de construção fornecidos pelo Windows Runtime. Mas agora o C++/WinRT torna muito mais fácil fornecer a função **wait_for**. Ela também é implementada em **IAsyncAction** e, novamente, é semelhante ao fornecido por **std::function**.

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** usa **std::chrono::duration** na interface, mas está limitado a algum intervalo menor do que o que **std::chrono::duration** oferece (aproximadamente 49,7 dias).

O **wait_for** neste próximo exemplo aguarda cerca de cinco segundos e, depois, verifica a conclusão. Se a comparação for favorável, você saberá que o objeto async foi concluído com êxito e pronto. Se você estiver aguardando algum resultado, poderá simplesmente seguir isso com uma chamada para a função **get** para recuperar o resultado.

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

Como o objeto async foi concluído por enquanto, **get** retorna o resultado imediatamente, sem nenhuma espera adicional. Como você pode ver, **wait_for** retorna o estado do objeto async. Portanto, você pode usá-lo para ter um controle mais refinado, como este.

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- Lembre-se de que **AsyncStatus::Completed** significa que o objeto async foi concluído com êxito e que você pode chamar a função **get** para recuperar qualquer resultado.
- **AsyncStatus::Canceled** significa que o objeto async foi cancelado. Um cancelamento normalmente é solicitado pelo chamador; portanto, seria raro lidar com esse estado. Normalmente, um objeto async cancelado é simplesmente descartado.
- **AsyncStatus::Error** significa que o objeto async falhou de alguma maneira. Você poderá chamar **get** para gerar novamente a exceção se desejar.
- **AsyncStatus::Started** significa que o objeto async ainda está em execução. O padrão assíncrono do Windows Runtime não permite várias esperas nem waiters. Isso significa que você não pode chamar **wait_for** em um loop. Se a espera tiver atingido efetivamente o tempo limite, restarão algumas opções para você. Você pode abandonar o objeto ou pode sondar seu status antes de chamar **get** para recuperar qualquer resultado. Mas é melhor descartar o objeto nesse momento.

## <a name="returning-an-array-asynchronously"></a>Retornar uma matriz de forma assíncrona

Veja abaixo um exemplo de [MIDL 3.0](/uwp/midl-3/) que produz o *erro MIDL2025: [msg]erro de sintaxe [contexto]: esperando > ou próximo de "["* .

```idl
Windows.Foundation.IAsyncOperation<Int32[]> RetrieveArrayAsync();
```

O motivo é que é inválido usar uma matriz como um argumento de tipo de parâmetro para uma interface com parâmetros. Portanto, é necessária uma maneira menos óbvia para atingir o objetivo de passar assincronamente uma matriz de volta de um método de classe de runtime. 

Você pode retornar a matriz demarcada em um objeto [PropertyValue](/uwp/api/windows.foundation.propertyvalue). O código de chamada cancela sua demarcação. Veja um exemplo de código, que você pode experimentar adicionando a classe de tempo de execução **SampleComponent** a um projeto do **Componente do Windows Runtime (C++/WinRT)** e, em seguida, consumindo (por exemplo) um projeto do **Aplicativo de Núcleo (C++/WinRT)** .

```cppwinrt
// SampleComponent.idl
namespace MyComponentProject
{
    runtimeclass SampleComponent
    {
        Windows.Foundation.IAsyncOperation<IInspectable> RetrieveCollectionAsync();
    };
}

// SampleComponent.h
...
struct SampleComponent : SampleComponentT<SampleComponent>
{
    ...
    Windows::Foundation::IAsyncOperation<Windows::Foundation::IInspectable> RetrieveCollectionAsync()
    {
        co_return Windows::Foundation::PropertyValue::CreateInt32Array({ 99, 101 }); // Box an array into a PropertyValue.
    }
}
...

// SampleCoreApp.cpp
...
MyComponentProject::SampleComponent m_sample_component;
...
auto boxed_array{ co_await m_sample_component.RetrieveCollectionAsync() };
auto property_value{ boxed_array.as<winrt::Windows::Foundation::IPropertyValue>() };
winrt::com_array<int32_t> my_array;
property_value.GetInt32Array(my_array); // Unbox back into an array.
...
```

## <a name="important-apis"></a>APIs importantes
* [Interface IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interface IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [Interface IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [Interface IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>Tópicos relacionados
* [Simultaneidade e operações assíncronas](concurrency.md)
* [Manipular eventos usando delegados em C++/WinRT](handle-events.md)