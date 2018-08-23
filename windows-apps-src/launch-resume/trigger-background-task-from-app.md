---
author: TylerMSFT
title: Ativar uma tarefa em segundo plano no seu aplicativo
description: Descreve como disparar uma tarefa de plano de fundo de dentro de um aplicativo
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: gatilho de tarefa do plano de fundo, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2816348"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Ativar uma tarefa em segundo plano no seu aplicativo

Saiba como usar o [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para ativar uma tarefa em segundo plano de dentro de seu aplicativo.

Para obter um exemplo de como criar um gatilho de aplicativo, consulte neste [exemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

Este tópico pressupõe que você tenha uma tarefa de plano de fundo que você deseja ativar a partir do seu aplicativo. Se você ainda não tiver uma tarefa de plano de fundo, há uma tarefa de plano de fundo de amostra em [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). Ou então, siga as etapas em [criar e registrar uma tarefa de plano de fundo de fora do processo](create-and-register-a-background-task.md) para criar um.

## <a name="why-use-an-application-trigger"></a>Por que usar um gatilho de aplicativo

Use um **ApplicationTrigger** para executar código em um processo separado a partir do aplicativo de primeiro plano. Um **ApplicationTrigger** é apropriada se seu aplicativo tiver trabalho que precisa ser feito em segundo plano — mesmo que o usuário feche o aplicativo de primeiro plano. Se o trabalho de plano de fundo deve parar quando o aplicativo for fechado, ou deve estar ligado para o estado do processo de primeiro plano, e em seguida, [Execução estendido](run-minimized-with-extended-execution.md) a ser usado, em vez disso.

## <a name="create-an-application-trigger"></a>Criar um gatilho de aplicativo

Crie um novo [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Você armazená-lo em um campo como é feito no trecho a seguir. Isso é para sua conveniência, para que não precisamos criar uma nova instância mais tarde quando queremos sinalizar o gatilho. Mas você pode usar qualquer instância **ApplicationTrigger** para sinalizar o gatilho.

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

Você pode criar uma condição de tarefa do plano de fundo ao controle quando a tarefa é executada. Uma condição impede que a tarefa de plano de fundo executando até que a condição for atendida. Para obter mais informações, consulte [definir condições para executar uma tarefa do plano de fundo](set-conditions-for-running-a-background-task.md).

Neste exemplo, que a condição é definida como **InternetAvailable** para que, uma vez acionado, a tarefa é executada somente depois que o acesso à internet está disponível. Para obter uma lista das possíveis condições, consulte [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

Para obter informações mais detalhadas sobre as condições e tipos de gatilhos de plano de fundo, consulte [suporte ao seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Chamar RequestAccessAsync()

Antes de registrar a tarefa de plano de fundo **ApplicationTrigger** , chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) para determinar o nível de atividade em segundo plano permite que o usuário porque o usuário pode ter sido desabilitada atividade em segundo plano para seu aplicativo. Consulte [otimizar atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre os usuários de maneiras podem controlar as configurações de atividade em segundo plano.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano

Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano e para ver a definição do método **RegisterBackgroundTask()** no código de exemplo abaixo, consulte [registrar uma tarefa de plano de fundo](register-a-background-task.md).

Se você estiver considerando o uso de um aplicativo de gatilho para estender o tempo de vida do seu processo de primeiro plano, considere o uso [Estendido de execução](run-minimized-with-extended-execution.md) . O gatilho do aplicativo foi projetado para criar um processo separadamente hospedado para trabalhar em. O trecho de código a seguir registra um gatilho fora do processo de plano de fundo.

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

## <a name="trigger-the-background-task"></a>Disparar a tarefa de plano de fundo

Antes de você disparar a tarefa de plano de fundo, use [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para verificar se a tarefa de plano de fundo está registrada. Uma boa hora para verificar se todas as tarefas de plano de fundo estiver registradas é durante a inicialização do aplicativo.

Disparar a tarefa de plano de fundo chamando [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). Qualquer instância **ApplicationTrigger** fará.

Observe que não pode ser chamado **ApplicationTrigger.RequestAsync** da tarefa em segundo plano em si, ou quando o aplicativo está em segundo plano que executa o estado (consulte o [ciclo de vida do aplicativo](app-lifecycle.md) para obter mais informações sobre estados de aplicativo).
Ele pode retornar [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) se o usuário tiver definido as políticas de energia ou de privacidade que impedem que o aplicativo executando atividade em segundo plano.
Além disso, apenas um AppTrigger pode executar ao mesmo tempo. Se você tentar executar um AppTrigger enquanto outro já está em execução, a função retornará [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Gerenciar os recursos de sua tarefa de plano de fundo

Use [Backgroundexecutionmanager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) para determinar se o usuário decidiu que a atividade em segundo plano do aplicativo deve ser limitada. Lembre-se do uso da bateria e só execute em segundo plano quando for necessário concluir uma ação desejada pelo usuário. Consulte [otimizar atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obter mais informações sobre os usuários de maneiras podem controlar as configurações de atividade em segundo plano.  

- Memória: Ajuste o uso de memória e de energia do seu aplicativo é a chave para garantir que o sistema operacional permitirá a execução de sua tarefa de plano de fundo. Use as [APIs de gerenciamento de memória](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) para ver quanta memória sua tarefa de plano de fundo está usando. Usa de sua tarefa de plano de fundo mais memória, mais difícil é para o sistema operacional para mantê-lo funcionando quando outro aplicativo estiver em primeiro plano. O usuário acaba ficando no controle de toda a atividade em segundo plano que o aplicativo pode realizar e tem visibilidade do impacto que o aplicativo tem sobre o uso da bateria.  
- Tempo da CPU: tarefas de plano de fundo estão limitadas pela quantidade de tempo de uso do relógio de parede eles obtém baseados em tipo de disparador. Tarefas de plano de fundo acionadas pelo gatilho de aplicativo são limitadas a aproximadamente 10 minutos.

Consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md) para conhecer as restrições de recursos que se aplicam às tarefas em segundo plano.

## <a name="remarks"></a>Comentários

Começando com o Windows 10, não é necessário para o usuário adicione seu aplicativo para a tela de bloqueio para poder usar as tarefas em segundo plano.

Uma tarefa de plano de fundo só será executado usando uma **ApplicationTrigger** se você tiver chamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) pela primeira vez.

## <a name="related-topics"></a>Tópicos relacionados

* [Diretrizes de tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Exemplo de código de tarefa do plano de fundo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Criar e registrar uma tarefa em segundo plano em processamento](create-and-register-an-inproc-background-task.md).
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memória quando seu app é movido para o segundo plano](reduce-memory-usage.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Adiar a suspensão do app com execução estendida](run-minimized-with-extended-execution.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
