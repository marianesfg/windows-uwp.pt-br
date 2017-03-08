---
author: TylerMSFT
title: Ciclo de vida de apps UWP do Windows 10
description: "Este tópico descreve o ciclo de vida de um app da Plataforma Universal do Windows (UWP) do Windows 10, do momento em que ele é ativado até quando ele é fechado."
keywords: "ciclo de vida do app suspenso, ativar o início da retomada"
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 75b7dde9ba658cc427845cb1c1d2cab9c2117d17
ms.lasthandoff: 02/07/2017

---

# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>O ciclo de vida do app da Plataforma Universal do Windows (UWP) para Windows 10

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico descreve o ciclo de vida de um app da Plataforma Universal do Windows (UWP), do momento em que ele é iniciado até quando ele é fechado.

## <a name="a-little-history"></a>Uma pequena história

Antes do Windows 8, os apps tinham um ciclo de vida simples. Aplicativos Win32 e .NET eram executados ou não eram. Quando um usuário minimizava-os ou saia deles, eles continuavam em execução. Isso não era um problema até que os dispositivos portáteis e o gerenciamento de energia se tornassem cada vez mais importantes.

O Windows 8 introduziu um novo modelo de app com os aplicativos da Windows Store. Em um nível alto, um novo estado suspenso foi adicionado. Um aplicativo da Windows Store é suspenso assim que o usuário minimiza ou alterna para outro app. Isso significa que os threads do app são interrompidos e o app é deixado na memória, a menos que o sistema operacional precise recuperar recursos. Quando o usuário alterna de volta para o app, ele pode ser restaurado rapidamente para um estado de execução.

Há várias maneiras para os apps que precisam continuar a serem executados quando estão em segundo plano, como [tarefas em segundo plano](support-your-app-with-background-tasks.md), [execução estendida](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.aspx) e atividades responsáveis pela execução (por exemplo, a funcionalidade **BackgroundMediaEnabled** que permite que um app continue a [reproduzir mídia em segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)). Além disso, as operações de transferência em segundo plano podem continuar mesmo se o app for suspenso ou até mesmo encerrado. Para saber mais, consulte [Como baixar um arquivo](https://msdn.microsoft.com/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer).

Por padrão, os apps que não estão em primeiro plano são suspensos, o que resulta em economia de energia e mais recursos disponíveis para o app atualmente em primeiro plano.

Como desenvolvedor, o estado suspenso representa um novo desafio para você, porque o sistema operacional poderá optar por encerrar um app suspenso a fim de liberar recursos. O app encerrado ainda aparecerá na barra de tarefas. Quando o usuário clicar nele, o app deverá ser restaurado ao estado em que estava antes de ser encerrado, pois o usuário não saberá que o app foi fechado pelo sistema. Ele terá a impressão de que o app estava aguardando em segundo plano enquanto fazia outras coisas e espera que o app esteja no mesmo estado que se encontrava quando saiu dele. Neste tópico veremos como fazer isso.

O Windows 10, versão 1607, apresenta mais dois estados de modelo de app: **Running in foreground** e **Running in background**. Também examinaremos esses novos estados nas seções que seguem.

## <a name="app-execution-state"></a>Estado de execução do app

Esta ilustração representa os estados de modelo de app possíveis a partir do Windows 10, versão 1607. Vamos examinar o ciclo de vida típico de um aplicativo da Windows Store.

![diagrama de estado mostrando as transições entre estados de execução do app](images/updated-lifecycle.png)

Os apps entram no estado de execução em segundo plano quando são iniciados ou ativados. Esses termos parecem semelhantes, mas eles se referem a maneiras diferentes pelas quais o sistema operacional pode iniciar o app. Vamos primeiro examinar o processo de inicialização de um app.

## <a name="app-launch"></a>Inicialização do app

O método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) é chamado quando um app é iniciado. Ele passa um parâmetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) que fornece, entre outras coisas, os argumentos passados para o app, o identificador do bloco que iniciou o app e o estado anterior em que o app estava.

Obtenha o estado anterior do app a partir de [Launchactivatedeventargs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate), que retorna um [ApplicationExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.applicationexecutionstate.aspx). Seus valores e a ação apropriada a ser tomada devido a esse estado são as seguintes:

