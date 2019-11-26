---
title: Ativar uma tarefa em segundo plano no seu aplicativo
description: Descreve como disparar uma tarefa em segundo plano em um aplicativo
ms.date: 07/06/2018
ms.topic: article
keywords: gatilho de tarefa em segundo plano, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 3d8917c6ed181607459d6126aa295d270cfea838
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259403"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Ativar uma tarefa em segundo plano no seu aplicativo

Saiba como usar o [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para ativar uma tarefa em segundo plano de dentro de seu aplicativo.

Para obter um exemplo de como criar um gatilho de aplicativo, consulte este [exemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

Este tópico pressupõe que você tenha uma tarefa em segundo plano que deseja ativar a partir do seu aplicativo. Se você ainda não tiver uma tarefa em segundo plano, há um exemplo de tarefa em segundo plano em [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Ou siga as etapas em [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) para criar uma.

## <a name="why-use-an-application-trigger"></a>Por que usar um gatilho de aplicativo

Use um **ApplicationTrigger** para executar um código em um processo separado do aplicativo em primeiro plano. Um **ApplicationTrigger** é adequado caso o aplicativo tenha o trabalho que precisa ser feito em segundo plano, mesmo se o usuário fechar o aplicativo em primeiro plano. Se o trabalho em segundo plano deve parar quando o aplicativo é fechado, ou deve estar relacionado ao estado do processo em primeiro plano, então [Execução estendida](run-minimized-with-extended-execution.md) deve ser usada.

## <a name="create-an-application-trigger"></a>Criar um gatilho de aplicativo

Crie um novo [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Você pode armazená-lo em um campo como é feito no trecho a seguir. Isso é para fins de praticidade, para que não seja necessário criar uma nova instância posteriormente quando queremos sinalizar o gatilho. Mas você pode usar qualquer instância de **ApplicationTrigger** para sinalizar o gatilho.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(Opcional) Adicionar uma condição

Você pode criar uma condição de tarefa em segundo plano para controlar quando a tarefa será executada. Uma condição impede que a tarefa em segundo plano seja executada até a condição ser atendida. Para obter mais informações, consulte [Definir condições para a execução de uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

Neste exemplo, a condição é definida como **InternetAvailable** para que, uma vez disparada, a tarefa seja executada somente quando o acesso à Internet estiver disponível. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

Para obter informações mais detalhadas sobre condições e tipo de gatilhos em segundo plano, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Chamar RequestAccessAsync()

Antes de registrar a tarefa **ApplicationTrigger** em segundo plano, chame [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar o nível de atividade em segundo plano permitida para o usuário, pois ele pode ter desativado a atividade em segundo plano para o seu aplicativo. Consulte [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre as formas como os usuários podem controlar as configurações de atividade em segundo plano.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano

Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano e ver a definição do método **RegisterBackgroundTask()** no código de exemplo abaixo, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

Se você estiver considerando usar um Gatilho de aplicativo para estender a duração de seu processo em primeiro plano, considere usar [Execução estendida](run-minimized-with-extended-execution.md). O Gatilho de aplicativo destina-se à criação de um processo hospedado separadamente para trabalho. O trecho de código a seguir registra um gatilho em segundo plano fora do processo.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## <a name="trigger-the-background-task"></a>Acionar a tarefa em segundo plano

Antes de você disparar a tarefa em segundo plano, use [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para verificar se a tarefa em segundo plano está registrada. É um bom momento para verificar se todas as suas tarefas em segundo plano estão registradas durante a inicialização do aplicativo.

Dispare a tarefa em segundo plano chamando [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). Qualquer instância de **ApplicationTrigger** pode fazer isso.

Observe que **ApplicationTrigger.RequestAsync** não pode ser chamado de tarefa em segundo plano em si ou quando o aplicativo está em segundo plano em estado de execução (consulte [Ciclo de vida do aplicativo](app-lifecycle.md) para obter mais informações sobre estados de aplicativo).
Ele pode retornar [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) se o usuário configurar políticas de energia ou privacidade que impedem o aplicativo de executar a atividade em segundo plano.
Além disso, apenas um AppTrigger pode ser executado por vez. Se você tenta executar um AppTrigger enquanto outro já está em execução, a função retornará [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Gerenciar recursos para a tarefa em segundo plano

Use [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar se o usuário decidiu que a atividade em segundo plano do aplicativo deve ser limitada. Lembre-se do uso da bateria e só execute em segundo plano quando for necessário concluir uma ação desejada pelo usuário. Consulte [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre as formas como os usuários podem controlar as configurações de atividade em segundo plano.  

- Memória: o ajuste do uso de memória e energia do aplicativo é fundamental para garantir que o sistema operacional permita a execução da tarefa em segundo plano. Use as [APIs de gerenciamento de memória](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) para saber quanta memória a tarefa em segundo plano está usando. Quanto mais memória a tarefa em segundo plano usar, mais difícil fica para o sistema operacional manter o aplicativo em execução quando outro aplicativo está em primeiro plano. O usuário acaba ficando no controle de toda a atividade em segundo plano que o aplicativo pode realizar e tem visibilidade do impacto que o aplicativo tem sobre o uso da bateria.  
- Tempo da CPU: as tarefas em segundo plano são limitadas pela quantidade de tempo de uso que elas obtêm com base no tipo de gatilho. As tarefas em segundo plano disparadas pelo gatilho Application são limitadas a cerca de dez minutos.

Consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md) para conhecer as restrições de recursos que se aplicam às tarefas em segundo plano.

## <a name="remarks"></a>Comentários

A partir do Windows 10, não é mais necessário que o usuário adicione seu aplicativo à tela de bloqueio para utilizar tarefas em segundo plano.

Uma tarefa em segundo plano será executada somente usando um **ApplicationTrigger** se você tiver chamado [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) primeiro.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Exemplo de código de tarefa em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Criar e registrar uma tarefa em segundo plano em processamento](create-and-register-an-inproc-background-task.md).
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memória quando seu aplicativo é movido para o segundo plano](reduce-memory-usage.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (ao depurar)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Adiar a suspensão do aplicativo com execução estendida](run-minimized-with-extended-execution.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
