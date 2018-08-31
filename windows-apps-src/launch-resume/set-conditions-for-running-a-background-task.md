---
author: TylerMSFT
title: Definir condições para executar uma tarefa em segundo plano
description: Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 556a787eb1e92e4c8adb7457235afb45c02df2dc
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3232648"
---
# <a name="set-conditions-for-running-a-background-task"></a>Definir condições para executar uma tarefa em segundo plano

**APIs importantes**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Saiba como definir condições que controlam quando a sua tarefa em segundo plano será executada.

Às vezes, tarefas em segundo plano exigem que certas condições sejam atendidas para a tarefa em segundo plano seja executado com êxito. Você pode definir uma ou mais das condições especificadas por [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) ao registrar sua tarefa em segundo plano. A condição será verificada após o gatilho já tiver sido disparado. A tarefa em segundo plano será, em seguida, ser colocados em fila, mas ele não será executado até que todas as condições forem atendidas.

Colocar condições em tarefas em segundo plano poupa bateria e CPU, impedindo que tarefas em execução desnecessariamente. Por exemplo, se a sua tarefa em segundo plano é executada com um temporizador e requer conectividade com a Internet, adicione a condição **InternetAvailable** a [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) antes de registrar a tarefa. Isso ajudará a evitar que a tarefa use recursos do sistema e duração da bateria desnecessariamente executando apenas a tarefa em segundo plano quando o temporizador tiver expirado *e* a Internet estiver disponível.

Também é possível integrar várias condições chamando **AddCondition** várias vezes no mesmo [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Tome cuidado para não adicionar condições conflitantes, como **UserPresent**e **UserNotPresent**.

## <a name="create-a-systemcondition-object"></a>Criar um objeto SystemCondition

Este tópico pressupõe que você tenha uma tarefa em segundo plano já associada ao aplicativo e que o aplicativo já inclua um código que cria um objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) que se chama **taskBuilder**.  Consulte [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md) ou [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md) se você precisar criar uma tarefa em segundo plano primeiro.

Este tópico se aplica a tarefas em segundo plano executadas fora do processo, bem como àquelas executadas no mesmo processo do aplicativo em primeiro plano.

Antes de adicionar a condição, crie um objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) para representar a condição que precisa estar em vigor para executar uma tarefa em segundo plano. No construtor, especifique a condição que deve ser atendida com um valor de enumeração [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) .

O código a seguir cria um objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) que especifica **a internetavailable** :

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

Para adicionar a condição, chame o método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) no objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) e passe a ele o objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

O código a seguir usa **taskBuilder** para adicionar **a internetavailable** .

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

Agora você pode registrar sua tarefa em segundo plano com o método de [**registrar**](https://msdn.microsoft.com/library/windows/apps/br224772) e a tarefa em segundo plano não será iniciado até que a condição especificada for atendida.

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
> Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para saber mais, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

> [!NOTE]
> Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo manipula tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## <a name="place-multiple-conditions-on-your-background-task"></a>Inserir várias condições na sua tarefa em segundo plano

Para adicionar várias condições, seu aplicativo faz várias chamadas ao método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) . Essas chamadas devem ser feitas antes que o registro da tarefa se torne efetivo.

> [!NOTE]
> Tome cuidado para não adicionar condições conflitantes a uma tarefa em segundo plano.

O trecho a seguir mostra várias condições no contexto de criar e registrar uma tarefa em segundo plano.

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
> Escolha as condições certas para a sua tarefa em segundo plano para que seja executada somente quando for necessária e não é executado quando não deveria. Confira [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) para ver descrições das diferentes condições de tarefas em tela de fundo.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)
