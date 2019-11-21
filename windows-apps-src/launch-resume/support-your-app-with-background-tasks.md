---
title: Dar suporte a seu app com tarefas em segundo plano
description: Os tópicos nesta seção mostram como fazer um código leve funcionar em segundo plano em resposta aos gatilhos.
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.date: 08/21/2017
ms.topic: article
keywords: Windows 10, UWP, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 7ca567d34c98deb75d7ebfa5ec9f70688ad18fdb
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259414"
---
# <a name="support-your-app-with-background-tasks"></a>Dar suporte a seu app com tarefas em segundo plano


Os tópicos nesta seção mostram como fazer um código leve funcionar em segundo plano em resposta aos gatilhos. Você pode usar tarefas em segundo plano para fornecer funcionalidade quando o seu aplicativo é suspenso ou não está em execução. Também é possível usar tarefas em segundo plano para aplicativos de comunicação em tempo real como VOIP, mail e IM.

## <a name="playing-media-in-the-background"></a>Execução de mídia no segundo plano

Desde o Windows 10, versão 1607, a execução de áudio no segundo plano está muito mais fácil. Consulte [Reproduzir mídia em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obter mais informações.

## <a name="in-process-and-out-of-process-background-tasks"></a>Tarefas em segundo plano dentro do processo e fora do processo

Existem duas abordagens para implementar tarefas em segundo plano:

* No processo: o aplicativo e seu processo em segundo plano são executados no mesmo processo
* Fora do processo: o aplicativo e o processo em segundo plano são executados em processos separados.

O suporte em segundo plano dentro do processo foi introduzido no Windows 10, versão 1607, para simplificar a gravação de tarefas em segundo plano. Mas você ainda pode desenvolver tarefas em segundo plano fora do processo. Consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md) para obter recomendações sobre quando gravar uma tarefa em segundo plano dentro versus fora do processo.

As tarefas em segundo plano fora do processo são mais resilientes, pois o processo em segundo plano não consegue anular o processo do aplicativo se algo der errado. Mas a resiliência vem com o preço de complexidade maior para gerenciar a comunicação dos processos entre o aplicativo e a tarefa em segundo plano.

As tarefas em segundo plano fora do processo são implementadas como classes leves implementadas na interface de [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) pelo sistema operacional em um processo à parte (backgroundtaskhost.exe). Registre uma tarefa em segundo plano usando a classe [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). O nome de classe é usado para especificar o ponto de entrada quando você registra a tarefa em segundo plano.

No Windows 10, versão 1607, você pode habilitar a atividade em segundo plano sem criar uma tarefa em segundo plano. Em vez disso, você pode executar o código em segundo plano diretamente no processo do aplicativo em primeiro plano.

Para começar rapidamente tarefas em segundo plano dentro do processo, consulte [Criar e registrar uma tarefa em segundo plano dentro do processo](create-and-register-an-inproc-background-task.md).

Para começar rapidamente tarefas em segundo plano fora do processo, consulte [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md).

> [!TIP]
> a partir do Windows 10, você não precisa mais inserir um aplicativo na tela de bloqueio como um pré-requisito para registrar uma tarefa em segundo plano para ele.

## <a name="background-tasks-for-system-events"></a>Tarefas em segundo plano de eventos do sistema

O aplicativo pode responder a eventos gerados pelo sistema registrando uma tarefa em segundo plano com a classe [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger). Um aplicativo pode usar qualquer um dos gatilhos de eventos do sistema a seguir (definidos em [**SystemTriggerType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType))

| Nome do gatilho                     | Descrição                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | A Internet fica disponível.                                                                                |
| **NetworkStateChange**           | Ocorre uma mudança de rede, como uma mudança no custo ou na conectividade.                                              |
| **OnlineIdConnectedStateChange** | A ID online associada à conta é alterada.                                                                 |
| **SmsReceived**                  | Um nova mensagem SMS é recebida por um disponível de banda larga móvel instalado.                                         |
| **TimeZoneChange**               | O fuso horário muda no dispositivo (por exemplo, quando o sistema ajusta o relógio para o horário de verão). |

