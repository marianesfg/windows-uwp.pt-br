---
author: mcleblanc
title: Diretrizes para tarefas em segundo plano
description: Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano.
ms.assetid: 18FF1104-1F73-47E1-9C7B-E2AA036C18ED
---

# Diretrizes para tarefas em segundo plano


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Verifique se seu aplicativo atende aos requisitos para executar tarefas em segundo plano.

## Diretrizes da tarefa em segundo plano


Considere as diretrizes a seguir ao desenvolver a tarefa em segundo plano e antes de publicar o aplicativo.

**Cotas de CPU:  **as tarefas em segundo plano são limitadas pela quantidade de tempo de uso que elas obtêm com base no tipo de gatilho. A maioria dos gatilhos são limitados a 30 segundos de uso de relógio, embora alguns tenham a capacidade de serem executados até 10 minutos para concluir tarefas de uso intensivo. As tarefas em segundo plano devem ser leves para economizar a duração da bateria e proporcionar melhor experiência do usuário para aplicativos em primeiro plano. Consulte [Dar suporte a seu aplicativo com tarefas em segundo plano](support-your-app-with-background-tasks.md) para conhecer as restrições de recursos que se aplicam às tarefas em segundo plano.

**Gerenciar tarefas em segundo plano:  **seu aplicativo deve obter uma lista de tarefas em segundo plano registradas, registrar os manipuladores de progresso e conclusão e manipular esses eventos de forma adequada. Suas classes de tarefa em segundo plano devem relatar progresso, cancelamento e conclusão. Para obter mais informações, consulte [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)e [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)

**Usar [**BackgroundTaskDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700499):  **se sua classe de tarefa em segundo plano executar código assíncrono, use adiamentos. Caso contrário, sua tarefa em segundo plano poderá ser encerrada prematuramente quando o método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) for concluído. Para obter mais informações, consulte [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)

Se preferir, solicite um adiamento e use **async/await** para concluir as chamadas de método assíncronas. Feche o adiamento após as chamadas do método **await**.

**Atualizar o manifesto do aplicativo:  **declare cada tarefa em segundo plano no manifesto do aplicativo, juntamente com o tipo dos gatilhos usados. Do contrário, seu aplicativo não vai poder registrar a tarefa em segundo plano no tempo de execução. Para obter mais informações, consulte [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)

**Preparar-se para atualizações do aplicativo:  **se o aplicativo for atualizado, crie e registre uma tarefa em segundo plano **ServicingComplete** (consulte [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839)) para ajudar a realizar atualizações do aplicativo que possam ser necessárias fora do contexto de execução em primeiro plano.

**Solicitar a execução de tarefas em segundo plano:  **

> **Importante**  A partir do Windows 10, os aplicativos não precisam mais estar na tela de bloqueio para executar tarefas em segundo plano.

Os aplicativos UWP (Plataforma Universal do Windows) podem executar todos os tipos de tarefas com suporte sem serem fixados na tela de bloqueio. No entanto, os aplicativos devem chamar [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar qualquer tipo de tarefa em segundo plano. Esse método retornará [**BackgroundAccessStatus.Denied**](https://msdn.microsoft.com/library/windows/apps/hh700439) se o usuário tiver negado explicitamente permissões de tarefas em segundo plano para seu aplicativo nas configurações do dispositivo.
## Lista de verificação da tarefa em segundo plano


A lista de verificação a seguir aplica-se a todas as tarefas em segundo plano.

-   Associe a sua tarefa em segundo plano ao gatilho correto.
-   Adicione condições que ajudem a garantir uma execução bem-sucedida da tarefa em segundo plano.
-   Manipule o progresso, a conclusão e o cancelamento de tarefas em segundo plano.
-   Não exiba elementos da interface do usuário diferentes de notificações do sistema, blocos e atualizações de selo da tarefa em segundo plano.
-   No método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx), solicite adiamentos para cada chamada de método assíncrona e feche-os quando o método for concluído. Ou use um adiamento com **async/await**
-   Use o armazenamento persistente para compartilhar dados entre a tarefa em segundo plano e o aplicativo.
-   Declare cada tarefa em segundo plano no manifesto do aplicativo, juntamente com o tipo dos gatilhos usados. Verifique se o ponto de entrada e os tipos de gatilho estão corretos.
-   Elabore tarefas em segundo plano de curta duração. As tarefas em segundo plano estão limitadas a 30 segundos de uso do relógio.
-   Não conte com a interação do usuário nas tarefas em segundo plano.
-   Registre novamente suas tarefas em segundo plano durante a inicialização do aplicativo. Isso garante que elas sejam registradas na primeira vez que o aplicativo for iniciado. Além disso, oferece uma forma de detectar se o usuário desabilitou as funcionalidades de execução em segundo plano do seu aplicativo (no caso de falhas de registro).
-   Não especifique um elemento executável no manifesto, a menos que você esteja usando um gatilho que deva ser executado no mesmo contexto do aplicativo (como o [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032))
-   Verifique se há erros de registro de tarefas em segundo plano. Se for apropriado, tente registrar a tarefa em segundo plano novamente com valores de parâmetros diferentes.
-   Para todas as famílias de dispositivos, exceto desktop, caso haja pouca memória, as tarefas em segundo plano podem ser encerradas. Se uma exceção de falta de memória não surgir ou se o aplicativo não manipulá-la, a tarefa em segundo plano será encerrada sem aviso e sem gerar o evento OnCanceled. Isso ajuda a assegurar a experiência do usuário do aplicativo em primeiro plano. A tarefa em segundo plano deve ser projetada para tratar desse cenário.

## Windows: lista de verificação de tarefa em segundo plano para aplicativos com recurso de tela de bloqueio


Siga essas diretrizes ao desenvolver tarefas em segundo plano para aplicativos que podem estar na tela de bloqueio. Siga as diretrizes em [Diretrizes e lista de verificação para blocos da tela de bloqueio](https://msdn.microsoft.com/library/windows/apps/hh465403)

-   Verifique se o seu aplicativo precisa estar na tela de bloqueio antes de desenvolvê-lo contendo recurso de tela de bloqueio. Para obter mais informações, consulte [Visão geral da tela de bloqueio](https://msdn.microsoft.com/library/windows/apps/hh779720)

-   Verifique se o aplicativo continua funcionando fora da tela de bloqueio.

-   Inclua uma tarefa em segundo plano registrada com [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543), [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) ou [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) e a declare no manifesto do aplicativo. Verifique se o ponto de entrada e os tipos de gatilho estão corretos. Isso é necessário para certificação e permite que o usuário coloque o aplicativo na tela de bloqueio.

**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132)

 

## Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano](create-and-register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Manipular uma tarefa em segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Monitorar o progresso e a conclusão de tarefas em segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Responder a eventos do sistema com tarefas em segundo plano](respond-to-system-events-with-background-tasks.md)
* [Definir condições para executar uma tarefa em segundo plano](set-conditions-for-running-a-background-task.md)
* [Atualizar um bloco dinâmico de uma tarefa em segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar um gatilho de manutenção](use-a-maintenance-trigger.md)
* [Executar uma tarefa em segundo plano em um temporizador](run-a-background-task-on-a-timer-.md)
* [Depurar uma tarefa em segundo plano](debug-a-background-task.md)
* [Como disparar eventos de suspensão, retomada e segundo plano em aplicativos da Windows Store (durante a depuração)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 





<!--HONumber=May16_HO2-->


