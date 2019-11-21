---
title: Executar uma tarefa em segundo plano em um temporizador
description: Saiba como agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp, background task
ms.localizationpriority: medium
ms.openlocfilehash: b0d3c9401ff71475e379b2959a1f0cdc03fe8d8b
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260440"
---
# <a name="run-a-background-task-on-a-timer"></a>Executar uma tarefa em segundo plano em um temporizador

Aprenda como usar [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) para agendar uma tarefa ocasional em segundo plano ou executar uma tarefa periódica em segundo plano.

Consulte **Scenario4** no [exemplo de ativação em Segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) para ver um exemplo de como implementar a tarefa em segundo plano disparada por tempo descrita neste tópico.

Este tópico pressupõe que você tenha uma tarefa em segundo plano que precisa ser executada periodicamente ou em um momento específico. Se você ainda não tiver uma tarefa em segundo plano, há um exemplo de tarefa em segundo plano em [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Ou siga as etapas em [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md) ou [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) se você precisar criar uma.

## <a name="create-a-time-trigger"></a>Criar um gatilho de tempo

Crie um novo [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). O segundo parâmetro, *OneShot*, especifica se a tarefa em segundo plano será executada somente uma única vez ou periodicamente. Se *OneShot* for definido como verdadeiro, o primeiro parâmetro (*FreshnessTime*) especificará o número de minutos aguardados até que a tarefa em segundo plano seja agendada. Se *OneShot* for definido como falso, *FreshnessTime* especificará a frequência com que a tarefa em segundo plano será executada.

O temporizador interno para aplicativos da Plataforma Universal do Windows (UWP) destinados à família de dispositivos móveis ou da área de trabalho executa tarefas em segundo plano em intervalos de 15 minutos. (o temporizador é executada em intervalos de 15 minutos para que o sistema precisa ativar apenas uma vez a cada 15 minutos para despertar os aplicativos que solicitaram TimerTriggers, o que economiza energia).

- Se *FreshnessTime* for definido como 15 minutos e *OneShot* for verdadeiro, a tarefa será agendada para ser executada uma vez começando entre 15 e 30 a partir do momento em que for registrada. Se for definido como 25 minutos e *OneShot* for verdadeiro, a tarefa será agendada para ser executada uma vez começando entre 25 e 40 a partir do momento em que for registrada.

- Se *FreshnessTime* for definido como 15 minutos e *OneShot* for falso, a tarefa será agendada para ser executada a cada 15 minutos começando entre 15 e 30 a partir do momento em que for registrada. Se ele estiver definido como n minutos e *OneShot* for falso, a tarefa será agendada para ser executada a cada n minutos começando entre n e n + 15 minutos depois que ele estiver registrado.

> [!NOTE]
> If *FreshnessTime* is set to less than 15 minutes, an exception is thrown when attempting to register the background task.

For example, this trigger will cause a background task to run once an hour.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(Opcional) Adicionar uma condição

Você pode criar uma condição de tarefa em segundo plano para controlar quando a tarefa será executada. Uma condição impede que a tarefa em segundo plano seja executada até a condição ser atendida. Para obter mais informações, consulte [Definir condições para a execução de uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

Neste exemplo, a condição é definida como **UserPresent** de maneira que, quando acionada, a tarefa seja executada somente quando o usuário estiver ativo. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

Para obter informações mais detalhadas sobre condições e tipo de gatilhos em segundo plano, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Chamar RequestAccessAsync()

Antes de registrar a tarefa **ApplicationTrigger** em segundo plano, chame [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar o nível de atividade em segundo plano permitida para o usuário, pois ele pode ter desativado a atividade em segundo plano para o seu aplicativo. Consulte [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre as formas como os usuários podem controlar as configurações de atividade em segundo plano.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano

Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano e ver a definição do método **RegisterBackgroundTask()** no código de exemplo abaixo, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

> [!IMPORTANT]
> For background tasks that run in the same process as your app, do not set `entryPoint`. For background tasks that run in a separate process from your app, set `entryPoint` to be the namespace, '.', and the name of the class that contains your background task implementation.

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## <a name="manage-resources-for-your-background-task"></a>Gerenciar recursos para a tarefa em segundo plano

Use [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar se o usuário decidiu que a atividade em segundo plano do aplicativo deve ser limitada. Lembre-se do uso da bateria e só execute em segundo plano quando for necessário concluir uma ação desejada pelo usuário. Consulte [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre as formas como os usuários podem controlar as configurações de atividade em segundo plano.

- Memória: o ajuste do uso de memória e energia do aplicativo é fundamental para garantir que o sistema operacional permita a execução da tarefa em segundo plano. Use as [APIs de gerenciamento de memória](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) para saber quanta memória a tarefa em segundo plano está usando. Quanto mais memória a tarefa em segundo plano usar, mais difícil fica para o sistema operacional manter o aplicativo em execução quando outro aplicativo está em primeiro plano. O usuário acaba ficando no controle de toda a atividade em segundo plano que o aplicativo pode realizar e tem visibilidade do impacto que o aplicativo tem sobre o uso da bateria.  
- Tempo da CPU: as tarefas em segundo plano são limitadas pela quantidade de tempo de uso que elas obtêm com base no tipo de gatilho.

Consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md) para conhecer as restrições de recursos que se aplicam às tarefas em segundo plano.

## <a name="remarks"></a>Comentários

Starting with Windows 10, it is no longer necessary for the user to add your app to the lock screen in order to utilize background tasks.

Uma tarefa em segundo plano será executada somente usando um **TimeTrigger** se você chamar [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) primeiro.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Background task code sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memória quando seu aplicativo é movido para o segundo plano](reduce-memory-usage.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [How to trigger suspend, resume, and background events in UWP apps (when debugging)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Adiar a suspensão do aplicativo com execução estendida](run-minimized-with-extended-execution.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
