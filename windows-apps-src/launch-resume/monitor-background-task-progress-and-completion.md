---
author: TylerMSFT
title: "Monitorar o progresso e a conclusão de tarefas em segundo plano"
description: "Saiba como o aplicativo pode reconhecer o progresso e a conclusão relatados por uma tarefa em segundo plano."
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 07d69b63b272153bc784ed19166e649f80a98297

---

# Monitorar o progresso e a conclusão de tarefas em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Saiba como o aplicativo pode reconhecer o progresso e a conclusão relatados por uma tarefa em segundo plano. As tarefas em segundo plano são separadas do aplicativo, e são executadas separadamente, mas o progresso e a conclusão dessas tarefas podem ser monitorados por um código do aplicativo. Para fazer isso, o aplicativo assina eventos das tarefas em segundo plano que ele registrou no sistema.

-   Este tópico considera que você tem um aplicativo que registra tarefas em segundo plano. Para criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

## Criar um manipulador de eventos para gerenciar as tarefas em segundo plano concluídas


1.  Crie uma função de manipulador de eventos para gerenciar as tarefas em segundo plano concluídas. Este código deve seguir um volume de memória específico, que tenha um objeto [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) e um objeto [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778).

    Use o seguinte volume de memória para o método manipulador de eventos de tarefa em segundo plano OnCompleted:

>  [!div class="tabbedCodeSnippets"]
>  ```cs
>  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>  {
>      // TODO: Add code that deals with background task completion.
>  }
>  ```
>  ```cpp
>  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>  {
>      // TODO: Add code that deals with background task completion.
>  };
>  ```

2.  [!div class="tabbedCodeSnippets"]

    Adicione código ao manipulador de eventos que gerencia a conclusão da tarefa em segundo plano.

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    >     ```

## Por exemplo, o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) atualiza a interface do usuário.


1.  [!div class="tabbedCodeSnippets"] ```cs
    private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    {
        UpdateUI();
    }
    ```
    ```cpp
    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {    
        UpdateUI();
    };
    ``` Criar uma função de manipulador de eventos para gerenciar o andamento das tarefas em segundo plano

    Crie uma função de manipulador de eventos para gerenciar as tarefas em segundo plano concluídas.

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    >     ```

2.  Este código deve seguir um volume de memória específico, que tenha um objeto [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) e um objeto [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782):

    Use o seguinte volume de memória para o método manipulador de eventos de tarefa em segundo plano OnProgress:

    > [!div class="tabbedCodeSnippets"]
    >     [!div class="tabbedCodeSnippets"] ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    ```
    ```cpp
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        // TODO: Add code that deals with background task progress.
    };
    ```
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    >     ```

## Adicione código ao manipulador de eventos que gerencia a conclusão da tarefa em segundo plano.


1.  Por exemplo, o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) atualiza a interface do usuário com o status de progresso informado pelo parâmetro *args*:

    [!div class="tabbedCodeSnippets"]     ```cs     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)     {         var progress = "Progress: " + args.Progress + "%";         BackgroundTaskSample.SampleBackgroundTaskProgress = progress;

    > [!div class="tabbedCodeSnippets"]
    >     ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    >     ```
    >     Registre as funções do manipulador de eventos com as tarefas em segundo plano novas e existentes.
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    >     ```

2.  Quando o aplicativo registra uma tarefa em segundo plano pela primeira vez, ele deve se registrar para receber atualizações de progresso e conclusão dessa tarefa, caso ela seja executada enquanto o aplicativo ainda estiver ativo no primeiro plano. Por exemplo, o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) chama a seguinte função em cada tarefa em segundo plano que ela registra:

    [!div class="tabbedCodeSnippets"] ```cs
    private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    {
        task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
        task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    }
    ```
    ```cpp     void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)     {         auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
                  {             auto progress = "Progress: " + args->Progress + "%";             BackgroundTaskSample::SampleBackgroundTaskProgress = progress;             UpdateUI();         };

    > [!div class="tabbedCodeSnippets"]
    >     Quando o aplicativo é iniciado ou navega para uma nova página onde o status da tarefa em segundo plano é irrelevante, ele deve obter uma lista das tarefas em segundo plano atualmente registradas e associá-las às funções do manipulador de eventos de progresso e conclusão.
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    >     ```

## A lista das tarefas em segundo plano atualmente registradas pelo aplicativo é mantida na propriedade [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787).


****

* [Por exemplo, o [exemplo de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) usa o seguinte código para anexar manipuladores de eventos quando a página SampleBackgroundTask é navegada para:](create-and-register-a-background-task.md)
* [[!div class="tabbedCodeSnippets"]     ```cs     protected override void OnNavigatedTo(NavigationEventArgs e)     {         foreach (var task in BackgroundTaskRegistration.AllTasks)         {             if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)             {                 AttachProgressAndCompletedHandlers(task.Value);                 BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);             }         }](declare-background-tasks-in-the-application-manifest.md)
* [Tópicos relacionados](handle-a-cancelled-background-task.md)
* [Criar e registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](respond-to-system-events-with-background-tasks.md)
* [Manipular uma tarefa em segundo plano cancelada](set-conditions-for-running-a-background-task.md)
* [Registrar uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](use-a-maintenance-trigger.md)
* [Definir condições para executar uma tarefa em segundo plano](run-a-background-task-on-a-timer-.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](guidelines-for-background-tasks.md)

****

* [Usar um gatilho de manutenção](debug-a-background-task.md)
* [Executar uma tarefa em segundo plano em um temporizador](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


