---
author: TylerMSFT
description: Saiba como usar a execução estendida para manter o aplicativo em execução enquanto ele está minimizado
title: Adiar a suspensão do app com execução estendida
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, execução estendida, minimizada, ExtendedExecutionSession, tarefa em segundo plano, ciclo de vida do aplicativo, tela de bloqueio
ms.assetid: e6a6a433-5550-4a19-83be-bbc6168fe03a
ms.localizationpriority: medium
ms.openlocfilehash: 30e05259306a222a3cb18268aeb58a8380f6d4d2
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7444699"
---
# <a name="postpone-app-suspension-with-extended-execution"></a>Adiar a suspensão do app com execução estendida

Este artigo mostra como usar a execução estendida para adiar quando o aplicativo é suspenso, de maneira que ele possa ser executado minimizado ou na tela de bloqueio.

Quando o usuário minimiza ou sai de um aplicativo, este é colocado em um estado suspenso.  A memória é mantida, mas o código não é executado. Isso acontece em todas as edições de sistema operacional com uma interface do usuário visual. Para obter mais detalhes sobre quando o aplicativo é suspenso, consulte [Ciclo de vida do aplicativo](app-lifecycle.md). 

Há casos em que um aplicativo pode precisar continuar em execução, em vez de ser suspenso, quando o usuário sai do aplicativo ou enquanto ele está minimizado. Por exemplo, um aplicativo de contagem de etapas precisa estar em execução e acompanhar as etapas mesmo quando o usuário sai para usar outros aplicativos. 

Se um aplicativo precisar continuar em execução, o sistema operacional poderá mantê-lo em execução ou ele pode solicitar que continue em execução. Por exemplo, durante a reprodução de áudio em segundo plano, o sistema operacional poderá manter um aplicativo em execução por mais tempo se você seguir estas etapas para [Reprodução de mídia em segundo plano](../audio-video-camera/background-audio.md). Do contrário, você deve solicitar manualmente mais tempo. A quantidade de tempo que você talvez receba para realizar a execução em segundo plano pode levar alguns minutos, mas você deve estar preparado para trabalhar com a possibilidade da sessão ser revogada a qualquer momento. Essas restrições de tempo de ciclo de vida do aplicativo são desabilitadas enquanto o aplicativo é executado em um depurador. Por esse motivo, é importante testar a Execução estendida e outras ferramentas para adiar a suspensão do aplicativo durante a execução em um depurador ou usando os Eventos de ciclo de vida disponíveis no Visual Studio. 
 
Crie um [ExtendedExecutionSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionsession.aspx) a fim de solicitar mais tempo para concluir uma operação em segundo plano. O tipo de **ExtendedExecutionSession** criado é determinado pelo [ExtendedExecutionReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.extendedexecutionreason.aspx) fornecido ao criá-lo. Existem três valores de enumeração **ExtendedExecutionReason**: **Unspecified, LocationTracking** e **SavingData**. Apenas uma **ExtendedExecutionSession** pode ser solicitada a qualquer momento; a tentativa de criar outra sessão enquanto uma solicitação de sessão aprovada está ativa fará com que a exceção 0x8007139F seja lançada do construtor **ExtendedExecutionSession**, informando que o grupo ou recurso não está no estado correto para realizar a operação solicitada. Não use [ExtendedExecutionForegroundSession](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundsession.aspx) e [ExtendedExecutionForegroundReason](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.foreground.extendedexecutionforegroundreason.aspx); pois exigem recursos restritos e não estão disponíveis para uso em aplicativos da Store.

## <a name="run-while-minimized"></a>Executar enquanto minimizado

Há dois casos em que a execução estendida pode ser usada:
- Em qualquer ponto durante a execução normal em primeiro plano, enquanto o aplicativo está no estado de execução.
- Depois que o aplicativo recebe um evento de suspensão (o sistema operacional está prestes a mover o aplicativo para o estado suspenso) no identificador de eventos de suspensão do aplicativo.

