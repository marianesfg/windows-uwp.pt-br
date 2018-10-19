---
author: TylerMSFT
title: Registrar uma tarefa em segundo plano
description: Aprenda a criar uma função que pode ser reutilizada para registrar com segurança a maioria das tarefas em segundo plano.
ms.assetid: 8B1CADC5-F630-48B8-B3CE-5AB62E3DFB0D
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 6bd0361886181d3c5a3395112c728db3bf57d58f
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4949131"
---
# <a name="register-a-background-task"></a>Registrar uma tarefa em segundo plano


**APIs Importantes**

-   [**Classe BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**Classe BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**Classe SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Aprenda a criar uma função que pode ser reutilizada para registrar com segurança a maioria das tarefas em segundo plano.

Este tópico se aplica a tarefas em segundo plano dentro do processo e em segundo plano executadas em um processo à parte. Este tópico considera que você já tem uma tarefa em segundo plano que precisa ser registrada. (Consulte [Criar e registrar uma tarefa em segundo plano que é executada em um processo à parte](create-and-register-a-background-task.md) ou [Criar e registrar uma tarefa em segundo plano dentro do processo](create-and-register-an-inproc-background-task.md) para obter informações sobre como escrever uma tarefa em segundo plano).

Este tópico discorre sobre a função utilitária que registra tarefas em segundo plano. Esta função utilitária verifica registros existentes antes de registrar a tarefa várias vezes para evitar problemas com registros múltiplos e pode aplicar uma condição de sistema à tarefa em segundo plano. O tópico inclui um exemplo completo e prático desta função utilitária.

**Observação**  

Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para saber mais, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

## <a name="define-the-method-signature-and-return-type"></a>Definir a assinatura do método e o tipo de retorno

Esta função obtém o ponto de entrada da tarefa, seu nome, um gatilho de tarefa em segundo plano pré-construído e (opcionalmente) uma [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) para a tarefa em segundo plano. Este método retorna um objeto [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).

> [!Important]
> `taskEntryPoint` - para tarefas em segundo plano executadas em um processo à parte; ele deve ser construído como o nome do namespace, '.', e o nome da classe que contém a classe em segundo plano. A cadeia de caracteres diferencia maiúsculas de minúsculas.  Por exemplo, se você tivesse um namespace "MyBackgroundTasks" e uma classe "BackgroundTask1" contendo o código da classe em segundo plano, a cadeia de caracteres para `taskEntryPoint` seria "MyBackgroundTasks.BackgroundTask1".
> Se a tarefa em segundo plano for executada no mesmo processo do aplicativo (ou seja, uma tarefa em segundo plano dentro do processo) `taskEntryPoint` não deverá ser definido.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     
>     // We'll add code to this function in subsequent steps.
>
> }
> ```

## <a name="check-for-existing-registrations"></a>Verificar registros existentes

Verifique se a tarefa já está registrada. É importante verificar isso porque, se a tarefa for registrada várias vezes, ela será executada mais de uma vez sempre que for disparada; isso pode usar excessivamente a CPU e causar comportamentos inesperados.

Você pode verificar se existem registros consultando a propriedade [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) e iterando no resultado. Verifique o nome de cada instância – se ele corresponder ao nome da tarefa que você está registrando, interrompa o loop e defina uma variável de sinalização, para que seu código possa escolher outro caminho na próxima etapa.

> **Observação**  Use nomes de tarefas em segundo plano exclusivos para seu aplicativo. Verifique se cada tarefa em segundo plano possui um nome exclusivo.

O código abaixo registra uma tarefa em segundo plano usando o [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) que criamos na última etapa:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == name)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>     
>     // We'll register the task in the next step.
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>     
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>     
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>         
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>             
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>         
>         hascur = iter->MoveNext();
>     }
>     
>     // We'll register the task in the next step.
> }
> ```

## <a name="register-the-background-task-or-return-the-existing-registration"></a>Registrar a tarefa em segundo plano (ou retornar ao registro existente)


