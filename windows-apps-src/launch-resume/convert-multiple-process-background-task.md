---
author: TylerMSFT
title: "Converter uma tarefa em segundo plano de vários processos em uma tarefa em segundo plano de processo único"
description: "Converta uma tarefa em segundo plano que é executada em um processo separado em uma tarefa em segundo plano que é executada em seu processo de aplicativo em primeiro plano."
translationtype: Human Translation
ms.sourcegitcommit: 2c34ca40d3c930254500477ab5a2e41e5206d823
ms.openlocfilehash: e342667347cf3b89a5aa193495cbf7195263b276

---

# Converter uma tarefa em segundo plano de vários processos em uma tarefa em segundo plano de processo único

A maneira mais simples de converter sua atividade de segundo plano de vários processos em um único processo é trazer seu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) em seu aplicativo e iniciá-lo em [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Se seu aplicativo tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano de processo único não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas em segundo plano de processo único não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**



<!--HONumber=Aug16_HO3-->