O código para esses dois casos é o mesmo, mas o comportamento do aplicativo é um pouco diferente em cada um. No primeiro caso, o aplicativo permanece no estado de execução, mesmo se ocorrer um evento que normalmente aciona a suspensão (por exemplo, o usuário sair do aplicativo). O aplicativo nunca receberá um evento de suspensão enquanto a extensão de execução está em vigor. Quando a extensão é descartada, o aplicativo se qualifica para a suspensão novamente.

No segundo caso, se o aplicativo está fazendo a transição para o estado suspenso, ele ficará em estado de suspensão até o período da extensão. Depois que a extensão expira, o aplicativo entra no estado suspenso sem mais notificações.

Use **ExtendedExecutionReason.Unspecified** quando você criar um **ExtendedExecutionSession** para solicitar tempo adicional antes do aplicativo ser movido para o segundo plano em cenários como processamento de mídia, compilação de projetos ou manutenção de uma conexão de rede ativa. Em dispositivos de área de trabalho nos quais o Windows 10 para edições desktop (Home, Pro, Enterprise e Education) esteja em execução, essa é a abordagem a ser usada se um aplicativo precisar evitar ser suspenso enquanto ele estiver minimizado.

Solicite a extensão ao iniciar uma operação de execução longa para adiar a transição de estado **Suspending** que, do contrário, ocorre quando o aplicativo é movido para o plano de fundo. Em dispositivos de desktop, as sessões de execução estendida criadas com **ExtendedExecutionReason.Unspecified** têm um tempo limite com reconhecimento de bateria. Se o dispositivo estiver conectado à tomada, não haverá limite para a duração do período de execução estendida. Se o dispositivo estiver funcionando com bateria, o período de execução estendida poderá durar até dez minutos em segundo plano.

Um usuário de tablet ou laptop pode obter o mesmo comportamento de execução longa às custas da duração da bateria quando a opção **Permitir que o aplicativo execute tarefas em segundo plano** está selecionada nas configurações de **Uso da bateria pelo aplicativo**. Para encontrar essa opção em um notebook, vá até **Configurações** > **Sistema** > **Bateria** > **Uso da bateria pelo aplicativo** (o link sob o percentual de energia da bateria restante) > selecione um aplicativo > desative **Gerenciado pelo Windows** > selecione **Permitir que o aplicativo execute tarefas em segundo plano**.  

Em todas as edições de sistema operacional, esse tipo de sessão de execução estendida para quando o dispositivo entra em modo de espera conectado. Em dispositivos móveis nos quais o Windows 10 Mobile esteja em execução, esse tipo de sessão de execução estendida será executado, desde que a tela esteja ligada. Quando a tela é desligada, o dispositivo tenta entrar imediatamente no modo de espera conectado de baixo consumo de energia. Em dispositivos de desktop, a sessão continuará sendo executada se a tela de bloqueio for exibida. O dispositivo não entrará em modo de espera conectado por um período depois que a tela desligar. Na edição do sistema operacional do Xbox, o dispositivo entra em modo de espera conectado após uma hora, a menos que o usuário altere o padrão.

## <a name="track-the-users-location"></a>Acompanhar o local do usuário

Especifique **ExtendedExecutionReason.LocationTracking** quando você criar um **ExtendedExecutionSession** se o aplicativo precisar registrar em log regularmente o local com base no [Geolocalizador](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx). Aplicativos para monitoramento de exercícios e navegação que precisam monitorar regularmente a localização do usuário e devem usar esse motivo.

Uma sessão de execução estendida de rastreamento de localização pode ser executada por quanto tempo for necessário, incluindo enquanto a tela estiver bloqueada em um dispositivo móvel. No entanto, só pode haver uma sessão assim em execução por dispositivo. Uma sessão de execução estendida de rastreamento do local só pode ser solicitada em primeiro plano, e o aplicativo deve estar no estado **Running**. Isso garante que o usuário esteja ciente de que o aplicativo iniciou uma sessão de rastreamento do local estendida. Ainda é possível usar o Geolocalizador enquanto o aplicativo está em segundo plano usando uma tarefa em segundo plano, ou um serviço de aplicativo, sem solicitar uma sessão de execução estendida de rastreamento do local.

