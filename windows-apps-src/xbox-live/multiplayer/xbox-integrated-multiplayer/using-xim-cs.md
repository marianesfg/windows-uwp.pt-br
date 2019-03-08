---
title: Usando XIM (C#)
description: Saiba como usar o Xbox integrado com vários participantes (XIM) com C#.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, com vários participantes integrado do xbox
ms.localizationpriority: medium
ms.openlocfilehash: 92ac7b9897b57de42fa56126b477f4db5b9b74dd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619431"
---
# <a name="using-xim-c"></a>Usando XIM (C#)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Isso é um breve passo a passo sobre como usar XIM C# API. Os desenvolvedores de jogos que desejam acessar XIM por meio de C++ devem ver [XIM usando (C++)](using-xim.md).
- [Usando XIM (C#)](#using-xim-c)
    - [Pré-requisitos](#prerequisites)
    - [Inicialização e inicialização](#initialization-and-startup)
    - [Operações assíncronas e alterações de estado de processamento](#asynchronous-operations-and-processing-state-changes)
    - [Tratamento de IXimPlayer básico](#basic-iximplayer-handling)
    - [Habilitando amigos unir e convidando-os](#enabling-friends-to-join-and-inviting-them)
    - [Emparelhamento básico e movendo a outra rede XIM com outras pessoas](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [Desabilitando a regra NAT de emparelhamento para fins de depuração](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [Sair de uma rede XIM e limpeza](#leaving-a-xim-network-and-cleaning-up)
    - [Enviando e recebendo mensagens](#sending-and-receiving-messages)
    - [Avaliar a qualidade do caminho de dados](#assessing-data-pathway-quality)
    - [Compartilhamento de dados usando as propriedades personalizadas do player](#sharing-data-using-player-custom-properties)
    - [Compartilhamento de dados usando as propriedades personalizadas da rede](#sharing-data-using-network-custom-properties)
    - [Para que haja correspondência usando habilidades por player](#matchmaking-using-per-player-skill)
    - [Para que haja correspondência usando a função por player](#matchmaking-using-per-player-role)
    - [Como XIM funciona com as equipes do player](#how-xim-works-with-player-teams)
    - [Trabalhando com os bate-papo](#working-with-chat)
    - [Mudo players](#muting-players)
    - [Configurar destinos de bate-papo usando as equipes do player](#configuring-chat-targets-using-player-teams)
    - [Preenchimento automático em segundo plano dos slots de player ("aterramento" para que haja correspondência)](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [Redes permite junções de consulta](#querying-joinable-networks)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a codificar com XIM, existem dois pré-requisitos.

1. Você deve ter configurado o AppXManifest do seu aplicativo com recursos de rede com vários participantes padrão e você deve ter configurado a parte de manifesto de rede para declarar o tráfego necessário modelos padrão usados pelo XIM.

    Manifestos de rede e recursos de AppXManifest são descritos mais detalhadamente em suas respectivas seções da documentação plataforma; o XML específico XIM típico para colar é fornecido no [configuração do projeto XIM](xim-manifest.md).

1. Você precisará ter duas partes de informações de identidade de aplicativo disponíveis:

    * O título atribuído do Xbox Live ID.
    * A ID de configuração de serviço fornecida como parte do provisionamento de seu aplicativo para o acesso ao serviço Xbox Live.

    Consulte seu representante da Microsoft para obter mais informações sobre a aquisição de esses. Essas duas informações serão usadas durante a inicialização.

## <a name="initialization-and-startup"></a>Inicialização e inicialização

Começar a interagir com a biblioteca inicializando as propriedades de classe estática XIM com sua cadeia de ID de configuração de serviço Xbox Live e número de identificação do título. Se você também estiver usando a API de serviços do Xbox, talvez seja conveniente para recuperar essas de `Microsoft.Xbox.Services.XboxLiveAppConfiguration`. O exemplo a seguir pressupõe que os valores já residem nas variáveis 'myServiceConfigurationId' e 'myTitleId', respectivamente:

```cs
XboxIntegratedMultiplayer.ServiceConfigurationId = myServiceConfigurationId;
XboxIntegratedMultiplayer.TitleId = myTitleId;
```

Depois de inicializado, o aplicativo deve recuperar as cadeias de caracteres de ID de usuário do Xbox para todos os usuários atualmente no dispositivo local que participarão e passá-los para um `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` chamar. O código de exemplo a seguir pressupõe um único usuário pressionou um botão do controlador de expressar a intenção para reproduzir e a cadeia de caracteres de ID de usuário do Xbox associada com o usuário já foi recuperada em uma variável 'myXuid':

```cs
XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds(new List<string>() { myXuid });
```

Uma chamada para `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` imediatamente define as IDs de usuário do Xbox associados com os usuários locais que devem ser adicionados à rede XIM. Esta lista de IDs de usuário do Xbox será usada em todas as operações de rede futuros até que as alterações de lista por meio de outra chamada para `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`.

No caso em que nenhuma rede XIM em todos os ainda não existe, a primeira etapa é mover para uma rede XIM a fim de que o processo iniciado. Se o usuário ainda não tiver uma rede XIM específica em mente, a prática recomendada é simplesmente mover a uma rede nova e vazia. Isso permite que os amigos do usuário ingressar na rede como uma espécie de "lobby" da qual eles podem colaborar em conjunto para selecionar sua próxima atividade com vários participantes (por exemplo, inserindo o relacionamento de pessoas).

A seguir está um exemplo de como mover apenas os usuários locais adicionados anteriormente com `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` a uma rede XIM vazia com espaço para até 8 players total:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringOnlyLocalPlayers);
```

A chamada para `XboxIntegratedMultiplayer.MovetoNewNetwork()` mover assíncrona inicia a operação que termina com um evento de alteração de estado que os desenvolvedores devem processar regularmente para.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Operações assíncronas e alterações de estado de processamento

O coração do XIM é chamadas de regulares e frequentes do aplicativo para o `XboxIntegratedMultiplayer.GetStateChanges()` método. Esse método é como XIM é informado de que o aplicativo está pronto para lidar com as atualizações para o estado com vários participantes e como XIM fornece essas atualizações, retornando um `XimStateChangeCollection` objeto que contém todas as atualizações em fila. `XboxIntegratedMultiplayer.GetStateChanges()` foi projetado para operar rapidamente, de modo que ele possa ser chamado todos os quadros de gráficos em seu loop de renderização da interface do usuário. Isso fornece um local conveniente para recuperar todas as alterações na fila sem se preocupar com a imprevisibilidade de intervalos de rede ou complexidade de retorno de chamada com multithread. A API XIM, na verdade, é otimizada para esse padrão de thread único. Ela garante que seu estado permanecerá inalterado durante um `XimStateChangeCollection` objeto está sendo processados e ainda não foi descartado.

O `XimStateChangeCollection` é uma coleção de `IXimStateChange` objetos.
Aplicativos devem:

1. Itere sobre a matriz.
1. Inspecione o `IXimStateChange` para seu tipo mais específico.
1. Conversão de `IXimStateChange` tipo para os respectivos mais detalhadas de tipo.
1. Lidar com essa atualização conforme apropriado.

Após a conclusão com todos os `IXimStateChange` objetos disponíveis no momento, essa matriz deve ser passada para XIM para liberar os recursos chamando `XimStateChangeCollection.Dispose()`. O `using` instrução é recomendada para que os recursos têm garantia de ser descartado após o processamento. Por exemplo:

```cs
using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
{
    foreach (var stateChange in stateChanges)
    {
        switch (stateChange.Type)
        {
            case XimStateChangeType.PlayerJoined:
                HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                break;

            case XimStateChangeType.PlayerLeft:
                HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                break;

            ...
        }
    }
}
```

Agora que você tem seu loop de processamento básico, você pode manipular as alterações de estado associadas inicial `XboxIntegratedMultiplayer.MoveToNewNetwork()` operação. Cada operação de movimentação de rede XIM começará com um `XimMoveToNetworkStartingStateChange`. Se a migração falha por algum motivo, seu aplicativo será fornecido um `XimNetworkExitedStateChange`, que é a mecanismo para qualquer erro assíncrono que impede que você mova para uma rede XIM ou se desconecta da rede XIM atual de tratamento de falha comuns. Caso contrário, a movimentação será concluída com um `XimMoveToNetworkSucceededStateChange` depois que todo o estado tiver sido finalizado e todos os jogadores foi adicionados com êxito à rede XIM.

Quando um usuário não tem o privilégio de plataforma para brincar com outras pessoas em uma sessão para múltiplos jogadores, XIM enviará a um dispositivo tentar criar ou ingressar em uma rede XIM uma `XimNetworkExitedStateChange` com o motivo pelo qual `UserMultiplayerRestricted`. Isso acontece quando qualquer um dos usuários locais definidos com `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` tem uma restrição para múltiplos jogadores do Xbox Live. Trate adequadamente pelo informando ao usuário do problema.

## <a name="basic-iximplayer-handling"></a>Tratamento de IXimPlayer básico

Supondo que o exemplo de movimentação de um único usuário local para uma nova rede XIM foi bem-sucedida, seu aplicativo será fornecido uma `XimPlayerJoinedStateChange` para um local `XimLocalPlayer` objeto. Esse objeto permanecerá válido até correspondente `XimPlayerLeftStateChange` para que ele foi fornecido e retornado por meio de `XimStateChangeCollection.Dispose()`. Seu aplicativo sempre será fornecido uma `XimPlayerLeftStateChange` para cada `XimPlayerJoinedStateChange`.

Você também pode recuperar uma lista de todos os `IXimPlayer` objetos no XIM de rede a qualquer momento usando `XboxIntegratedMultiplayer.GetPlayers()`.

O `IXimPlayer` objeto tem muitas propriedades úteis e métodos, tais como `IXimPlayer.Gamertag()` para recuperar a cadeia de caracteres de nome de jogador do Xbox Live atual associado com o player para fins de exibição. Se o `IXimPlayer` é local para o dispositivo, então IXimPlayer.Local retornará true. Um local `IXimPlayer` pode ser convertido em um `XimLocalPlayer`, que tem métodos adicionais disponíveis apenas para empresas locais.

É claro, o estado mais importante para os jogadores não é informações comuns que XIM sabe, mas o que seu aplicativo específico que deseja rastrear. Como você provavelmente tem sua própria construção para obter essas informações de rastreamento, você desejará vincular a `IXimPlayer` do objeto ao objeto de construção de seu próprio player para que os relatórios XIM a qualquer momento um `IXimPlayer`, você pode recuperar rapidamente seu estado sem ter que executar um pesquisa pelo objeto de contexto de um player personalizado de configuração. O exemplo a seguir supõe um objeto que contém o estado particular está no recém-adicionado e de variável 'myPlayerStateObject' `IXimPlayer` objeto está na variável 'newXimPlayer':

```cs
newXimPlayer.CustomPlayerContext = myPlayerStateObject;
```

Isso salva o objeto especificado com o objeto jogador localmente (nunca transferido pela rede para dispositivos remotos em que a memória não seria válida). Em seguida, você poderá sempre voltar ao seu objeto de contexto personalizado de recuperando e convertê-la de volta para seu objeto, como o exemplo a seguir:

```cs
myPlayerStateObject = (MyPlayerState)(newXimPlayer.CustomPlayerContext);
```

Você pode alterar esse contexto de player personalizado a qualquer momento.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Habilitando amigos unir e convidando-os

Privacidade e segurança, todas as novas redes XIM são automaticamente configuradas por padrão para ser apenas permite junções por empresas locais e cabe ao aplicativo para permitir explicitamente outros quando ele estiver pronto. O exemplo a seguir mostra como usar `XboxIntegratedMultiplayer.NetworkConfiguration` para recuperar a configuração de rede atual e atualizar joinability usando `XboxIntegratedMultiplayer.SetNetworkConfiguration()` para começar a permitir que novos usuários locais para ingressar como players, bem como outros usuários que foram convidados ou que estão sendo " seguido de"(uma Xbox Live social relação) players já na rede XIM:

```cs
XimNetworkConfiguration currentConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
currentConfiguration.AllowedPlayerJoins = XimAllowedPlayerJoins.Local | XimAllowedPlayerJoins.Invited | XimAllowedPlayerJoins.Followed;
XboxIntegratedMultiplayer.SetNetworkConfiguration(currentConfiguration);
```

`XboxIntegratedMultiplayer.SetNetworkConfiguration()` executa de forma assíncrona. Quando a chamada de exemplo de código anterior for concluída, uma `XimNetworkConfigurationChangedStateChange` é fornecido para notificar o aplicativo que o valor de joinability foi alterado do seu padrão de `XimAllowedPlayerJoins.None`. Em seguida, você pode consultar o novo valor, verificando o valor de `XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins`.

`XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` pode ser verificado enquanto o dispositivo está em uma rede XIM para determinar o joinability da rede.

Um dos participantes locais deseja enviar convites para usuários remotos para associar a esta rede XIM, o aplicativo pode chamar `XimLocalPlayer.ShowInviteUI()` para iniciar o convite da interface do usuário do sistema. Aqui, o usuário local pode selecionar as pessoas que desejam convidar e enviar convites.

```cs
XimLocalPlayer.ShowInviteUI();
```

Depois que a declaração acima é executado, o convite de sistema interface do usuário será exibido. Depois que o usuário enviou os convites (ou ignorado caso contrário, a interface do usuário), um `xim_show_invite_ui_completed_state_change` será fornecido. Como alternativa, o aplicativo pode enviar os convites diretamente usando `XimLocalPlayer.InviteUsers()` sem abrir o convite da interface do usuário do sistema.

De qualquer forma, os usuários remotos receberá uma mensagem de convite do Xbox Live onde quer que eles se conectaram e podem optar por aceitar. Isso inicializará seu aplicativo nesses dispositivos se ele já não estiver em execução e "ativar o protocolo"-o com os argumentos do evento que podem ser usados para mover a esta rede XIM mesmo.

Consulte a documentação da plataforma para obter mais informações sobre a ativação de protocolo em si.

O exemplo a seguir mostra como uma chamada `XboxIntegratedMultiplayer.ExtractProtocolActivationInformation()` determina se os argumentos de evento são aplicáveis a XIM. Isso pressupõe que você recuperou a cadeia de caracteres bruta do URI de `Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs` a uma variável 'uriString':

```cs
XimProtocolActivationInformation protocolActivationInformation;
XboxIntegratedMultiplayer.ExtractProtocolActivationInformation(uriString, out protocolActivationInformation);
if (protocolActivationInformation != null)
{
    // It is a XIM activation.
}
```

Se ele for uma ativação XIM, você desejará garantir que o usuário local identificado na propriedade 'LocalXboxUserId' das `XimProtocolActivationInformation` objeto está conectado e entre os usuários especificados para `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`. Em seguida, você pode iniciar a mover para a rede XIM especificada com uma chamada para `XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` usando a mesma cadeia de caracteres do URI. Por exemplo:

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs(uriString);
```

Também Observe que "seguido" usuários remotos possa navegar para o cartão de player do usuário local no sistema de interface do usuário e iniciar uma tentativa de junção em si sem um convite (supondo que você permitiu tais junções de player, conforme mostrado acima). Essa ação também será de protocolo ativar seu aplicativo exatamente como são tratados da mesma forma e fazem a convites.

Mover para uma rede XIM usando a ativação de protocolo é idêntico ao mover para uma nova rede XIM, conforme mostrado na [inicialização e inicialização](#initialization-and-startup). A única diferença é que quando a movimentação for bem-sucedida, o dispositivo móvel será fornecido player local e remoto `XimPlayerJoinedStateChange` objetos que representam os jogadores aplicáveis. Naturalmente, o dispositivo original que já está na rede XIM não estar movendo, mas verá que os usuários do novo dispositivo ser adicionado como players por outras `XimPlayerJoinedStateChange` objetos.

Depois que os jogadores em diferentes dispositivos são Unidos em uma rede XIM, pode iniciar os cenários com vários participantes, se comunicar através de voz e texto automaticamente e enviar mensagens específicas de aplicativo.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Emparelhamento básico e movendo a outra rede XIM com outras pessoas

Você pode expandir ainda mais a experiência de rede para um grupo de amigos, movendo os jogadores a uma rede XIM que também tenha estranhos. Estranhos são adversários de todo o mundo que sejam colocados juntos usando o serviço de cruzamento Xbox Live com base nos interesses semelhantes.

Uma maneira simples para iniciar emparelhamento básico com XIM é chamando `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` em um dos dispositivos com um populados `MatchmakingConfiguration` objeto, levando os jogadores da rede XIM atual junto com ele.

O exemplo a seguir inicia uma movimentação usando uma configuração de emparelhamento configurada para encontrar um total de 8 players para uma correspondência de equipes não festa para todos. Se o total de 8 players não é encontrados, o sistema ainda pode corresponder players de 2 a 7 em conjunto. Este exemplo usa uma constante definida pelo aplicativo, do tipo uint, chamado MYGAMEMODE_DEATHMATCH, que representa o modo de jogo para filtrar fora de. Para que haja correspondência do XIM corresponderá apenas a essa rede com outros jogadores especificando que mesmo valor e traz ao longo de jogadores tudo socialmente ingressados da rede XIM atual ao fornecer o segundo parâmetro `XimPlayersToMove.BringExistingSocialPlayers` para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Como mover anterior, isso fornecerá um inicial `MoveToNetworkStartingStateChange` em todos os dispositivos e um `MoveToNetworkSucceededStateChange` depois que a migração seja concluída com êxito. Como essa é uma mudança de uma rede XIM para outra, uma diferença é que já existem `IXimPlayer` objetos adicionados para usuários locais e remotos, e eles permanecerão de todos os jogadores que estão migrando em conjunto para a nova rede XIM. Bate-papo e dados de comunicação entre os jogadores continuarão funcionando sem interrupções enquanto o emparelhamento está em andamento.

Para que haja correspondência pode ser um processo demorado, dependendo do número de jogadores potencias no pool de relacionamento de pessoas que também ter chamado `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

Um `XimMatchmakingProgressUpdatedStateChange` será fornecida periodicamente durante a operação para manter os usuários informados sobre o status atual. Quando a correspondência foi encontrada, os jogadores adicionais são adicionados à rede com o típico XIM `PlayerJoinedStateChange` e a migração seja concluída.

Depois que terminar a experiência para múltiplos jogadores com esse conjunto de jogadores "matchmade", você pode repetir o processo para mover para uma rede XIM diferente com outra rodada de emparelhamento. Você verá cada jogador que unidas por meio de prévia `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` operação fornecem uma `PlayerLeftStateChange` para indicar que seus `IXimPlayer` objetos não estão mais na mesma rede XIM.

Se quando você escolhe iniciar emparelhamento novamente usando `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, você especifica `XimPlayersToMove.BringExistingSocialPlayers`, os jogadores que unidas por meio de pontos de entrada social (`XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` ou `xXboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId`) permanecerá enquanto ocorre o cruzamento de novo. Como alternativa, especificando `XimPlayersToMove.BringOnlyLocalPlayers` desconectará socialmente conectados players remotos, deixando apenas os jogadores locais para permanecer. Em ambos os casos, um conjunto diferente de estranhos será adicionado quando a segunda operação de movimentação é concluída.

Você também tem a opção de mover os players não matchmade (ou apenas locais players) a uma rede XIM completamente nova durante a decisão a próxima atividade multiplayer/configuração de emparelhamento.

O exemplo a seguir demonstra como um dispositivo chamaria `XboxIntegratedMultiplayer.MoveToNewNetwork()` para uma rede XIM com um máximo de 8 jogadores, mas desta vez levando os socialmente ingressados players existentes também:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringExistingSocialPlayers);
```

Um `MoveToNetworkStartingStateChange` e `MoveToNetworkSucceededStateChange` será fornecido para todos os dispositivos participantes, juntamente com um `PlayerLeftStateChange` para o matchmade que foram deixados para trás. Nesses dispositivos, eles da mesma forma verão um `PlayerLeftStateChange` para cada jogador que é movido para fora da rede.

Você pode continuar a movimentação de rede XIM para rede XIM da maneira descrita acima, quantas vezes conforme desejado.

Para obter desempenho, o serviço Xbox Live não tentará correspondam aos grupos de jogadores em dispositivos que podem não ser capaz de estabelecer quaisquer conexões ponto a ponto diretas. Se você estiver desenvolvendo em um ambiente de rede não está configurado adequadamente para dar suporte a standard Xbox Live para múltiplos jogadores, a mudança para a nova rede usando a operação de emparelhamento pode continuar indefinidamente sem correspondência bem-sucedida, mesmo quando você tiver certeza de que você tem suficientes jogadores atendem aos critérios de relacionamento de pessoas que estão usando e movendo todos os dispositivos no mesmo ambiente local. Certifique-se de executar o teste de conectividade para múltiplos jogadores nas configurações de rede o aplicativo de área/Xbox e siga as recomendações se ele relata problemas, particularmente em relação a um "NAT estrito".

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>Desabilitando a regra NAT de emparelhamento para fins de depuração

Se o administrador da rede não puder fazer as alterações de ambiente necessárias para dar suporte a regras NAT do XIM, você pode desbloquear seu relacionamento de pessoas que um teste no Xbox One kits de desenvolvimento por meio da configuração XIM para permitir que dispositivos "Estrito NAT" sem pelo menos um "abrir de correspondência Dispositivo NAT". Isso é feito colocando um arquivo chamado "xim_disable_matchmaking_nat_rule" (o conteúdo não importa) na raiz da unidade do "zero título" de todos os consoles Xbox One. Uma maneira de exemplo para fazer isso é executando o seguinte em um prompt de comando do XDK antes de iniciar seu aplicativo, substituindo o espaço reservado "{console_name_or_ip_address}" para cada console conforme apropriado:

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> Essa solução alternativa de desenvolvimento no momento, só está disponível para aplicativos de recurso exclusivo Xbox One e não há suporte para aplicativos universais do Windows. Também Observe que os consoles que estão usando essa configuração jamais corresponderá com dispositivos que também não têm esse arquivo estiver presente, independentemente do ambiente de rede, então, certifique-se adicionar e remover o arquivo em todos os lugares.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>Sair de uma rede XIM e limpeza

Quando os usuários locais são feitos participando de uma XIM rede, muitas vezes que eles serão simplesmente mover de volta para uma nova rede XIM que permite que os usuários locais, convites e "seguido" os usuários para uni-lo para que eles possam continuar coordenando com seus amigos para localizar a próxima atividade. Mas se o usuário é feito completamente com todas as experiências com vários participantes, seu aplicativo talvez queira começar deixando o XIM completamente de rede e retornar ao estado como se apenas `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds` tivesse sido chamada. Isso é feito usando o `XboxIntegratedMultiplayer.LeaveNetwork()` método:

```cs
 XboxIntegratedMultiplayer.LeaveNetwork();
```

Esse método inicia o processo de forma assíncrona desconexão de outros participantes normalmente. Isso fará com que os dispositivos remotos a ser fornecido um `PlayerLeftStateChange` para cada jogador local que desconectadas. O dispositivo local será também ser fornecido um `PlayerLeftStateChange` para cada jogador de local e remota foi desconectada. Quando se desconecta todas as operações tenham terminado, um final `NetworkExitedStateChange` será fornecido.

Invocando `XboxIntegratedMultiplayer.LeaveNetwork()` e aguardando a `NetworkExitedStateChange` para sair de um XIM rede normalmente é sempre recomendável quando um `NetworkExitedStateChange` já não foi fornecido.

## <a name="sending-and-receiving-messages"></a>Enviando e recebendo mensagens

XIM e seus componentes subjacentes fazem todo o trabalho tedioso de estabelecer canais de comunicação seguro pela Internet para que você não precise se preocupar sobre problemas de conectividade ou que está sendo capaz de alcançar a alguns, mas nem todos os jogadores. Se houver quaisquer problemas de conectividade de ponto a ponto fundamental, movendo-se a uma rede XIM não terá êxito.

Quando uma rede XIM é formada e confirmada com `XimMoveToNetworkSucceededStateChange`, todas as instâncias do seu aplicativo em todos os dispositivos Unidos têm garantia de ser informado sobre cada `IXimPlayer` conectado à rede XIM e pode enviar mensagens para qualquer um deles.

Vamos demonstrar como enviar mensagens pela rede XIM. O exemplo a seguir pressupõe que a variável 'sendingPlayer' é um objeto de reprodutor de local válido. O exemplo usa ' sendingPlayer ' método s `SendDataToOtherPlayers()` para enviar uma estrutura de mensagem que foi copiada para 'msgBuffer' para todos os jogadores (locais ou remotos) na rede XIM. Observe que ele vai a todos os jogadores porque uma lista de jogadores específicos não foi passada para o método:

```cs
SendingPlayer.SendDataToOtherPlayers(msgBuffer, null, XimSendType.GuaranteedAndSequential);
```

Todos os destinatários da mensagem serão fornecidos uma `XimPlayertoPlayerDataReceivedStateChange` que inclui uma cópia dos dados, bem como o `IXimPlayer` objeto que é enviado a ele e uma lista da `IXimPlayer` objetos que são localmente o recebimento.

É claro, entrega sequencial garantida é conveniente, mas também pode ser um tipo de envio ineficiente, pois XIM precisa retransmitir ou adiar as mensagens se os pacotes são descartados ou fora de ordem pela Internet. Não se esqueça de considerar o uso de outros tipos de envio para mensagens que seu aplicativo pode tolerar perder ou ter que chegam fora de ordem. Como os dados da mensagem provêm de um computador remoto, a prática recomendada é tenham definido claramente os formatos de dados, como empacotamento valores de vários bytes em uma ordem de byte específico ("endianness"). O aplicativo também deve validar os dados antes de agir sobre ele.

XIM fornece segurança em nível de rede, portanto, não há nenhuma necessidade de esquemas adicionais de criptografia ou assinatura. No entanto, é sempre recomendável empregar "defesa em profundidade" para proteger contra a aplicação acidental de bugs e ser capaz de lidar com diferentes versões do seu aplicativo protocolo coexistindo normalmente (durante o desenvolvimento, atualizações de conteúdo, etc.). Conexões de Internet dos usuários podem ser limitados e em constante mudança de recursos. É altamente recomendável o uso de formatos de dados de mensagens eficiente e evitando designs que enviar cada quadro de interface do usuário.

## <a name="assessing-data-pathway-quality"></a>Avaliar a qualidade do caminho de dados

Para saber mais sobre a qualidade atual do caminho de dados entre dois jogadores inspecionando o `XimPlayer.NetworkPathInformation` propriedade. Você pode aprender mais sobre a qualidade atual do caminho entre dois jogadores inspecionando o `XimPlayer.NetworkPathInformation` propriedade. A propriedade inclui a latência de viagem estimado e quantas mensagens ainda estão na fila localmente para transmissão no caso de que a conexão não dá suporte ao transmitir mais dados para o momento.

Se você vir que as filas estiver fazendo backup para 'IXimPlayer' específico, você deve reduzir a taxa na qual você está enviando dados.

O `NetworkPathInformation.RoundTripLatency` campo representa a latência da rede subjacente e a latência de estimado do XIM sem enfileiramento de mensagens. Aumenta a latência eficaz como `XimNetworkPathInformation.SendQueueSizeInMessages` cresce e XIM funciona por meio da fila.

Escolha um ponto razoável para começar a limitá-chamadas para `SendDataToOtherPlayers` com base nos requisitos e o uso do jogo. Todas as mensagens na fila de envio representa um aumento na latência de rede em vigor.

Um valor próximo ao limite máximo do XIM (atualmente 3500 mensagens) é muito alto para a maioria dos jogos e provavelmente representa vários segundos de dados que estão aguardando para ser enviado, dependendo da taxa de chamada `SendDataToOtherPlayers` e é o tamanho máximo de cada carga de dados. Em vez disso, escolha um número que leva em conta os requisitos de latência do jogo, juntamente com trêmulo como o jogo `SendDataToOtherPlayers` é padrão de chamada.

## <a name="sharing-data-using-player-custom-properties"></a>Compartilhamento de dados usando as propriedades personalizadas do player

A maioria das trocas de dados de aplicativo acontecem com o `XimLocalPlayer.SendDataToOtherPlayers()` método, pois ele permite mais controle sobre quem recebe a ele e quando, como ele deve lidar com a perda de pacotes e assim por diante. No entanto, há vezes quando poderia ser bom para os jogadores compartilhar o estado básico, raramente são alterado sobre si mesmos com outras pessoas sem complicação. Por exemplo, cada jogador pode ter uma cadeia de caracteres fixa, que representa o modelo de caractere selecionado antes de entrar com vários participantes para que todos os jogadores usam para renderizar sua representação no jogo.

Para dados que não são alterados com muita frequência sobre um player, XIM fornece player propriedades personalizadas. Essas propriedades consistem em um nome definido pelo aplicativo e um valor, que são pares de cadeia de caracteres terminada em nulo que pode ser aplicado ao player local e que são propagadas automaticamente para todos os dispositivos sempre que eles são alterados.

Propriedades de player personalizado têm sincronização interna mais sobrecarga da `XimLocalPlayer.SendDataToOtherPlayers()` método, para mudar rapidamente os dados (ou seja, a posição de player), você ainda deve usar envia direta.

Pares de chave-valor de propriedades personalizadas do Player têm seus valores atuais fornecidas automaticamente para novos dispositivos participantes quando esses dispositivos ingressar em uma rede XIM e ver o player adicionado. Valores podem ser definidos por meio da chamada `XimLocalPlayer.SetPlayerCustomProperty()` com o nome e valor de cadeias de caracteres, como no exemplo a seguir, que define uma propriedade denominada "modelo" para que o "bruta" de valor em um local `IXIMPlayer` objeto apontado pela variável 'localPlayer':

```cs
localPlayer.SetPlayerCustomProperty("model","brute");
```

As alterações nas propriedades do player fará com que um `XimPlayerCustomPropertiesChangedStateChange` ser fornecida para todos os dispositivos, avisando-os para os nomes das propriedades que foram alterados. O valor para um determinado nome pode ser recuperado em qualquer player, local ou remoto, com `IXIMPlayer.GetPlayerCustomProperty()`. O exemplo a seguir recupera o valor de uma propriedade chamada "modelo" de um `IXIMPlayer` apontado pela variável 'ximPlayer':

```cs
string propertyValue = localPlayer.GetPlayerCustomProperty("model");
```

Definindo um novo valor para um nome de propriedade determinado substituirá qualquer valor existente. Configurar um nome de propriedade com um valor de um ponteiro de cadeia de caracteres do valor nulo é tratado o mesmo que uma cadeia de caracteres do valor vazio, que é o mesmo que a propriedade ter que ainda não foi especificada. Nomes e valores não são interpretados pelo XIM, portanto, é deixado no aplicativo para validar o conteúdo de cadeia de caracteres conforme necessário.

Propriedades de player personalizado sempre são redefinidas ao mover de uma rede XIM para outro. No entanto, os novos jogadores que se associar à rede XIM receberá propriedades atuais do player personalizado para todos os jogadores que têm um conjunto.

## <a name="sharing-data-using-network-custom-properties"></a>Compartilhamento de dados usando as propriedades personalizadas da rede

Propriedades personalizadas da rede destinam-se como uma conveniência a sincronização de dados específicos à rede XIM que não muda com frequência. Forma idêntica ao trabalho de propriedades personalizadas de rede [propriedades personalizadas do player](#sharing-data-using-player-custom-properties), exceto que eles são definidos no objeto de singleton XIM com `XboxIntegratedMultiplayer.SetNetworkCustomProperty()`. O exemplo a seguir define uma propriedade de "mapa" para que o valor "sol":

```cs
XboxIntegratedMultiplayer.SetNetworkCustomProperty("map", "stronghold");
```

As alterações nas propriedades de rede fará com que um `NetworkCustomPropertiesChanged` ser fornecida para todos os dispositivos, avisando-os para os nomes das propriedades que foram alterados. O valor para um determinado nome pode ser recuperado com `XboxIntegratedMultiplayer.GetNetworkCustomProperty()`, como no exemplo a seguir, que recupera o valor de um propriedade nomeada "mapa":

```cs
string property = XboxIntegratedMultiplayer.GetNetworkCustomProperty("map")
```

Assim como [propriedades personalizadas do player](#sharing-data-using-player-custom-properties), definindo um novo valor para um nome de propriedade determinado substituirá qualquer valor existente. Configurar um nome de propriedade com um valor de um ponteiro de cadeia de caracteres do valor nulo é tratado o mesmo que uma cadeia de caracteres do valor vazio, que é o mesmo que a propriedade ter que ainda não foi especificada. Nomes e valores não são interpretados pelo XIM, portanto, é deixado no aplicativo para validar o conteúdo de cadeia de caracteres conforme necessário.

Recém-criados XIM redes sempre iniciar com nenhum conjunto de propriedades personalizadas de rede. No entanto, novos jogadores que unem a uma rede existente do XIM receberão os valores atuais das propriedades personalizadas de rede definido para essa rede XIM.

## <a name="matchmaking-using-per-player-skill"></a>Para que haja correspondência usando habilidades por player

Correspondência de jogadores por interesse comum em um determinado modo de jogo especificado pelo aplicativo é uma boa estratégia de base. À medida que aumenta o pool de jogadores disponíveis, você deve considerar a correspondência também players com base em suas habilidades pessoais ou experiência com o seu jogo para que os jogadores veteranos podem aproveitar o desafio de concorrência Íntegro com outros veteranos enquanto players mais recentes podem crescer por competir com outras pessoas com capacidades semelhantes.

Para fazer isso, inicie, fornecendo o nível de habilidade de todos os jogadores locais em suas funções por player e a estrutura de configuração de habilidades especificadas em chamas para `XimLocalPlayer.SetRolesAndSkillConfiguration()` antes de começar a mover para um XIM usando emparelhamento de rede. Nível de habilidade é um conceito específico do aplicativo e o número não é interpretado pelo XIM, exceto pelo fato de relacionamento de pessoas primeiro tentará localizar jogadores com o mesmo valor de habilidade e, em seguida, periodicamente, ampliar sua pesquisa em incrementos de + /-10 para localizar outros jogadores declarando a habilidade valores dentro de um intervalo em torno dessa habilidade. O exemplo a seguir pressupõe que o local `XimLocalPlayer` objeto, cuja variável está 'localPlayer', tem um valor de habilidades de inteiro de específicos do aplicativo associado recuperado do armazenamento local ou Xbox Live em uma variável denominada 'playerSkillValue':

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Skill = playerSkillValue;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Quando isso for concluído, todos os participantes serão fornecidos um `XimPlayerRolesAndSkillConfigurationChangedStateChange` indicando isso `IXIMPlayer` mudou sua funções por player e a configuração de habilidade. O novo valor pode ser recuperado chamando `IXIMPlayer.RolesAndSkillConfiguration`. Quando todos os jogadores têm a configuração de emparelhamento não nula aplicada, você pode mover para uma rede XIM usando o emparelhamento com um valor de verdadeiro para o `RequirePlayerRolesAndSkillConfiguration` campo do `XimMatchmakingConfiguration` estrutura especificada para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

O exemplo a seguir preenche uma configuração de emparelhamento para localizar um total de jogadores de 2 a 8 para uma festa para todos não equipes. Além disso, este exemplo usa uma constante definida pelo aplicativo, que é do tipo Uint64 e chamado MYGAMEMODE_DEATHMATCH, que representa o modo de jogo para filtrar fora de. Isso configura o cruzamento de acordo com os participantes da rede XIM com outros jogadores especificando os mesmos valores, bem como a necessidade de configuração de emparelhamento por player.

```cs
XimMatchmakingConfiguration matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.RequirePlayerRolesAndSkillConfiguration = true;
```

Quando essa estrutura é fornecida para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, a operação de movimentação será iniciado normalmente desde que os jogadores movendo tem chamado `XimLocalPlayer.SetRolesAndSkillConfiguration()` não nulo `XimPlayerRolesAndSkillConfiguration` objeto. Se não tiver qualquer player, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `XimMatchmakingProgressUpdatedStateChange` com um `WaitingForPlayerRolesAndSkillConfiguration` valor. Isso inclui os jogadores que subsequentemente ingressarem na rede XIM por meio de um convite enviado anteriormente ou outros meios de redes sociais (por exemplo, uma chamada para `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) antes de cruzamento foi concluída. Depois que tem fornecido a todos os jogadores seus `XimPlayerRolesAndSkillConfiguration` objetos, para que haja correspondência será retomada.

Para que haja correspondência usando habilidades por player também pode ser combinada com a função de por player do usuário para que haja correspondência, conforme explicado na próxima seção. Se apenas uma for desejada, você pode especificar um valor de 0 para o outro. Isso ocorre porque todos os jogadores declarando que eles têm um `XimPlayerRolesAndSkillConfiguration` valor da habilidade de 0 será sempre correspondem uns aos outros.

Uma vez a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` ou qualquer outra rede XIM mover a operação for concluída, todos os jogadores `XimPlayerRolesAndSkillConfiguration` estruturas serão limpo automaticamente como um ponteiro nulo (com uma que acompanha `XimPlayerRolesAndSkillConfigurationChangedStateChange` notificação). Se você planeja mover para outra rede XIM usando emparelhamento que requer a configuração por player, você precisará chamar `XimLocalPlayer.SetRolesAndSkillConfiguration()` novamente com um objeto que contém as informações mais atualizadas.

## <a name="matchmaking-using-per-player-role"></a>Para que haja correspondência usando a função por player

Outro método do uso de funções por player e a configuração de habilidades para melhorar a experiência dos usuários para que haja correspondência é com o uso de funções necessárias do player. Isso é ideal para jogos que fornecem tipos de caractere selecionável que incentivam cooperativo diferente reproduzir estilos. Esses tipos de caractere são aqueles que não simplesmente alterar a representação gráfica do jogo e, em vez disso, alterar o estilo de jogo para o player. Usuários talvez prefira serem jogados como uma especialização específica. No entanto, se seu jogo é projetado de forma que funcionalmente, não é possível concluir objetivos sem pelo menos uma pessoa que atender a cada função, às vezes, é melhor coincidir com esses players junto primeiro que a correspondência qualquer jogadores em conjunto, em seguida, exigem que eles negociar play estilos entre si coletadas uma vez. Você pode fazer isso primeiro definindo um sinalizador de bit exclusivo que representa cada função a ser especificado em um player determinado `XimPlayerRolesAndSkillConfiguration` estrutura.

O exemplo a seguir define um valor de função específica ao aplicativo, que é do tipo byte e chamado MYROLEBITFLAG_HEALER, para o local `XimLocalPlayer` objeto cujo ponteiro é 'localPlayer':

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Roles = MYROLEBITFLAG_HEALER;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Quando isso for concluído, todos os participantes serão fornecidos um `XimPlayerRolesAndSkillConfigurationChangedStateChange` indicando isso `IXimPlayer` mudou sua funções por player e a configuração de habilidade. O novo valor pode ser recuperado chamando `IXimPlayer.RolesAndSkillConfiguration`.

Global `XimMatchmakingConfiguration` estrutura especificada para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` devem ter todos os sinalizadores de funções necessárias combinados usando OR bit a bit e um valor de verdadeiro para o `RequirePlayerRolesAndSkillConfiguration` campo.

Para que haja correspondência usando a função por player também pode ser combinada com a habilidade de-player de usuário para que haja correspondência. Se apenas uma for desejada, especifique um valor de 0 para o outro. Isso ocorre porque todos os jogadores declarando que eles têm uma `XimPlayerRolesAndSkillConfiguration` valor da habilidade de 0 será sempre correspondem uns aos outros; e, se todos os bits são zero no `XimMatchmakingConfiguration.RequiredRoles` campo, então nenhum bit de função é necessárias para corresponder.

Uma vez a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` ou qualquer outra rede XIM mover a operação for concluída, todos os jogadores `XimPlayerRolesAndSkillConfiguration` objetos serão limpo automaticamente como nulo (com uma que acompanha `XimPlayerRolesAndSkillConfigurationChangedStateChange` notificação). Se você planeja mover para outra rede XIM usando emparelhamento que requer a configuração por player, você precisará chamar `XimLocalPlayer.SetRolesAndSkillConfiguration()` novamente com um objeto que contém as informações mais atualizadas.

## <a name="how-xim-works-with-player-teams"></a>Como XIM funciona com as equipes do player

Com vários participantes jogos geralmente envolve players organizados em opostos equipes. Torna XIM fácil atribuir equipes ao emparelhamento definindo `XimMatchmakingConfiguration.TeamConfiguration`. O exemplo a seguir inicia uma movimentação usando o emparelhamento configurado para localizar um total de 8 players para colocar em duas equipes de 4 (embora se 4 não são encontrados, players de 1 a 3 também são aceitos). Além disso, este exemplo usa uma constante definida pelo aplicativo, que é do tipo uint e chamado MYGAMEMODE_CAPTURETHEFLAG, que representa o modo de jogo para filtrar fora de.  Além disso, a configuração está configurada para trazer todos socialmente ingressados players da rede XIM atual:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 2;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 4;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Quando tal uma operação de movimentação do XIM rede for concluído, os jogadores serão atribuídos um equipe valor de índice 1 por meio de {n} correspondente para as equipes de {n} solicitadas. Valor de índice de equipe de um jogador é recuperado via `IXIMPlayer.TeamIndex`. Ao usar um `XimMatchmakingConfiguration.TeamConfiguration` com duas ou mais equipes, jogadores nunca receberá um valor de índice da equipe de zero pela chamada para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`. Isso ocorre em oposição a jogadores são adicionados à rede XIM com qualquer outra configuração ou o tipo de operação de movimentação (como por meio de uma ativação de protocolo resultante de aceitar um convite), que sempre terá um índice de zero de equipe. Pode ser útil tratar o índice da equipe 0 como uma equipe "não atribuída" especial.

O exemplo a seguir recupera o índice de equipe para um objeto de player XIM armazenado na variável 'ximPlayer':

```cs
byte playerIndex = ximPlayer.TeamIndex;
```

Para a experiência do usuário preferencial (não a oportunidade de menção reduzida para o comportamento de jogadores negativo), o serviço de cruzamento Xbox Live nunca será jogadores de divisão que estão migrando para uma rede XIM juntas em equipes diferentes.

O valor de índice da equipe atribuído inicialmente pelo emparelhamento é apenas uma recomendação e o aplicativo pode alterá-la para players locais a qualquer momento usando `XimLocalPlayer.SetTeamIndex()`. Isso também pode ser chamado em redes XIM que não usam o relacionamento de pessoas em todos os. O exemplo a seguir configura um objeto de local de player XIM armazenado na variável 'ximLocalPlayer' para ter um novo valor de índice da equipe de um:

```cs
ximLocalPlayer.SetTeamIndex(1);
```

Todos os dispositivos serão informados de que o jogador tem um novo valor de índice de equipe em vigor quando elas são fornecidas uma `XimPlayerTeamIndexChangedStateChange` para esse jogador. Você pode chamar `XimLocalPlayer.SetTeamIndex()` a qualquer momento.

Para que haja correspondência avalia as funções de player de necessária independentemente das equipes. Portanto, não tem recomendável para usar as equipes e funções necessárias como critérios de configuração de emparelhamento simultâneas porque as equipes serão balanceadas por contagem de player, não por funções de player foi atendida.

## <a name="working-with-chat"></a>Trabalhando com os bate-papo

Comunicação de bate-papo de voz e texto são habilitadas automaticamente entre jogadores em uma rede XIM. XIM manipula a interagir com todos os hardwares de fone de ouvido e microfone de voz para você. Seu aplicativo não precisará fazer muita coisa para bate-papo, mas ele tem um requisito em relação à bate-papo do texto: suporte a entrada e exibição. Entrada de texto é necessária porque, até mesmo em plataformas ou gêneros de jogos que ainda não tiveram generalizado físico do teclado usar, os jogadores podem configurar o sistema para usar tecnologias assistenciais texto em fala. Da mesma forma, a exibição de texto é necessária porque os jogadores podem configurar o sistema para usar a fala em texto.

Se você quiser habilitar a mecanismos de texto condicionalmente, essas preferências podem ser detectadas em empresas locais acessando a `XimLocalPlayer.ChatTextToSpeechConversionPreferenceEnabled` e `XimLocalPlayer.ChatSpeechToTextConversionPreferenceEnabled` campos respectivamente, e talvez você queira habilitar condicionalmente os mecanismos de texto. No entanto, recomendamos que você considere tornar o texto de entrada e exibir as opções que estão sempre disponíveis.

`Windows::Xbox::UI::Accessability` uma classe Xbox One projetada especificamente para fornecer processamento simples de bate-papo do texto no jogo com foco nas tecnologias assistenciais de fala em texto.

Depois que a entrada de texto fornecida por um teclado real ou virtual, passe a cadeia de caracteres para o `XimLocalPlayer.SendChatText()` método. O seguinte código mostra o envio de uma cadeia de caracteres embutido em código do exemplo de um local `IXIMPlayer` objeto apontado pela variável 'localPlayer':

```cs
localPlayer.SendChatText(text);
```

Esse texto de bate-papo é entregue a todos os jogadores na rede XIM que podem receber comunicação de bate-papo do player de local de origem como um `XimChatTextReceivedStateChange`. Ele pode sintetizado áudio fala se TTS está habilitado.

Seu aplicativo deve fazer uma cópia de qualquer cadeia de caracteres de texto recebida e exibi-lo junto com sua identificação do player de origem para uma quantidade apropriada de tempo (ou em uma janela rolável).

Também há algumas práticas recomendadas em relação à bate-papo. É recomendável que em qualquer lugar os jogadores são mostrados, especialmente em uma lista de gamertags como um placar também exibem ícones mudo/falando como comentários do usuário. Isso é feito, acessando `IXimPlayer.ChatIndicator` para recuperar um `XboxIntegratedMultiplayer.XimPlayerChatIndicator` que representa o status atual, o instantâneo de bate-papo para esse jogador.

O valor relatado pelo `IXimPlayer.ChatIndicator` deve ser alterado com frequência como players de início e parada falando, por exemplo. Ele é projetado para dar suporte a aplicativos sondando cada quadro de interface do usuário como resultado.

> [!NOTE]
> Se um usuário local não tem privilégios suficientes de comunicações devido a suas configurações de dispositivo `XimLocalPlayer.ChatIndicator` retornará um `XboxIntegratedMultiplayer.XimPlayerChatIndicator` com valor `XIM_PLAYER_CHAT_INDICATOR_PLATFORM_RESTRICTED`. A expectativa para atender aos requisitos para a plataforma é o aplicativo mostrar a iconografia indicando uma restrição de plataforma para bate-papo ou sistema de mensagens e uma mensagem ao usuário indicando o problema. Uma mensagem de exemplo, que é recomendável é: "Desculpe, você não tem permissão para conversar agora."

## <a name="muting-players"></a>Mudo players

Outra prática recomendada é dar suporte a tirar o som de jogadores. XIM controla automaticamente o sistema mudo iniciado por usuários por meio de cartões de player, mas aplicativos devem oferecer suporte ao jogo específico transitório mudo que podem ser executadas na interface do usuário jogo por meio de `IXimPlayer.ChatMuted` propriedade. O exemplo a seguir inicia um silenciament remota `IXIMPlayer` objeto apontado pela variável 'remotePlayer' para que nenhum bate-papo de voz seja ouvida e chat nenhum texto é recebido dele:

```cs
remotePlayer.ChatMuted = true;
```

A tirar o som entra em vigor imediatamente e lá não está nenhuma alteração de estado XIM associada a ele. Ele pode ser alterando o valor da `IXimPlayer.ChatMuted` a propriedade como false. O exemplo a seguir unmutes remota `IXIMPlayer` objeto apontado pela variável 'remotePlayer':

```cs
remotePlayer.ChatMuted = false;
```

Desativa permanecem em vigor para desde que o `IXIMPlayer` existir, incluindo ao mudar para uma nova rede XIM com o player. Ele não será mantido se o jogador sai e reassocia-se o mesmo usuário (como um novo `IXIMPlayer` instância).

Normalmente, os jogadores começam no estado unmuted. Se seu aplicativo deseja iniciar um player no estado mudo por motivos de jogo, pode definir `IXIMPlayer.ChatMuted` sobre o `IXIMPlayer` objeto antes de concluir o processamento associado `PlayerJoinedStateChange`, e XIM garantirá que não haja nenhum período de tempo de áudio de voz em que o player pode ser ouvido.

Uma verificação de mudo automático com base em reputação do player ocorre quando a rede XIM ingressa em um player remoto. Se o jogador tem um sinalizador de Reputação ruim, o player é desligado automaticamente. Mudo afeta apenas o estado local e persiste, portanto, se um jogador move entre redes. A verificação automática de sem som com base na reputação é executada uma vez e não reavaliada novamente para desde que o `IXIMPlayer` permanece válido.

## <a name="configuring-chat-targets-using-player-teams"></a>Configurar destinos de bate-papo usando as equipes do player

Conforme mencionado na [XIM como trabalha com equipes de player](#how-xim-works-with-player-teams) seção deste documento, o verdadeiro significado de qualquer valor de índice específico da equipe é depende do aplicativo. XIM não interpretá-los, exceto para comparações de igualdade em relação à configuração de bate-papo de destino. Se a configuração de destino de bate-papo relatado pelo `XboxIntegratedMultiplayer.ChatTargets` está atualmente `XimChatTargets.SameTeamIndexOnly`, em seguida, qualquer determinada player trocarão comunicação de bate-papo com outro somente se os dois tiverem o mesmo valor relatado pelo `IXIMPlayer.TeamIndex` (e a privacidade / diretiva também permite que ele).

Seja conservador e suporte a cenários de concorrência, recém-criados redes XIM são automaticamente configurados para o padrão para `XimChatTargets.SameTeamIndexOnly`. No entanto, bate-papo com adversários vanquished em outra equipe pode ser desejável, por exemplo, em um pós-jogo de "lobby". Você pode instruir o XIM para permitir que todos se comunicar com qualquer outra pessoa (no qual política de privacidade e permitem) chamando `XboxIntegratedMultiplayer.SetChatTargets()`. O exemplo a seguir começa a configurar todos os participantes na rede XIM para usar um `XimChatTargets.AllPlayers` valor:

```cs
XboxIntegratedMultiplayer.SetChatTargets(XimChatTargets.AllPlayers)
```

Todos os participantes são informados de que uma nova configuração de destino está em vigor quando elas são fornecidas uma `XimChatTargetsChangedStateChange`.

Conforme observado anteriormente, a maioria dos tipos de movimentação de rede XIM inicialmente atribuirá todos os jogadores o valor de índice da equipe de zero. Isso significa que uma configuração de `XimChatTargets.SameTeamIndexOnly` provavelmente indistinguível de `XimChatTargets.AllPlayers` por padrão. No entanto, os jogadores que mover para uma rede XIM usando o emparelhamento serão possuem diferentes valores de índice da equipe se a configuração de emparelhamento `TeamMatchmakingMode` valor declarado de duas ou mais equipes. Você também pode chamar `XimLocalPlayer.SetTeamIndex()` a qualquer momento, conforme mostrado acima. Se seu aplicativo está usando valores de índice de equipe diferente de zero por meio de qualquer um desses métodos, não se esqueça de gerenciar os destinos de bate-papo atual configuração adequadamente.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Preenchimento automático em segundo plano dos slots de player ("aterramento" para que haja correspondência)

Grupos diferentes de jogadores chamando `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` ao mesmo tempo oferece o serviço de cruzamento Xbox Live a maior flexibilidade para organizá-los rapidamente em redes XIM novos e otimizados. No entanto, alguns cenários de jogo gostaria de manter uma determinada rede XIM intacta e apenas matchmake de players adicionais apenas para preencher os slots de vagas de player. XIM o dá suporte à configuração de emparelhamento para operar em um plano automático preenchendo o modo ou "o aterramento", chamando `XboxIntegratedMultiplayer.SetNetworkConfiguration()` com um `XimNetworkConfiguration` que tem `XimAllowedPlayerJoins.Matchmade` sinalizador definido em seu `XimNetworkConfiguration.AllowedPlayerJoins` propriedade.

O exemplo a seguir configura o cruzamento de aterramento para tentar encontrar um total de 8 players para uma festa para todos não equipes (embora se 8 não são encontrados, players de 2 a 7 também são aceitos), usar um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor MYGAMEMODE_ Combate MORTAL corresponderá apenas com outros jogadores especificando o mesmo valor:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Matchmade;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Isso torna o XIM existente de rede disponíveis para dispositivos chamando `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` da maneira normal. Esses dispositivos nenhum comportamento, consulte alterar. Os participantes na rede o aterramento XIM não será movido, mas serão fornecidos uma `XimNetworkConfigurationChangedStateChange` significando aterramento ativar, bem como vários `XimMatchmakingProgressUpdatedStateChange` notificações quando aplicável. Qualquer player matchmade será adicionado à rede XIM usando normal `XimPlayerJoinedStateChange`.

Por padrão, para que haja correspondência aterramento permanece ativada por tempo indeterminado, embora ele não tente adicionar os jogadores se a rede XIM já tem o número máximo de jogadores especificado pelo `XimNetworkConfiguration.TeamConfiguration` configuração. O aterramento pode ser desabilitado definindo `XimNetworkConfiguration.AllowedPlayerJoins` não incluindo `XimAllowedPlayerJoins.Matchmade`:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins &= ~XimAllowedPlayerJoins.Matchmade;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Um correspondente `XimNetworkConfigurationChangedStateChange` será fornecido para todos os dispositivos e, depois que esse processo assíncrono for concluída, um final `XimMatchmakingProgressUpdatedStateChange` será fornecido com `MatchmakingStatus.None` para indicar que nenhum jogador matchmade adicionais serão adicionados à rede XIM.

Ao habilitar o emparelhamento de aterramento com um `XimNetworkConfiguration.TeamConfiguration` definir que declara duas ou mais equipes, todos os jogadores existentes devem ter um índice de equipe válido está entre 1 e o número de equipes. Isso inclui os jogadores que chamaram `XimLocalPlayer.SetTeamIndex()` para especificar um valor personalizado ou que se associaram usando um convite ou por meio de outro social significa (por exemplo, uma chamada para `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) e foram adicionados com um valor de índice de equipe padrão de 0. Se qualquer player não tem um índice de equipe válido, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `XimMatchmakingProgressUpdatedStateChange` com um `MatchmakingStatus.WaitingForPlayerTeamIndex` valor. Depois que todos os jogadores tem fornecido ou corrigido seus valores de índice de equipe com `XimLocalPlayer.SetTeamIndex()`, para que haja correspondência aterramento será retomada. Mais informações podem ser encontradas na [XIM como trabalha com equipes de player](#how-xim-works-with-player-teams) seção deste documento.

Da mesma forma, ao habilitar o emparelhamento de aterramento com um `XimNetworkConfiguration` estrutura com o `RequirePlayerRolesAndSkillConfiguration` campo definido como true, em seguida, todos os jogadores devem ter especificado uma configuração de emparelhamento não nulo por player. Se não tiver qualquer player, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `XimMatchmakingProgressUpdatedStateChange` com um `XimMatchMakingStatus.WaitingForPlayerRolesAndSkillConfiguration` valor. Depois que tem fornecido a todos os jogadores seus `XimPlayerRolesAndSkillConfiguration` objetos, para que haja correspondência aterramento será retomada. Mais informações podem ser encontradas na [cruzamento usando habilidades por player](#matchmaking-using-per-player-skill) e [cruzamento usando a função por player](#matchmaking-using-per-player-role) seções deste documento.

## <a name="querying-joinable-networks"></a>Redes permite junções de consulta

Enquanto o emparelhamento é uma ótima maneira de conectar os jogadores juntos rapidamente, às vezes, é melhor permitir que os jogadores descobrir permite junções redes usando critérios de pesquisa personalizado e, em seguida, selecione a rede que desejam ingressar. Isso pode ser particularmente vantajoso quando uma sessão de jogo pode ter um grande conjunto de regras configuráveis de jogos e preferências de player. Para fazer isso, uma rede existente deve primeiro ser feita passível de consulta, permitindo `XimAllowedPlayerJoins.Queried` joinability e configurando as informações de rede disponíveis para outras pessoas fora da rede por meio de uma chamada para `XboxIntegratedMultiplayer.SetNetworkConfiguration()`.

O exemplo a seguir habilita `XimAllowedPlayerJoins.Queried` joinability, rede de conjuntos de configuração com uma configuração de equipe que permite um total de jogadores de 1 a 8 juntos em 1 equipe, um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor GAME_MODE_BRAWL, uma descrição "cat e conversão boxing do Ovelha corresponderem", um uint32_t constante de índice específico do aplicativo de mapa definido pelo valor MAP_KITCHEN e inclui spectatorallowed"marcas"chatrequired","simples",":

```cs
string[] tags = { "chatrequired", "easy", "spectatorallowed" };
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Queried;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = GAME_MODE_BRAWL;
networkConfiguration.Description = "Cat and sheep's boxing match";
networkConfiguration.MapIndex = MAP_KITCHEN;
networkConfiguration.SetTags(tags);
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Outros jogadores fora da rede, em seguida, podem encontrar a rede por meio da chamada xim::start_joinable_network_query() com um conjunto de filtros que corresponde às informações de rede na chamada xim::set_network_configuration() anterior. O exemplo a seguir inicia uma consulta de rede permite junções com a opção de filtro do modo de jogo que consultará apenas para redes que usam o modo de jogo específico do aplicativo definido pelo valor GAME_MODE_BRAWL:

```cs
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Aqui está outro exemplo que usa a opção de filtros de marca de consulta para redes com a marca "simples" e "spectatorallowed" na configuração pública que podem ser consultada:

```cs
string[] tagFilters = { "easy", "spectatorallowed" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Opções de filtro diferentes também podem ser combinadas. O exemplo a seguir que usa a opção de filtro do modo de jogo e a marca de opção de filtro para iniciar uma consulta para redes que têm o GAME_MODE_BRAWL constante de modo de jogo específico do aplicativo e a marca "simples":

```cs
string[] tagFilters = { "easy" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Se a operação de consulta for bem-sucedida, o aplicativo receberá um xim_start_joinable_network_query_completed_state_change do qual o aplicativo pode recuperar uma lista de redes permite junções. O aplicativo receberá também continuamente `XimJoinableNetworkUpdatedStateChange` para redes permite junções adicionais ou quaisquer alterações que ocorrem com a lista retornada de redes permite junções até que ela seja interrompida manualmente ou automaticamente. A consulta em andamento pode ser interrompida manualmente chamando `XboxIntegratedMultiplayer.StopJoinableNetworkQuery()`. Ele será interrompido automaticamente ao chamar `XboxIntegratedMultiplayer.StartJoinableNetworkQuery()` para iniciar uma nova consulta.

O aplicativo pode tentar ingressar em uma rede na lista de redes permite junções chamando `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()`. O exemplo a seguir pressupõe que você está tentando ingressar em uma rede referenciada por 'selectedNetwork' que não é protegida por uma senha (de modo que estamos passando a cadeia de caracteres vazia para o segundo parâmetro):

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation(selectedNetwork, "");
```

Ao habilitar a consulta de rede com um `XimNetworkConfiguration.TeamConfiguration` que declara duas ou mais equipes, jogadores Unidos chamando XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation() será tem um valor de índice de equipe padrão de 0.

> [!NOTE]
> Se o aplicativo tiver especificado a vários usuários locais e ingresse em uma rede com menos espaço do que o número de usuários locais, ainda pode ter êxito, a junção. Mas apenas o número permitido de usuários locais pode se associar à rede.