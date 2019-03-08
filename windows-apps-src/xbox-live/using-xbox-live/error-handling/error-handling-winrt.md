---
title: Tratamento de erros da API WinRT
description: Saiba como tratar erros ao fazer uma chamada de serviço Xbox Live com as APIs do WinRT.
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, tratamento de erros
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598631"
---
# <a name="winrt-api-error-handling"></a>Tratamento de erros da API WinRT

Na API do WinRT, que pode ser chamado do C + + c++ /CX, C#, ou Javascript, erros são relatados usando exceções, blocos try/catch devem ser usado para tratar erros.

Observe que o para as chamadas assíncronas, dois blocos de try/catch são necessários como este:

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

Os métodos assíncronos XSAPI ter algum código que é executado de forma síncrona quando um método é chamado.  O código síncrono funcionam como validar argumentos de entrada e iniciar as operações assíncronas ou ações.  Portanto, até mesmo chamar os métodos assíncronos pode resultar em uma exceção – Embora isso não deve ocorrer para cenários normais (ou seja, erro do programador, não o erro de rede).  Esteja consciente dessa possibilidade e o programa de forma defensiva, validação de entrada e testar seu código completamente durante o desenvolvimento.  Se você colocar um try/catch em torno da chamada em si ou colocar um nível mais alto na pilha de chamadas, o importante é ter um.

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>Ajuda! Meu jogo falha quando eu chamar a API de serviço do XYZ Xbox

Portanto, falhas do seu jogo e a pilha de chamadas diz que ele é uma chamada à API de serviços do Xbox, mas faz isso?

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

Essas pilhas de chamadas são bastante confusas.  Simultaneidade, simultaneidade que...  Ah, Microsoft::Xbox::Services::Social::XboxUserProfile está na pilha de chamadas, portanto, que deve ser o culpado.  Na realidade, isso é uma pilha de chamadas gerada por uma chamada para GetUserProfileAsync para uma ID de usuário Xbox inválido.  O código de exemplo que gerou essa pilha de chamadas tem esta aparência:

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

Sua pilha de chamadas contêm Concurrency::_ReportUnobservedException()?  Examine a pilha de chamadas acima ao outro.  Isso é importante.  Se sua pilha de chamadas contiver Concurrency:: _ReportUnobservedException() terá encontrado um bug no código do jogo.

PPL cria tarefas, que podem ser seguidas por outras tarefas.  No exemplo acima, create_task() cria a tarefa para chamar GetUserProfileAsync() e o .then() cria a seguinte tarefa.  Geral, eles são conhecidos como a tarefa antecedente (primeiro uma) e a continuação (segundo).  No exemplo, a continuação está faltando o tratamento de erros.  O tempo de execução encerra o aplicativo se uma tarefa gera uma exceção, e essa exceção não é capturada pela tarefa ou uma de suas continuações.

Quando se trata de tarefas de continuação, observe que há realmente dois tipos diferentes.  Um tipo, a continuação baseada em tarefas, usa a tarefa anterior como o argumento de entrada.  Essa tarefa sempre é executado, mesmo se a tarefa antecedente gera uma exceção.  Para obter o resultado da tarefa antecedente, você deve chamar .get() no argumento.  O segundo, com base em valor, recebe a saída da tarefa anterior é realmente açúcar sintático, exceto por um motivo diretamente – – continuações de valor com base não são executadas em todos os se o antecessor gerará uma exceção.

Recomendação:  Para evitar falhas, use uma continuação baseada em tarefa no final da sua cadeia de continuação e coloque todos os concurrency:: task::get() ou simultaneidade:: task::wait() chama em blocos try/catch para tratar erros que podem ser recuperados.

Vamos examinar alguns exemplos:

Exemplo de continuação baseada em tarefa

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

Exemplo de continuação baseada em valor

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

Há uma terceira solução – usar continuações com base no valor completamente, mas chamar .get() ou .wait() em outro thread e capturar a exceção lá.  Aqui está um exemplo simples:

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**Se você não usar PPL** se você estiver usando AsyncOperationCompletionHandler ou AsyncActionCompletionHandler em vez de PPL, você também tem alguns tratamentos de erro caso seja realizado incorretamente, resultará em falhas do aplicativo.  Abaixo está um exemplo que mostra como tratar erros

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**Exceções e GetNextAsync()** usando a APIs de paginação?  Os objetos AchievementResult, LeaderboardResult, InventoryItemResult e TitleStorageBlobMetadataResult contêm um método GetNextAsync() para solicitar a próxima página de resultados.  Há um caso especial, quando não há mais dados disponíveis, que dispara uma exceção ao chamar GetNextAsync().  Essa exceção é lançada durante a execução síncrona de GetNextAsync().  Nesse caso, o método GetNextAsync lança INET_E_DATA_NOT_AVAILABLE (0x800C0007).

Recomendação: Certifique-se de que você encapsule GetNextAsync() método chama em um bloco try/catch e tratar normalmente o caso INET_E_DATA_NOT_AVAILABLE.

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
