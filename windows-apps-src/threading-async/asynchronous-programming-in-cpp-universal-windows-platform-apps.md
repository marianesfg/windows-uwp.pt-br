---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Este artigo descreve a maneira recomendada para consumir métodos assíncronos em extensões de componente do Visual C++ (C++/CX) usando a classe task definida no namespace concurrency em ppltasks.h.
title: Programação assíncrona em C++
ms.date: 05/14/2018
ms.topic: article
keywords: windows 10, uwp, threads, assíncrono, C++
ms.localizationpriority: medium
ms.openlocfilehash: 73268a2aab0212277fb1caa158220cb7dbc180bf
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730067"
---
# <a name="asynchronous-programming-in-ccx"></a>Programação assíncrona em C++/CX
> [!NOTE]
> Este tópico existe para ajudar você na manutenção do seu aplicativo C++/CX. Recomendamos que você use [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para novos aplicativos. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do WinRT (Windows Runtime), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows.

Este artigo descreve a maneira recomendada para consumir métodos assíncronos em extensões de componente do Visual C++ (C++/CX) usando a classe `task` definida no namespace `concurrency` em ppltasks.h.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Tipos assíncronos da Plataforma Universal do Windows (UWP)
A Plataforma Universal do Windows (UWP) apresenta um modelo bem definido para chamar métodos assíncronos e fornece os tipos necessários para consumir esses métodos. Se você não estiver familiarizado com o modelo assíncrono UWP, leia [Programação Assíncrona][AsyncProgramming] antes de continuar a ler este artigo.

Embora você possa consumir as APIs de Windows Runtime assíncronas diretamente em C++, a abordagem preferida é usar a [**classe Task**][task-class] e seus tipos e funções relacionados, que estão contidas no namespace de [**simultaneidade**][concurrencyNamespace] e definidas em `<ppltasks.h>`. A **concurrency::task** é um tipo de finalidade geral, mas quando é usado o switch do compilador **/ZW**, que é obrigatório para aplicativos e componentes Plataforma Universal do Windows (UWP), a classe task encapsula os tipos assíncronos UWP para que seja mais fácil:

-   encadear diversas operações assíncronas e síncronas

-   lidar com exceções em cadeias de tarefas

-   executar cancelamentos em cadeias de tarefas

-   assegurar que as tarefas individuais executem no apartment ou contexto do thread apropriado

Este artigo fornece orientação básica sobre como usar a classe **task** com as APIS assíncronas UWP. Para obter uma documentação mais completa sobre a **tarefa** e seus métodos relacionados, incluindo [**\_criar tarefa**][createTask], consulte [paralelismo de tarefas (tempo de execução de simultaneidade)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Consumindo uma operação assíncrona usando uma tarefa
O exemplo a seguir mostra como usar a classe task para consumir um método **async** que retorna uma interface [**IAsyncOperation**][IAsyncOperation] e cuja operação produz um valor. Veja a seguir as etapas básicas:

1.  Chame o método `create_task` e passe o objeto **IAsyncOperation^**.

2.  Chame a função de membro [**task::then**][taskThen] na tarefa e forneça um lambda que será invocado quando a operação assíncrona for concluída.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

A tarefa criada e retornada pela função [**task::then**][taskThen] é conhecida como *continuação*. O argumento de entrada (neste caso) para o lambda fornecido pelo usuário é o resultado que a operação da tarefa produz quando concluída. É o mesmo valor que seria recuperado chamando [**IAsyncOperation::GetResults**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) se você estivesse usando diretamente a interface **IAsyncOperation**.

O método [**task::then**][taskThen] retorna imediatamente, e seu delegado não é executado enquanto o trabalho assíncrono não for concluído com êxito. Neste exemplo, se a operação assíncrona causar o lançamento de uma exceção ou terminar no estado cancelado como resultado de uma solicitação de cancelamento, a continuação jamais será executada. Posteriormente, descreveremos como gravar continuações que são executadas mesmo quando a tarefa anterior é cancelada ou apresenta falha.

Apesar de você declarar a variável da tarefa na pilha local, ela gerencia seu tempo de vida para que não seja excluída até que todas as suas operações sejam concluídas e todas as referências a ela saiam do escopo, mesmo que o método seja retornado antes da conclusão das operações.

## <a name="creating-a-chain-of-tasks"></a>Criando uma cadeia de tarefas
Na programação assíncrona, é comum definir uma sequência de operações, também conhecida como *cadeias de tarefas*, nas quais cada continuação é executada somente quando a anterior for concluída. Em alguns casos, a tarefa anterior (ou *antecedente*) produz um valor que a continuação aceita como entrada. Usando o método [**task::then**][taskThen], você pode criar cadeias de tarefas de maneira intuitiva e simples; o método retorna uma **task<T>** onde **T** é o tipo de retorno da função lambda. Você pode compor várias continuações em uma cadeia de tarefas:`myTask.then(…).then(…).then(…);`

Cadeias de tarefas são especialmente úteis quando uma continuação cria uma nova operação assíncrona. Esse tipo de tarefa é conhecida como uma tarefa assíncrona. O exemplo a seguir ilustra uma cadeia de tarefas com duas continuações. A tarefa inicial adquire o identificador para um arquivo existente e, quando essa operação é concluída, a primeira continuação inicia uma nova operação assíncrona para excluir o arquivo. Quando essa operação é concluída, a segunda continuação é executada e gera uma mensagem de confirmação.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

O exemplo anterior ilustra quatro pontos importantes:

-   A primeira continuação converte o objeto [**IAsyncAction^**][IAsyncAction] em uma **tarefa<void>** e retorna a **tarefa**.

-   A segunda continuação não trata erros e, portanto, aceita **void** e não **tarefa<void>** como entrada. Ela é uma continuação baseada em valores.

-   A segunda continuação não é executada até que a operação [**DeleteAsync**][deleteAsync] seja concluída.

-   Como a segunda continuação é baseada em valores, se a operação que foi iniciada pela chamada a [**DeleteAsync**][deleteAsync] lançar uma exceção, a segunda continuação não será executada.

**Observação**  a criação de uma cadeia de tarefas é apenas uma das maneiras de usar a classe **Task** para compor operações assíncronas. Você também pode compor operações usando operadores **&&** de junção e escolha e **||**. Para obter mais informações, consulte [Paralelismo de tarefas (Tempo de Execução de Simultaneidade)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Tipos de retorno da função lambda e tipos de retorno de tarefas
Em uma continuação de tarefa, o tipo de retorno da função lambda é encapsulado em um objeto **task**. Se lambda retornar um **double**, então, o tipo de tarefa de continuação será **task<double>**. No entanto, o objeto task foi desenvolvido para que não produza tipos de retorno desnecessariamente aninhados. Se uma lambda retorna um **IAsyncOperation<SyndicationFeed^>^**, a continuação retorna um **task<SyndicationFeed^>** e não um **task<task<SyndicationFeed^>>** ou **task<IAsyncOperation<SyndicationFeed^>^>^**. Esse processo é conhecido como *não encapsulamento assíncrono* e assegura que a operação assíncrona dentro da continuação seja concluída antes que a próxima continuação seja invocada.

No exemplo anterior, observe que a tarefa retorna uma **tarefa<void>** apesar de seu lambda ter retornado um objeto [**IAsyncInfo**][IAsyncInfo]. A tabela a seguir resume as conversões de tipo que ocorrem entre uma função lambda e a tarefa que a delimita:

| | |
|--------------------------------------------------------|---------------------|
| tipo de retorno de lambda                                     | `.then`tipo de retorno |
| TResult                                                | agenda<TResult> |
| IAsyncOperation<TResult>^                        | agenda<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | agenda<TResult> |
|IAsyncAction^                                           | agenda<void>    |
| IAsyncActionWithProgress<TProgress>^             |agenda<void>     |
| agenda<TResult>                                    |agenda<TResult>  |


## <a name="canceling-tasks"></a>Cancelando tarefas
É uma boa ideia dar ao usuário a opção de cancelar uma operação assíncrona. Em alguns casos, talvez seja necessário cancelar uma operação programaticamente de fora da cadeia da tarefa. Embora cada \*tipo de retorno **assíncrono** tenha um método [**Cancel**][IAsyncInfoCancel] que ele herde de [**IAsyncInfo**][IAsyncInfo], é estranho expô-lo a métodos externos. A maneira preferida de dar suporte ao cancelamento em uma cadeia de tarefas é usar uma [**origem\_de\_token de cancelamento**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class) para criar um [**\_token de cancelamento**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-class)e, em seguida, passar o token para o construtor da tarefa inicial. Se uma tarefa assíncrona for criada com um token de cancelamento e a [**origem\_do\_token de cancelamento:: Cancel**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017) for chamada, a tarefa chamará o **cancelamento** automaticamente na operação **IAsync\* ** e passará a solicitação de cancelamento para baixo em sua cadeia de continuação. O pseudocódigo a seguir demonstra a abordagem básica.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

Quando uma tarefa é cancelada, uma exceção de [**tarefa\_cancelada**][taskCanceled] é propagada para baixo na cadeia de tarefas. As continuações baseadas em valores simplesmente não executam, mas as continuações baseadas em tarefas fazem com que a exceção seja lançada quando [**task::get**][taskGet] é chamado. Se você tiver uma continuação de tratamento de erros, verifique se ela captura a exceção **de\_tarefa cancelada** explicitamente. (Essa exceção não é derivada de [**Platform::Exception**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class).)

O cancelamento é cooperativo. Se a continuação realiza algum trabalho de execução demorada além de apenas invocar um método UWP, então, é sua responsabilidade verificar o estado do token de cancelamento periodicamente e parar a execução caso ele seja cancelado. Depois de limpar todos os recursos que foram alocados na continuação, chame [**Cancelar\_tarefa\_atual**](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) para cancelar essa tarefa e propagar o cancelamento para qualquer continuação baseada em valor que o siga. Veja outro exemplo: você pode criar uma cadeia de tarefa que representa o resultado de uma operação [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker). Se o usuário escolhe o botão **Cancel**, o método [**IAsyncInfo::Cancel**][IAsyncInfoCancel] não é chamado. Em vez disso, a operação tem êxito mas retorna **nullptr**. A continuação pode testar o parâmetro de entrada e **chamar\_cancelar\_tarefa atual** se a entrada for **nullptr**.

Para obter mais informações, consulte [Cancellation in the PPL](https://docs.microsoft.com/cpp/parallel/concrt/cancellation-in-the-ppl)

## <a name="handling-errors-in-a-task-chain"></a>Manipulando erros em uma cadeia de tarefa
Se você deseja que uma continuação seja executada mesmo que a antecedente tenha sido cancelada ou tenha lançado uma exceção, torne-a uma continuação baseada em tarefa especificando a entrada de sua função lambda como uma **tarefa<TResult>** ou **tarefa<void>** se a lambda da tarefa antecedente retorna uma [**IAsyncAction^**][IAsyncAction].

Para manipular erros e cancelamentos em uma cadeia de tarefa, não é necessário transformar cada baseada em tarefa ou encapsular toda operação que possa lançar em um bloco `try…catch`. Em vez disso, adicione uma continuação baseada em tarefa no fim da cadeia e manipule todos os erros nela. Qualquer exceção — isso inclui uma exceção de [**tarefa\_cancelada**][taskCanceled] – propagará a cadeia de tarefas e ignorará qualquer continuação baseada em valor, para que você possa tratá-la na continuação baseada em tarefa de tratamento de erros. Podemos recriar o exemplo anterior para usar uma continuação baseada em tarefas de tratamento de erro:

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

Em uma continuação baseada em tarefa, a função membro [**task::get**][taskGet] é chamada para obter os resultados da tarefa. Ainda é obrigatório chamar **task::get** mesmo que a operação seja uma [**IAsyncAction**][IAsyncAction] que não produz resultados porque **task::get** também obtém todas as exceções que foram transportadas adiante para a tarefa. Se a tarefa de entrada armazena uma exceção, ela será lançada na chamada de **task::get**. Se você não chamar a **tarefa:: Get**ou não usar uma continuação baseada em tarefa no final da cadeia, ou não capturar o tipo de exceção que foi lançado, uma exceção de **\_\_tarefa não observada** será lançada quando todas as referências à tarefa tiverem sido excluídas.

Capture somente as exceções que você pode manipular. Se o aplicativo encontrar um erro do qual não possa se recuperar, será melhor deixar que o aplicativo falhe em vez de deixar que ele continue a ser executado em um estado desconhecido. Além disso, em geral, não tente capturar a própria **exceção de\_tarefa\_não observada** . A finalidade principal dessa exceção é diagnóstico. Quando a **exceção de\_tarefa\_não observada** é lançada, ela geralmente indica um bug no código. A causa costuma ser uma exceção que precisa ser manipulada ou uma exceção irrecuperável causada por outro erro no código.

## <a name="managing-the-thread-context"></a>Gerenciando o contexto do thread
A interface do usuário de um app UWP é executada em um STA. Uma tarefa cujo lambda retornar [**IAsyncAction**][IAsyncAction] ou [**IAsyncOperation**][IAsyncOperation] reconhece o apartment. Se a tarefa for criada no STA, todas as suas continuações também serão executadas nele por padrão, a menos que você especifique de outra forma. Em outras palavras, a cadeia de tarefas inteira herda o reconhecimento de apartment da tarefa pai. Esse comportamento ajuda a simplificar as interações com os controles da interface do usuário, que podem ser acessados somente pelo STA.

Por exemplo, em um app UWP, na função membro de qualquer classe que representa uma página XAML, você pode preencher um controle [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) de dentro de um método [**task::then**][taskThen] sem ter que usar o objeto [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher).

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

Se uma tarefa não retorna uma [**IAsyncAction**][IAsyncAction] ou [**IAsyncOperation**][IAsyncOperation], ela não reconhece o apartment e, por padrão, suas continuações são executadas no primeiro thread em segundo plano disponível.

Você pode substituir o contexto de thread padrão por qualquer tipo de tarefa usando a sobrecarga de [**Task::,**][taskThen] que usa um [**contexto\_de\_continuação de tarefa**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class). Por exemplo, em alguns casos, pode ser desejável agendar a continuação de uma tarefa que reconhece o apartment em um thread de segundo plano. Nesse caso, você pode passar o [**contexto de\_continuação\_da tarefa::\_use arbitrário**][useArbitrary] para agendar o trabalho da tarefa no próximo thread disponível em um apartamento multi-threaded. Isso pode melhorar o desempenho da continuação, pois a sua execução não precisa ser sincronizada com as demais execuções realizadas no thread da interface do usuário.

O exemplo a seguir demonstra quando é útil especificar o [**contexto de\_continuação\_da tarefa: use\_**][useArbitrary] a opção arbitrária e também mostra como o contexto de continuação padrão é útil para sincronizar operações simultâneas em coleções sem segurança de thread. Nesse código, executamos um loop por uma lista de URLs para RSS Feeds e, para cada URL, iniciamos uma operação assíncrona para recuperar os dados de feed. Não é possível controlar a ordem na qual os feeds são recuperados, mas isso não é relevante. Quando cada operação [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync) é concluída, a primeira continuação aceita o objeto [**SyndicationFeed^**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed) e o usa para inicializar um objeto `FeedData^` definido pelo aplicativo. Como cada uma dessas operações é independente das outras, podemos, potencialmente, acelerar as coisas especificando o **contexto de\_continuação\_da tarefa::\_usar** o contexto de continuação arbitrária. No entanto, depois que cada objeto `FeedData` é inicializado, é necessário adicioná-lo a um [**Vector**](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class), que não é uma coleção não segura de threads. Portanto, criamos uma continuação e especificamos o [**contexto de continuação\_\_da\_tarefa:: Use Current**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) para garantir que todas as chamadas a [**Append**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.ivector_t_.append) ocorram no mesmo contexto de aplicativo single-threaded apartment (ASTA). Como [**o\_contexto\_de continuação da tarefa\_:: usar o padrão**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) é o contexto padrão, não precisamos especificá-lo explicitamente, mas fazemos isso aqui para fins de clareza.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

Tarefas aninhadas, que são novas tarefas criadas dentro de uma continuação, não herdam o reconhecimento de apartment da tarefa inicial.

## <a name="handing-progress-updates"></a>Manipulando atualizações de progresso
Os métodos que dão suporte a [**IAsyncOperationWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) ou [**IAsyncActionWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) fornecem atualizações de progresso periodicamente enquanto a operação está em andamento, antes de ser concluída. O relatório do progresso é independente da noção de tarefas e continuações. Basta fornecer o delegado da propriedade [**Progress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) do objeto. Um uso comum do delegado é para atualizar uma barra de progresso na interface do usuário.

## <a name="related-topics"></a>Tópicos relacionados
* [Criar operações assíncronas em C++/CX para aplicativos UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Referência da linguagem Visual C++](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)
* [Programação assíncrona][AsyncProgramming]
* [Paralelismo de tarefas (Tempo de Execução de Simultaneidade)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Namespace de simultaneidade"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Classe de tarefa"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "Paralelismo de Tarefas"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
