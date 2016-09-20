---
author: TylerMSFT
title: Dar suporte a seu aplicativo com tarefas em segundo plano
description: "Os tópicos nesta seção mostram como executar seu próprio código leve em segundo plano ao responder a gatilhos com tarefas em segundo plano."
ms.assetid: EFF7CBFB-D309-4ACB-A2A5-28E19D447E32
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 38942aa2a274828cc36677a93d0923beb03060dc

---

# Dar suporte a seu aplicativo com tarefas em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Os tópicos nesta seção mostram como executar seu próprio código leve em segundo plano ao responder a gatilhos com tarefas em segundo plano. As tarefas em segundo plano são classes leves que o SO executa em segundo plano. Você pode usar tarefas em segundo plano para fornecer funcionalidade quando o seu aplicativo é suspenso ou não está em execução. Também é possível usar tarefas em segundo plano para aplicativos de comunicação em tempo real como VOIP, mail e IM.

As tarefas em segundo plano são classes separadas que implementam a interface de [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794). Você registra uma tarefa em segundo plano usando a classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). O nome de classe é usado para especificar o ponto de entrada quando você registra a tarefa em segundo plano.

Para começar rapidamente com uma tarefa em segundo plano, consulte [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md).


            **Dica**  A partir do Windows 10, você não precisa mais colocar um aplicativo na tela de bloqueio para registrar tarefas em segundo plano.

 

## Tarefas em segundo plano de eventos do sistema


O aplicativo pode responder a eventos gerados pelo sistema registrando uma tarefa em segundo plano com a classe [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). Um aplicativo pode usar qualquer um dos gatilhos de eventos do sistema a seguir (definidos em [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839))

| Nome do gatilho                     | Descrição                                                                                                    |
|----------------------------------|----------------------------------------------------------------------------------------------------------------|
| **InternetAvailable**            | A Internet fica disponível.                                                                                |
| **NetworkStateChange**           | Ocorre uma mudança de rede, como uma mudança no custo ou na conectividade.                                              |
| **OnlineIdConnectedStateChange** | A ID online associada à conta é alterada.                                                                 |
| **SmsReceived**                  | Um nova mensagem SMS é recebida por um disponível de banda larga móvel instalado.                                         |
| **TimeZoneChange**               | O fuso horário muda no dispositivo (por exemplo, quando o sistema ajusta o relógio para o horário de verão). |

 

Para obter mais informações, consulte o tópico sobre [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md).

## Condições para tarefas em segundo plano


Você pode controlar quando a tarefa em segundo plano é executada, mesmo depois que for disparado, adicionando a condição. Depois de disparada, a tarefa em segundo plano não será executada até que todas as suas condições sejam atendidas. As seguintes condições (representadas pela enumeração [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)) podem ser usadas.

| Nome da condição           | Descrição                       |
|--------------------------|-----------------------------------|
| **InternetAvailable**    | A Internet deve estar disponível.   |
| **InternetNotAvailable** | A Internet deve estar indisponível. |
| **SessionConnected**     | A sessão deve estar conectada.    |
| **SessionDisconnected**  | A sessão deve estar desconectada. |
| **UserNotPresent**       | O usuário deve estar ausente.            |
| **UserPresent**          | O usuário deve estar presente.         |

 

Para obter mais informações, consulte o tópico sobre [Definir condições para a execução de uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md).

## Requisitos do manifesto do aplicativo


Antes que o aplicativo possa registrar com êxito uma tarefa em segundo plano, ela deve ser declarada no manifesto do aplicativo. Para obter mais informações, consulte [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md).

## Tarefas em segundo plano