## <a name="save-critical-data-locally"></a>Salvar dados críticos localmente

Especifique **ExtendedExecutionReason.SavingData** quando você criar um **ExtendedExecutionSession** para salvar os dados do usuário caso os dados não sejam salvos antes do aplicativo ser encerrado, o que resultará na perda de dados e em uma experiência do usuário negativa.

Não use esse tipo de sessão para prolongar a vida útil de um aplicativo para carregar ou baixar dados. Se você precisar carregar dados, solicite uma [transferência em segundo plano](https://msdn.microsoft.com/windows/uwp/networking/background-transfers) ou registre um **MaintenanceTrigger** para manusear a transferência quando houver energia CA disponível. A sessão de execução estendida **ExtendedExecutionReason.SavingData** pode ser solicitada quando o aplicativo está em primeiro plano e no estado **Running** ou em segundo plano e no estado **Suspending**.

O estado **Suspending** é a última oportunidade durante o ciclo de vida do aplicativo em que um aplicativo pode fazer o trabalho antes de ser encerrado. **ExtendedExecutionReason.SavingData** é o único tipo de **ExtendedExecutionSession** que pode ser solicitada no estado **Suspending**. A solicitação de uma sessão de execução estendida **ExtendedExecutionReason.SavingData** com o aplicativo no estado **Suspending** cria um possível problema do qual você deve estar ciente. Se for solicitada uma sessão de execução estendida ainda no estado **Suspending** e o usuário solicitar que o aplicativo seja reiniciado, ele poderá demorar muito tempo para ser iniciado. Isso ocorre porque o período da sessão de execução estendida deve terminar antes da instância anterior do aplicativo ser fechada e uma nova instância do aplicativo ser iniciada. O tempo de desempenho de inicialização é sacrificado para garantir que o estado do usuário não seja perdido.

## <a name="request-disposal-and-revocation"></a>Solicitação, alienação e revogação

Existem três interações fundamentais com uma sessão de execução estendida: solicitação, alienação e revogação.  A solicitação é modelada no trecho de código a seguir.

### <a name="request"></a>Solicitação

```csharp
var newSession = new ExtendedExecutionSession();
newSession.Reason = ExtendedExecutionReason.Unspecified;
newSession.Revoked += SessionRevoked;
ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

switch (result)
{
    case ExtendedExecutionResult.Allowed:
        DoLongRunningWork();
        break;

    default:
    case ExtendedExecutionResult.Denied:
        DoShortRunningWork();
        break;
}
```
[Consultar exemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L81-L110)  

A chamada de **RequestExtensionAsync** verifica junto ao sistema operacional se o usuário aprovou uma atividade em segundo plano para o aplicativo e se o sistema tem os recursos disponíveis para permitir a execução em segundo plano. Somente uma sessão será aprovada para um aplicativo a qualquer momento, resultando em chamadas adicionais para **RequestExtensionAsync** a fim de produzir a sessão negada.

É possível verificar o [BackgroundExecutionManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) com antecedência para determinar o [BackgroundAccessStatus](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundaccessstatus.aspx?f=255&MSPPError=-2147217396), que é a configuração do usuário que indica se o aplicativo pode ser executado em segundo plano ou não. Para saber mais sobre essas configurações do usuário, consulte [Atividade em segundo plano e reconhecimento de energia](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#XWK8mEgWD7JHvC10.97).

O **ExtendedExecutionReason** indica a operação que o aplicativo está realizado em segundo plano. A cadeia de caracteres **Description** é uma cadeia de caracteres legível por humanos que explica por que o aplicativo precisa realizar a operação. Essa cadeia de caracteres não é apresentada ao usuário, mas pode ser disponibilizada em uma versão futura do Windows. O manipulador de eventos **Revoked** é necessário para que uma sessão de execução estendida possa ser interrompida normalmente se o usuário, ou o sistema, decidir que o aplicativo não pode mais ser executado em segundo plano.

### <a name="revoked"></a>Revoked

Se um aplicativo tiver uma sessão de execução estendida ativa e o sistema exigir que a atividade em segundo plano seja parada, pois o aplicativo precisa dos recursos, a sessão será revogada. Um período da sessão de execução estendida jamais termina sem antes acionar o manipulador de eventos **Revoked**.

Quando o evento **Revoked** é acionado para uma sessão de execução estendida **ExtendedExecutionReason.SavingData**, o aplicativo tem um segundo para concluir a operação que estava realizando e concluir **Suspending**.

A revogação pode ocorrer por muitos motivos: um limite de tempo de execução foi atingido, uma cota de energia em segundo plano foi alcançada ou a memória precisa ser recuperada para que o usuário abra um novo aplicativo em primeiro plano.

Eis um exemplo de um manipulador de eventos Revoked:

```cs
private async void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        switch (args.Reason)
        {
            case ExtendedExecutionRevokedReason.Resumed:
                rootPage.NotifyUser("Extended execution revoked due to returning to foreground.", NotifyType.StatusMessage);
                break;

            case ExtendedExecutionRevokedReason.SystemPolicy:
                rootPage.NotifyUser("Extended execution revoked due to system policy.", NotifyType.StatusMessage);
                break;
        }

        EndExtendedExecution();
    });
}
```
[Consultar exemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L124-L141)

### <a name="dispose"></a>Dispose

A etapa final é descartar a sessão de execução estendida. Convém descartar a sessão e todos os outros ativos que consomem memória, porque, do contrário, a energia usada pelo aplicativo enquanto ele estiver aguardando a sessão fechar será descontada da cota de energia do aplicativo. Para preservar o máximo da cota de energia para o aplicativo possível, é importante descartar a sessão quando terminar com o trabalho da sessão para que o aplicativo possa ser movido para o estado **Suspended** mais rapidamente.

Descartar a sessão por conta própria, em vez de aguardar o evento de revogação, reduz o uso da cota de energia do aplicativo. Isso significa que o aplicativo terá permissão para ser executado em segundo plano por mais tempo em sessões futuras, pois você terá mais cota de energia disponível para isso. Você deve manter uma referência para o objeto **ExtendedExecutionSession** até o final da operação, de maneira que possa chamar o método **Dispose**.

Um trecho que descarta uma sessão de execução estendida a seguir:

```cs
void ClearExtendedExecution(ExtendedExecutionSession session)
{
    if (session != null)
    {
        session.Revoked -= SessionRevoked;
        session.Dispose();
        session = null;
    }
}
```
[Consultar exemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario1_UnspecifiedReason.xaml.cs#L49-L63)

Um aplicativo só pode ter uma **ExtendedExecutionSession** ativa por vez. Muitos aplicativos usam tarefas assíncronas para concluir operações complexas que exigem acesso a recursos, como armazenamento, rede ou serviços baseados em rede. Se uma operação exigir várias tarefas assíncronas para ser concluída, o estado de cada uma dessas tarefas deverá ser levado em conta antes de descartar a **ExtendedExecutionSession** e permitir que o aplicativo seja suspenso. Isso exige que a referência conte o número de tarefas que ainda estão em execução e não descarte a sessão até esse valor atingir zero.

Veja um código de exemplo para gerenciar várias tarefas durante uma período de sessão de execução estendida. Para obter mais informações sobre como usar esse recurso em seu aplicativo, consulte o exemplo de código a seguir:

```cs
static class ExtendedExecutionHelper
{
    private static ExtendedExecutionSession session = null;
    private static int taskCount = 0;

    public static bool IsRunning
    {
        get
        {
            if (session != null)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

    public static async Task<ExtendedExecutionResult> RequestSessionAsync(ExtendedExecutionReason reason, TypedEventHandler<object, ExtendedExecutionRevokedEventArgs> revoked, String description)
    {
        // The previous Extended Execution must be closed before a new one can be requested.       
        ClearSession();

        var newSession = new ExtendedExecutionSession();
        newSession.Reason = reason;
        newSession.Description = description;
        newSession.Revoked += SessionRevoked;

        // Add a revoked handler provided by the app in order to clean up an operation that had to be halted prematurely
        if(revoked != null)
        {
            newSession.Revoked += revoked;
        }

        ExtendedExecutionResult result = await newSession.RequestExtensionAsync();

        switch (result)
        {
            case ExtendedExecutionResult.Allowed:
                session = newSession;
                break;
            default:
            case ExtendedExecutionResult.Denied:
                newSession.Dispose();
                break;
        }
        return result;
    }

    public static void ClearSession()
    {
        if (session != null)
        {
            session.Dispose();
            session = null;
        }

        taskCount = 0;
    }

    public static Deferral GetExecutionDeferral()
    {
        if (session == null)
        {
            throw new InvalidOperationException("No extended execution session is active");
        }

        taskCount++;
        return new Deferral(OnTaskCompleted);
    }

    private static void OnTaskCompleted()
    {
        if (taskCount > 0)
        {
            taskCount--;
        }
        
        //If there are no more running tasks than end the extended lifetime by clearing the session
        if (taskCount == 0 && session != null)
        {
            ClearSession();
        }
    }

    private static void SessionRevoked(object sender, ExtendedExecutionRevokedEventArgs args)
    {
        //The session has been prematurely revoked due to system constraints, ensure the session is disposed
        if (session != null)
        {
            session.Dispose();
            session = null;
        }
        
        taskCount = 0;
    }
}
```
[Consultar exemplo de código](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/ExtendedExecution/cs/Scenario4_MultipleTasks.xaml.cs)

## <a name="ensure-that-your-app-uses-resources-well"></a>Verificar se o aplicativo usa bem os recursos

Ajustar o uso da memória e da energia do aplicativo é fundamental para garantir que o sistema operacional permita que o aplicativo continue sendo executado quando não for mais o aplicativo em primeiro plano. Use as [APIs de gerenciamento de memória](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) para saber quanta memória o aplicativo está usando. Quanto mais memória o aplicativo usa, mais difícil fica para o sistema operacional manter o aplicativo em execução quando outro aplicativo está em primeiro plano. O usuário acaba ficando no controle de toda a atividade em segundo plano que o aplicativo pode realizar e tem visibilidade do impacto que o aplicativo tem sobre o uso da bateria.

Use [Backgroundexecutionmanager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) para determinar se o usuário decidiu que a atividade em segundo plano do aplicativo deve ser limitada. Lembre-se do uso da bateria e só execute em segundo plano quando for necessário concluir uma ação desejada pelo usuário.

## <a name="see-also"></a>Consulte também

[Exemplo de execução estendida](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ExtendedExecution)  
[Ciclo de vida do aplicativo](https://msdn.microsoft.com/windows/uwp/launch-resume/app-lifecycle)  
[Ciclo de vida do aplicativo - Manter aplicativos ativos com tarefas em segundo plano e execução estendida](https://msdn.microsoft.com/en-us/magazine/mt590969.aspx)
[Gerenciamento de memória em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/reduce-memory-usage)  
[Transferências em segundo plano](https://msdn.microsoft.com/windows/uwp/networking/background-transfers)  
[Reconhecimento de bateria e atividade em segundo plano](https://blogs.windows.com/buildingapps/2016/08/01/battery-awareness-and-background-activity/#I2bkQ6861TRpbRjr.97)  
[Classe MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx)  
[Reproduzir mídia em segundo plano](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)  