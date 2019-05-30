---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: Mantenha o thread de interface do usuário responsivo
description: Os usuários esperam que um aplicativo continue respondendo enquanto executa cálculos, independentemente do tipo de computador.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 607b7956786f5713b6133633c2609b801883c76b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362397"
---
# <a name="keep-the-ui-thread-responsive"></a>Mantenha o thread de interface do usuário responsivo


Os usuários esperam que um aplicativo continue respondendo enquanto executa cálculos, independentemente do tipo de computador. Isso significa coisas diferentes para aplicativos diferentes. Para alguns, isso significa oferecer física mais realista, carregar dados do disco ou da Web mais rapidamente, apresentar cenas complexas e navegar entre páginas, encontrar referências de local ou processar dados com mais agilidade. Independentemente do tipo de cálculo, os usuários querem que o aplicativo aja com sua entrada, e instâncias nas quais ele parece não responder enquanto "pensa" sejam eliminadas.

Seu aplicativo é controlado por eventos, o que significa que seu código executa um trabalho em resposta a um evento e fica ocioso até o próximo. O código da plataforma para a interface do usuário (layout, entrada, acionar eventos, etc.) e o código do aplicativo para a interface do usuário são executados no mesmo thread de interface do usuário. Apenas uma instrução pode ser executada nesse thread de cada vez. Então, se o código do aplicativo demorar muito para processar um evento, a estrutura não poderá executar o layout ou acionar novos eventos que representam a interação do usuário. A capacidade de resposta do seu aplicativo está relacionada à disponibilidade do thread de interface do usuário para processar o trabalho.

Você precisa usar o thread de interface do usuário para fazer quase todas as alterações no thread de interface do usuário, inclusive criar tipos de interface do usuário e acessar seus membros. Você não pode atualizar a interface do usuário de um thread em segundo plano, mas você pode postar uma mensagem para ele com [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) para fazer com que o código seja executado ali.

> **Observação**  a única exceção é que há um thread de renderização separado que pode aplicar alterações de interface do usuário que não afetarão como a entrada é tratada ou o layout básico. Por exemplo, muitas animações e transições que não afetam o layout podem ser executadas nesse thread de renderização.

## <a name="delay-element-instantiation"></a>Atrasar a instanciação de elementos

Alguns dos estágios mais lentos de um aplicativo podem incluir a inicialização e a troca de exibições. Não execute mais trabalho do que o necessário para exibir a interface do usuário que o usuário vê inicialmente. Por exemplo, não crie a interface do usuário para IU progressivamente revelada e o conteúdo de pop-ups.

-   Use o [Atributo x:Load](../xaml-platform/x-load-attribute.md) ou o [x:DeferLoadStrategy](https://docs.microsoft.com/windows/uwp/xaml-platform/x-deferloadstrategy-attribute) para atrasar a instanciação de elementos.
-   Insira de forma programada elementos na árvore sob demanda.

[**CoreDispatcher.RunIdleAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) as filas trabalham para o thread de interface do usuário processar quando não está ocupado.

## <a name="use-asynchronous-apis"></a>Usar APIs assíncronas

Para ajudar a manter a capacidade de resposta de seu aplicativo, a plataforma fornece versões assíncronas de várias APIs. Uma API assíncrona impede que seu thread de execução ativo bloqueie por muito tempo. Para chamar uma API do thread da interface do usuário, use a versão assíncrona, se ela estiver disponível. Para obter mais informações sobre a programação com padrões **async**, consulte [Programação assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps) ou [Chamar APIs assíncronas no Visual Basic ou C#](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic).

## <a name="offload-work-to-background-threads"></a>Descarregue o trabalho em threads de segundo plano

Escreva manipuladores de eventos para retornarem rapidamente. Em casos em que uma quantidade de trabalho incomum tenha de ser executada, agende isso em um thread em segundo plano e retorne.

Você pode agendar o trabalho assíncrono usando o operador **await** no C#, o operador **Await** no Visual Basic ou representantes no C++. Mas isso não garante que o trabalho que você agendar será executado em um thread de segundo plano. Muitas APIs da Plataforma Universal do Windows (UWP) trabalham no thread de segundo plano, mas se você chamar o código do aplicativo usando apenas **await** ou um representante, executará esse representante ou método no thread de interface do usuário. É preciso informar explicitamente quando você quer executar o código do aplicativo em um thread de segundo plano. No C# e você pode fazer isso passando o código do Visual Basic [ **Task. Run**](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run?redirectedfrom=MSDN#overloads).

Lembre-se de que os elementos de interface do usuário só podem ser acessados no thread de interface do usuário. Use o thread de interface do usuário para acessar elementos de interface do usuário antes de iniciar o trabalho em segundo plano e/ou use [**CoreDispatcher.RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.windows) ou [**CoreDispatcher.RunIdleAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runidleasync) no thread em segundo plano.

Um exemplo de trabalho que pode ser realizado em um thread de segundo plano é o cálculo da inteligência artificial do computador em um jogo. O código que calcula o próximo movimento do computador pode demorar muito tempo para ser executado.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

Neste exemplo, o manipulador `NextMove_Click` retornará em **await** para manter o thread de interface do usuário responsivo. Mas a execução seleciona esse manipulador novamente depois que `ComputeNextMove` (que é executado em um thread em segundo plano) é concluído. O restante do código no manipulador atualiza a interface do usuário com os resultados.

> **Observação**  há também um [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) e [ **ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/windows.system.threading.threadpooltimer) API para a UWP, que pode ser usado para cenários semelhantes. Para obter mais informações, consulte [Programação threading e assíncrona](https://docs.microsoft.com/windows/uwp/threading-async/index).

## <a name="related-topics"></a>Tópicos relacionados

* [Interações personalizadas do usuário](https://developer.microsoft.com/windows/design/inputs-devices)
