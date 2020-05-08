---
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Enviar um item de trabalho ao pool de threads
description: Aprenda a trabalhar em um thread separado enviando um item de trabalho ao pool de threads.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, threads, pool de threads
ms.localizationpriority: medium
ms.openlocfilehash: a9da63e05380987d69d97a74123e593acd0b8cb1
ms.sourcegitcommit: 2dbf4a3f3473c1d3a0ad988bcbae6e75dfee3640
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82619340"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>Enviar um item de trabalho ao pool de threads

\[Atualizado para aplicativos UWP no Windows 10. Para artigos sobre o Windows 8. x, consulte o [arquivo](https://docs.microsoft.com/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

<b>APIs importantes</b>

-   [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync)
-   [**IAsyncAction**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction)

Aprenda a trabalhar em um thread separado enviando um item de trabalho ao pool de threads. Use esta opção para manter uma interface de usuário responsiva enquanto realiza um trabalho que leva uma quantidade considerável de tempo e use-a para concluir várias tarefas em paralelo.

## <a name="create-and-submit-the-work-item"></a>Criar e enviar um item de trabalho

Crie um item de trabalho chamando [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync). Forneça um representante para fazer o trabalho (você pode usar um lambda ou uma função de representante). Observe que **RunAsync** retorna um objeto [**IAsyncAction**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncAction); armazene esse objeto para uso na próxima etapa.

Três versões de [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync) estão disponíveis para que, opcionalmente, você possa especificar a prioridade do item de trabalho e controlar se ele é executado simultaneamente com outros itens de trabalho.

>[!NOTE]
>Use [**CoreDispatcher. RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para acessar o thread da interface do usuário e mostrar o progresso do item de trabalho.

O exemplo a seguir cria um item de trabalho e fornece um lambda para fazer o trabalho:

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = std::make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new Windows::System::Threading::WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

Após a chamada ao [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpool.runasync), o item de trabalho é enfileirado pelo pool de threads e é executado quando um thread está disponível. Os itens de trabalho do pool de threads são executados de forma assíncrona e podem ser executados em qualquer ordem, então verifique se seus itens de trabalho funcionam de forma independente.

Observe que o item de trabalho verifica a propriedade [**IAsyncInfo.Status**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncinfo.status) e sai se o item de trabalho for cancelado.

## <a name="handle-work-item-completion"></a>Manipular a conclusão de item de trabalho

Forneça um manipulador de conclusão definindo a propriedade [**IAsyncAction.Completed**](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncaction.completed) do item de trabalho. Forneça um representante (você pode usar um lambda ou uma função de representante) para lidar com a conclusão do item de trabalho. Por exemplo, use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para acessar o thread de interface do usuário e mostrar o resultado.

O seguinte exemplo atualiza a interface do usuário com o resultado do item de trabalho enviado na etapa 1:

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Observe que o manipulador de conclusão verifica se o item de trabalho foi cancelado antes de expedir uma atualização de interface do usuário.

## <a name="summary-and-next-steps"></a>Resumo e próximas etapas

Você pode saber mais baixando o código deste guia de início rápido no [exemplo criando um item de trabalho ThreadPool](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) escrito para Windows 8.1 e reutilizando o código-fonte em um\_aplicativo Win UNAP Windows 10.

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar um item de trabalho ao pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Práticas recomendadas para usar o pool de threads](best-practices-for-using-the-thread-pool.md)
* [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md)
 
