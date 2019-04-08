---
title: Usar um gatilho de manutenção
description: Saiba como usar a classe MaintenanceTrigger para executar um código leve em segundo plano enquanto o dispositivo estiver conectado.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 53107ca6add4193737ab0d00497bbe6324bee44f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661851"
---
# <a name="use-a-maintenance-trigger"></a>Usar um gatilho de manutenção

**APIs importantes**

- [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)

Saiba como usar a classe [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) para executar um código leve em segundo plano enquanto o dispositivo estiver conectado.

## <a name="create-a-maintenance-trigger-object"></a>Criar um objeto de gatilho de manutenção

Este exemplo pressupõe que você tenha um código leve que possa executar em segundo plano para melhorar o aplicativo enquanto o dispositivo está conectado. Este tópico se concentra na [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), que é semelhante a [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

Mais informações sobre como escrever uma classe de tarefa em segundo plano estão disponíveis em [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md) ou em [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md).

Crie um novo objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517). O segundo parâmetro, *OneShot*, especifica se a tarefa de manutenção só será executada uma vez ou se continuará a ser executada periodicamente. Se *OneShot* for definido como verdadeiro, o primeiro parâmetro (*FreshnessTime*) especificará o número de minutos aguardados até que a tarefa em segundo plano seja agendada. Se *OneShot* for definido como falso, *FreshnessTime* especificará com que frequência a tarefa em segundo plano será executada.

> [!NOTE]
> Se *FreshnessTime* for definido como menor que 15 minutos, uma exceção é lançada ao tentar registrar a tarefa em segundo plano.

Esse código de exemplo cria um gatilho que é executado uma vez por hora.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(Opcional) Adicionar uma condição

- Se necessário, crie uma condição de tarefa em segundo plano para controlar quando a tarefa será executada. Uma condição impede que sua tarefa em segundo plano seja executada até que a condição em questão seja atendida. Para obter mais informações, consulte [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

Neste exemplo, a condição é definida como **InternetAvailable** para que a manutenção seja executada quando a Internet estiver (ou ficar) disponível. Para obter uma lista das possíveis condições de tarefa em segundo plano, veja [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

O código a seguir adiciona uma condição ao criador de tarefa de manutenção:

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano

- Registre a tarefa em segundo plano chamando sua função de registro da tarefa em segundo plano. Para obter mais informações sobre como registrar tarefas em segundo plano, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

O código a seguir registra a tarefa de manutenção. Ele presume que a tarefa em segundo plano seja executada em um processo à parte do aplicativo porque especifica `entryPoint`. Se a tarefa em segundo plano for executada no mesmo processo do aplicativo, você não especificará `entryPoint`.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> Para todas as famílias de dispositivos, exceto desktop, caso haja pouca memória, as tarefas em segundo plano podem ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

> [!NOTE]
> Aplicativos da plataforma Windows universal devem chamar [ **RequestAccessAsync** ](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer um dos tipos de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização para o aplicativo, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

> [!NOTE]
> Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano no processo](create-and-register-an-inproc-background-task.md).
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar suspender, continuar e eventos em aplicativos UWP do plano de fundo (durante a depuração)](https://go.microsoft.com/fwlink/p/?linkid=254345)
