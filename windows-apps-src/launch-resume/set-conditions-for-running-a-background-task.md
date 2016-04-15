---
title: Definir condições para executar uma tarefa em segundo plano
description: Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
---

# Definir condições para executar uma tarefa em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
-   [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.

Às vezes, tarefas em tela de fundo exigem que certas condições sejam atendidas, além do evento que as disparam, para que possam ser concluídas com sucesso. Você pode definir uma ou mais das condições especificadas por [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) ao registrar sua tarefa em segundo plano. A condição será verificada depois que o gatilho tiver sido disparado. A tarefa em segundo plano será enfileirada, mas não será executada até que todas as condições necessárias sejam atendidas.

Colocar condições em tarefas em segundo plano poupa bateria e tempo de execução da CPU, evitando que as tarefas sejam executadas sem necessidade. Por exemplo, se a sua tarefa em segundo plano é executada com um temporizador e requer conectividade com a Internet, adicione a condição **InternetAvailable** a [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) antes de registrar a tarefa. Isso ajudará a evitar que a tarefa use recursos do sistema e duração da bateria desnecessariamente, permitindo que ela seja executada quando o temporizador tiver expirado e a Internet estiver disponível.

**Observação**  É possível integrar várias condições chamando AddCondition muitas vezes no mesmo [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Tome cuidado para não adicionar condições conflitantes, como **UserPresent**e **UserNotPresent**.

 

## Criar um objeto SystemCondition


Este tópico pressupõe que você tenha uma tarefa em segundo plano já associada ao aplicativo e que o aplicativo já inclua um código que cria um objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) que se chama **taskBuilder**

Antes de adicionar a condição, crie um objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) representando a condição que precisa estar em vigor para uma tarefa em segundo plano ser executada. No construtor, especifique a condição que precisa ser atendida, fornecendo um valor de enumeração [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

O código a seguir cria um objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) que especifica a disponibilidade da Internet como o requisito condicional:

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## Adicionar o objeto SystemCondition à sua tarefa em segundo plano


Para adicionar a condição, chame o método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) no objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) e passe a ele o objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

O código a seguir registra a condição da tarefa em segundo plano InternetAvailable em TaskBuilder:

> [!div class="tabbedCodeSnippets"]
> ```cs
> taskBuilder.AddCondition(internetCondition);
> ```
> ```cpp
> taskBuilder->AddCondition(internetCondition);
> ```

## Registrar sua tarefa em segundo plano


Agora, você pode registrar sua tarefa em segundo plano no método [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772), e ela apenas será iniciada quando a condição especificada for atendida.

O código a seguir registra a tarefa e armazena o objeto BackgroundTaskRegistration resultante:

> [!div class="tabbedCodeSnippets"]
> ```cs
> BackgroundTaskRegistration task = taskBuilder.Register();
> ```
> ```cpp
> BackgroundTaskRegistration ^ task = taskBuilder->Register();
> ```

> **Observação** Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para saber mais, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

> **Observação** Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo manipula tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## Inserir várias condições na sua tarefa em segundo plano

Para adicionar várias condições, seu aplicativo faz várias chamadas ao método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) . Essas chamadas devem ser feitas antes que o registro da tarefa se torne efetivo.

> **Observação**  Tome cuidado para não adicionar condições conflitantes a uma tarefa em segundo plano.
 

O trecho a seguir mostra várias condições no contexto de criação e registro de uma tarefa em segundo plano:

> [!div class="tabbedCodeSnippets"]
```cs
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
> 
> var recurringTaskBuilder = new BackgroundTaskBuilder();
> 
> recurringTaskBuilder.Name           = "Hourly background task";
> recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder.SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
> SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> 
> recurringTaskBuilder.AddCondition(userCondition);
> recurringTaskBuilder.AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration task = recurringTaskBuilder.Register();
> ```
> ```cpp
> // 
> // Set up the background task.
> // 
> 
> TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
> 
> auto recurringTaskBuilder = ref new BackgroundTaskBuilder();
> 
> recurringTaskBuilder->Name           = "Hourly background task";
> recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
> recurringTaskBuilder->SetTrigger(hourlyTrigger);
> 
> // 
> // Begin adding conditions.
> // 
> 
> SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
> SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> 
> recurringTaskBuilder->AddCondition(userCondition);
> recurringTaskBuilder->AddCondition(internetCondition);
> 
> // 
> // Done adding conditions, now register the background task.
> // 
> 
> BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
> ```

## Remarks


> **Note**  Chose the right conditions for your background task so that it only runs when it's needed, and doesn't run when it won't work. See [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) for descriptions of the different background task conditions.

> **Note**  This article is for Windows 10 developers writing Universal Windows Platform (UWP) apps. If you’re developing for Windows 8.x or Windows Phone 8.x, see the [archived documentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Related topics


****

* [Create and register a background task](create-and-register-a-background-task.md)
* [Declare background tasks in the application manifest](declare-background-tasks-in-the-application-manifest.md)
* [Handle a cancelled background task](handle-a-cancelled-background-task.md)
* [Monitor background task progress and completion](monitor-background-task-progress-and-completion.md)
* [Register a background task](register-a-background-task.md)
* [Respond to system events with background tasks](respond-to-system-events-with-background-tasks.md)
* [Update a live tile from a background task](update-a-live-tile-from-a-background-task.md)
* [Use a maintenance trigger](use-a-maintenance-trigger.md)
* [Run a background task on a timer](run-a-background-task-on-a-timer-.md)
* [Guidelines for background tasks](guidelines-for-background-tasks.md)

****

* [Debug a background task](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in Windows Store apps (when debugging)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=Mar16_HO1-->


