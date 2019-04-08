---
title: Monitorar o progresso e a conclusão de tarefas em segundo plano
description: Saiba como o aplicativo pode reconhecer o progresso e a conclusão relatados por uma tarefa em segundo plano.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.date: 07/06/2018
ms.topic: article
keywords: o Windows 10, uwp, tarefas em segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 5417a2cded6dab98747569acc4e6e92356361482
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595811"
---
# <a name="monitor-background-task-progress-and-completion"></a>Monitorar o progresso e a conclusão de tarefas em segundo plano

**APIs importantes**

- [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
- [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
- [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Saiba como o aplicativo pode reconhecer o progresso e a conclusão relatados por uma tarefa em segundo plano executada em um processo à parte. (Para tarefas em segundo plano no processo, você pode definir variáveis compartilhadas para indicar o andamento e a conclusão.)

O progresso e a conclusão da tarefa em segundo plano podem ser monitorados pelo código do aplicativo. Para fazer isso, o aplicativo assina eventos das tarefas em segundo plano que ele registrou no sistema.

- Este tópico considera que você tem um aplicativo que registra tarefas em segundo plano. Para começar a criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano em processamento](create-and-register-an-inproc-background-task.md) ou [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>Criar um manipulador de eventos para gerenciar as tarefas em segundo plano concluídas

### <a name="step-1"></a>Etapa 1
Crie uma função de manipulador de eventos para gerenciar as tarefas em segundo plano concluídas. Esse código deve seguir um volume específico, o que leva um [ **IBackgroundTaskRegistration** ](https://msdn.microsoft.com/library/windows/apps/br224803) objeto e uma [ **BackgroundTaskCompletedEventArgs** ](https://msdn.microsoft.com/library/windows/apps/br224778) objeto.

Use o seguinte volume para o **OnCompleted** método do manipulador de eventos de tarefa do plano de fundo.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>Etapa 2
Adicione código ao manipulador de eventos que gerencia a conclusão da tarefa em segundo plano.

Por exemplo, o [exemplo de tarefa em segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666) atualiza a interface do usuário.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>Criar uma função de manipulador de eventos para gerenciar o andamento das tarefas em segundo plano

### <a name="step-1"></a>Etapa 1
Crie uma função de manipulador de eventos para gerenciar as tarefas em segundo plano concluídas. Este código deve seguir um volume de memória específico, que tenha um objeto [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) e um objeto [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782):

Use o seguinte volume de memória para o método manipulador de eventos de tarefa em segundo plano OnProgress:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>Etapa 2
Adicione código ao manipulador de eventos que gerencia a conclusão da tarefa em segundo plano.

Por exemplo, o [exemplo de tarefa em segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666) atualiza a interface do usuário com o status de progresso informado pelo parâmetro *args*:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>Registre as funções do manipulador de eventos com as tarefas em segundo plano novas e existentes

### <a name="step-1"></a>Etapa 1
Quando o aplicativo registra uma tarefa em segundo plano pela primeira vez, ele deve se registrar para receber atualizações de progresso e conclusão dessa tarefa, caso ela seja executada enquanto o aplicativo ainda estiver ativo no primeiro plano.

Por exemplo, o [exemplo de tarefa em segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666) chama a seguinte função em cada tarefa em segundo plano que ela registra:

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>Etapa 2
Quando o aplicativo é iniciado ou navega para uma nova página onde o status da tarefa em segundo plano é irrelevante, ele deve obter uma lista das tarefas em segundo plano atualmente registradas e associá-las às funções do manipulador de eventos de progresso e conclusão. A lista das tarefas em segundo plano atualmente registradas pelo aplicativo é mantida na propriedade [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787).

Por exemplo, o [exemplo de tarefa em segundo plano](https://go.microsoft.com/fwlink/p/?LinkId=618666) usa o seguinte código para anexar manipuladores de eventos quando a página SampleBackgroundTask é navegada para:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md).
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar suspender, continuar e eventos em aplicativos UWP do plano de fundo (durante a depuração)](https://go.microsoft.com/fwlink/p/?linkid=254345)
