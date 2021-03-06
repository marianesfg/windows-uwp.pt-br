---
title: Diretrizes para tarefas em segundo plano
description: Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 7709e93ba14d3ecf5418accc41a9fe52c968fcec
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262757"
---
# <a name="guidelines-for-background-tasks"></a>Diretrizes para tarefas em segundo plano


Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano.

## <a name="background-task-guidance"></a>Diretrizes da tarefa em segundo plano

Considere as diretrizes a seguir ao desenvolver a tarefa em segundo plano e antes de publicar o aplicativo.

Se você usar uma tarefa em segundo plano para reproduzir mídia em segundo plano, consulte [Reproduzir mídia em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) para obter informações sobre os aprimoramentos no Windows 10, versão 1607, que tornam a tarefa mais fácil.

**Tarefas em segundo plano dentro do processo versus fora do processo:** o Windows 10, versão 1607, introduziu [tarefas em segundo plano dentro do processo](create-and-register-an-inproc-background-task.md) que permitem executar código em segundo plano no mesmo processo que o seu aplicativo em primeiro plano. Leve em consideração os seguintes fatores ao decidir se deseja ter tarefas em segundo plano dentro X fora do processo:

|Consideração | Impacto |
|--------------|--------|
|Resiliência   | Se o seu processo em segundo plano está em execução em outro processo, uma falha em seu processo em segundo plano não prejudica seu aplicativo em primeiro plano. Além disso, a atividade em segundo plano poderá ser encerrada, mesmo em seu aplicativo, se ela ultrapassar os limites de tempo de execução. Separar o trabalho em segundo plano em uma tarefa separada do aplicativo em primeiro plano pode ser uma opção melhor quando não é necessário que os processos em primeiro e segundo planos se comuniquem uns com os outros (uma vez que uma das principais vantagens de tarefas em segundo plano dentro do processo é que eles eliminam a necessidade de comunicação entre processos). |
|Simplicidade    | Tarefas em segundo plano dentro do processo único não exigem comunicação entre processos e são menos complexas de escrever.  |
|Gatilhos disponíveis | As tarefas em segundo plano em andamento não dão suporte aos seguintes gatilhos: [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396), [DeviceServicingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) e **IoTStartupTask**. |
|VoIP | Tarefas em segundo plano dentro do processo único não dão suporte à ativação de uma tarefa em segundo plano VoIP em seu aplicativo. |  

**Limites sobre o número de instâncias de gatilho:** existem limites para a quantidade de instâncias de alguns gatilhos que um aplicativo pode registrar. Um aplicativo pode registrar somente [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger), [MediaProcessingTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger) e [DeviceUseTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396) uma vez a cada instância do aplicativo. Se um aplicativo ultrapassa esse limite, o registro emite uma exceção.

**Cotas de CPU:** as tarefas em segundo plano são limitadas pela quantidade de tempo de uso que elas obtêm com base no tipo de gatilho. A maioria dos gatilhos são limitados a 30 segundos de uso de relógio, embora alguns tenham a capacidade de serem executados até 10 minutos para concluir tarefas de uso intensivo. As tarefas em segundo plano devem ser leves para economizar a duração da bateria e proporcionar melhor experiência do usuário para aplicativos em primeiro plano. Consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md) para conhecer as restrições de recursos que se aplicam às tarefas em segundo plano.

**Gerenciar tarefas em segundo plano:** seu aplicativo deve obter uma lista de tarefas em segundo plano registradas, registrar os manipuladores de progresso e conclusão e tratar esses eventos de forma adequada. Suas classes de tarefa em segundo plano devem relatar progresso, cancelamento e conclusão. Para obter mais informações, consulte [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)e [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md).

**Usar [BackgroundTaskDeferral](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral):**  se sua classe de tarefa em segundo plano executar código assíncrono, use adiamentos. Caso contrário, a tarefa em segundo plano pode ser encerrada prematuramente quando o método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) retorna (ou o método [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) no caso de tarefas em segundo plano em processo). Para obter mais informações, consulte [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)

Se preferir, solicite um adiamento e use **async/await** para concluir as chamadas de método assíncronas. Feche o adiamento após as chamadas do método **await**.

**Atualizar o manifesto do aplicativo:** para tarefas em segundo plano executadas fora do processo, declare cada tarefa em segundo plano no manifesto do aplicativo, juntamente com o tipo dos gatilhos usados. Do contrário, seu aplicativo não vai poder registrar a tarefa em segundo plano no tempo de execução.

Se você tiver várias tarefas em segundo plano, considere se devem ser executadas no mesmo processo de host ou ser divididas em processos de host diferentes. Coloque-as em processos de host separados se estiver preocupado que uma falha na tarefa em segundo possa gerar problemas para as tarefas em segundo plano.  Use a entrada do **Grupo de recursos** no designer de manifesto para agrupar tarefas em segundo plano nos processos de host diferentes. 