Os gatilhos em tempo real a seguir podem ser usados para executar código personalizado leve em segundo plano:


            **Canal de Controle:  **As tarefas em segundo plano podem manter a conexão ativada e receber mensagens no canal de controle usando o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032). Se seu aplicativo estiver ouvindo um soquete, você poderá usar o Agente de Soquete em vez do **ControlChannelTrigger**. Para obter mais detalhes sobre como usar o Agente de Soquete, consulte [SocketActivityTrigger](https://msdn.microsoft.com/library/windows/apps/dn806009). O **ControlChannelTrigger** não é compatível com o Windows Phone.


            **Temporizador:  **É possível executar tarefas em segundo plano a cada 15 minutos, e elas podem ser configuradas para execução em um horário específico com [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). Para obter mais informações, consulte [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md).


            **Notificação por Push:  **As tarefas em segundo plano respondem ao [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) para receber notificações por push brutas.

**Observação**  

Os aplicativos Universais do Windows devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de gatilho em segundo plano.

Para garantir que seu aplicativo Universal do Windows continue a ser executado corretamente depois que você liberar uma atualização, chame [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) e, em seguida, chame [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) quando seu aplicativo for iniciado após a atualização. Para obter mais informações, consulte [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md).

## Gatilhos de eventos do sistema


> 
            **Observação**  A enumeração [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839) inclui os gatilhos de eventos do sistema a seguir.

| Nome do gatilho            | Descrição                                                       |
|-------------------------|-------------------------------------------------------------------|
| **UserPresent**         | A tarefa em segundo plano é disparada quando o usuário está presente.   |
| **UserAway**            | A tarefa em segundo plano é disparada quando o usuário se ausenta.    |
| **ControlChannelReset** | A tarefa em segundo plano é disparada quando um canal de controle é redefinido. |
| **SessionConnected**    | A tarefa em segundo plano é disparada quando a sessão é conectada.   |

 

Os gatilhos de eventos do sistema a seguir tornam possível reconhecer quando o usuário moveu um aplicativo de ou para a tela de bloqueio.

| Nome do gatilho                     | Descrição                                  |
|----------------------------------|----------------------------------------------|
| **LockScreenApplicationAdded**   | Um bloco de aplicativos é adicionado à tela de bloqueio.     |
| **LockScreenApplicationRemoved** | Um bloco de aplicativos é removido da tela de bloqueio. |

 
## Restrições de recursos de tarefas em segundo plano


As tarefas em segundo plano são leves. Manter a execução em segundo plano em um nível mínimo garante a melhor experiência do usuário com aplicativos em primeiro plano e para a vida da bateria. Isso é reforçado pela aplicação de restrições de recursos a tarefas em segundo plano:

-   As tarefas em segundo plano estão limitadas a 30 segundos de uso do relógio.

## Restrições adicionais de recursos de tarefas em segundo plano


### Restrições de memória

Devido às restrições de recurso para dispositivos de baixa memória, as tarefas em segundo plano podem ter um limite de memória que determina a quantidade máxima de memória que as tarefas em segundo plano podem usar. Se a tarefa em segundo plano tentar uma operação que exceda esse limite, a operação falhará e poderá gerar uma exceção de falta de memória que a tarefa pode manipular. Se a tarefa não manipular a exceção de falta de memória, ou a natureza da operação não tenha gerado uma exceção de falta de memória, a tarefa será encerrada imediatamente. Você pode usar as APIs [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) para consultar o uso atual da memória e o limite para descobrir seu limite (se houver), bem como para monitorar o uso de memória contínuo de sua tarefa em segundo plano.

### Limite por dispositivo para aplicativos com tarefas em segundo plano para dispositivos de pouca memória

Em dispositivos com memória restrita, existe um limite quanto ao número de aplicativos que podem ser instalados em um dispositivo e usam tarefas em segundo plano em um determinado tempo. Se esse número for excedido, a chamada para [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485), que é necessária para todas as tarefas em segundo plano, falhará.

### Economia de Bateria

A menos que você isente seu aplicativo para que ele ainda possa executar tarefas em segundo plano e receber notificações por push quando a Economia de Bateria estiver ativada, o recurso Economia de Bateria, quando habilitado, impedirá que tarefas em segundo plano sejam executadas quando o dispositivo não estiver conectado à energia externa e a bateria estiver com uma carga restante abaixo do especificado. Isso não o impedirá de registrar tarefas em segundo plano.

## O recurso de tarefa em segundo plano garante a comunicação em tempo real


Para evitar que as cotas de recursos interfiram na funcionalidade de comunicação em tempo real, as tarefas em segundo plano que usam o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) e o [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) recebem cotas de recursos da CPU garantidas para cada execução de tarefa. As cotas de recursos são conforme mencionadas acima e permanecem constantes para essas tarefas em segundo plano.

O aplicativo não precisa fazer nada além de obter as cotas de recursos garantidas para as tarefas em segundo plano [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) e [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543). O sistema sempre trata essas tarefas em segundo plano como essenciais.

## Gatilho de manutenção


As tarefas de manutenção são executadas apenas quando o dispositivo está conectado à alimentação AC. Para obter mais informações, consulte [Usar um gatilho de manutenção](use-a-maintenance-trigger.md).

## Tarefa em segundo plano para sensores e dispositivos


Seu aplicativo pode acessar sensores e dispositivos periféricos a partir de uma tarefa em segundo plano com a classe [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Você pode usar esse gatilho para operações de longa duração, como sincronização ou monitoramento de dados. Ao contrário das tarefas para eventos de sistema, uma tarefa **DeviceUseTrigger** só pode ser disparada enquanto o aplicativo está sendo executado em primeiro plano e nenhuma condição pode ser definida nele.

Algumas operações críticas de dispositivos, como atualizações de firmware de longa execução não podem ser executadas com o [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Elas podem ser executadas somente no computador por um aplicativo privilegiado que usa o [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Um *aplicativo privilegiado* é um aplicativo que o fabricante do dispositivo autorizou a executar tais operações. Os metadados do dispositivo são usados para especificar qual aplicativo, se houver, foi designado como o aplicativo privilegiado para um dispositivo. Para obter mais informações, consulte [Sincronização e atualização de dispositivos para aplicativos de dispositivos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306619)

## Gerenciando tarefas em segundo plano


Tarefas em segundo plano podem indicar o progresso, a conclusão e o cancelamento para o seu aplicativo usando eventos e o armazenamento local. Seu aplicativo também pode capturar exceções lançadas por uma tarefa em segundo plano, além de gerenciar o registro desse tipo de tarefa durante atualizações de aplicativo. Para obter mais informações, consulte:

[Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)

[Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)

**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


**Orientação conceitual sobre multitarefa no Windows 10**

* [Inicialização, continuação e multitarefa](index.md)

**Diretrizes relacionadas para tarefas em segundo plano**

* [Acessar sensores e dispositivos a partir de uma tarefa em segundo plano](access-sensors-and-devices-from-a-background-task.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Sincronização e atualização de dispositivos para aplicativos de dispositivos da Windows Store](http://go.microsoft.com/fwlink/p/?LinkId=306619)

 

 



<!--HONumber=Jun16_HO5-->


