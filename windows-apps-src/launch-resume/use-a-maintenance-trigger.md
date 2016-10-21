---
author: TylerMSFT
title: "Usar um gatilho de manutenção"
description: "Saiba como usar a classe MaintenanceTrigger para executar um código leve em segundo plano enquanto o dispositivo estiver conectado."
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
translationtype: Human Translation
ms.sourcegitcommit: b877ec7a02082cbfeb7cdfd6c66490ec608d9a50
ms.openlocfilehash: 1181605c097f876af49e8055e245a2c445fc30d3

---

# Usar um gatilho de manutenção

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs Importantes**

-   [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Saiba como usar a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para executar um código leve em segundo plano enquanto o dispositivo estiver conectado.

## Criar um objeto de gatilho de manutenção

Este exemplo pressupõe que você tenha um código leve que possa executar em segundo plano para melhorar o aplicativo enquanto o dispositivo está conectado. Este tópico se concentra na [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), que é semelhante a [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Mais informações sobre como escrever uma classe de tarefa em segundo plano estão disponíveis em [Criar e registrar uma tarefa em segundo plano de processo único](create-and-register-a-singleprocess-background-task.md) ou em [Criar e registrar uma tarefa em segundo plano que é executada em um processo separado](create-and-register-a-background-task.md).

Crie um novo objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). O segundo parâmetro, *OneShot*, especifica se a tarefa de manutenção só será executada uma vez ou se continuará a ser executada periodicamente. Se *OneShot* for definido como verdadeiro, o primeiro parâmetro (*FreshnessTime*) especificará o número de minutos aguardados até que a tarefa em segundo plano seja agendada. Se *OneShot* for definido como falso, *FreshnessTime* especificará com que frequência a tarefa em segundo plano será executada.

> **Observação**  Se *FreshnessTime* for definido como menos de 15 minutos, uma exceção será lançada quando você tentar registrar a tarefa em segundo plano.

Esse código de exemplo cria um gatilho que é executada uma vez a cada hora:

> [!div class="tabbedCodeSnippets"]
> ```cs
> uint waitIntervalMinutes = 60;
>
> MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
> ```
> ```cpp
> unsigned int waitIntervalMinutes = 60;
>
> MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
> ```

## (Opcional) Adicionar uma condição

-   Se necessário, crie uma condição de tarefa em segundo plano para controlar quando a tarefa será executada. Uma condição impede que sua tarefa em segundo plano seja executada até que a condição em questão seja atendida. Para obter mais informações, consulte [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

Neste exemplo, a condição é definida como **InternetAvailable** para que a manutenção seja executada quando a Internet estiver (ou ficar) disponível. Para obter uma lista das possíveis condições de tarefa em segundo plano, veja [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

O código a seguir adiciona uma condição ao criador de tarefa de manutenção:

> [!div class="tabbedCodeSnippets"]
> ```cs
> SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
> ```
> ```cpp
> SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
> ```

## Registrar a tarefa em segundo plano

-   Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

    O código a seguir registra a tarefa de manutenção. Ele presume que a tarefa em segundo plano seja executada em um processo à parte do aplicativo porque especifica `entryPoint`. Se a tarefa em segundo plano for executada no mesmo processo do aplicativo, você não especificará `entryPoint`.

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > string entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > string taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```
    > ```cpp
    > String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
    > String ^ taskName   = "Maintenance background task example";
    >
    > BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
    > ```

    > **Observação**  Para todas as famílias de dispositivos, exceto desktop, se o dispositivo estiver com memória insuficiente, as tarefas em segundo plano poderão ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

    > **Observação**  Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

    Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização para o aplicativo, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para saber mais, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

    > **Observações**  Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo manipula tranquilamente cenários em que o registro de tarefas em segundo plano apresenta falha. Se, em vez disso, o aplicativo precisar ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.


> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

****

* [Criar e registrar uma tarefa em segundo plano de processo único](create-and-register-a-singleprocess-background-task.md).
* [Criar e registrar uma tarefa em segundo plano que é executada em um processo separado](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)

****

* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Aug16_HO3-->


