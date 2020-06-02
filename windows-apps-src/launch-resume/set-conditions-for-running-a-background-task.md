---
title: Definir condições para executar uma tarefa em segundo plano
description: Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, tarefa em segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 04f351a2eed5290e31a3f40c5421addf01422154
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262777"
---
# <a name="set-conditions-for-running-a-background-task"></a>Definir condições para executar uma tarefa em segundo plano

**APIs importantes**

- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)
- [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.

Às vezes, tarefas em tela de fundo exigem que certas condições sejam atendidas para que possam ser concluídas com sucesso. Você pode definir uma ou mais das condições especificadas por [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) ao registrar sua tarefa em segundo plano. A condição será verificada depois que o gatilho tiver sido disparado. A tarefa em segundo plano será enfileirada, mas não será executada até que todas as condições necessárias sejam atendidas.

Colocar condições em tarefas em segundo plano poupa bateria e CPU, evitando que as tarefas sejam executadas sem necessidade. Por exemplo, se a sua tarefa em segundo plano é executada com um temporizador e requer conectividade com a Internet, adicione a condição **InternetAvailable** a [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) antes de registrar a tarefa. Isso ajudará a evitar que a tarefa use recursos do sistema e duração da bateria desnecessariamente executando apenas a tarefa em segundo plano quando o temporizador tiver expirado *e* a Internet estiver disponível.

Também é possível combinar várias condições chamando **addcondition** várias vezes no mesmo [**TaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Tome cuidado para não adicionar condições conflitantes, como **UserPresent**e **UserNotPresent**.

## <a name="create-a-systemcondition-object"></a>Criar um objeto SystemCondition

Este tópico pressupõe que você tenha uma tarefa em segundo plano já associada ao aplicativo e que o aplicativo já inclua um código que cria um objeto [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) que se chama **taskBuilder**.  Consulte [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md) ou [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) se você precisar criar uma tarefa em segundo plano primeiro.

Este tópico se aplica a tarefas em segundo plano executadas fora do processo, bem como àquelas executadas no mesmo processo do aplicativo em primeiro plano.

Antes de adicionar a condição, crie um objeto [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) para representar a condição que precisa estar em vigor para uma tarefa em segundo plano ser executada. No construtor, especifique a condição que deve ser atendida com um valor de enumeração [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

O código a seguir cria um objeto [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition) que especifica a condição **InternetAvailable**:

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Adicionar o objeto SystemCondition à sua tarefa em segundo plano

Para adicionar a condição, chame o método [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) no objeto [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) e passe a ele o objeto [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition).

O código a seguir usa **taskBuilder** para adicionar a condição **InternetAvailable**.

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>Registrar sua tarefa em segundo plano

Agora, você pode registrar sua tarefa em segundo plano no método [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register), e a tarefa em segundo plano não será iniciada até a condição especificada ser atendida.

O código a seguir registra a tarefa e armazena o objeto BackgroundTaskRegistration resultante:

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## <a name="place-multiple-conditions-on-your-background-task"></a>Inserir várias condições na sua tarefa em segundo plano

Para adicionar várias condições, seu aplicativo faz várias chamadas ao método [**AddCondition**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.addcondition) . Essas chamadas devem ser feitas antes que o registro da tarefa se torne efetivo.

> [!NOTE]
> Tome cuidado para não adicionar condições conflitantes a uma tarefa em segundo plano.

O trecho a seguir mostra várias condições no contexto de criação e registro de uma tarefa em segundo plano.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Comentários

> [!NOTE]
> Escolha as condições da tarefa em segundo plano para que ela seja executada apenas quando for necessária e não seja executada quando ela não deveria. Confira [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType) para ver descrições das diferentes condições de tarefas em tela de fundo.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo.](create-and-register-a-background-task.md)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