Para obter mais informações, consulte o tópico sobre [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md).

## <a name="conditions-for-background-tasks"></a>Condições para tarefas em segundo plano

Você pode controlar quando a tarefa em segundo plano é executada, mesmo depois que for disparado, adicionando a condição. Depois de disparada, a tarefa em segundo plano não será executada até que todas as suas condições sejam atendidas. As seguintes condições (representadas pela enumeração [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)) podem ser usadas.

| Nome da condição           | Descrição                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | A Internet deve estar disponível.   |
| **InternetNotAvailable** | A Internet deve estar indisponível. |
| **SessionConnected**     | A sessão deve estar conectada.    |
| **SessionDisconnected**  | A sessão deve estar desconectada. |
| **UserNotPresent**       | O usuário deve estar ausente.            |
| **Userpresente**          | O usuário deve estar presente.         |

Adicione a condição **InternetAvailable** à sua tarefa em segundo plano [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para atrasar o disparo da tarefa em segundo plano até que a pilha de rede esteja em execução. Essa condição economiza energia, pois a tarefa em segundo plano não é executada até a rede estar disponível. Essa condição não fornece ativação em tempo real.

Se a tarefa em segundo plano exige a conectividade de rede, defina [IsNetworkRequested](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para garantir que a rede permaneça ativa enquanto a tarefa em segundo plano é executada. Isso solicita que a infraestrutura de tarefas em segundo plano acompanhe a rede enquanto a tarefa está em execução, mesmo se o dispositivo entrar no modo de Espera Conectado. Se a sua tarefa em segundo plano não definir **IsNetworkRequested**, a tarefa em segundo plano não poderá acessar a rede quando estiver no modo de espera conectado (por exemplo, quando a tela de um telefone estiver desligada).  
Para obter mais informações sobre as condições da tarefa em segundo plano, consulte [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

## <a name="application-manifest-requirements"></a>Requisitos do manifesto do aplicativo

Antes que o aplicativo possa registrar com êxito uma tarefa em segundo plano executada fora do processo, ela deve ser declarada no manifesto do aplicativo. As tarefas em segundo plano que são executadas no mesmo processo do aplicativo de host não precisam ser declaradas no manifesto do aplicativo. Para obter mais informações, consulte [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md).

## <a name="background-tasks"></a>Tarefas em segundo plano

Os gatilhos em tempo real a seguir podem ser usados para executar código personalizado leve em segundo plano:

| Gatilho em tempo real  | Descrição |
|--------------------|-------------|
| **Canal de controle** | As tarefas em segundo plano podem manter a conexão ativada e receber mensagens no canal de controle usando o [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Se seu aplicativo estiver ouvindo um soquete, você poderá usar o Agente de Soquete em vez do **ControlChannelTrigger**. Para obter mais detalhes sobre como usar o Agente de Soquete, consulte [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). O **ControlChannelTrigger** não é compatível com o Windows Phone. |
| **Tempo** | É possível executar tarefas em segundo plano a cada 15 minutos, e elas podem ser configuradas para execução em um horário específico com [**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). Para obter mais informações, consulte [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md). |
| **Notificação por push** | As tarefas em segundo plano respondem ao [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) para receber notificações por push brutas. |

**Observação**  

Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) e, em seguida, chame [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) quando seu aplicativo for iniciado após a atualização. Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

**Limites sobre o número de instâncias de gatilho:** existem limites para a quantidade de instâncias de alguns gatilhos que um aplicativo pode registrar. Um aplicativo pode registrar somente [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) e [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) uma vez a cada instância do aplicativo. Se um aplicativo ultrapassa esse limite, o registro emite uma exceção.

## <a name="system-event-triggers"></a>Gatilhos de eventos do sistema

A enumeração [**SystemTriggerType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) representa os gatilhos de eventos do sistema a seguir:

| Nome do gatilho            | Descrição                                                       |
|-------------------------|-------------------------------------------------------------------|
| **Userpresente**         | A tarefa em segundo plano é disparada quando o usuário está presente.   |
| **Desaparecer**            | A tarefa em segundo plano é disparada quando o usuário se ausenta.    |
| **ControlChannelReset** | A tarefa em segundo plano é disparada quando um canal de controle é redefinido. |
| **SessionConnected**    | A tarefa em segundo plano é disparada quando a sessão é conectada.   |

   
Os gatilhos de eventos do sistema a seguir quando o usuário moveu um aplicativo de ou para a tela de bloqueio.

| Nome do gatilho                     | Descrição                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | Um bloco de aplicativos é adicionado à tela de bloqueio.     |
| **LockScreenApplicationRemoved** | Um bloco de aplicativos é removido da tela de bloqueio. |

 
## <a name="background-task-resource-constraints"></a>Restrições de recursos de tarefas em segundo plano

As tarefas em segundo plano são leves. Manter a execução em segundo plano em um nível mínimo garante a melhor experiência do usuário com aplicativos em primeiro plano e para a vida da bateria. Isso é reforçado pela aplicação de restrições de recursos a tarefas em segundo plano.

As tarefas em segundo plano estão limitadas a 30 segundos de uso do relógio.

### <a name="memory-constraints"></a>Restrições de memória

Devido às restrições de recurso para dispositivos de baixa memória, as tarefas em segundo plano podem ter um limite de memória que determina a quantidade máxima de memória que as tarefas em segundo plano podem usar. Se a tarefa em segundo plano tentar uma operação que exceda esse limite, a operação falhará e poderá gerar uma exceção de falta de memória que a tarefa pode manipular. Se a tarefa não manipular a exceção de falta de memória, ou a natureza da operação não tenha gerado uma exceção de falta de memória, a tarefa será encerrada imediatamente.  

Você pode usar as APIs [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) para consultar o uso atual da memória e o limite para descobrir seu limite (se houver), bem como para monitorar o uso de memória contínuo de sua tarefa em segundo plano.

### <a name="per-device-limit-for-apps-with-background-tasks-for-low-memory-devices"></a>Limite por dispositivo para aplicativos com tarefas em segundo plano para dispositivos de pouca memória

Em dispositivos com memória restrita, existe um limite quanto ao número de aplicativos que podem ser instalados em um dispositivo e usam tarefas em segundo plano em um determinado tempo. Se esse número for excedido, a chamada para [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync), que é necessária para todas as tarefas em segundo plano, falhará.

### <a name="battery-saver"></a>Economia de Bateria

A menos que você isente seu aplicativo para que ele ainda possa executar tarefas em segundo plano e receber notificações por push quando a Economia de Bateria estiver ativada, o recurso Economia de Bateria, quando habilitado, impedirá que tarefas em segundo plano sejam executadas quando o dispositivo não estiver conectado à energia externa e a bateria estiver com uma carga restante abaixo do especificado. Isso não o impedirá de registrar tarefas em segundo plano.

No entanto, para aplicativos empresariais e aplicativos que não serão publicados no Microsoft Store, consulte [executar em segundo plano indefinidamente](run-in-the-background-indefinetly.md) para aprender a usar um recurso para executar uma tarefa em segundo plano ou uma sessão de execução estendida em segundo plano indefinidamente.

## <a name="background-task-resource-guarantees-for-real-time-communication"></a>O recurso de tarefa em segundo plano garante a comunicação em tempo real

Para evitar que as cotas de recursos interfiram na funcionalidade de comunicação em tempo real, as tarefas em segundo plano que usam o [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) e o [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) recebem cotas de recursos da CPU garantidas para cada execução de tarefa. As cotas de recursos são conforme mencionadas acima e permanecem constantes para essas tarefas em segundo plano.

O aplicativo não precisa fazer nada além de obter as cotas de recursos garantidas para as tarefas em segundo plano [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) e [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger). O sistema sempre trata essas tarefas em segundo plano como essenciais.

## <a name="maintenance-trigger"></a>Gatilho de manutenção

As tarefas de manutenção são executadas apenas quando o dispositivo está conectado à alimentação AC. Para obter mais informações, consulte [Usar um gatilho de manutenção](use-a-maintenance-trigger.md).

## <a name="background-tasks-for-sensors-and-devices"></a>Tarefas em segundo plano para sensores e dispositivos

Seu aplicativo pode acessar sensores e dispositivos periféricos a partir de uma tarefa em segundo plano com a classe [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). Você pode usar esse gatilho para operações de longa duração, como sincronização ou monitoramento de dados. Ao contrário das tarefas para eventos de sistema, uma tarefa **DeviceUseTrigger** só pode ser disparada enquanto o aplicativo está sendo executado em primeiro plano e nenhuma condição pode ser definida nele.

> [!IMPORTANT]
> O **DeviceUseTrigger** e o **DeviceServicingTrigger** não podem ser usados com tarefas em segundo plano dentro do processo.

Algumas operações críticas de dispositivos, como atualizações de firmware de longa execução não podem ser executadas com o [**DeviceUseTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceUseTrigger). Elas podem ser executadas somente no computador por um aplicativo privilegiado que usa o [**DeviceServicingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.DeviceServicingTrigger). Um *aplicativo privilegiado* é um aplicativo que o fabricante do dispositivo autorizou a executar tais operações. Os metadados do dispositivo são usados para especificar qual aplicativo, se houver, foi designado como o aplicativo privilegiado para um dispositivo. Para obter mais informações, consulte [sincronização e atualização de dispositivo para aplicativos de dispositivo Microsoft Store](https://msdn.microsoft.com/library/windows/hardware/dn265139(v=vs.85).aspx)

## <a name="managing-background-tasks"></a>Gerenciando tarefas em segundo plano

Tarefas em segundo plano podem indicar o progresso, a conclusão e o cancelamento para o seu aplicativo usando eventos e o armazenamento local. Seu aplicativo também pode capturar exceções lançadas por uma tarefa em segundo plano, além de gerenciar o registro desse tipo de tarefa durante atualizações de aplicativo. Para obter mais informações, consulte:

[Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)  
[Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)

Verifique o registro de tarefa em segundo plano durante a inicialização do aplicativo. Certifique-se de que as tarefas em segundo plano não agrupadas do aplicativo estão presentes em BackgroundTaskBuilder.AllTasks. Registre novamente aqueles que não estão presentes. Cancele o registro de todas as tarefas que não são mais necessárias. Isso garante que todos os registros de tarefas em segundo plano são atualizados sempre que o aplicativo é iniciado.

## <a name="related-topics"></a>Tópicos relacionados

**Diretrizes conceituais para multitarefa no Windows 10**

* [Inicialização, retomada e multitarefas](index.md)

**Diretrizes de tarefa em segundo plano relacionadas**

* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Acessar sensores e dispositivos de uma tarefa em segundo plano](access-sensors-and-devices-from-a-background-task.md)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Converter uma tarefa em segundo plano fora do processo em uma tarefa em segundo plano em andamento](convert-out-of-process-background-task.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Registo de tarefas em segundo plano em grupo](group-background-tasks.md)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (ao depurar)](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Reproduzir mídia em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Executar uma tarefa em segundo plano quando seu aplicativo UWP for atualizado](run-a-background-task-during-updatetask.md)
* [Executar em segundo plano indefinidamente](run-in-the-background-indefinetly.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Disparar uma tarefa em segundo plano de seu aplicativo](trigger-background-task-from-app.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
