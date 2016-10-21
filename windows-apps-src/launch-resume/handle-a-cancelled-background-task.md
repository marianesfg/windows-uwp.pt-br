---
author: TylerMSFT
title: Manipular uma tarefa em segundo plano cancelada
description: "Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente."
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: e1a843448accb5ae2d689a6105c8254b0f868b5b

---

# Manipular uma tarefa em segundo plano cancelada

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

Saiba como criar uma tarefa em segundo plano que reconhece uma solicitação de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.

Este tópico pressupõe que você já tenha criado uma classe de tarefa em segundo plano, inclusive o método Run que é usado como o ponto de entrada da tarefa em segundo plano. Para começar a criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano que é executada em um processo separado](create-and-register-a-background-task.md) ou. Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

Este tópico também é aplicável a tarefas em segundo plano de processo único. Mas, em vez do método Run(), substitua OnBackgroundActivated(). As tarefas em segundo plano de processo único não exigem que você use o armazenamento persistente para sinalizar o cancelamento, pois você pode comunicar o cancelamento usando o estado do aplicativo desde que a tarefa em segundo plano esteja em execução no mesmo processo do seu aplicativo em primeiro plano.

## Usar o método OnCanceled para reconhecer as solicitações de cancelamento

Grave um método para manipular o evento de cancelamento.

> **Observação**  Para todas as famílias de dispositivos, exceto desktop, se o dispositivo estiver com memória insuficiente, as tarefas em segundo plano poderão ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

Crie um método chamado OnCanceled da seguinte maneira. Este método é o ponto de entrada chamado pelo Windows Runtime quando uma solicitação de cancelamento é feita em relação a sua tarefa em segundo plano.

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

Adicione uma variável de sinalizador chamada **\_CancelRequested** à classe da tarefa em segundo plano. Essa variável será usada para indicar quando uma solicitação de cancelamento tiver sido feita.

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

No método OnCanceled que você criou na etapa 1, defina a variável de sinalizador **\_CancelRequested** como **true**.

O método OnCanceled do [amostra de tarefa em segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509) define **\_CancelRequested** como **true** e grava uma saída de depuração potencialmente útil:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         _cancelRequested = true;
>
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         CancelRequested = true;
>     }
> ```

No método Run da tarefa em segundo plano, registre o método manipulador de eventos OnCanceled antes de começar a trabalhar. Em uma tarefa em segundo plano de processo único, você pode fazer esse registro como parte da inicialização do seu aplicativo. Por exemplo, use a seguinte linha de código:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## Manipular o cancelamento saindo da sua tarefa em segundo plano

Quando uma solicitação de cancelamento é recebida, o método que realiza a tarefa em segundo plano precisa parar o trabalho e sair reconhecendo quando **\_cancelRequested** está definido como **true**. Para tarefas em segundo plano de processo único, isso significa retornar do método `OnBackgroundActivated()`. Para tarefas em segundo plano que são executadas em um processo separado, isso significa retornar do método `Run()`.

Modifique o código da classe de tarefa em segundo plano para verificar a variável de sinalizador enquanto ela está funcionando. Se **\_cancelRequested** estiver definido como true, o trabalho será interrompido.

O [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) inclui uma verificação que interrompe o retorno de chamada do temporizador periódico quando a tarefa em segundo plano é cancelada:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> **Observação**  O código de exemplo mostrado anteriormente usa a propriedade [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) que está sendo usada para registrar o progresso da tarefa em segundo plano. O progresso é relatado para o aplicativo usando a classe [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782).

Modifique o método Run para que depois que o trabalho tiver parado, ele registre se a tarefa foi concluída ou foi cancelada. Esta etapa é válida para tarefas em segundo plano que são executadas em um processo separado porque você precisa de uma maneira de comunicação entre processos quando a tarefa em segundo plano foi cancelada. Para tarefas em segundo plano de processo único, você pode simplesmente compartilhar o estado com o aplicativo para indicar que a tarefa foi cancelada.

O [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) registra o status em LocalSettings:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
>
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## Comentários

Você pode baixar a [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver esses exemplos de código contextualizados dentro de métodos.

Por questões ilustrativas, o código de amostra mostra apenas partes do método Run (e temporizador de chamada de retorno) da [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

## Exemplo do método Run

O método Run completo e o código de retorno de chamada do temporizador do [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) são mostrados a seguir para contextualização:

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
>
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
>
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
>
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
>
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
>
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &&
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
>
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
>
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
>
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Diretrizes de tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)

* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Aug16_HO3-->