Verifique se a tarefa foi encontrada na lista de registros de tarefas em segundo plano existentes. Caso afirmativo, retorne aquela instância da tarefa.

Em seguida, registre a tarefa usando um novo objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Este código deve verificar se o parâmetro da condição é nulo e, se não for, adicionar a condição ao objeto de registro. Retorne a [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) retornada pela função [**BackgroundTaskBuilder.Register**](https://msdn.microsoft.com/library/windows/apps/br224772).

> **Observações**  Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.
> **Observação** Se você registrar uma tarefa em segundo plano executada no mesmo processo do aplicativo, envie `String.Empty` ou `null` para o parâmetro `taskEntryPoint`.

O exemplo a seguir retorna a tarefa existente ou adiciona código que registra a tarefa em segundo plano (incluindo a condição de sistema opcional, se presente):

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> public static BackgroundTaskRegistration RegisterBackgroundTask(
>                                                 string taskEntryPoint,
>                                                 string name,
>                                                 IBackgroundTrigger trigger,
>                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = name;
>
>     // in-process background tasks don't set TaskEntryPoint
>     if ( taskEntryPoint != null && taskEntryPoint != String.Empty)
>     {
>         builder.TaskEntryPoint = taskEntryPoint;
>     }
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(
>                                              Platform::String ^ taskEntryPoint,
>                                              Platform::String ^ taskName,
>                                              IBackgroundTrigger ^ trigger,
>                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>         
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="complete-background-task-registration-utility-function"></a>Complete a função utilitária de registro de tarefa em segundo plano

Este exemplo mostra a função completa de registro de tarefa em segundo plano. Esta função pode ser usada para registrar a maioria das tarefas em segundo plano, com exceção das tarefas em segundo plano de rede.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> public static BackgroundTaskRegistration RegisterBackgroundTask(string taskEntryPoint,
>                                                                 string taskName,
>                                                                 IBackgroundTrigger trigger,
>                                                                 IBackgroundCondition condition)
> {
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     foreach (var cur in BackgroundTaskRegistration.AllTasks)
>     {
>
>         if (cur.Value.Name == taskName)
>         {
>             //
>             // The task is already registered.
>             //
>
>             return (BackgroundTaskRegistration)(cur.Value);
>         }
>     }
>
>     //
>     // Register the background task.
>     //
>
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = taskName;
>     builder.TaskEntryPoint = taskEntryPoint;
>     builder.SetTrigger(trigger);
>
>     if (condition != null)
>     {
>
>         builder.AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration task = builder.Register();
>
>     return task;
> }
> ```
> ``` cpp
> //
> // Register a background task with the specified taskEntryPoint, name, trigger,
> // and condition (optional).
> //
> // taskEntryPoint: Task entry point for the background task.
> // taskName: A name for the background task.
> // trigger: The trigger for the background task.
> // condition: Optional parameter. A conditional event that must be true for the task to fire.
> //
> BackgroundTaskRegistration^ MainPage::RegisterBackgroundTask(Platform::String ^ taskEntryPoint,
>                                                              Platform::String ^ taskName,
>                                                              IBackgroundTrigger ^ trigger,
>                                                              IBackgroundCondition ^ condition)
> {
>
>     //
>     // Check for existing registrations of this background task.
>     //
>
>     auto iter   = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == name)
>         {
>             //
>             // The task is registered.
>             //
>
>             return (BackgroundTaskRegistration ^)(cur);
>         }
>
>         hascur = iter->MoveNext();
>     }
>
>
>     //
>     // Register the background task.
>     //
>
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = name;
>     builder->TaskEntryPoint = taskEntryPoint;
>     builder->SetTrigger(trigger);
>
>     if (condition != nullptr) {
>
>         builder->AddCondition(condition);
>     }
>
>     BackgroundTaskRegistration ^ task = builder->Register();
>
>     return task;
> }
> ```

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)