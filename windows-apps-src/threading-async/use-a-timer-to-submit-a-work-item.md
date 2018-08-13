---
author: normesta
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Usar um temporizador para enviar um item de trabalho
description: Saiba como criar um item de trabalho que seja executado após um temporizador.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, temporizador, threads
ms.openlocfilehash: 214a3ad9d84ffb8bc26a4aa02d79d0b1c06f2bfe
ms.sourcegitcommit: 378382419f1fda4e4df76ffa9c8cea753d271e6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2017
ms.locfileid: "665342"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>Usar um temporizador para enviar um item de trabalho

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** APIs importantes **

-   [**Windows.UI.Core namespace**](https://msdn.microsoft.com/library/windows/apps/BR208383)
-   [**Windows.System.Threading namespace**](https://msdn.microsoft.com/library/windows/apps/BR229642)

Saiba como criar um item de trabalho que seja executado após um temporizador.

## <a name="create-a-single-shot-timer"></a>Criar um temporizador de disparo único

Use o método [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) para criar um temporizador para o item de trabalho. Forneça um lambda que realiza o trabalho e use o parâmetro *delay* para especificar por quanto tempo o pool de threads deve aguardar antes de atribuir o item de trabalho a um thread disponível. O atraso é especificado usando uma estrutura [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996).

> **Observação**  Você pode usar [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) para acessar a interface do usuário e mostrar o progresso do item de trabalho.

O seguinte exemplo cria um item de trabalho que é executado em três minutos:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
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
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>Forneça um manipulador de conclusão.

Se necessário, manipule o cancelamento e a conclusão do item de trabalho com um [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926). Use a sobrecarga de [**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967921) para fornecer um lambda adicional. Isso é executado quando o temporizador é cancelado ou quando o item de trabalho é concluído.

O seguinte exemplo cria um timer que envia o item de trabalho e chama um método quando o item de trabalho é concluído ou o timer é cancelado:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Cancelar o timer

Se o temporizador ainda estiver em contagem regressiva, mas o item de trabalho não for mais necessário, chame [**Cancelar**](https://msdn.microsoft.com/library/windows/apps/BR230588). O temporizador é cancelado e o item de trabalho não é enviado para o pool de threads.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>Comentários

Os aplicativos UWP (Plataforma Universal do Windows) não podem usar **Thread.Sleep** porque esse método pode bloquear o thread da interface do usuário. Em vez disso, você pode usar um [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587) para criar um item de trabalho. Isso atrasará a tarefa realizada pelo item de trabalho sem bloquear o thread da interface do usuário.

Veja o [exemplo do pool de threads](http://go.microsoft.com/fwlink/p/?linkid=255387) para obter um exemplo de código completo que demonstra os itens de trabalho, os itens de trabalho de timer e os itens de trabalho periódico. O exemplo de código foi originalmente escrito para o Windows 8.1, mas o código pode ser reutilizado no Windows 10.

Para saber mais sobre temporizadores repetidos, veja [Criar um item de trabalho periódico](create-a-periodic-work-item.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Enviar um item de trabalho ao pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Práticas recomendadas para usar o pool de threads](best-practices-for-using-the-thread-pool.md)
* [Usar um temporizador para enviar um item de trabalho](use-a-timer-to-submit-a-work-item.md)
 

 
