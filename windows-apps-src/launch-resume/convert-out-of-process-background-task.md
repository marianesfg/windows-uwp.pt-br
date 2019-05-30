---
title: Fazer a portabilidade de uma tarefa em segundo plano fora do processo para uma tarefa em segundo plano no processo
description: A porta de uma tarefa em segundo plano do out-of-process para uma tarefa de plano de fundo em processo executado dentro de seu processo de aplicativo de primeiro plano.
ms.date: 09/19/2018
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano, o serviço de aplicativo
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: 42aaa5600b30924acc84aa61c2a15dbb2a320c10
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366266"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>Fazer a portabilidade de uma tarefa em segundo plano fora do processo para uma tarefa em segundo plano no processo

A maneira mais simples de sua atividade de plano de fundo do out-of-process (OOP) para a atividade no processo da porta é trazer sua [IBackgroundTask.Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) método de código dentro de seu aplicativo e inicie-o de [OnBackgroundActivated ](/uwp/api/windows.ui.xaml.application.onbackgroundactivated). A técnica que está sendo descrita aqui não se trata de criação de uma correção de uma tarefa de plano de fundo OOP para uma tarefa de plano de fundo em processo; é sobre a reconfiguração (ou portabilidade de) uma versão OOP para uma versão em processo.

Se seu aplicativo tem várias tarefas em segundo plano, o [Exemplo de ativação em segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation) mostra como usar `BackgroundActivatedEventArgs.TaskInstance.Task.Name` para identificar qual tarefa está sendo iniciada.

Se você estiver atualmente se comunicando entre processos em primeiro e segundo planos, poderá remover esse código de comunicação e gerenciamento de estado.

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>Tarefas em segundo plano e tipos de gatilho que não podem ser convertidos

* Tarefas em segundo plano no processo não dão suporte à ativação de uma tarefa em segundo plano de VoIP.
* Tarefas do plano de fundo em processo não dão suporte os seguintes gatilhos:  [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) e **IoTStartupTask**
