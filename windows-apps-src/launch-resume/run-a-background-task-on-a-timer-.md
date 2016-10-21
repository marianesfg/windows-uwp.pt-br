---
author: TylerMSFT
title: Executar uma tarefa em segundo plano em um temporizador
description: "Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
translationtype: Human Translation
ms.sourcegitcommit: 16202eeb37421acf75a9032dfc1eec397d23ce4f
ms.openlocfilehash: dd0d0fe0081eac112ce22e8a035b4bb70be3bef0

---

# Executar uma tarefa em segundo plano em um temporizador

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.

-   Este exemplo pressupõe que você tenha uma tarefa em segundo plano que precisa ser executada periodicamente ou em um momento específico para dar suporte ao aplicativo. Uma tarefa em segundo plano só será executada usando um [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) se você tiver chamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   Este tópico pressupõe que você já criou uma classe de tarefa em segundo plano. Para começar a criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano de processo único](create-and-register-a-singleprocess-background-task.md) ou [Criar e registrar uma tarefa em segundo plano que é executada em um processo separado](create-and-register-a-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

## Criar um gatilho de tempo

-   Crie um novo [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). O segundo parâmetro, *OneShot*, especifica se a tarefa em segundo plano será executada somente uma única vez ou periodicamente. Se *OneShot* for definido como verdadeiro, o primeiro parâmetro (*FreshnessTime*) especificará o número de minutos aguardados até que a tarefa em segundo plano seja agendada. Se *OneShot* for definido como falso, *FreshnessTime* especificará a frequência com que a tarefa em segundo plano será executada.

    O temporizador interno para aplicativos da Plataforma Universal do Windows (UWP) destinados à família de dispositivos móveis ou da área de trabalho executa tarefas em segundo plano em intervalos de 15 minutos.

    -   Se *FreshnessTime* for definido como 15 minutos e *OneShot* for verdadeiro, a tarefa será agendada para ser executada uma vez começando entre 15 e 30 a partir do momento em que for registrada. Se for definido como 25 minutos e *OneShot* for verdadeiro, a tarefa será agendada para ser executada uma vez começando entre 25 e 40 a partir do momento em que for registrada.

    -   Se *FreshnessTime* for definido como 15 minutos e *OneShot* for falso, a tarefa será agendada para ser executada a cada 15 minutos começando entre 15 e 30 a partir do momento em que for registrada. Se ele estiver definido como n minutos e *OneShot* for falso, a tarefa será agendada para ser executada a cada n minutos começando entre n e n + 15 minutos depois que ele estiver registrado.

    **Observação**  Se *FreshnessTime* for definido como menos de 15 minutos, uma exceção será lançada quando você tentar registrar a tarefa em segundo plano.
 

    Por exemplo, este gatilho fará com que uma tarefa em segundo plano seja executada uma vez de hora em hora:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## (Opcional) Adicionar uma condição

-   Se necessário, crie uma condição de tarefa em segundo plano para controlar quando a tarefa será executada. Condições impedem que sua tarefa em segundo plano seja executada até que a condição em questão seja atendida. Para obter mais informações, consulte [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

    Neste exemplo, a condição é definida como **UserPresent** de maneira que, quando acionada, a tarefa seja executada somente quando o usuário estiver ativo. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  Chamar RequestAccessAsync()

-   Antes de tentar registrar a tarefa em segundo plano [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494).

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## Registrar a tarefa em segundo plano

-   Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

> [!Important]
> Para tarefas em segundo plano executadas no mesmo processo do aplicativo, não defina `entryPoint` para tarefas em segundo plano executadas em um processo à parte do aplicativo, defina `entryPoint` para ser o namespace '.' e o nome da classe que contém a implementação da tarefa em segundo plano.

    The following code registers a background task that runs in a separate process:

    > > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Example hourly background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
    > ```

    > **Note**  Background task registration parameters are validated at the time of registration. An error is returned if any of the registration parameters are invalid. Ensure that your app gracefully handles scenarios where background task registration fails - if instead your app depends on having a valid registration object after attempting to register a task, it may crash.


## Comentários

> **Observação**  A partir do Windows 10, o usuário não precisa mais adicionar o aplicativo à tela de bloqueio para usar tarefas em segundo plano. Para obter diretrizes sobre os tipos de gatilhos de tarefa em segundo plano, consulte [Dar suporte ao aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md).

> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano de processo único](create-and-register-a-singleprocess-background-task.md).
* [Criar e registrar uma tarefa em segundo plano que é executada em um processo separado](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Sep16_HO2-->


