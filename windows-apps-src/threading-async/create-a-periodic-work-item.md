---
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Criar um item de trabalho periódico
description: Saiba como criar um item de trabalho periódico que se repete periodicamente.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, item de trabalho periódico, threading, temporizadores
ms.localizationpriority: medium
ms.openlocfilehash: 8e045c12f96cc9404abb4ba9be395eb49b1ab850
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321995"
---
# <a name="create-a-periodic-work-item"></a>Criar um item de trabalho periódico


<b>APIs importantes</b>

-   [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer)
-   [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)

Saiba como criar um item de trabalho periódico que se repete periodicamente.

## <a name="create-the-periodic-work-item"></a>Criar o item de trabalho periódico

Use o método [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) para criar um item de trabalho periódico. Forneça um lambda que realize o trabalho e use o parâmetro *period* para especificar o intervalo entre os envios. O período é especificado usando uma estrutura [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan). O item de trabalho será reenviado sempre que o período acabar, assim, verifique se o período é longo o suficiente para o trabalho ser concluído.

[**CreateTimer** ](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createtimer) retorna um [ **ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) objeto. Armazene esse objeto no caso de o temporizador ter que ser cancelado.

> **Observação**  Evite especificar um valor de zero (ou qualquer valor menor que um milissegundo) para o intervalo. Isso faz com que o temporizador periódico se comporte como um temporizador de disparo único.

> **Observação**  você pode usar [ **CoreDispatcher.RunAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) para acessar a interface do usuário e mostrar o progresso do item de trabalho.

O seguinte exemplo cria um item de trabalho que é executado a cada 60 segundos:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>                         
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>Manipular o cancelamento do item de trabalho periódico (opcional)

Se necessário, você pode manipular o cancelamento do temporizador periódico com um [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler). Use a sobrecarga [**CreatePeriodicTimer**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.createperiodictimer) para fornecer um lambda adicional que manipula o cancelamento do item de trabalho periódico.

O exemplo a seguir cria um item de trabalho periódico que se repete a cada 60 segundos, e também fornece um manipulador de cancelamento:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>                 
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Cancelar o timer

Quando necessário, chame o método [**Cancel**](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer.cancel) para impedir que o item de trabalho periódico se repita. Se o item de trabalho estiver sendo executado quando o temporizador periódico for cancelado, ele poderá ser concluído. O [**TimerDestroyedHandler**](https://docs.microsoft.com/uwp/api/windows.system.threading.timerdestroyedhandler) (se fornecido) é chamado quando todas as instâncias do item de trabalho periódico foram concluídas.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>Comentários

Para obter mais informações sobre temporizadores de uso único, consulte [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar um item de trabalho ao pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Práticas recomendadas para usar o pool de threads](best-practices-for-using-the-thread-pool.md)
* [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md)
 
