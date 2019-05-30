---
title: Depurar uma tarefa em segundo plano
description: Aprenda a depurar uma tarefa em segundo plano, incluindo ativação e rastreamento de depuração de tarefas em seguindo plano no log de eventos do Windows.
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.date: 02/08/2017
ms.topic: article
keywords: o Windows 10, uwp, tarefas em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 11ebd180ebc3bc08b418f3b22ebed190bf73c18d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366201"
---
# <a name="debug-a-background-task"></a>Depurar uma tarefa em segundo plano


**APIs importantes**
-   [Windows.ApplicationModel.Background](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)

Aprenda a depurar uma tarefa em segundo plano, incluindo ativação e rastreamento de depuração de tarefas em seguindo plano no log de eventos do Windows.

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>Depurando tarefas em segundo plano fora do processo vs. no processo
Este tópico aborda principalmente tarefas em segundo plano que são executadas em um processo separado do aplicativo host. Se você estiver depurando uma tarefa em segundo plano no processo, não terá um projeto de tarefa em segundo plano separado e poderá definir um ponto de interrupção em **OnBackgroundActivated()** (onde seu código em segundo plano no processo é executado) e consulte a etapa 2 em [Disparar tarefas em segundo plano manualmente para depurar o código da tarefa em segundo plano](#trigger-background-tasks-manually-to-debug-background-task-code)abaixo, para obter instruções sobre como disparar seu código em segundo plano para ser executado.

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>Verificar se o projeto de tarefa em segundo plano está configurado corretamente

Este tópico pressupõe que você já tenha um aplicativo com uma tarefa em segundo plano para depurar. As instruções a seguir são específicas de tarefas em segundo plano que são executadas fora do processo e não se aplicam a tarefas em segundo plano no processo.

-   Em C# e C++, verifique se o projeto principal faz referência ao projeto de tarefa em segundo plano. Se essa referência não estiver em vigor, a tarefa em segundo plano não será incluída no pacote de aplicativo.
-   Em C# e C++, verifique se o **Tipo de saída** do projeto de tarefa em segundo plano é "componente do Tempo de Execução do Windows".
-   A classe de segundo plano deve ser declarada no atributo de ponto de entrada no manifesto do pacote.

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>Disparar tarefas em segundo plano manualmente para depurar o código dessas tarefas

As tarefas em segundo plano podem ser ativadas manualmente por meio do Microsoft Visual Studio. Em seguida, você pode percorrer o código e depurá-lo.

1.  Em C#, insira um ponto de interrupção no método Run da classe em segundo plano (para tarefas em segundo plano no processo, coloque o ponto de interrupção em App.OnBackgroundActivated()) e/ou crie a saída de depuração usando [**System.Diagnostics**](https://docs.microsoft.com/dotnet/api/system.diagnostics?view=netframework-4.7.2).

    Em C++, insira um ponto de interrupção na função Run da classe em segundo plano (para tarefas em segundo plano no processo, coloque o ponto de interrupção em App.OnBackgroundActivated()) e/ou crie a saída de depuração usando [**OutputDebugString**](https://docs.microsoft.com/windows/desktop/api/debugapi/nf-debugapi-outputdebugstringw).

2.  Execute seu aplicativo no depurador e, em seguida, dispare a tarefa em segundo plano usando a barra de ferramentas **Eventos de Ciclo de Vida**. Esse menu suspenso mostra os nomes das tarefas em segundo plano que podem ser ativadas pelo Visual Studio.

    Para que isso funcione, a tarefa em segundo plano já deve ter sido registrada e ainda deve estar aguardando ser acionada. Por exemplo, se a tarefa em segundo plano foi registrada com um TimeTrigger único e esse gatilho já tiver sido disparado, iniciar a tarefa através do Visual Studio não produzirá nenhum efeito.

> [!Note]
> Tarefas em segundo plano usando os seguintes gatilhos não podem ser ativadas dessa maneira: [**Gatilho de aplicativo**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger), [ **MediaProcessing gatilho**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger), [ **ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), [ **PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger)e tarefas em segundo plano usando um [ **SystemTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTrigger) com o [  **SmsReceived** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) o tipo de disparador.  
> O **Gatilho de aplicativo** e o **MediaProcessingTrigger** podem ser sinalizados manualmente no código com `trigger.RequestAsync()`.

![depurando tarefas em segundo plano](images/debugging-activation.png)

3.  Quando a tarefa em segundo plano é ativada, o depurador é anexado a ela e exibe a saída da depuração no VS.

## <a name="debug-background-task-activation"></a>Depurar a ativação de tarefas em segundo plano

> [!NOTE]
> Esta seção é específica de tarefas em segundo plano que são executadas fora do processo e não se aplica a tarefas em segundo plano no processo.

A ativação da tarefa em segundo plano depende de três coisas:

-   O nome e o namespace da classe de tarefa em segundo plano
-   O atributo de ponto de entrada especificado no manifesto do pacote
-   O ponto de entrada especificado pelo seu aplicativo durante o registro da tarefa em segundo plano

1.  Use o Visual Studio para anotar o ponto de entrada da tarefa em segundo plano:

    -   Em C# e C++, anote o nome e o namespace da classe de tarefa em segundo plano especificada no projeto de tarefa em segundo.

2.  Use o designer de manifesto para conferir se a tarefa em segundo plano está declarada corretamente no manifesto do pacote:

    -   Em C# e C++, o atributo de ponto de entrada deve corresponder ao namespace da tarefa em segundo plano, seguido do nome da classe. Por exemplo:  RuntimeComponent1.MyBackgroundTask.
    -   Todo(s) o(s) tipo(s) de gatilho usado(s) com a tarefa também deve(m) ser especificado(s).
    -   O executável NÃO DEVE ser especificado, a não ser que você esteja usando [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) ou [**PushNotificationTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger).

3.  Windows somente. Para ver o ponto de entrada usado pelo Windows para ativar a tarefa em segundo plano, habilite o rastreamento de depuração e use o log de eventos do Windows.

    Se você seguir este procedimento, e o log de eventos mostrar o gatilho ou ponto de entrada incorreto para a tarefa em segundo plano, é sinal de que o seu aplicativo não está registrando essa tarefa corretamente. Para obter ajuda com essa tarefa, consulte [Registrar uma tarefa em segundo plano](register-a-background-task.md).

    1.  Abra o visualizador de eventos acessando a tela inicial e procurando eventvwr.exe.
    2.  Vá para **Logs de aplicativos e serviços**  - &gt; **Microsoft**  - &gt; **Windows**  - &gt; **BackgroundTaskInfrastructure** no Visualizador de eventos.
    3.  No painel de ações, selecione **modo de exibição**  - &gt; **Mostrar Logs analíticos e depuração** para habilitar o log de diagnóstico.
    4.  Selecione o **Log de diagnósticos** e clique em **Habilitar Log**.
    5.  Agora, tente usar seu aplicativo para registrar e ativar a tarefa em segundo plano de novo.
    6.  Examine os logs de diagnósticos para obter detalhes dos erros. Essas informações incluirão o ponto de entrada registrada para a tarefa em segundo plano.

![visualizador de eventos para informações de depuração das tarefas em segundo plano](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>Implantação do pacote Visual Studio e tarefas em segundo plano

Se um aplicativo que utiliza tarefas em segundo plano for implantado com o Visual Studio, e a versão (principal e/ou secundária) especificada no Criador de Manifestos for atualizada, a posterior reimplantação do aplicativo com o Visual Studio poderá causar interrupção nas tarefas em segundo plano do aplicativo. Isso pode ser remediado da seguinte maneira:

-   Use o Windows PowerShell para implantar o aplicativo atualizado (em vez do Visual Studio) executando o script gerado junto com o pacote.
-   Se você já tiver implantado o aplicativo usando o Visual Studio, e suas tarefas em segundo plano estiverem interrompidas, reinicialize ou faça logoff/logon para que essas tarefas voltem a funcionar.
-   Você pode selecionar a opção de depuração "Sempre reinstalar meu pacote" para evitar isso em projetos C#.
-   Aguarde até que o aplicativo esteja pronto para implantação final para incrementar a versão do pacote (não o altere durante a depuração).

## <a name="remarks"></a>Comentários

-   Veja se seu aplicativo verifica os registros de tarefas em segundo plano existentes antes de registrar novamente a tarefa em segundo plano. Vários registros da mesma tarefa em segundo plano podem causar resultados inesperados ao executarem a tarefa em segundo plano mais de uma vez sempre que ela é disparada.
-   Se a tarefa em segundo plano exigir acesso à tela de bloqueio, coloque o aplicativo na tela de bloqueio antes de tentar depurar essa tarefa. Para obter informações sobre como especificar opções de manifesto para aplicativos compatíveis com a tela de bloqueio, consulte [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md).
-   Os parâmetros de registro de tarefas em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Verifique se o aplicativo trata tranquilamente cenários em que o registro de tarefas de segundo plano apresenta falha. Se, em vez disso, o aplicativo depender de ter um objeto de registro válido depois de tentar registrar uma tarefa, ele poderá travar.

Para obter mais informações sobre como usar o VS para depurar uma tarefa em segundo plano, consulte [como disparar suspender, continuar e eventos em aplicativos UWP em segundo plano](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015).

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e registrar uma tarefa em segundo plano fora do processo](create-and-register-a-background-task.md)
* [Criar e registrar uma tarefa em segundo plano em processo](create-and-register-an-inproc-background-task.md)
* [Registrar uma tarefa em segundo plano](register-a-background-task.md)
* [Declarar tarefas em segundo plano no manifesto do aplicativo](declare-background-tasks-in-the-application-manifest.md)
* [Diretrizes para tarefas em segundo plano](guidelines-for-background-tasks.md)
* [Como disparar suspender, continuar e eventos em aplicativos UWP em segundo plano](https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015)
* [Analisando a qualidade do código de aplicativos UWP com análise de código do Visual Studio](https://docs.microsoft.com/visualstudio/test/analyze-the-code-quality-of-store-apps-using-visual-studio-static-code-analysis?view=vs-2015)

 

 
