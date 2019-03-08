---
title: Padrões assíncronos de chamada C API
description: Aprender sobre a API de C assíncrona chamando padrões para a API de C XSAPI
ms.date: 06/10/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o programa de desenvolvedores
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608001"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>Padrão de chamada para XSAPI simples chamadas de async de camada de C

Uma **API assíncrona** é uma API que retorna rapidamente, mas será iniciado um **tarefa assíncrona** e o resultado é retornado depois que a tarefa for concluída.

Tradicionalmente jogos têm pouco controle sobre qual thread executa o **tarefa assíncrona** e o segmento que retorna os resultados ao usar um **retorno de chamada de conclusão**.  Alguns jogos são projetados para que uma seção do heap somente é atingida por um único thread para evitar a necessidade de sincronização de thread. Se o **retorno de chamada de conclusão** não é chamado de um thread, o jogo controles, atualizando o estado compartilhado com o resultado de uma **tarefa assíncrona** exigirá a sincronização de thread.

A API de C XSAPI expõe uma nova API de C assíncrona que fornece aos desenvolvedores controle de thread direto ao fazer uma **API assíncrona** chamar, como **XblSocialGetSocialRelationshipsAsync()**,  **XblProfileGetUserProfileAsync()** e **XblAchievementsGetAchievementsForTitleIdAsync()**.

Aqui está um exemplo básico de chamada a **XblProfileGetUserProfileAsync** API.

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

Para entender esse padrão de chamada, você precisará entender como usar o **AsyncBlock** e o **AsyncQueue**.

* O **AsyncBlock** executa todas as informações referentes à **tarefa assíncrona** e **retorno de chamada de conclusão**.

* O **AsyncQueue** permite que você determine qual thread executa o **tarefa assíncrona** e o segmento que chama o AsyncBlock **retorno de chamada de conclusão**.

## <a name="the-asyncblock"></a>O **AsyncBlock**

Vamos dar uma olhada a **AsyncBlock** em detalhes. É um struct definido da seguinte maneira:

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

O **AsyncBlock** contém:

* *retorno de chamada* -uma função de retorno de chamada opcional que será chamada depois que o trabalho assíncrono foi feito.  Se você não especificar um retorno de chamada, você pode aguardar os **AsyncBlock** para ser concluído com **GetAsyncStatus** e, em seguida, obtenha os resultados.
* *contexto* -permite que você passe dados para a função de retorno de chamada.
* *fila* - uma async_queue_handle_t que é um identificador que designa um **AsyncQueue**. Se isso não for definido, uma fila padrão será usada.

Você deve criar um novo AsyncBlock no heap para cada async é chamada à API.  O AsyncBlock deve residir até que o retorno de chamada de conclusão do AsyncBlock é chamado e, em seguida, ele pode ser excluído.

> [!IMPORTANT]
> Uma **AsyncBlock** devem permanecer na memória até que o **tarefa assíncrona** é concluída. Se ele é alocado dinamicamente, ele poderá ser excluído dentro do AsyncBlock **retorno de chamada de conclusão**.

### <a name="waiting-for-asynchronous-task"></a>Aguardando **tarefa assíncrona**

Você pode informar um **tarefa assíncrona** é realizar um número de maneiras diferentes:

* o AsyncBlock **retorno de chamada de conclusão** é chamado
* Chame **GetAsyncStatus** por true para aguardar até que ela seja concluída.
* Definir um waitEvent na **AsyncBlock** e aguarde até que o evento seja sinalizado

Com o **GetAsyncStatus** e waitEvent, o **tarefa assíncrona** será considerada concluída após o AsyncBlock **retorno de chamada de conclusão** executa, no entanto o AsyncBlock **retorno de chamada de conclusão** é opcional.

Uma vez a **tarefa assíncrona** é concluída, você pode obter os resultados.

### <a name="getting-the-result-of-the-asynchronous-task"></a>Obtendo o resultado do **tarefa assíncrona**

Para obter o resultado, a maioria **API assíncrona** funções têm um correspondente \[nome da função\]resultar de função para receber o resultado da chamada assíncrona. Em nosso código de exemplo, **XblProfileGetUserProfileAsync** tem um correspondente **XblProfileGetUserProfileResult** função. Você pode usar essa função para retornar o resultado da função e aja de forma adequada.  Consulte a documentação de cada **API assíncrona** função para obter detalhes completos sobre como recuperar os resultados.

## <a name="the-asyncqueue"></a>O **AsyncQueue**

