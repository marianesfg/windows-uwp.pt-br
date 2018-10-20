---
author: TylerMSFT
title: Manipular uma tarefa em segundo plano cancelada
description: Saiba como criar uma tarefa em segundo plano que reconhece solicitações de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 2c78f5f43d93002b90902a7f9e5a943c7239946c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2018
ms.locfileid: "5166351"
---
# <a name="handle-a-cancelled-background-task"></a>Tratar uma tarefa em segundo plano cancelada

**APIs Importantes**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

Saiba como criar uma tarefa em segundo plano que reconhece uma solicitação de cancelamento e interrompe o trabalho, relatando o cancelamento ao aplicativo usando armazenamento persistente.

Este tópico pressupõe que você já criou uma classe de tarefa em segundo plano, incluindo o método **Run** que é usado como o ponto de entrada de tarefa em segundo plano. Para começar a criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) ou [Criar e registrar uma tarefa em segundo plano dentro do processo](create-and-register-an-inproc-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

Este tópico também é aplicável a tarefas em segundo plano dentro do processo. Mas, em vez do método **Run** , substitua **OnBackgroundActivated**. As tarefas em segundo plano dentro do processo não exigem que você use o armazenamento persistente para sinalizar o cancelamento, pois você pode comunicar o cancelamento usando o estado do aplicativo desde que a tarefa em segundo plano esteja em execução no mesmo processo do seu aplicativo em primeiro plano.

## <a name="use-the-oncanceled-method-to-recognize-cancellation-requests"></a>Usar o método OnCanceled para reconhecer as solicitações de cancelamento

Grave um método para manipular o evento de cancelamento.

> [!NOTE]
> Para todas as famílias de dispositivos, exceto desktop, caso haja pouca memória, as tarefas em segundo plano podem ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

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

Adicione uma variável de sinalizador chamada **\_CancelRequested** à classe da tarefa em segundo plano. Essa variável será usada para indicar quando uma solicitação de cancelamento tiver sido feita.

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

No método **OnCanceled** que você criou na etapa 1, defina a variável de sinalizador **\_CancelRequested** como **true**.

[Exemplo de tarefa em segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509) completo método **OnCanceled** define **\_CancelRequested** como **true** e grava uma saída de depuração potencialmente útil.

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

No método **Run** da tarefa em segundo plano, registre o método de manipulador de eventos **OnCanceled** antes de começar a trabalhar. Em uma tarefa em segundo plano dentro do processo único, você pode fazer esse registro como parte da inicialização do seu aplicativo. Por exemplo, use a seguinte linha de código.

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

Quando uma solicitação de cancelamento é recebida, o método que realiza a tarefa em segundo plano precisa parar o trabalho e sair reconhecendo quando **\_cancelRequested** está definido como **true**. Para tarefas em segundo plano no processo, isso significa retornar do método **OnBackgroundActivated** . Para tarefas em segundo plano fora do processo, isso significa retornar do método **Executar** .

Modifique o código da classe de tarefa em segundo plano para verificar a variável de sinalizador enquanto ela está funcionando. Se **\_cancelRequested** torna-se definido como true, o trabalho de continuar.

O [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) inclui uma verificação que interrompe o retorno de chamada do temporizador periódico se a tarefa em segundo plano é cancelada.

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
> O código de exemplo mostrado acima usa a [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797). Propriedade de [**progresso**](https://msdn.microsoft.com/library/windows/apps/br224800) que está sendo usada para registrar o progresso da tarefa em segundo plano. O progresso é relatado para o aplicativo usando a classe [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782).

Modifique o método **Run** para que depois que o trabalho tiver parado, ele registre se a tarefa foi concluída ou foi cancelada. Esta etapa é válida para tarefas em segundo plano fora do processo porque você precisa de uma maneira de comunicação entre processos quando a tarefa em segundo plano foi cancelada. Para tarefas em segundo plano dentro do processo, você pode simplesmente compartilhar o estado com o aplicativo para indicar que a tarefa foi cancelada.

A [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) registra o status em LocalSettings.

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

Você pode baixar a [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver esses exemplos de código contextualizados dentro de métodos.

Por questões ilustrativas, o código de exemplo mostra apenas partes do método **Run** (e temporizador de chamada de retorno) da [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

## <a name="run-method-example"></a>Exemplo do método Run

A concluir o método **Run** e o código de retorno de chamada do temporizador, da [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) são mostrados abaixo de contexto.

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
- [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)