| ApplicationExecutionState | Explicação | Ação a ser tomada |
|-------|-------------|----------------|
| **NotRunning** | Um app poderia estar nesse estado porque ele ainda não foi iniciado desde a última vez que o usuário reinicializou ou fez login. Ele também pode estar nesse estado se estava em execução e travou ou porque o usuário fechou-o anteriormente.| Inicialize o app como se ele estivesse sendo executado pela primeira vez na sessão atual do usuário. |
|**Suspenso** | O usuário minimizou ou alternou para outro app e não retornou a ele dentro de alguns segundos. | Quando o app foi suspenso, seu estado permaneceu na memória. Só é necessário readquirir os identificadores de arquivo ou outros recursos liberados quando o app foi suspenso. |
| **Encerrado** | O app foi suspenso anteriormente, mas depois foi desligado em algum momento porque o sistema precisou recuperar memória. | Restaure o estado em que o app estava quando o usuário alternou para outro app.|
|**ClosedByUser** | O usuário fechou o app com o gesto de fechar no modo tablet ou com Alt+F4. Quando o usuário fecha o app, ele é suspenso primeiro e então é encerrado. | Como o app basicamente seguiu as mesmas etapas que levam ao estado Encerrado, trate isso da mesma maneira que faria com o estado Encerrado.|
|**Running** | O app já foi aberto quando o usuário tentou iniciá-lo novamente. | Nada. Observe que outra instância do app não é iniciada. A instância em execução é simplesmente ativada. |


**Observação**  *A sessão de usuário atual* se baseia no logon no Windows. Desde que o usuário atual não tenha explicitamente feito logoff, desligado, ou reiniciado o Windows, a sessão do usuário atual persiste nos eventos, como autenticação de tela de bloqueio, troca de usuário, etc. 

Uma circunstância importante da qual você deve estar ciente é que, se o dispositivo tiver recursos suficientes, o sistema operacional fará a pré-inicialização dos apps usados com frequência que aceitaram esse comportamento para otimizar a capacidade de resposta. Aplicativos que são pré-inicializados são iniciados em segundo plano e suspensos rapidamente, para que quando o usuário alterne para eles, eles possam ser retomados, o que é mais rápido do que iniciar o app.

Por causa da pré-inicialização, o método **OnLaunched ()** do app pode ser iniciado pelo sistema, em vez de ser iniciado pelo usuário. Como o aplicativo é pré-inicializado em segundo plano, talvez seja necessário executar uma ação diferente em **OnLaunched ()**. Por exemplo, se seu app começa a tocar música quando iniciado, o usuário não saberá de onde ela é proveniente porque o app é pré-inicializado em segundo plano. Depois que o app é pré-inicializado em segundo plano, ele é seguido por uma chamada **Application. Suspending**. Em seguida, quando o usuário iniciar o app, o evento de retomada é invocado, assim como o método **OnLaunched ()**. Consulte [Tratar a pré-inicialização de apps](handle-app-prelaunch.md) para obter informações adicionais sobre como tratar o cenário de pré-inicialização. Somente os apps que realizam a aceitação são pré-inicializados.

