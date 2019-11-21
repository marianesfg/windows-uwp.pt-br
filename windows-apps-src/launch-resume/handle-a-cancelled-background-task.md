---
title: Tratar uma tarefa em segundo plano cancelada
description: Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp, background task
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c59982c174909a3fb8ab0b21d5dd792969cfeebc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259481"
---
# <a name="handle-a-cancelled-background-task"></a>Tratar uma tarefa em segundo plano cancelada

**APIs importantes**

-   [**BackgroundTaskCanceledEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcanceledeventhandler)
-   [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)
-   [**ApplicationData.Current**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current)

Saiba como criar uma tarefa em segundo plano que reconhece uma solicitação de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.

This topic assumes you have already created a background task class, including the **Run** method that is used as the background task entry point. Para começar a criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) ou [Criar e registrar uma tarefa em segundo plano dentro do processo](create-and-register-an-inproc-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

Este tópico também é aplicável a tarefas em segundo plano dentro do processo. But instead of the **Run** method, substitute **OnBackgroundActivated**. As tarefas em segundo plano dentro do processo não exigem que você use o armazenamento persistente para sinalizar o cancelamento, pois você pode comunicar o cancelamento usando o estado do aplicativo desde que a tarefa em segundo plano esteja em execução no mesmo processo do seu aplicativo em primeiro plano.

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>Usar o método OnCanceled para reconhecer as solicitações de cancelamento

Grave um método para manipular o evento de cancelamento.

> [!NOTE]
> Para todas as famílias de dispositivos, exceto desktop, caso haja pouca memória, as tarefas em segundo plano podem ser encerradas. If an out of memory exception is not surfaced, or the app doesn't handle it, then the background task will be terminated without warning and without raising the OnCanceled event. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

Crie um método chamado **OnCanceled** da seguinte maneira. Este método é o ponto de entrada chamado pelo Windows Runtime quando uma solicitação de cancelamento é feita em relação a sua tarefa em segundo plano.

```csharp
private void OnCanceled(
    IBackgroundTaskInstance sender,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(
    IBackgroundTaskInstance^ taskInstance,
    BackgroundTaskCancellationReason reason)
{
    // TODO: Add code to notify the background task that it is cancelled.
}
```

Add a flag variable called **\_CancelRequested** to the background task class. Essa variável será usada para indicar quando uma solicitação de cancelamento tiver sido feita.

```csharp
volatile bool _CancelRequested = false;
```

```cppwinrt
private:
    volatile bool m_cancelRequested;
```

```cpp
private:
    volatile bool CancelRequested;
```

In the **OnCanceled** method you created in step 1, set the flag variable **\_CancelRequested** to **true**.

The full [background task sample]( https://go.microsoft.com/fwlink/p/?linkid=227509) **OnCanceled** method sets **\_CancelRequested** to **true** and writes potentially useful debug output.

```csharp
private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    _cancelRequested = true;

    Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
}
```

```cppwinrt
void ExampleBackgroundTask::OnCanceled(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance,
    Windows::ApplicationModel::Background::BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    m_cancelRequested = true;
}
```

```cpp
void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
{
    // Indicate that the background task is canceled.
    CancelRequested = true;
}
```

In the background task's **Run** method, register the **OnCanceled** event handler method before starting work. Em uma tarefa em segundo plano dentro do processo único, você pode fazer esse registro como parte da inicialização do seu aplicativo. For example, use the following line of code.

```csharp
taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
```

```cppwinrt
taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });
```

```cpp
taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);
```

## <a name="handle-cancellation-by-exiting-your-background-task"></a>Manipular o cancelamento saindo da sua tarefa em segundo plano

When a cancellation request is received, your method that does background work needs to stop work and exit by recognizing when **\_cancelRequested** is set to **true**. For in-process background tasks, this means returning from the **OnBackgroundActivated** method. For out-of-process background tasks, this means returning from the **Run** method.

Modifique o código da classe de tarefa em segundo plano para verificar a variável de sinalizador enquanto ela está funcionando. If **\_cancelRequested** becomes set to true, stop work from continuing.

The [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) includes a check that stops the periodic timer callback if the background task is canceled.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
    // TODO: Record whether the task completed or was cancelled.
}
```

> [!NOTE]
> The code sample shown above uses the [**IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance).[**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) property being used to record background task progress. O progresso é relatado para o aplicativo usando a classe [**BackgroundTaskProgressEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs).

Modify the **Run** method so that after work has stopped, it records whether the task completed or was cancelled. Esta etapa é válida para tarefas em segundo plano fora do processo porque você precisa de uma maneira de comunicação entre processos quando a tarefa em segundo plano foi cancelada. Para tarefas em segundo plano dentro do processo, você pode simplesmente compartilhar o estado com o aplicativo para indicar que a tarefa foi cancelada.

The [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) records status in LocalSettings.

```csharp
if ((_cancelRequested == false) && (_progress < 100))
{
    _progress += 10;
    _taskInstance.Progress = _progress;
}
else
{
    _periodicTimer.Cancel();

    var settings = ApplicationData.Current.LocalSettings;
    var key = _taskInstance.Task.TaskId.ToString();

    // Write to LocalSettings to indicate that this background task ran.
    if (_cancelRequested)
    {
        settings.Values[key] = "Canceled";
    }
    else
    {
        settings.Values[key] = "Completed";
    }
        
    Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
        
    // Indicate that the background task has completed.
    _deferral.Complete();
}
```

```cppwinrt
if (!m_cancelRequested && m_progress < 100)
{
    m_progress += 10;
    m_taskInstance.Progress(m_progress);
}
else
{
    m_periodicTimer.Cancel();

    // Write to LocalSettings to indicate that this background task ran.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    auto key{ m_taskInstance.Task().Name() };
    settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

    // Indicate that the background task has completed.
    m_deferral.Complete();
}
```

```cpp
if ((CancelRequested == false) && (Progress < 100))
{
    Progress += 10;
    TaskInstance->Progress = Progress;
}
else
{
    PeriodicTimer->Cancel();
        
    // Write to LocalSettings to indicate that this background task ran.
    auto settings = ApplicationData::Current->LocalSettings;
    auto key = TaskInstance->Task->Name;
    settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
        
    // Indicate that the background task has completed.
    Deferral->Complete();
}
```

## <a name="remarks"></a>Comentários

Você pode baixar a [amostra de tarefa em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) para ver esses exemplos de código contextualizados dentro de métodos.

For illustrative purposes, the sample code shows only portions of the **Run** method (and callback timer) from the [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

## <a name="run-method-example"></a>Exemplo do método Run

The complete **Run** method, and timer callback code, from the [background task sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) are shown below for context.

```csharp
// The Run method is the entry point of a background task.
public void Run(IBackgroundTaskInstance taskInstance)
{
    Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");

    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
    var settings = ApplicationData.Current.LocalSettings;
    settings.Values["BackgroundWorkCost"] = cost.ToString();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance;
    _deferral = taskInstance.GetDeferral();
    _taskInstance = taskInstance;

    _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
}

// Simulate the background task activity.
private void PeriodicTimerCallback(ThreadPoolTimer timer)
{
    if ((_cancelRequested == false) && (_progress < 100))
    {
        _progress += 10;
        _taskInstance.Progress = _progress;
    }
    else
    {
        _periodicTimer.Cancel();

        var settings = ApplicationData.Current.LocalSettings;
        var key = _taskInstance.Task.Name;

        // Write to LocalSettings to indicate that this background task ran.
        settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
        Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);

        // Indicate that the background task has completed.
        _deferral.Complete();
    }
}
```

```cppwinrt
void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost{ Windows::ApplicationModel::Background::BackgroundWorkCost::CurrentBackgroundWorkCost() };
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
    std::wstring costAsString{ L"Low" };
    if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::Medium) costAsString = L"Medium";
    else if (cost == Windows::ApplicationModel::Background::BackgroundWorkCostValue::High) costAsString = L"High";
    settings.Values().Insert(L"BackgroundWorkCost", winrt::box_value(costAsString));

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled({ this, &ExampleBackgroundTask::OnCanceled });

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    m_deferral = taskInstance.GetDeferral();
    m_taskInstance = taskInstance;

    Windows::Foundation::TimeSpan period{ std::chrono::seconds{1} };
    m_periodicTimer = Windows::System::Threading::ThreadPoolTimer::CreatePeriodicTimer([this](Windows::System::Threading::ThreadPoolTimer timer)
    {
        if (!m_cancelRequested && m_progress < 100)
        {
            m_progress += 10;
            m_taskInstance.Progress(m_progress);
        }
        else
        {
            m_periodicTimer.Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings() };
            auto key{ m_taskInstance.Task().Name() };
            settings.Values().Insert(key, (m_progress < 100) ? winrt::box_value(L"Canceled") : winrt::box_value(L"Completed"));

            // Indicate that the background task has completed.
            m_deferral.Complete();
        }
    }, period);
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    // Query BackgroundWorkCost
    // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
    // of work in the background task and return immediately.
    auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
    auto settings = ApplicationData::Current->LocalSettings;
    settings->Values->Insert("BackgroundWorkCost", cost.ToString());

    // Associate a cancellation handler with the background task.
    taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &ExampleBackgroundTask::OnCanceled);

    // Get the deferral object from the task instance, and take a reference to the taskInstance.
    TaskDeferral = taskInstance->GetDeferral();
    TaskInstance = taskInstance;

    auto timerDelegate = [this](ThreadPoolTimer^ timer)
    {
        if ((CancelRequested == false) &&
            (Progress < 100))
        {
            Progress += 10;
            TaskInstance->Progress = Progress;
        }
        else
        {
            PeriodicTimer->Cancel();

            // Write to LocalSettings to indicate that this background task ran.
            auto settings = ApplicationData::Current->LocalSettings;
            auto key = TaskInstance->Task->Name;
            settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");

            // Indicate that the background task has completed.
            TaskDeferral->Complete();
        }
    };

    TimeSpan period;
    period.Duration = 1000 * 10000; // 1 second
    PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
}
```

## <a name="related-topics"></a>Tópicos relacionados

- [Criar e registrar uma tarefa em segundo plano em processamento](create-and-register-an-inproc-background-task.md).
- [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
- [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
- [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
- [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
- [Registrar uma tarefa em segundo plano](register-a-background-task.md)
- [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
- [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
- [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
- [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
- [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
- [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
- [How to trigger suspend, resume, and background events in UWP apps (when debugging)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
