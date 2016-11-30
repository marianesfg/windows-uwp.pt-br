---
author: TylerMSFT
title: Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo
description: "Converta uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo que é executada em seu processo de aplicativo em primeiro plano."
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: b361a558ecef2030370590eedbef69bd04cf68bd

---

# Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo

A maneira mais simples de converter sua atividade de segundo plano fora do processo em uma atividade no processo é trazer seu código de método [IBackgroundTask.Run](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) em seu aplicativo e iniciá-lo em [OnBackgroundActivated](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx).

Se seu aplicativo tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano no processo não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas em segundo plano no processo não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**



<!--HONumber=Nov16_HO1-->