O Windows exibe a tela inicial do app quando ele é iniciado. Para configurar a tela inicial, consulte [Adicionando uma tela inicial](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Enquanto a tela inicial é exibida, o app deve registrar manipuladores de eventos e configurar qualquer interface do usuário personalizada necessária para a página inicial. Note que essas tarefas em execução no construtor do app e no **OnLaunched ()** devem ser concluídas dentro de alguns segundos ou o sistema pode achar que seu app não está respondendo e encerrá-lo. Se um app precisa solicitar dados da rede ou recuperar grandes quantidades de dados do disco, essas atividades deverão ser concluídas fora da inicialização. Um app pode usar sua própria interface do usuário de carregamento personalizada ou uma tela inicial estendida enquanto aguarda a conclusão dessas operações de longa duração. Consulte [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) e [Exemplo de tela inicial](http://go.microsoft.com/fwlink/p/?linkid=234889) para saber mais.

Depois que o app conclui a inicialização, ele entra no estado **Running** e a tela inicial desaparece e todos os recursos e objetos são limpos.

## <a name="app-activation"></a>Ativação do app

Em vez de ser iniciado pelo usuário, um app pode ser ativado pelo sistema. Um app pode ser ativado por um contrato, como o contrato de compartilhamento. Ou ele pode ser ativado para tratar com um protocolo de URI personalizado ou um arquivo com uma extensão à qual o app esteja registrado para tratar. Para obter uma lista das maneiras de ativar o app, consulte [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

A classe [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define métodos que você pode substituir para tratar as várias maneiras pelas quais o app pode ser ativado.
[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) pode tratar todos os tipos de ativação possíveis. No entanto, é mais comum usar métodos específicos para tratar dos tipos de ativação mais comuns e usar **OnActivated** somente como o método de contingência para os tipos de ativação menos comuns. Estes são os métodos adicionais para ativações específicas:

[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)  
[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)  
[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)  
[**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)  
[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)  
[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)

Os dados de evento para esses métodos incluem a mesma propriedade [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) vista acima, que informa em qual estado o app estava antes de ser ativado. Interprete o estado e o que você deve fazer da mesma maneira descrita acima na seção [Inicialização do app](#app-launch).

**Observação** Se você fizer logon usando a conta do administrador do computador, não poderá ativar nenhum app UWP.

## <a name="running-in-the-background"></a>Executando em segundo plano ##

A partir do Windows 10, versão 1607, os apps podem executar tarefas em segundo plano no mesmo processo do app em si. Leia mais sobre isso em [Atividade em segundo plano com o modelo de processo único](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99). Não nos aprofundaremos no processamento em segundo plano no processo neste artigo, mas como isso afeta o ciclo de vida do app é o fato de que foram adicionados dois novos eventos que estão relacionados a quando o app está em segundo plano. São eles: [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) e [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground).

Esses eventos também refletem se o usuário pode ver a interface do usuário do app.

Running in the background é o estado padrão no qual um app é iniciado, ativado ou retomado. Nesse estado, a interface do usuário do app ainda não está visível.

## <a name="running-in-the-foreground"></a>Running in the foreground ##

Running in the foreground significa que a interface do usuário do app está visível.

O evento **LeavingBackground** é acionado antes que a interface do usuário do app esteja visível e antes de entrar no estado de execução em primeiro plano. Ele também é acionado quando o usuário alterna de volta para o seu app.

Anteriormente, o melhor local para carregar ativos de interface do usuário era nos manipuladores de eventos **Activated** ou **Resuming**. Agora **LeavingBackground** é o melhor lugar para verificar se sua interface do usuário está pronta.

É importante verificar que os ativos visuais estejam prontos nesse momento porque esta é a última oportunidade de corrigir algo antes que o app fique visível ao usuário. Todo o trabalho de interface do usuário nesse manipulador de eventos deve ser concluído rapidamente, pois afeta o tempo de inicialização e de retomada que o usuário experiencia. **LeavingBackground** é o momento para certificar-se de que o primeiro quadro da interface do usuário esteja pronto. Em seguida, chamadas de armazenamento de execução longa ou chamadas de rede devem ser tratadas de forma assíncrona para que o manipulador de eventos possa retornar.

Quando o usuário sair do app, ele entrará novamente no estado de execução em segundo plano.

## <a name="reentering-the-background-state"></a>Entrando novamente no estado em segundo plano

O evento **EnteredBackground** indica que seu app não está mais visível em primeiro plano. Na área de trabalho, **EnteredBackground** é acionado quando o app é minimizado. No telefone, ao alternar para a tela inicial ou para outro app.

### <a name="reduce-your-apps-memory-usage"></a>Reduzir o uso da memória do app

Uma vez que o app não está mais visível ao usuário, esse é o melhor lugar para interromper a renderização de animações e de trabalho pela interface do usuário. Você pode usar **LeavingBackground** para iniciar esse trabalho novamente.

Se você pretende trabalhar em segundo plano, esse é o local a ser preparado para isso. É recomendável verificar [MemoryManager.AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelevel.aspx) e, se necessário, reduzir a quantidade de memória usada pelo app quando ele está em execução em segundo plano para que o app não corra o risco de ser encerrado pelo sistema para a liberação de recursos.

Consulte [Reduzir o uso da memória quando o app se move para o estado em segundo plano](reduce-memory-usage.md) para saber mais.

### <a name="save-your-state"></a>Salvar seu estado

Anteriormente, o manipulador de eventos de suspensão era o melhor lugar para salvar o estado do app. Mas se você estiver fazendo o trabalho em segundo plano (por exemplo, a reprodução de áudio ou o uso de uma sessão de execução estendida), é melhor salvar seus dados de forma assíncrona a partir do manipulador de eventos **EnteredBackground**. Isso ocorre porque é possível que o app seja encerrado enquanto estiver em segundo plano. E devido ao app não ter passado pelo estado suspenso nesse caso, seus dados serão perdidos.

Salvar seus dados no manipulador de eventos **EnteredBackground** antes do início da atividade em segundo plano garante uma boa experiência do usuário quando o usuário traz o app de volta para o primeiro plano. Você pode usar as APIs de dados do app para salvar dados e configurações. Para saber mais, consulte [Armazene e recupere configurações e outros dados de apps](https://msdn.microsoft.com/library/windows/apps/mt299098).

Depois de salvar seus dados, se estiver acima do limite de uso de memória, você poderá liberar os dados da memória uma vez que pode recarregá-los posteriormente. Isso liberará a memória que pode ser usada pelos ativos necessários para a atividade em segundo plano.

Lembre-se de que se seu app tiver atividade em segundo plano em andamento, ele poderá mudar do estado de execução em segundo plano para o estado de execução em primeiro plano sem nunca entrar no estado suspenso.

### <a name="asynchronous-work-and-deferrals"></a>Trabalho assíncrono e adiamentos

Se você fizer uma chamada assíncrona no manipulador, o controle será retornado imediatamente daquela chamada assíncrona. Isso significa que a execução pode retornar do manipulador de eventos e o app mudará para o próximo estado, mesmo que a chamada assíncrona ainda não tenha sido concluída. Use o método [**GetDeferral**](http://aka.ms/Kt66iv) no objeto [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) que é passado para o manipulador de eventos para atrasar a suspensão até depois que você chamar o método [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) no objeto [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) retornado.

Um adiamento não aumenta a quantidade de código que precisa ser executado antes que o app seja encerrado. Ele somente atrasa o encerramento até que o método *Complete* do adiamento seja chamado ou o prazo termine, *o que ocorrer primeiro*.

Se você precisar de mais tempo para salvar seu estado, investigue maneiras de salvar o estado em estágios antes que o app entre no estado em segundo plano, para que haja menos a ser salvo no manipulador de eventos **EnteredBackground**. Ou você pode solicitar uma [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx) para obter mais tempo. No entanto, não há nenhuma garantia de que a solicitação será concedida, portanto, é melhor encontrar formas de reduzir a quantidade de tempo que você precisa para salvar o estado.

## <a name="app-suspend"></a>Suspensão de app

Quando o usuário minimiza um app, o Windows aguarda alguns segundos para ver se o usuário alternará de volta para ele. Se ele não alternar de volta dentro dessa janela de tempo, e não houver nenhuma execução estendida, tarefa em segundo plano ou atividade responsável pela execução ativa, o Windows suspenderá o app. Um app também é suspenso quando a tela de bloqueio aparece desde que nenhuma sessão de execução estendida, etc. esteja ativa no app.

Quando um app é suspenso, ele invoca o evento [**Application. Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341). Modelos de projeto da UWP do Visual Studio fornecem um manipulador para esse evento chamado **OnSuspending** em **App.xaml.cs**. Antes do Windows 10, versão 1607, era necessário colocar o código para salvar seu estado aqui. Agora, a recomendação é salvar o estado quando você entrar no estado em segundo plano, conforme descrito acima.

Você deve liberar recursos e identificadores de arquivo exclusivos para que outros apps tenham acesso a eles enquanto seu app estiver suspenso. Exemplos de recursos exclusivos incluem câmeras, dispositivos de E/S, dispositivos externos e recursos de rede. Liberar explicitamente os indicadores de arquivos e os recursos exclusivos ajuda a garantir que outros apps possam acessá-los enquanto seu app estiver suspenso. Quando o app for retomado, ele deverá readquirir seus identificadores de arquivos e recursos exclusivos.

### <a name="be-aware-of-the-deadline"></a>Lembre-se do prazo

Para garantir que um dispositivo seja rápido e responsivo, há um limite para o tempo necessário para executar seu código no manipulador de eventos de suspensão. Ele é diferente para cada dispositivo, e você pode descobrir qual está sendo empregado usando uma propriedade do objeto [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) chamada Deadline.

Da mesma maneira que o manipulador de eventos **EnteredBackground**, se você fizer uma chamada assíncrona do manipulador, o controle é retornado imediatamente daquela chamada assíncrona. Isso significa que a execução pode retornar do manipulador de eventos e o app mudará para o estado de suspensão, mesmo que a chamada assíncrona ainda não tenha sido concluída. Use o método [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) no objeto [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponível via argumentos de evento) para atrasar a entrada no estado de suspensão até que você chame o método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) no objeto retornado [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684).

Se precisar de mais tempo, você pode solicitar uma [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx). No entanto, não há nenhuma garantia de que a solicitação será concedida, portanto, é melhor encontrar formas de reduzir o período de tempo que você precisa no manipulador de eventos **Suspended**.

### <a name="app-terminate"></a>Encerramento do app

O sistema tenta manter o app e seus dados na memória enquanto ele está suspenso. Entretanto, se o sistema não tiver recursos suficientes para manter o app na memória, ele encerrará o app. Os apps não recebem uma notificação que estão sendo encerrados, portanto, a única oportunidade que você tem para salvar os dados do app é no manipulador de eventos **OnSuspension**, ou de forma assíncrona no manipulador **EnteredBackground**.

Quando o app determina que foi ativado depois de ter sido encerrado, ele deve carregar os dados que foram salvos para que o app fique no mesmo estado que estava antes do encerramento. Quando o usuário retorna a um app suspenso que tenha sido encerrado, o app deve restaurar seus dados no método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). O sistema não notifica um app quando ele é encerrado, por isso o app deve salvar seus dados de app e liberar identificadores de arquivo e recursos exclusivos antes de ser suspenso e restaurá-los quando for ativado após o encerramento.

**Observação sobre a depuração com Visual Studio:** o Visual Studio impede que o Windows suspenda um app que esteja anexado ao depurador. Isso ocorre para permitir que o usuário exiba a interface de usuário de depuração do Visual Studio enquanto o app está em execução. Quando você está depurando um app, é possível enviar a ele um evento de suspensão usando o Visual Studio. Verifique se a barra de ferramentas **Local de Depuração** está sendo mostrada e clique no ícone **Suspender**.

## <a name="app-resume"></a>Retomada do app

Um app suspenso pode ser retomado quando o usuário alterna para ele ou ele é o app ativo no momento em que o dispositivo sai de um estado de baixo consumo de energia.

Quando um app é retomado do estado **Suspended**, ele entra no estado **Running in background** e o sistema restaura o app de onde ele parou, para que o usuário tenha a impressão de que ele estava em execução o tempo todo. Nenhum dado de app armazenado na memória é perdido. Portanto, a maioria dos apps não precisa restaurar o estado quando são retomados, embora eles devam readquirir qualquer arquivo ou identificador de dispositivo liberados quando eles foram suspensos e restaurar qualquer estado que tenha sido liberado explicitamente quando o app estava suspenso.

O app pode ser suspenso por horas ou dias. Se o app tiver conteúdo ou conexões de rede que possam ter ficado obsoletos, eles deverão ser atualizados quando o app for retomado. Se um app tiver registrado um manipulador de eventos para o evento [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), ele será chamado quando o app for retomado a partir do estado **Suspended**. Você pode atualizar o conteúdo e os dados do app nesse manipulador de eventos.

Se um app suspenso for ativado para participar de uma extensão ou contrato do app, ele receberá primeiro o evento **Resuming** e, depois, o evento **Activated**.

Se o app suspenso for encerrado, não haverá nenhum evento **Resuming**. Em vez disso, **OnLaunched ()** será chamado com um **ApplicationExecutionState** de **Terminated**. Como você salvou seu estado quando o app foi suspenso, poderá restaurar esse estado durante **OnLaunched ()** para que o app apareça como estava quando o usuário alternou para outro app.

Enquanto o app estiver suspenso, ele não receberá nenhum evento de rede que esteja registrado para receber. Esses eventos de rede não são colocados em fila, eles são simplesmente perdidos. Sendo assim, o app deve testar o status da rede quando for retomado.

**Observação**  Como o evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) não é gerado do thread da interface do usuário, um dispatcher deverá ser usado se o código no manipulador de retomada se comunicar com sua interface. Consulte [Atualizar o thread da interface do usuário a partir de um thread em segundo plano](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md) para um código de exemplo sobre como fazer isso.

Para obter diretrizes gerais, consulte [Diretrizes para suspensão e retomada de apps](https://msdn.microsoft.com/library/windows/apps/hh465088).

## <a name="app-close"></a>Fechamento do app

Em geral, os usuários não precisam fechar os apps; eles podem deixar que o Windows os gerencie. No entanto, os usuários podem optar por fechar um app usando o gesto de fechar ou pressionando Alt+F4 ou usando o Alternador de Tarefas no Windows Phone.

Não há um evento para indicar que o usuário fechou o app. Quando um app é fechado pelo usuário, ele é primeiramente suspenso para oferecer a você a oportunidade de salvar seu estado. No Windows 8.1 e versões posteriores, depois que um app é fechado pelo usuário, o app é removido da tela e da lista de alternância, mas não é explicitamente encerrado.

**Fechamento de fechamento pelo usuário:** se o app precisar realizar tarefas diferentes quando é fechado pelo usuário e quando é fechado pelo Windows, use o manipulador de evento para determinar se o app foi encerrado pelo usuário ou pelo Windows. Consulte as descrições dos estados **ClosedByUser** e **Terminated** na referência da enumeração [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

Recomendamos que os apps não sejam fechados programaticamente a menos que isso seja absolutamente necessário. Por exemplo, se um app detectar um vazamento de memória, ele poderá se fechar para garantir a segurança dos dados pessoais do usuário.

## <a name="app-crash"></a>Falha de app

A experiência de falha do sistema foi projetada para que os usuários voltem ao que eles estavam fazendo o mais rápido possível. Você não deve fornecer uma caixa de diálogo de aviso ou outra notificação porque isso atrasará o usuário.

Se o app falhar, parar de responder ou gerar uma exceção, um relatório do problema será enviado para a Microsoft segundo as [configurações de comentários e diagnóstico](http://go.microsoft.com/fwlink/p/?LinkID=614828). A Microsoft fornece um subconjunto dos dados de erro no relatório de problema para você para que você possa usá-lo para melhorar o app. Você pode ver esses dados na página Qualidade do app no Painel.

Quando o usuário ativa um app após um travamento, o manipulador de eventos de ativação recebe em [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) o valor **NotRunning**, e deve exibir os dados e a interface do usuário iniciais. Após um travamento, não use sistematicamente os dados do app que usaria para **Resuming** com **Suspended** porque esses dados podem estar corrompidos; consulte [Guidelines for app suspend and resume](https://msdn.microsoft.com/library/windows/apps/hh465088).

## <a name="app-removal"></a>Remoção do app

Quando um usuário exclui seu app, ele é removido, juntamente com os dados locais. A remoção de um app não afeta os dados do usuário que foram armazenados em locais comuns, como as bibliotecas de Documentos ou Imagens.

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>Ciclo de vida do app e os modelos de projeto do Visual Studio

O código básico que é relevante ao ciclo de vida do app é fornecido nos modelos de projeto do Visual Studio. O app básico trata a ativação de inicialização, fornece um local para a restauração dos dados do app e exibe a interface do usuário principal, mesmo antes que você adicione seu próprio código. Para saber mais, consulte [Modelos de projeto para apps em C#, VB e C++](https://msdn.microsoft.com/library/windows/apps/hh768232).

## <a name="key-application-lifecycle-apis"></a>APIs principais do ciclo de vida do app

-   [Namespace **Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691)
-   [Namespace **Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [Namespace **Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [Classe **Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) (XAML)
-   [Classe **Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) (XAML)

**Observação**  
Este artigo se destina a desenvolvedores do Windows 10 que escrevem apps da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## <a name="related-topics"></a>Tópicos relacionados

* [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)
* [Diretrizes para suspensão e retomada de apps](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Tratar a pré-inicialização do app](handle-app-prelaunch.md)
* [Tratar a ativação do app](activate-an-app.md)
* [Tratar a suspensão do app](suspend-an-app.md)
* [Tratar a retomada do app](resume-an-app.md)
* [Atividade em segundo plano com o modelo de processo único](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [Reproduzir mídia em segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)

 

 

