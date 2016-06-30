---
author: TylerMSFT
title: Responder a eventos do sistema com tarefas em segundo plano
description: Saiba como criar uma tarefa em segundo plano que responda a eventos de SystemTrigger.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: f6845dce428f5e22ec68744293b1668da52002bf

---

# Responder a eventos do sistema com tarefas em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Saiba como criar uma tarefa em segundo plano que responde a eventos de [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Este tópico pressupõe que você tenha uma classe de tarefa em segundo plano criada para o aplicativo e que essa tarefa precise ser executada em resposta a um evento acionado pelo sistema, como a disponibilização da Internet ou o logon do usuário. Este tópico se concentra na classe [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Para saber mais sobre como criar uma classe de tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md).

## Crie um objeto SystemTrigger


-   No código do seu aplicativo, crie um novo objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). O primeiro parâmetro, *triggerType*, especifica o tipo de gatilho de evento do sistema que ativará esta tarefa em segundo plano. Para obter uma lista dos tipos de evento, consulte [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

    O segundo parâmetro, *OneShot*, especifica se a tarefa em segundo plano será executada uma única vez na próxima vez em que o evento do sistema ocorrer e dispara tarefas em segundo plano ou sempre que o evento do sistema ocorrer, até que o registro da tarefa seja cancelado.

    O código a seguir determina que a tarefa em segundo plano seja executada sempre que a Internet ficar disponível:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
    > ```
    > ```cpp
    > SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
    > ```

## Registrar a tarefa em segundo plano


-   Registre a tarefa em segundo plano chamando sua função de registro de tarefa em segundo plano. Para saber mais sobre como registrar tarefas em segundo plano, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

    O código a seguir registra a tarefa em segundo plano:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Internet-based background task";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
    > ```

    > **Observação** Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

    Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para saber mais, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

    > **Observações** Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo manipula tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

     

## Comentários


Para ver o registro da tarefa em segundo plano em ação, baixe a [amostra de tarefa em segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Tarefas em segundo plano podem ser executadas em resposta aos eventos de [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) e [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) sem entrar na tela de bloqueio, mas ainda será necessário [declarar as tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md). Você também deve chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de tarefa em segundo plano.

Os aplicativos com recurso de tela de bloqueio podem registrar tarefas em segundo plano que respondem aos eventos de [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) e [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831), permitindo que forneçam comunicação em tempo real com o usuário, mesmo quando o aplicativo não está em primeiro plano. Para saber mais, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 
## Tópicos relacionados


****

* [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)

****

* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


