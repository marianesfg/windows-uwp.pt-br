---
title: Fazer a portabilidade de uma tarefa em segundo plano fora do processo para uma tarefa em segundo plano no processo
description: Portabilidade de uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano no processo que é executada em seu processo de aplicativo em primeiro plano.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano, o serviço de aplicativo
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 97dd249165877591743892a136d51e0969dd902a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8741774"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Fazer a portabilidade de uma tarefa em segundo plano fora do processo para uma tarefa em segundo plano no processo

A maneira mais simples de portar sua atividade de segundo plano fora do processo (OOP) a atividade no processo é trazer seu código de método [ibackgroundtask. Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx?f=255&MSPPError=-2147217396) dentro de seu aplicativo e iniciá-lo de [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). A técnica que está sendo descrita aqui não é sobre a criação de uma correção de uma tarefa de plano de fundo OOP em uma tarefa em segundo plano no processo; seu sobre reconfiguração (ou portabilidade) uma versão OOP para uma versão em processo.

Se seu aplicativo tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano no processo não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas em segundo plano no processo não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceservicingtrigger.aspx) e **IoTStartupTask**
