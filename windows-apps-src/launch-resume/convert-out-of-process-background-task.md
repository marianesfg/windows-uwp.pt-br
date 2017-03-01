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
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: f67d3ea2293e50a04bdbb4277fa4ad9e46834473
ms.lasthandoff: 02/08/2017

---

# <a name="convert-an-out-of-process-background-task-to-an-in-process-background-task"></a>Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo

A maneira mais simples de converter sua atividade de segundo plano fora do processo em uma atividade no processo é trazer seu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) em seu app e iniciá-lo em [OnBackgroundActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Se seu app tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano no processo não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas em segundo plano no processo não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**

