---
title: Registo de tarefas em segundo plano em grupo
description: Registre/cancele o registro de tarefas em segundo plano como parte de um grupo para isolar esses registros.
ms.date: 04/05/2017
ms.topic: article
keywords: windows 10, tarefa em segundo plano
ms.openlocfilehash: a70c814e5e35359746076c5418d1f1d973e61773
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623851"
---
# <a name="group-background-task-registration"></a>Registo de tarefas em segundo plano em grupo

**APIs importantes**

[Classe BackgroundTaskRegistrationGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup)

Agora, as tarefas em segundo plano podem ser registradas em um grupo, o que você pode pensar como um namespace lógico. Esse isolamento ajuda a garantir que os diferentes componentes de um aplicativo ou bibliotecas diferentes não interfiram no registro de tarefa em segundo plano.

Quando um aplicativo e a estrutura (ou biblioteca) usada registra uma tarefa em segundo plano com o mesmo nome, o aplicativo pode inadvertidamente remover registros de tarefas em segundo plano da estrutura. Os autores de aplicativo também poderiam remover acidentalmente os registros de tarefas em segundo plano da estrutura e da biblioteca, pois podem cancelar o registro de todas as tarefas em segundo plano registradas usando [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks).  Com grupos, você pode isolar os registros de tarefas em segundo plano para que isso não aconteça.

## <a name="features-of-groups"></a>Recursos de grupos

* Grupos podem ser exclusivamente identificados por uma GUID. Eles também podem ter uma cadeia de caracteres de nome amigável associada, que é mais fácil de ler durante a depuração.
* Várias tarefas em segundo plano podem ser registradas em um grupo.
* As tarefas em segundo plano registradas em um grupo não serão exibidas em [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks). Assim, os aplicativos que atualmente usam **BackgroundTaskRegistration.AllTasks** para cancelar o registro das tarefas não cancelar o registro de tarefas em segundo plano registradas em um grupo de modo inadvertido. Ver [cancelar o registro de tarefas em segundo plano em um grupo de](#unregister-background-tasks-in-a-group) abaixo para ver como cancelar o registro de todos os gatilhos de plano de fundo que foram registrados como parte de um grupo.
* Cada Registro de tarefa em segundo plano terá uma propriedade Grupo para determinar o grupo ao qual ele está associado.
* Tarefas em segundo plano de registro em processo com um grupo fará com que a ativação percorrer [BackgroundTaskRegistrationGroup.BackgroundActivated](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistrationgroup.BackgroundActivated) eventos em vez de [Application.OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated#Windows_UI_Xaml_Application_OnBackgroundActivated_Windows_ApplicationModel_Activation_BackgroundActivatedEventArgs_).

## <a name="register-a-background-task-in-a-group"></a>Registrar uma tarefa em segundo plano em um grupo

O item a seguir mostra como registrar uma tarefa em segundo plano (disparada por uma alteração de fuso horário, neste exemplo) como parte de um grupo.

```csharp
private const string groupFriendlyName = "myGroup";
private const string groupId = "3F2504E0-4F89-41D3-9A0C-0305E82C3301";
private const string myTaskName = "My Background Trigger";

public static void RegisterBackgroundTaskInGroup()
{
   BackgroundTaskRegistrationGroup group = BackgroundTaskRegistration.GetTaskGroup(groupId);
   bool isTaskRegistered = false;

   // See if this task already belongs to a group
   if (group != null)
   {
       foreach (var taskKeyValue in group.AllTasks)
       {
           if (taskKeyValue.Value.Name == myTaskName)
           {
               isTaskRegistered = true;
               break;
           }
       }
   }

   // If the background task is not in a group, register it
   if (!isTaskRegistered)
   {
       if (group == null)
       {
           group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
       }

       var builder = new BackgroundTaskBuilder();
       builder.Name = myTaskName;
       builder.TaskGroup = group; // we specify the group, here
       builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));

       // Because builder.TaskEntryPoint is not specified, OnBackgroundActivated() will be raised when the background task is triggered
       BackgroundTaskRegistration task = builder.Register();
   }
}
```

## <a name="unregister-background-tasks-in-a-group"></a>Cancelar o registro de tarefas em segundo plano em um grupo

O item a seguir mostra como cancelar o registro de tarefas em segundo plano registradas como parte de um grupo.
Como tarefas em segundo plano registradas em um grupo não aparecem em [BackgroundTaskRegistration.AllTasks](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.AllTasks), você deve percorrer os grupos, encontrar as tarefas em segundo plano registradas para cada grupo e cancelar o registro deles.

```csharp
private static void UnRegisterAllTasks()
{
    // Unregister tasks that are part of a group
    foreach (var groupKeyValue in BackgroundTaskRegistration.AllTaskGroups)
    {
        foreach (var groupedTask in groupKeyValue.Value.AllTasks)
        {
            groupedTask.Value.Unregister(true); // passing true to cancel currently running instances of this background task
        }
    }

    // Unregister tasks that aren't part of a group
    foreach(var taskKeyValue in BackgroundTaskRegistration.AllTasks)
    {
        taskKeyValue.Value.Unregister(true); // passing true to cancel currently running instances of this background task
    }
}
```

## <a name="register-persistent-events"></a>Registrar eventos persistentes

Ao usar Grupos de registro de tarefas em segundo plano com tarefas em segundo plano em processo, as ativações em segundo plano são direcionadas ao evento do grupo em vez do encontrado no objeto Application ou CoreApplication. Isso permite que vários componentes no aplicativo manipulem a ativação em vez de colocar todos os caminhos de código de ativação no objeto Application. O item a seguir mostra como se registrar no evento ativado em segundo plano do grupo. Primeiro, verifique [BackgroundTaskRegistration.GetTaskGroup](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.gettaskgroup) para determinar se o grupo já foi registrado. Caso contrário, crie um novo grupo com sua id e o nome amigável. Em seguida, registre um manipulador de eventos para o evento BackgroundActivated no grupo.

```csharp
void RegisterPersistentEvent()
{
    var group = BackgroundTaskRegistration.GetTaskGroup(groupId);
    if (group == null)
    {
        group = new BackgroundTaskRegistrationGroup(groupId, groupFriendlyName);
    }

    group.BackgroundActivated += MyEventHandler;
}
```
