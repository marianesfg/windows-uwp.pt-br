---
author: TylerMSFT
title: Executar uma tarefa em segundo plano em um temporizador
description: "Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano."
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 3fc1e3efa742ff8ab24f78856872fe322703f152

---

# Executar uma tarefa em segundo plano em um temporizador


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843)
-   [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494)

Aprenda a agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.

-   Este exemplo pressupõe que você tenha uma tarefa em segundo plano que precisa ser executada periodicamente ou em um momento específico para dar suporte ao aplicativo. Uma tarefa em segundo plano só será executada usando um [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) se você tiver chamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485).
-   Este tópico pressupõe que você já tenha criado uma classe de tarefa em segundo plano, inclusive o método Run que é usado como o ponto de entrada da tarefa em segundo plano. Para criar rapidamente uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md). Para obter informações mais detalhadas sobre condições e gatilhos, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

## Criar um gatilho de tempo


-   Crie um novo [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). O segundo parâmetro, *OneShot*, especifica se a tarefa em segundo plano será executada uma única vez ou periodicamente. Se *OneShot* for definido como verdadeiro, o primeiro parâmetro (*FreshnessTime*) especificará o número de minutos aguardados até que a tarefa em segundo plano seja agendada. Se *OneShot* for definido como falso, *FreshnessTime* especificará a frequência com que a tarefa em segundo plano será executada.

    O temporizador interno para aplicativos da Plataforma Universal do Windows (UWP) destinados à família de dispositivos móveis ou da área de trabalho executa tarefas em segundo plano em intervalos de 15 minutos.

    -   Se *FreshnessTime* for definido como 15 minutos e *OneShot* for verdadeiro, a tarefa será executada uma vez começando entre 0 e 15 a partir do momento em que for registrada.

    -   Se *FreshnessTime* for definido como 15 minutos e *OneShot* for falso, a tarefa será executada a cada 15 minutos começando entre 0 e 15 a partir do momento em que for registrada.

    **Observação** Se *FreshnessTime* for definido como menos de 15 minutos, uma exceção será lançada quando você tentar registrar a tarefa em segundo plano.

     

    Por exemplo, este gatilho fará com que uma tarefa em segundo plano seja executada uma vez de hora em hora:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
    > ```
    > ```cpp
    > TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
    > ```

## [!div class="tabbedCodeSnippets"]


-   (Opcional) Adicionar uma condição Se necessário, crie uma condição de tarefa em segundo plano para controlar quando a tarefa será executada.

    Condições impedem que sua tarefa em segundo plano seja executada até que a condição em questão seja atendida. Para obter mais informações, consulte [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md). Neste exemplo, a condição é definida como **UserPresent** de maneira que, quando acionada, a tarefa seja executada somente quando o usuário estiver ativo.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
    > ```
    > ```cpp
    > SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent)
    > ```

##  Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).


-   [!div class="tabbedCodeSnippets"]

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > BackgroundExecutionManager.RequestAccessAsync();
    > ```
    > ```cpp
    > BackgroundExecutionManager::RequestAccessAsync();
    > ```

## Chamar RequestAccessAsync()


-   Antes de tentar registrar a tarefa em segundo plano [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494). [!div class="tabbedCodeSnippets"]

    Registrar a tarefa em segundo plano

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

    > Registre a tarefa em segundo plano chamando sua função de registro de tarefa em segundo plano. Para saber mais sobre como registrar tarefas em segundo plano, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md). O código a seguir registra a tarefa em segundo plano:


## [!div class="tabbedCodeSnippets"]

> **Observações** Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido.

> Verifique se o aplicativo manipula tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar. Comentários


## **Observação**  A partir do Windows 10, o usuário não precisa mais adicionar o aplicativo à tela de bloqueio para usar tarefas em segundo plano.


* [Para obter diretrizes sobre os tipos de gatilhos de tarefa em segundo plano, consulte [Dar suporte ao aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md).](create-and-register-a-background-task.md)
* [**Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows).](declare-background-tasks-in-the-application-manifest.md)
* [Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).](handle-a-cancelled-background-task.md)
* [Tópicos relacionados](monitor-background-task-progress-and-completion.md)
* [Criar e registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](respond-to-system-events-with-background-tasks.md)
* [Manipular uma tarefa em segundo plano cancelada](set-conditions-for-running-a-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Registrar uma tarefa em segundo plano](use-a-maintenance-trigger.md)
* [Responder a eventos do sistema com tarefas em segundo plano](guidelines-for-background-tasks.md)

* [Definir condições para executar uma tarefa em segundo plano](debug-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO5-->


