---
author: TylerMSFT
title: Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo
description: "Converta uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo que é executada em seu processo de app em primeiro plano."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.openlocfilehash: 390e7255e381bdbb004bfe5e0e95f3ccbcbeb490
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo

A maneira mais simples de converter sua atividade de segundo plano fora do processo em uma atividade no processo é trazer seu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) em seu aplicativo e iniciá-lo em [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Se seu aplicativo tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano no processo não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas em segundo plano no processo não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**