O **AsyncQueue** permite que você determine qual thread executa o **tarefa assíncrona** e o segmento que chama o AsyncBlock **retorno de chamada de conclusão**.

Você pode controlar qual thread executa essas operações, definindo uma *modo de expedição*. Há três modos de expedição disponíveis:

* *Manual* -fila manual não são automaticamente enviados.  Cabe ao desenvolvedor distribuí-los em qualquer thread que desejarem. Isso pode ser usado para atribuir o trabalho ou o retorno de chamada lado de uma chamada assíncrona para um thread específico.  Isso é discutido em mais detalhes abaixo.

* *Pool de threads* - expede usando um pool de threads.  O pool de threads invoca as chamadas em paralelo, levando a uma chamada para executar da fila por sua vez, como os threads do threadpool se tornam disponíveis.  Isso é o administrador para usar, mas oferece a menor quantidade de controle sobre o qual o thread é usado.

* *Corrigido o Thread* - expede usando QueueUserAPC no thread que criou a fila de async. Quando uma APC de modo de usuário está na fila, o thread não é direcionado para chamar a função APC, a menos que ele está em um estado de alerta. Um thread entra em um estado de alerta por meio **SleepEx**, **SignalObjectAndWait**, **WaitForSingleObjectEx**, **WaitForMultipleObjectsEx**, ou **MsgWaitForMultipleObjectsEx** para executar uma operação de espera alertável

Para criar um novo **AsyncQueue** você precisará chamar **CreateAsyncQueue**.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

onde AsyncQueueDispatchMode contém os modos de três expedição apresentados anteriormente:

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode** determina o modo de expedição para o thread que manipula o trabalho assíncrono, enquanto **completionDispatchMode** determina o modo de expedição para o thread que lida com a conclusão de async operação.

Você também pode chamar **CreateSharedAsyncQueue** para criar um **AsyncQueue** com o mesmo tipo de fila, especificando uma ID para a fila.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> Se já houver uma fila com essa ID e a expedição modos, ele será referenciado.  Caso contrário, será criada uma nova fila.

Uma vez que você criou seu **AsyncQueue** simplesmente adicioná-lo para o **AsyncBlock** para controlar o threading em suas funções de trabalho e a conclusão.
Quando tiver terminado de usar o **AsyncQueue**, normalmente quando o jogo está terminando, você pode fechá-lo com **CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>Expedição manualmente um **AsyncQueue**

Se você tiver usado o modo de expedição de fila manual para um **AsyncQueue** fila de trabalho ou a conclusão, você precisará de expedição manualmente.
Digamos que um **AsyncQueue** foi criado em que a fila de trabalho e a fila de conclusão são definidos para o qual expedir manualmente da seguinte forma:

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

Para expedir trabalhos que tem recebido **AsyncQueueDispatchMode_Manual** você precisará enviá-la com o **DispatchAsyncQueue** função.

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *fila* -qual fila para expedir trabalhos em
* *tipo de* - uma instância das **AsyncQueueCallbackType** enum
* *timeoutInMs* -um uint32_t para o tempo limite em milissegundos.

Há dois tipos de retorno de chamada definidos pelos **AsyncQueueCallbackType** enum:

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>Quando chamar **DispatchAsyncQueue**

Para verificar quando a fila recebeu um novo item você pode chamar **AddAsyncQueueCallbackSubmitted** para definir um manipulador de eventos para informar o código que o trabalho ou preenchimentos estão prontos para ser expedida.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** usa os seguintes parâmetros:

* *fila* -fila assíncrona estiver enviando o retorno de chamada para.
* *contexto* -um ponteiro para dados que devem ser passados para o retorno de chamada de envio.
* *retorno de chamada* -a função que será invocada quando um retorno de chamada de novo for enviado para a fila.
* *token* -um token que será usado em uma chamada posterior a **RemoveAsynceCallbackSubmitted** para remover o retorno de chamada.

Por exemplo, aqui está uma chamada para **AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

O correspondente **AsyncQueueCallbackSubmitted** retorno de chamada pode ser implementado da seguinte maneira:

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

Em seguida, em um thread em segundo plano, você pode ouvir para esse sinal despertar e chamar **DispatchAsyncQueue**.

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

É melhor prática usar implementar com o objeto de sinal Win32.  Se em vez disso, você implementar usando um objeto de evento do Win32, em seguida, você precisará garantir que não perca todos os eventos com o código como:

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


Você pode exibir um exemplo de como as práticas recomendadas para a integração do async em [AsyncIntegration.cpp Social de exemplo C](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