Para definir o **Grupo de recursos**, abra o designer de Package.appxmanifest, escolha **Declarações** e adicione uma declaração de **Serviço de aplicativo**:

![Configuração do grupo de recursos](images/resourcegroup.png)

Consulte a [referência de esquemas de aplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) para obter mais informações sobre a configuração do grupo de recursos.

As tarefas em segundo plano que são executadas no mesmo processo do aplicativo em primeiro plano não precisam se declarar no manifesto do aplicativo. Para obter mais informações sobre como declarar tarefas em segundo plano executadas fora do processo no manifesto, consulte [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md).

**Preparar-se para atualizações do aplicativo:** se o seu aplicativo for atualizado, crie e registre uma tarefa em segundo plano **ServicingComplete** (consulte [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)) para cancelar o registro das tarefas em segundo plano para a versão anterior do aplicativo e registre-as para a nova versão. Também é um momento apropriado para realizar atualizações do aplicativo que possam ser necessárias fora do contexto de execução em primeiro plano.

**Solicitar a execução de tarefas em segundo plano:**

> **Importante**    A partir do Windows 10, os aplicativos não precisam mais estar na tela de bloqueio como um pré-requisito para executar tarefas em segundo plano.

Os aplicativos UWP (Plataforma Universal do Windows) podem executar todos os tipos de tarefas com suporte sem serem fixados na tela de bloqueio. No entanto, os aplicativos devem chamar [**Getaccessstate**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) e verificar se o aplicativo não foi negado de ser executado em segundo plano. Verifique se [**GetAccessStatus**] não retorna uma das enumerações negadas do [**BackgroundAccessStatus**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) . Por exemplo, esse método retornará ( https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundAccessStatus) se o usuário tiver explicitamente permissões de tarefa em segundo plano negadas para seu aplicativo nas configurações do dispositivo.

Se seu aplicativo for negado de ser executado em segundo plano, seu aplicativo deverá chamar [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.getaccessstatus) e garantir que a resposta não seja negada antes de registrar tarefas em segundo plano.

Para obter mais informações sobre a escolha do usuário em relação à atividade em segundo plano e economia de bateria, consulte [Otimizar a atividade em segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity). 
## <a name="background-task-checklist"></a>Lista de verificação da tarefa em segundo plano

*Aplica-se a ambas as tarefas em segundo plano dentro e fora do processo*

-   Associe a sua tarefa em segundo plano ao gatilho correto.
-   Adicione condições que ajudem a garantir uma execução bem-sucedida da tarefa em segundo plano.
-   Trate o progresso, a conclusão e o cancelamento de tarefas em segundo plano.
-   Registre novamente suas tarefas em segundo plano durante a inicialização do aplicativo. Isso garante que elas sejam registradas na primeira vez que o aplicativo for iniciado. Além disso, oferece uma forma de detectar se o usuário desabilitou as funcionalidades de execução em segundo plano do seu aplicativo (no caso de falhas de registro).
-   Verifique se há erros de registro de tarefas em segundo plano. Se for apropriado, tente registrar a tarefa em segundo plano novamente com valores de parâmetros diferentes.
-   Para todas as famílias de dispositivos, exceto desktop, caso haja pouca memória, as tarefas em segundo plano podem ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

*Aplica-se apenas a tarefas em segundo plano fora do processo*

-   Crie sua tarefa em segundo plano em um componente Windows Runtime.
-   Não exiba elementos da interface do usuário diferentes de notificações do sistema, blocos e atualizações de selo da tarefa em segundo plano.
-   No método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run), solicite adiamentos para cada chamada de método assíncrona e feche-os quando o método for concluído. Ou use um adiamento com **async/await**.
-   Use o armazenamento persistente para compartilhar dados entre a tarefa em segundo plano e o aplicativo.
-   Declare cada tarefa em segundo plano no manifesto do aplicativo, juntamente com o tipo dos gatilhos usados. Verifique se o ponto de entrada e os tipos de gatilho estão corretos.
-   Não especifique um elemento executável no manifesto, a menos que você esteja usando um gatilho que deva ser executado no mesmo contexto do aplicativo (como o [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)).

*Aplica-se apenas a tarefas em segundo plano dentro do processo*

- Ao cancelar uma tarefa, certifique-se de que o manipulador de eventos `BackgroundActivated` seja encerrado antes que ocorra o cancelamento ou todo o processo será finalizado.
-   Elabore tarefas em segundo plano de curta duração. As tarefas em segundo plano estão limitadas a 30 segundos de uso do relógio.
-   Não conte com a interação do usuário nas tarefas em segundo plano.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano em andamento](create-and-register-an-inproc-background-task.md).
* [Criar e registrar uma tarefa em segundo plano fora do processo.](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do app](declare-background-tasks-in-the-application-manifest.md)
* [Reproduzir mídia em segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)
* [Tratar uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos UWP (durante a depuração)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

 

 
