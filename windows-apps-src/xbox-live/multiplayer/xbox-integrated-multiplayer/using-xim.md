---
title: Usando XIM (C++)
description: Saiba como usar o Xbox integrado com vários participantes (XIM) com o C++.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, xbox um, com vários participantes integrado do xbox
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644051"
---
# <a name="using-xim-c"></a>Usando XIM (C++)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Este é um breve passo a passo sobre como usar a API do C++ do XIM. Desenvolvedores que desejam acessar XIM por meio de jogos C# deve ver [XIM usando (C#)](using-xim-cs.md).

- [Usando XIM (C++)](#using-xim-c)
    - [Pré-requisitos](#prerequisites)
    - [Inicialização e inicialização](#initialization-and-startup)
    - [Operações assíncronas e alterações de estado de processamento](#asynchronous-operations-and-processing-state-changes)
    - [Manipulação de objetos de player XIM](#handling-xim-player-objects)
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

Compilar XIM requer a inclusão do primário `XboxIntegratedMultiplayer.h` cabeçalho. Para vincular corretamente, seu projeto também deve incluir `XboxIntegratedMultiplayerImpl.h` na unidade de compilação de pelo menos um (um cabeçalho pré-compilado comum é recomendado, pois essas implementações de função de stub são pequeno e fácil para o compilador gere como "embutido").

Conforme mencionado na [visão geral de XIM](../xbox-integrated-multiplayer.md), a interface XIM não exige um projeto para escolher entre a compilação com C + + c++ /CLI CX versus C++ tradicionais; ele pode ser usado com qualquer um. A implementação também não gera exceções como um meio de erro não fatal reporting para que você pode consumi-lo facilmente em projetos sem exceções, se preferir.

## <a name="initialization-and-startup"></a>Inicialização e inicialização

Você começa a interagir com a biblioteca Inicializando o singleton de objeto XIM com o Xbox Live serviço configuração ID cadeia de caracteres e o título do número de identificação. Se você também estiver usando a API de serviços do Xbox, talvez seja conveniente para recuperar essas de `xbox::services::xbox_live_app_config`. O exemplo a seguir pressupõe que os valores já residem nas variáveis 'myServiceConfigurationId' e 'myTitleId', respectivamente:

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

Depois de inicializado, o aplicativo deve recuperar as cadeias de caracteres de ID de usuário do Xbox para todos os usuários atualmente no dispositivo local que participarão e passá-los para um `xim::set_intended_local_xbox_user_ids()` chamar. O código de exemplo a seguir pressupõe um único usuário pressionou um botão do controlador de expressar a intenção para reproduzir e a cadeia de caracteres de ID de usuário do Xbox associada com o usuário já foi recuperada em uma variável 'myXuid':

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

Uma chamada para `xim::set_intended_local_xbox_user_ids()` imediatamente define as IDs de usuário do Xbox associados com os usuários locais que devem ser adicionados à rede XIM. Esta lista de IDs de usuário do Xbox será usada em todas as operações de rede futuros até que as alterações de lista por meio de outra chamada para `xim::set_intended_local_xbox_user_ids()`.

No caso em que nenhuma rede XIM em todos os ainda não existe, a primeira etapa é mover para uma rede XIM a fim de que o processo iniciado. Se o usuário ainda não tiver uma rede XIM específica em mente, a prática recomendada é simplesmente mover a uma rede nova e vazia. Isso permite que os amigos do usuário ingressar na rede como uma espécie de "lobby" da qual eles podem colaborar em conjunto para selecionar sua próxima atividade com vários participantes (por exemplo, inserindo o relacionamento de pessoas).

A seguir está um exemplo de como mover apenas os usuários locais adicionados anteriormente com `xim::set_intended_local_xbox_user_ids()` a uma rede XIM vazia com espaço para até 8 players total:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

A chamada para `xim::move_to_new_network()` mover assíncrona inicia a operação que termina com um evento de alteração de estado que os desenvolvedores devem processar regularmente para.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Operações assíncronas e alterações de estado de processamento

O coração do XIM é chamadas de regulares e frequentes do aplicativo para o `xim::start_processing_state_changes()` e `xim::finish_processing_state_changes()` par de métodos. Esses métodos são como XIM é informado de que o aplicativo está pronto para lidar com as atualizações para o estado com vários participantes e como XIM oferece essas atualizações. Eles são projetados para operar rapidamente, de modo que eles podem ser chamados todos os quadros de gráficos em seu loop de renderização da interface do usuário. Isso fornece um local conveniente para recuperar todas as alterações na fila sem se preocupar com a imprevisibilidade de intervalos de rede ou complexidade de retorno de chamada com multithread. A API XIM, na verdade, é otimizada para esse padrão de thread único. Ele garante que seu estado permanecerá inalterado fora essas duas funções, portanto, você pode usá-lo de forma direta e eficiente.

Pela mesma razão, devem todos os objetos retornados pela API do XIM *não* ser consideradas thread-safe. A biblioteca tem proteção multithreading interna, mas você ainda precisará implementar seu próprio bloqueio, se você precisar de um thread arbitrário para acessar qualquer um dos valores do XIM. Por exemplo, no caso de ter um thread a percorrer os `xim::players()` listar enquanto outro thread poderia ser invocar qualquer um `xim::start_processing_state_changes()` ou `xim::finish_processing_state_changes()` e alterar a memória associada a essa lista de player.

Quando `xim::start_processing_state_changes()` é chamado, todas as atualizações em fila são relatadas em uma matriz de `xim_state_change` estrutura ponteiros.

Aplicativos devem:

1. Itere sobre a matriz.
1. Inspecione a estrutura de base para seu tipo mais específico.
1. Converter o tipo de estrutura de base para os respectivos mais detalhadas de tipo.
1. Lidar com essa atualização conforme apropriado.

Após a conclusão com todos os `xim_state_change` estruturas disponíveis no momento, essa matriz deve ser passada para XIM para liberar os recursos chamando `xim::finish_processing_state_changes()`. Por exemplo:

```cpp
uint32_t stateChangeCount;
xim_state_change_array stateChanges;
xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
{
   const xim_state_change * stateChange = stateChanges[stateChangeIndex];
   switch (stateChange->state_change_type)
   {
       case xim_state_change_type::player_joined:
       {
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Agora que você tem seu loop de processamento básico, você pode manipular as alterações de estado associadas inicial `xim::move_to_new_network()` operação. Cada operação de movimentação de rede XIM começará com um `xim_move_to_network_starting_state_change`. Se a migração falha por algum motivo, seu aplicativo será fornecido um `xim_network_exited_state_change`, que é a mecanismo para qualquer erro assíncrono que impede que você mova para uma rede XIM ou se desconecta da rede XIM atual de tratamento de falha comuns. Caso contrário, a movimentação será concluída com um `xim_move_to_network_succeeded_state_change` depois que todo o estado tiver sido finalizado e todos os jogadores foi adicionados com êxito à rede XIM.

Quando um usuário não tem o privilégio de plataforma para brincar com outras pessoas em uma sessão para múltiplos jogadores, XIM enviará a um dispositivo tentar criar ou ingressar em uma rede XIM uma `xim_network_exited_state_change` com o motivo pelo qual `xim_network_exit_reason::user_multiplayer_restricted`. Isso acontece quando qualquer um dos usuários locais definidos com `xim::set_intended_local_xbox_user_ids()` tem uma restrição para múltiplos jogadores do Xbox Live. Aplicativos do Xbox ERA usando XIM devem chamar `Store::Product::CheckPrivilegeAsync` em players locais antes de tentar movê-los em uma rede XIM com attemptResolution definido como true, ou fazer isso em resposta ao recebimento `xim_network_exit_reason::user_multiplayer_restricted` como um motivo para um `xim_network_exited_state_change`.

## <a name="handling-xim-player-objects"></a>Manipulação de objetos de player XIM

Supondo que o exemplo de movimentação de um único usuário local para uma nova rede XIM foi bem-sucedida, seu aplicativo será fornecido uma `xim_player_joined_state_change` para um local `xim_player` objeto. Esse ponteiro de objeto permanecerá válido para desde que a própria instância do player é válida. A instância do player torna-se inválido quando correspondente `xim_player_left_state_change` para esse jogador instância foi retornada por meio de uma chamada para `xim::finish_processing_state_changes()`. Seu aplicativo sempre será fornecido uma `xim_player_left_state_change` para cada `xim_player_joined_state_change`.

Você também pode recuperar uma matriz de todos os `xim_player` objetos no XIM de rede a qualquer momento usando `xim::get_players()`.

O `xim_player` objeto tem muitos métodos úteis, como `xim_player::gamertag()` para recuperar a cadeia de caracteres de nome de jogador do Xbox Live atual associado com o player para fins de exibição. Se o `xim_player` é local para o dispositivo, em seguida, ele também relata um não-nulo `xim_player::xim_local` ponteiro de objeto de `xim_player::local()`, que tem métodos adicionais disponíveis apenas para empresas locais.

É claro, o estado mais importante para os jogadores não é informações comuns que XIM sabe, mas o que seu aplicativo específico que deseja rastrear. Como você provavelmente tem sua própria construção para obter essas informações de rastreamento, você desejará vincular a `xim_player` do objeto ao objeto de construção de seu próprio player para que os relatórios XIM a qualquer momento um `xim_player`, você pode recuperar rapidamente seu estado sem ter que popular e Pesquise um ponteiro de contexto de player personalizado. O exemplo a seguir pressupõe que é um ponteiro para seu estado particular na variável 'myPlayerStateObject' e recém-adicionada `xim_player` objeto está na variável 'newXimPlayer':

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

Isso salva o valor do ponteiro especificada com o objeto jogador localmente (nunca transferido pela rede para dispositivos remotos em que a memória não seria válida). Em seguida, você poderá sempre voltar ao seu objeto de contexto personalizado de recuperando e convertê-la de volta para seu objeto, como o exemplo a seguir:

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

Você pode alterar o ponteiro de contexto de player personalizado atribuído um `xim_player` a qualquer momento.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Habilitando amigos unir e convidando-os

Privacidade e segurança, todas as novas redes XIM são automaticamente configuradas por padrão para ser apenas permite junções por empresas locais e cabe ao aplicativo para permitir explicitamente outros quando ele estiver pronto. O exemplo a seguir mostra como usar `xim::network_configuration()` para recuperar a configuração de rede atual e atualizar joinability usando `xim::set_network_configuration()` para começar a permitir que novos usuários locais para ingressar como players, bem como outros usuários que foram convidados ou que estão sendo " seguido de"(uma Xbox Live social relação) players já na rede XIM:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` executa asynchronoulsy. Quando a chamada de exemplo de código anterior for concluída, uma `xim_network_configuration_changed_state_change` é fornecido para notificar o aplicativo que o valor de joinability foi alterado do seu padrão de `xim_allowed_player_joins::none`. Em seguida, você pode consultar para o novo valor, verificando o `allowed_player_joins` propriedade do `xim_network_configuration` retornado pela `xim::network_configuration()`. 

O `allowed_player_joins` pode ser verificado enquanto o dispositivo está em uma rede XIM para determinar o joinability da rede.

Um dos participantes locais deseja enviar convites para usuários remotos para associar a esta rede XIM, o aplicativo pode chamar `xim_player::xim_local::show_invite_ui()` para iniciar o convite da interface do usuário do sistema. Aqui, o usuário local pode selecionar as pessoas que desejam convidar e enviar convites. O exemplo a seguir demonstra como isso funciona e pressupõe que a variável 'ximPlayer' aponta para um local válido `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

Depois que a declaração acima é executado, o convite de sistema interface do usuário será exibido. Depois que o usuário enviou os convites (ou ignorado caso contrário, a interface do usuário), uma `xim_show_invite_ui_completed_state_change` serão fornecidos na próxima chamada para `xim::start_processing_state_changes()`. Como alternativa, o aplicativo pode enviar os convites diretamente usando `xim_player::xim_local::invite_users()` sem abrir o convite da interface do usuário do sistema.

De qualquer forma, os usuários remotos receberão mensagens de convite Xbox Live independentemente de onde eles estão conectados, que eles podem optar por aceitar ou rejeitar. Se aceitar, uma ativação de protocolo iniciará seu aplicativo nesses dispositivos, se ele já não estiver em execução e a ativação de protocolo será entregue ao seu aplicativo com seus argumentos de evento que pode ser usado para mover para a rede XIM correspondente.

Consulte a documentação da plataforma para obter mais informações sobre a ativação de protocolo em si.

O exemplo a seguir mostra como uma chamada para `xim::extract_protocol_activation_information()` determina se os argumentos de evento são aplicáveis a XIM. Isso pressupõe que você recuperou a cadeia de caracteres bruta do URI de `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` a uma variável 'uriString':

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

Se ele for uma ativação XIM, você desejará garantir que o usuário local identificado no campo 'local_xbox_user_id' de populada `xim_protocol_activation_information` estrutura está conectada e está entre os usuários especificados para `xim::set_intended_local_xbox_user_ids()`. Em seguida, você pode iniciar a mover para a rede XIM especificada com uma chamada para `xim::move_to_network_using_protocol_activated_event_args()` usando a mesma cadeia de caracteres do URI. Por exemplo:

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

Também Observe que "seguido" usuários remotos possa navegar para o cartão de player do usuário local no sistema de interface do usuário e iniciar uma tentativa de junção em si sem um convite (supondo que você permitiu tais junções de player, conforme mostrado acima). Essa ação também será de protocolo ativar seu aplicativo exatamente como são tratados da mesma forma e fazem a convites.

Mover para uma rede XIM usando a ativação de protocolo é idêntico ao mover para uma nova rede XIM, conforme mostrado na [inicialização e inicialização](#initialization-and-startup). A única diferença é que quando a movimentação for bem-sucedida, o dispositivo móvel será fornecido player local e remoto `xim_player_joined_state_change` estruturas que representam todos os jogadores aplicáveis. Naturalmente, o dispositivo original que já está na rede XIM não estar movendo, mas verá que os usuários do novo dispositivo ser adicionado como players por outras `xim_player_joined_state_change` estruturas.

Depois que os jogadores em diferentes dispositivos são Unidos em uma rede XIM, pode iniciar os cenários com vários participantes, se comunicar através de voz e texto automaticamente e enviar mensagens específicas de aplicativo.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Emparelhamento básico e movendo a outra rede XIM com outras pessoas

Você pode expandir ainda mais a experiência de rede para um grupo de amigos, movendo os jogadores a uma rede XIM que também tenha estranhos. Estranhos são adversários de todo o mundo que sejam colocados juntos usando o serviço de cruzamento Xbox Live com base nos interesses semelhantes.

Uma maneira simples para iniciar emparelhamento básico com XIM é chamando `xim::move_to_network_using_matchmaking()` em um dos dispositivos com um populados `xim_matchmaking_configuration` estrutura, levando os jogadores da rede XIM atual junto com ele.

O exemplo a seguir inicia uma movimentação usando uma configuração de emparelhamento configurada para encontrar um total de 8 players para uma correspondência de equipes não festa para todos. Se o total de 8 players não é encontrados, o sistema ainda pode corresponder players de 2 a 7 em conjunto. Este exemplo usa uma constante definida pelo aplicativo, do tipo uint64_t, chamado MYGAMEMODE_DEATHMATCH, que representa o modo de jogo para filtrar fora de. Para que haja correspondência do XIM corresponderá apenas a essa rede com outros jogadores especificando que mesmo valor e traz ao longo de jogadores tudo socialmente ingressados da rede XIM atual ao fornecer o segundo parâmetro `xim_players_to_move::bring_existing_social_players` para `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Como move anterior, esse processo de emparelhamento fornecerá um inicial `xim_move_to_network_starting_state_change` em todos os dispositivos e um `xim_move_to_network_succeeded_state_change` depois que a migração seja concluída com êxito. Como essa é uma mudança de uma rede XIM para outra, uma diferença é que já existem `xim_player` objetos adicionados para usuários locais e remotos, e eles permanecerão de todos os jogadores que estão migrando em conjunto para a nova rede XIM. Bate-papo e dados de comunicação entre os jogadores continuarão funcionando sem interrupções enquanto o emparelhamento está em andamento.

Para que haja correspondência pode ser um processo demorado, dependendo do número de jogadores potencias no pool de relacionamento de pessoas que também ter chamado `xim::move_to_network_using_matchmaking()`.

Um `xim_matchmaking_progress_updated_state_change` será fornecida periodicamente durante a operação para manter os usuários informados sobre o status atual. Quando a correspondência foi encontrada, os jogadores adicionais são adicionados à rede com o típico XIM `xim_player_joined_state_change` e a migração seja concluída.

Depois que terminar a experiência para múltiplos jogadores com esse conjunto de jogadores "matchmade", você pode repetir o processo para mover para uma rede XIM diferente com outra rodada de emparelhamento. Você verá cada jogador que unidas por meio de prévia `xim::move_to_network_using_matchmaking()` operação fornecem uma `xim_player_left_state_change` para indicar que seus `xim_player` objetos não estão mais na mesma rede XIM.

Se quando você escolhe iniciar emparelhamento novamente usando `xim::move_to_network_using_matchmaking()`, você especifica `xim_players_to_move::bring_existing_social_players`, os jogadores que unidas por meio de pontos de entrada social (`xim::move_to_network_using_protocol_activated_event_args()` ou `xim::move_to_network_using_joinable_xbox_user_id()`) permanecerá enquanto ocorre o cruzamento de novo. Como alternativa, especificando `xim_players_to_move::bring_only_local_players` desconectará socialmente conectados players remotos, deixando apenas os jogadores locais para permanecer. Em ambos os casos, um conjunto diferente de estranhos será adicionado quando a segunda operação de movimentação é concluída.

Você também tem a opção de mover os players não matchmade (ou apenas locais players) a uma rede XIM completamente nova durante a decisão a próxima atividade multiplayer/configuração de emparelhamento. O exemplo a seguir demonstra como um dispositivo chamaria `xim::move_to_new_network()` para uma rede XIM com um máximo de 8 jogadores, mas desta vez levando os socialmente ingressados players existentes também:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

Um `xim_move_to_network_starting_state_change` e `xim_move_to_network_succeeded_state_change` será fornecido para todos os dispositivos participantes, juntamente com um `xim_player_left_state_change` para os jogadores matchmade que foram deixados para trás. Nesses dispositivos, eles da mesma forma verão um `xim_player_left_state_change` para cada jogador que é movido para fora da rede.

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

Quando terminar de usuários locais participando de uma XIM rede, muitas vezes que eles simplesmente serão movidos para uma nova rede XIM que permite que os usuários locais, usuários convidados e "seguido" os usuários ingressem para que eles possam continuar coordenando com seus amigos em localizar a próxima atividade. Mas se o usuário é feito completamente com todas as experiências com vários participantes, seu aplicativo talvez queira começar deixando o XIM completamente de rede e retornar ao estado como se apenas `xim::initialize()` e `xim::set_intended_local_xbox_user_ids()` tivesse sido chamada. Isso é feito usando o `xim::leave_network()` método:

```cpp
xim::singleton_instance().leave_network();
```

Esse método inicia o processo de forma assíncrona desconexão de outros participantes normalmente. Isso fará com que os dispositivos remotos a ser fornecido um `xim_player_left_state_change` para cada jogador local que desconectadas. O dispositivo local será também ser fornecido um `xim_player_left_state_change` para cada jogador de local e remota foi desconectada. Quando se desconecta todas as operações tenham terminado, um final `xim_network_exited_state_change` será fornecido. O aplicativo pode chamar ' xim::cleanup() ' para liberar todos os recursos e retornar para o estado não inicializado:

```cpp
xim::singleton_instance().cleanup();
```

Invocando `xim::leave_network()` e aguardando a `xim_network_exited_state_change` para sair de um XIM rede normalmente é sempre recomendável quando um `xim_network_exited_state_change` já não foi fornecido.

Se `xim::cleanup()` é chamado diretamente sem `xim::leave_network()` que está sendo chamado, problemas de desempenho de comunicação para os demais participantes ocorrerá, pois eles serão forçados a enviar mensagens não entregues ao dispositivo que simplesmente "desapareceu", chamar `xim::cleanup()`.

## <a name="sending-and-receiving-messages"></a>Enviando e recebendo mensagens

XIM e seus componentes subjacentes fazem todo o trabalho tedioso de estabelecer canais de comunicação seguro pela Internet para que você não precise se preocupar sobre problemas de conectividade ou que está sendo capaz de alcançar a alguns, mas nem todos os jogadores. Se houver quaisquer problemas de conectividade de ponto a ponto fundamental, movendo-se a uma rede XIM não terá êxito.

Quando uma rede XIM é formada e confirmada com `xim_move_to_network_succeeded_state_change`, todas as instâncias do seu aplicativo em todos os dispositivos Unidos têm garantia de ser informado sobre cada `xim_player` conectado à rede XIM e pode enviar mensagens para qualquer um deles.

Vamos demonstrar como enviar mensagens pela rede XIM. O exemplo a seguir pressupõe que a variável 'sendingPlayer' é um ponteiro para um objeto player local válido. 'sendingPlayer' invoca `local()` e chama `send_data_to_other_players()` para enviar a estrutura de mensagens 'msgData' para todos os jogadores (locais e remotos) com entrega garantida, sequencial. Observe que ele vai a todos os jogadores porque uma lista de jogadores específicos não foi passada para o método:

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

As mensagens podem ser entregues a todos os jogadores usando `send_data_to_other_players()` , não passando uma matriz de jogadores específicos para o método.

Todos os destinatários da mensagem são fornecidos uma `xim_player_to_player_data_received_state_change` que inclui um ponteiro para uma cópia dos dados, bem como ponteiros para o xim_player correspondente do objeto que o enviou e localmente estão recebendo.

É claro, entrega sequencial garantida é conveniente, mas também pode ser um tipo de envio ineficiente, pois XIM precisa retransmitir ou adiar as mensagens se os pacotes são descartados ou fora de ordem pela Internet. Não se esqueça de considerar o uso de outros tipos de envio para mensagens que seu aplicativo pode tolerar perder ou ter que chegam fora de ordem. Como os dados da mensagem provêm de um computador remoto, a prática recomendada é tenham definido claramente os formatos de dados, como empacotamento valores de vários bytes em uma ordem de byte específico ("endianness"). O aplicativo também deve validar os dados antes de agir sobre ele.

XIM fornece segurança em nível de rede, portanto, não há nenhuma necessidade de esquemas adicionais de criptografia ou assinatura. No entanto, é sempre recomendável empregar "defesa em profundidade" para proteger contra a aplicação acidental de bugs e ser capaz de lidar com diferentes versões do seu aplicativo protocolo coexistindo normalmente (durante o desenvolvimento, atualizações de conteúdo, etc.). Conexões de Internet dos usuários podem ser limitados e em constante mudança de recursos. É altamente recomendável o uso de formatos de dados de mensagens eficiente e evitando designs que enviar cada quadro de interface do usuário.

## <a name="assessing-data-pathway-quality"></a>Avaliar a qualidade do caminho de dados

Para saber mais sobre a qualidade atual do caminho de dados entre dois jogadores, chame o `xim_player::network_path_information()` método. O exemplo a seguir recupera um ponteiro para o `xim_network_path_information` de estrutura para um `xim_player` ponteiro contido na variável 'remotePlayer':

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

Estrutura retornada inclui a latência de viagem estimado e quantas mensagens ainda estão na fila localmente para transmissão no caso de que a conexão não dá suporte ao transmitir mais dados para o momento.

Se você vir que as filas estiver fazendo backup para um determinado 'xim_player', você deve reduzir a taxa na qual você está enviando dados.

O `xim_network_path_information::round_trip_latency_in_milliseconds` campo representa a latência da rede subjacente e a latência de estimado do XIM sem enfileiramento de mensagens. Aumenta a latência eficaz como `xim_network_path_information::send_queue_size_in_messages` cresce e XIM funciona por meio da fila.

Escolha um ponto razoável para começar a limitá-chamadas para `send_data_to_other_players` com base nos requisitos e o uso do jogo. Todas as mensagens na fila de envio representa um aumento na latência de rede em vigor.

Um valor próximo ao limite máximo do XIM (atualmente 3500 mensagens) é muito alto para a maioria dos jogos e provavelmente representa vários segundos de dados que estão aguardando para ser enviado, dependendo da taxa de chamada `send_data_to_other_players` e é o tamanho máximo de cada carga de dados. Em vez disso, escolha um número que leva em conta os requisitos de latência do jogo, juntamente com trêmulo como o jogo `send_data_to_other_players` é padrão de chamada.

## <a name="sharing-data-using-player-custom-properties"></a>Compartilhamento de dados usando as propriedades personalizadas do player

A maioria das trocas de dados de aplicativo acontecem com o `xim_player::xim_local::send_data_to_other_players()` método, pois ele permite mais controle sobre quem recebe dados, quando eles devem receber esses dados e como o sistema deve lidar com perda de pacotes. No entanto, há vezes quando poderia ser bom para os jogadores compartilhar o estado básico, raramente são alterado sobre si mesmos com outras pessoas sem complicação. Por exemplo, cada jogador pode ter uma cadeia de caracteres fixa, que representa o modelo de caractere selecionado antes de entrar com vários participantes para que todos os jogadores usam para renderizar sua representação no jogo.

Para dados que não são alterados com muita frequência sobre um player, XIM fornece player propriedades personalizadas. Essas propriedades consistem em um nome definido pelo aplicativo e um valor, que são pares de cadeia de caracteres terminada em nulo que pode ser aplicado ao player local e que são propagadas automaticamente para todos os dispositivos sempre que eles são alterados.

Propriedades de player personalizado têm sincronização interna mais sobrecarga da `xim_player::xim_local::send_data_to_other_players()` método, para mudar rapidamente os dados (ou seja, a posição de player), você ainda deve usar envia direta.

Pares de chave-valor de propriedades personalizadas do Player têm seus valores atuais fornecidas automaticamente para novos dispositivos participantes quando esses dispositivos ingressar em uma rede XIM e ver o player adicionado. Valores podem ser definidos por meio da chamada `xim_player::xim_local::set_player_custom_property()` com cadeias de caracteres de nome e valor. O exemplo a seguir, definimos uma propriedade chamada "modelo" para que o valor "bruta" em um local `xim_player` objeto apontado pela variável 'localPlayer':

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

As alterações nas propriedades do player fará com que um `xim_player_custom_properties_changed_state_change` ser fornecida para todos os dispositivos, avisando-os para os nomes das propriedades que foram alterados. O valor para um determinado nome pode ser recuperado em qualquer player, local ou remoto, com `xim_player::get_player_custom_property()`. O exemplo a seguir recupera o valor de uma propriedade chamada "modelo" de um `xim_player` apontado pela variável 'ximPlayer':

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

Definindo um novo valor para um nome de propriedade determinado substituirá qualquer valor existente. Configurar um nome de propriedade com um valor de um ponteiro de cadeia de caracteres do valor nulo é tratado o mesmo que uma cadeia de caracteres do valor vazio, que é o mesmo que a propriedade ter que ainda não foi especificada. Nomes e valores não são interpretados pelo XIM, portanto, é deixado no aplicativo para validar o conteúdo de cadeia de caracteres conforme necessário.

Propriedades de player personalizado sempre são redefinidas ao mover de uma rede XIM para outro. No entanto, os novos jogadores que se associar à rede XIM receberá propriedades atuais do player personalizado para todos os jogadores que têm um conjunto.

## <a name="sharing-data-using-network-custom-properties"></a>Compartilhamento de dados usando as propriedades personalizadas da rede

Propriedades personalizadas da rede destinam-se como uma conveniência a sincronização de dados específicos à rede XIM que não muda com frequência. Forma idêntica ao trabalho de propriedades personalizadas de rede [propriedades personalizadas do player](#sharing-data-using-player-custom-properties), exceto que eles são definidos no objeto de singleton XIM com `xim::set_network_custom_property()`. O exemplo a seguir define uma propriedade de "mapa" para que o valor "sol":

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

As alterações nas propriedades de rede fará com que um `xim_network_custom_properties_changed_state_change` ser fornecida para todos os dispositivos, avisando-os para os nomes das propriedades que foram alterados. O valor para um determinado nome pode ser recuperado com `xim::get_network_custom_property()`, como no exemplo a seguir, que recupera o valor de um propriedade nomeada "mapa":

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

Assim como [propriedades personalizadas do player](#sharing-data-using-player-custom-properties), definindo um novo valor para um nome de propriedade determinado substituirá qualquer valor existente. Configurar um nome de propriedade com um valor de um ponteiro de cadeia de caracteres do valor nulo é tratado o mesmo que uma cadeia de caracteres do valor vazio, que é o mesmo que a propriedade ter que ainda não foi especificada. Nomes e valores não são interpretados pelo XIM, portanto, é deixado no aplicativo para validar o conteúdo de cadeia de caracteres conforme necessário.

Recém-criados XIM redes sempre iniciar com nenhum conjunto de propriedades personalizadas de rede. No entanto, novos jogadores que unem a uma rede existente do XIM receberão os valores atuais das propriedades personalizadas de rede definido para essa rede XIM.

## <a name="matchmaking-using-per-player-skill"></a>Para que haja correspondência usando habilidades por player

Correspondência de jogadores por interesse comum em um determinado modo de jogo especificado pelo aplicativo é uma boa estratégia de base. À medida que aumenta o pool de jogadores disponíveis, você deve considerar a correspondência também players com base em suas habilidades pessoais ou experiência com o seu jogo para que os jogadores veteranos podem aproveitar o desafio de concorrência Íntegro com outros veteranos enquanto players mais recentes podem crescer por competir com outras pessoas com capacidades semelhantes.

Para fazer isso, inicie, fornecendo o nível de habilidade de todos os jogadores locais em suas funções por player e a estrutura de configuração de habilidades especificadas em chamas para `xim_player::xim_local::set_roles_and_skill_configuration()` antes de começar a mover para um XIM usando emparelhamento de rede. Nível de habilidade é um conceito específico do aplicativo e o número não é interpretado pelo XIM, exceto pelo fato de relacionamento de pessoas primeiro tentará localizar jogadores com o mesmo valor de habilidade e, em seguida, periodicamente, ampliar sua pesquisa em incrementos de + /-10 para localizar outros jogadores declarando a habilidade valores dentro de um intervalo em torno dessa habilidade. O exemplo a seguir pressupõe que o local `xim_player` objeto cujo ponteiro é 'localPlayer', tem um valor de habilidades de uint32_t de específicos do aplicativo associado recuperado do armazenamento local ou Xbox Live em uma variável denominada 'playerSkillValue':

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Quando isso for concluído, todos os participantes serão fornecidos um `xim_player_roles_and_skill_configuration_changed_state_change` indicando isso `xim_player` mudou sua funções por player e a configuração de habilidade. O novo valor pode ser recuperado chamando `xim_player::roles_and_skill_configuration()`. Quando todos os jogadores têm funções de não-nulo e a configuração de habilidade aplicado, você pode mover para uma rede XIM usando o emparelhamento com um valor de verdadeiro para o `require_player_roles_and_skill_configuration` campo do `xim_matchmaking_configuration` estrutura especificada para `xim::move_to_network_using_matchmaking()`. O exemplo a seguir preenche uma configuração de emparelhamento que encontrará um total de jogadores de 2 a 8 para uma festa para todos não equipes, usando um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor MYGAMEMODE_DEATHMATCH corresponderá apenas com outros jogadores especificando o mesmo valor e que requer configuração de habilidade e funções por player:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Quando essa estrutura é fornecida para `xim::move_to_network_using_matchmaking()`, a operação de movimentação será iniciado normalmente desde que os jogadores movendo tem chamado `xim_player::xim_local::set_roles_and_skill_configuration()` não nulo `xim_player_roles_and_skill_configuration` ponteiro. Se não tiver qualquer player, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `xim_matchmaking_progress_updated_state_change` com um `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valor. Isso inclui os jogadores que subsequentemente ingressarem na rede XIM por meio de um convite enviado anteriormente ou outros meios (por exemplo, uma chamada para `xim::move_to_network_using_joinable_xbox_user_id()`) antes de cruzamento foi concluída. Depois que tem fornecido a todos os jogadores seus `xim_player_roles_and_skill_configuration` estruturas, para que haja correspondência será retomada.

Para que haja correspondência usando habilidades por player também pode ser combinada com a função de por player do usuário para que haja correspondência, conforme explicado na próxima seção. Se apenas uma for desejada, você pode especificar um valor de 0 para o outro. Isso ocorre porque todos os jogadores declarando que eles têm um `xim_player_roles_and_skill_configuration` valor da habilidade de 0 será sempre correspondem uns aos outros.

Uma vez a `xim::move_to_network_using_matchmaking()` ou qualquer outra rede XIM mover a operação for concluída, todos os jogadores `xim_player_roles_and_skill_configuration` estruturas serão limpo automaticamente como um ponteiro nulo (com uma que acompanha `xim_player_roles_and_skill_configuration_changed_state_change` notificação). Se você planeja mover para outra rede XIM usando emparelhamento que requer a configuração por player, você precisará chamar `xim_player::xim_local::set_roles_and_skill_configuration()` novamente com um novo ponteiro de estrutura que contém as informações mais atualizadas.

## <a name="matchmaking-using-per-player-role"></a>Para que haja correspondência usando a função por player

Outro método do uso de funções por player e a configuração de habilidades para melhorar a experiência dos usuários para que haja correspondência é com o uso de funções necessárias do player. Isso é ideal para jogos que fornecem os tipos de caractere selecionável que incentivam play cooperativo diferentes estilos; ou seja, tipos que simplesmente não alteram a representação gráfica do jogo, mas controlam complementares, com impacto atributos como defensivas "healers" versus ofensas de fechamento "melee" versus "range" distante de ataque de suporte. Personalidades dos usuários significa que podem preferir serem jogados como uma especialização específica. Mas se o seu jogo é projetado de forma que funcionalmente, não é possível concluir objetivos sem pelo menos uma pessoa que atender a cada função, às vezes é melhor coincidir com esses players exigem que primeiro junto que a correspondência qualquer jogadores em conjunto, em seguida, eles negociar uma vez coletados Play estilos entre si. Você pode fazer isso primeiro definindo um sinalizador de bit exclusivo que representa cada função a ser especificado em um player determinado `xim_player_roles_and_skill_configuration` estrutura.

O exemplo a seguir define um valor da função específicos do aplicativo MYROLEBITFLAG_HEALER uint8_t para o objeto local xim_player, cujo ponteiro é 'localPlayer':

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Quando isso for concluído, todos os participantes serão fornecidos um `xim_player_roles_and_skill_configuration_changed_state_change` indicando isso `xim_player` alterou sua configuração de função por player. O novo valor pode ser recuperado chamando `xim_player::roles_and_skill_configuration()`.

Global `xim_matchmaking_configuration` estrutura especificada para `xim::move_to_network_using_matchmaking()` devem ter todos os sinalizadores de funções necessárias combinados usando OR bit a bit e um valor true para o campo require_player_matchmaking_configuration.

O exemplo a seguir preenche uma configuração de emparelhamento que encontrará um total de 3 jogadores para uma festa para todos não equipes. Além disso, este exemplo usa uma constante definida pelo aplicativo, que é do tipo uint64_t e chamado MYGAMEMODE_COOPERATIVE, que representa o modo de jogo para filtrar fora de. Além disso, a configuração está definida para exigir onde pelo menos um player atende cada funções específicas do aplicativo uint8_t que eram OR bit a bit de configuração de habilidade e funções por player tinham juntos e colocada na configuração (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ MELEE e MYROLEBITFLAG_RANGE):

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Quando essa estrutura é fornecida para `xim::move_to_network_using_matchmaking()`, a operação de movimentação será iniciado conforme descrito acima.

Para que haja correspondência usando a função por player também pode ser combinada com a habilidade de-player de usuário para que haja correspondência. Se apenas uma for desejada, especifique um valor de 0 para o outro. Isso ocorre porque todos os jogadores declarando que eles têm uma `xim_player_roles_and_skill_configuration` valor da habilidade de 0 será sempre correspondem uns aos outros; e, se todos os bits são zero no `xim_matchmaking_configuration` required_roles campo, então nenhum bit de função é necessárias para corresponder.

Uma vez a `xim::move_to_network_using_matchmaking()` ou qualquer outra rede XIM mover a operação for concluída, todos os jogadores `xim_player_roles_and_skill_configuration` estruturas serão limpo automaticamente como um ponteiro nulo (com uma que acompanha `xim_player_roles_and_skill_configuration_changed_state_change` notificação). Se você planeja mover para outra rede XIM usando emparelhamento que requer a configuração por player, você precisará chamar `xim_player::xim_local::set_roles_and_skill_configuration()` novamente com um novo ponteiro de estrutura que contém as informações mais atualizadas.

## <a name="how-xim-works-with-player-teams"></a>Como XIM funciona com as equipes do player

Com vários participantes jogos geralmente envolve players organizados em opostos equipes. Torna XIM fácil atribuir equipes ao emparelhamento definindo `xim_team_configuration`. O exemplo a seguir inicia uma movimentação usando o emparelhamento configurado para localizar um total de 8 players para colocar em duas equipes iguais de 4 (embora se 4 não são encontrados, players de 1 a 3 também são aceitos), usar um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor MYGAMEMODE_CAPTURETHEFLAG que será correspondente somente com outros jogadores especificando o mesmo valor e trazendo todos os jogadores socialmente Unido da rede XIM atual:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Quando tal uma operação de movimentação do XIM rede for concluído, os jogadores serão atribuídos um equipe valor de índice 1 por meio de {n} correspondente para as equipes de {n} solicitadas. É o verdadeiro significado de qualquer valor de índice específico da equipe depende do aplicativo. Valor de índice de equipe de um jogador é recuperado via `xim_player::team_index()`. Ao usar um `xim_team_configuration` com duas ou mais equipes, jogadores nunca receberá um valor de índice da equipe de zero pela chamada para `xim::move_to_network_using_matchmaking()`. Isso ocorre em oposição a jogadores são adicionados à rede XIM com qualquer outra configuração ou o tipo de operação de movimentação (como por meio de uma ativação de protocolo resultante de aceitar um convite), que sempre terá um índice de zero de equipe. Pode ser útil tratar o índice da equipe 0 como uma equipe "não atribuída" especial.

O exemplo a seguir recupera o índice de equipe para um objeto xim_player cujo ponteiro está na variável 'ximPlayer':

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

Para a experiência do usuário preferencial (não a oportunidade de menção reduzida para o comportamento de jogadores negativo), o serviço de cruzamento Xbox Live nunca será jogadores de divisão que estão migrando para uma rede XIM juntas em equipes diferentes.

O valor de índice da equipe atribuído inicialmente pelo emparelhamento é apenas uma recomendação e o aplicativo pode alterá-la para players locais a qualquer momento usando `xim_player::xim_local::set_team_index()`. Isso também pode ser chamado em redes XIM que não usam o relacionamento de pessoas em todos os. O exemplo a seguir configura um ponteiro de player localPlayer para ter um novo valor de índice da equipe de um:

```cpp
localPlayer->local()->set_team_index(1);
```

Todos os dispositivos serão informados de que o jogador tem um novo valor de índice de equipe em vigor quando elas são fornecidas uma `xim_player_team_index_changed_state_change` para esse jogador. Você pode chamar `xim_player::xim_local::set_team_index()` a qualquer momento.

Para que haja correspondência avalia as funções de player de necessária independentemente das equipes. Portanto, não tem recomendável para usar as equipes e funções necessárias como critérios de configuração de emparelhamento simultâneas porque as equipes serão balanceadas por contagem de player, não por funções de player foi atendida.

## <a name="working-with-chat"></a>Trabalhando com os bate-papo

Comunicação de bate-papo de voz e texto são habilitadas automaticamente entre jogadores em uma rede XIM. XIM manipula a interagir com todos os hardwares de fone de ouvido e microfone de voz para você. Seu aplicativo não precisará fazer muita coisa para bate-papo, mas ele tem um requisito em relação à bate-papo do texto: suporte a entrada e exibição. Entrada de texto é necessária porque, até mesmo em plataformas ou gêneros de jogos que ainda não tiveram generalizado físico do teclado usar, os jogadores podem configurar o sistema para usar tecnologias assistenciais texto em fala. Da mesma forma, a exibição de texto é necessária porque os jogadores podem configurar o sistema para usar a fala em texto.

Essas preferências podem ser detectadas em players locais chamando `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` e `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, e talvez você queira habilitar condicionalmente os mecanismos de texto. No entanto, recomendamos que você considere tornar o texto de entrada e exibir as opções que estão sempre disponíveis.

 `Windows::Xbox::UI::Accessability` uma classe Xbox One projetada especificamente para fornecer processamento simples de bate-papo do texto no jogo com foco nas tecnologias assistenciais de fala em texto.

Depois que a entrada de texto fornecida por um teclado real ou virtual, passe a cadeia de caracteres para o `xim_player::xim_local::send_chat_text()` método. O seguinte código mostra o envio de uma cadeia de caracteres embutido em código do exemplo de um local `xim_player` objeto apontado pela variável 'localPlayer':

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

Esse texto de bate-papo é entregue a todos os jogadores na rede XIM que podem receber comunicação de bate-papo do player de local de origem. Ele pode ser sintetizado áudio de fala e ele pode ser fornecido como um `xim_chat_text_received_state_change`.

Seu aplicativo deve fazer uma cópia de qualquer cadeia de caracteres de texto recebida e exibi-lo junto com sua identificação do player de origem para uma quantidade apropriada de tempo (ou em uma janela rolável).

Também há algumas práticas recomendadas em relação à bate-papo. É recomendável que em qualquer lugar os jogadores são mostrados, especialmente em uma lista de gamertags como um placar também exibem ícones mudo/falando como comentários do usuário. Isso é feito chamando `xim_player::chat_indicator()` para recuperar um `xim_player_chat_indicator` que representa o status atual, o instantâneo de bate-papo para esse jogador. O exemplo a seguir demonstra o recuperando o valor do indicador para um `xim_player` objeto apontado pela variável ximPlayer para determinar um valor constante do ícone específico para atribuir a uma variável 'iconToShow':

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

O valor relatado pelo `xim_player::chat_indicator()` deve ser alterado com frequência como players de início e parada falando, por exemplo. Ele é projetado para dar suporte a aplicativos sondando cada quadro de interface do usuário como resultado.

> [!NOTE]
> Se um usuário local não tem privilégios suficientes de comunicações devido a suas configurações de dispositivo `xim_player::chat_indicator()` retornará `xim_player_chat_indicator::platform_restricted`. A expectativa para atender aos requisitos para a plataforma é o aplicativo mostrar a iconografia indicando uma restrição de plataforma para bate-papo ou sistema de mensagens e uma mensagem ao usuário indicando o problema. Uma mensagem de exemplo, que é recomendável é: "Desculpe, você não tem permissão para conversar agora."

## <a name="muting-players"></a>Mudo players

Outra prática recomendada é dar suporte a tirar o som de jogadores. XIM controla automaticamente o sistema mudo iniciado por usuários por meio de cartões de player, mas aplicativos devem oferecer suporte ao jogo específico transitório mudo que podem ser executadas na interface do usuário jogo por meio de `xim_player::set_chat_muted()` método. O exemplo a seguir inicia um silenciament remota `xim_player` objeto apontado pela variável 'remotePlayer' para que nenhum bate-papo de voz seja ouvida e chat nenhum texto é recebido dele:

```cpp
remotePlayer->set_chat_muted(true);
```

O mudo entra em vigor imediatamente e não há nenhum `xim_state_change` associados a ele. Ele pode ser desfeito chamando `xim_player::set_chat_muted()` novamente com o valor false. O exemplo a seguir unmutes remota `xim_player` objeto apontado pela variável 'remotePlayer':

```cpp
remotePlayer->set_chat_muted(false);
```

Desativa permanecem em vigor para desde que o `xim_player` existir, incluindo ao mudar para uma nova rede XIM com o player. Ele não será mantido se o jogador sai e reassocia-se o mesmo usuário (como um novo `xim_player` instância).

Normalmente, os jogadores começam no estado unmuted. Se seu aplicativo deseja iniciar um player no estado mudo por motivos de jogo, ele poderá chamar `xim_player::set_chat_muted()` sobre o `xim_player` objeto antes de concluir o processamento associado `xim_player_joined_state_change`, e XIM garantirá que não haja nenhum período de tempo onde o áudio de voz do player pode ser ouvido.

Uma verificação de mudo automático com base em reputação do player ocorre quando a rede XIM ingressa em um player remoto. Se o jogador tem um sinalizador de Reputação ruim, o player é desligado automaticamente. Mudo afeta apenas o estado local e persiste, portanto, se um jogador move entre redes. A verificação automática de sem som com base na reputação é executada uma vez e não reavaliada novamente para desde que o `xim_player` permanece válido.

## <a name="configuring-chat-targets-using-player-teams"></a>Configurar destinos de bate-papo usando as equipes do player

Conforme mencionado na [XIM como trabalha com equipes de player](#how-xim-works-with-player-teams) seção deste documento, o verdadeiro significado de qualquer valor de índice específico da equipe é depende do aplicativo. XIM não interpretá-los, exceto para comparações de igualdade em relação à configuração de bate-papo de destino. Se a configuração de destino de bate-papo relatado pelo `xim::chat_targets()` está atualmente `xim_chat_targets::same_team_index_only`, em seguida, qualquer determinada player trocarão comunicação de bate-papo com outro somente se os dois tiverem o mesmo valor relatado pelo `xim_player::team_index()` (e a privacidade / diretiva também permite que ele).

Seja conservador e suporte a cenários de concorrência, recém-criados redes XIM são automaticamente configurados para o padrão para `xim_chat_targets::same_team_index_only`. No entanto, bate-papo com adversários vanquished em outra equipe pode ser desejável, por exemplo, em um pós-jogo de "lobby". Você pode instruir o XIM para permitir que todos se comunicar com qualquer outra pessoa (no qual política de privacidade e permitem) chamando `xim::set_chat_targets()`. O exemplo a seguir começa a configurar todos os participantes na rede XIM para usar um `xim_chat_targets::all_players` valor:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Todos os participantes são informados de que uma nova configuração de destino está em vigor quando elas são fornecidas uma `xim_chat_targets_changed_state_change`.

Conforme observado anteriormente, a maioria dos tipos de movimentação de rede XIM inicialmente atribuirá todos os jogadores o valor de índice da equipe de zero. Isso significa que uma configuração de `xim_chat_targets::same_team_index_only` provavelmente indistinguível de `xim_chat_targets::all_players` por padrão. No entanto, os jogadores que mover para uma rede XIM usando o emparelhamento serão possuem diferentes valores de índice da equipe se a configuração de emparelhamento `xim_team_matchmaking_mode` valor declarado de duas ou mais equipes. Você também pode chamar `xim_player::xim_local::set_team_index()` a qualquer momento, conforme mostrado acima. Se seu aplicativo está usando valores de índice de equipe diferente de zero por meio de qualquer um desses métodos, não se esqueça de gerenciar os destinos de bate-papo atual configuração adequadamente.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Preenchimento automático em segundo plano dos slots de player ("aterramento" para que haja correspondência)

Grupos diferentes de jogadores chamando `xim::move_to_network_using_matchmaking()` ao mesmo tempo oferece o serviço de cruzamento Xbox Live a maior flexibilidade para organizá-los rapidamente em redes XIM novos e otimizados. No entanto, alguns cenários de jogo gostaria de manter uma determinada rede XIM intacta e apenas matchmake de players adicionais apenas para preencher os slots de vagas de player. XIM o dá suporte à configuração de emparelhamento para operar em um plano automático preenchendo o modo ou "o aterramento", chamando `xim::set_network_configuration()` com um `xim_network_configuration` que tem `xim_allowed_player_joins::matchmade` sinalizador definido em seu `xim_network_configuration::allowed_player_joins` propriedade.

O exemplo a seguir configura o cruzamento de aterramento para tentar encontrar um total de 8 players para uma festa para todos não equipes (embora se 8 não são encontrados, players de 2 a 7 também são aceitos), usar um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor MYGAMEMODE_ Combate MORTAL corresponderá apenas com outros jogadores especificando o mesmo valor:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Isso torna o XIM existente de rede disponíveis para dispositivos chamando `xim::move_to_network_using_matchmaking()` da maneira normal. Esses dispositivos nenhum comportamento, consulte alterar. Os participantes na rede o aterramento XIM não será movido, mas serão fornecidos uma `xim_network_configuration_changed_state_change` significando aterramento ativar, bem como vários `xim_matchmaking_progress_updated_state_change` notificações quando aplicável. Qualquer player matchmade será adicionado à rede XIM usando normal `xim_player_joined_state_change`.

Por padrão, para que haja correspondência aterramento permanece em andamento indefinidamente, embora ele não tente adicionar os jogadores se a rede XIM já tem o número máximo de jogadores especificado pela configuração xim_team_configuration. O aterramento pode ser desabilitado pela configuração xim_allowed_player_joins para não permitir matchmade. O exemplo a seguir desabilita o aterramento ao desmarcar o sinalizador xim_allowed_player_joins::matchmade, preservando todos os outros sinalizadores existentes e as configurações de rede.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Um correspondente `xim_network_configuration_changed_state_change` será fornecido para todos os dispositivos e, depois que esse processo assíncrono for concluída, um final `xim_matchmaking_progress_updated_state_change` será fornecido com `xim_matchmaking_status::none` para indicar que nenhum jogador matchmade adicionais serão adicionados à rede XIM.

Ao habilitar o emparelhamento de aterramento com um `xim_team_configuration` definir que declara duas ou mais equipes, todos os jogadores existentes devem ter um índice de equipe válido está entre 1 e o número de equipes. Isso inclui os jogadores que chamaram `xim_player::xim_local::set_team_index()` para especificar um valor personalizado ou que se associaram usando um convite ou por meio de outro social significa (por exemplo, uma chamada para `xim::move_to_network_using_joinable_xbox_user_id`) e foram adicionados com um valor de índice de equipe padrão de 0. Se qualquer player não tem um índice de equipe válido, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `xim_matchmaking_progress_updated_state_change` com um `xim_matchmaking_status::waiting_for_player_team_index` valor. Depois que todos os jogadores tem fornecido ou corrigido seus valores de índice de equipe com `xim_player::xim_local::set_team_index()`, para que haja correspondência aterramento será retomada. Mais informações podem ser encontradas na [XIM como trabalha com equipes de player](#how-xim-works-with-player-teams) seção deste documento.

Da mesma forma, ao habilitar o emparelhamento de aterramento com um `xim_network_configuration` estrutura com o `require_player_roles_and_skill_configuration` campo definido como true para funções ou habilidade, em seguida, todos os jogadores devem ter especificado uma configuração de emparelhamento não nulo por player. Se não tiver qualquer player, em seguida, o processo de emparelhamento será pausado e todos os participantes serão fornecidos uma `xim_matchmaking_progress_updated_state_change` com um `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valor. Depois que tem fornecido a todos os jogadores seus `xim_player_roles_and_skill_configuration` estruturas, para que haja correspondência aterramento será retomada. Mais informações podem ser encontradas na [cruzamento usando habilidades por player](#matchmaking-using-per-player-skill) e [cruzamento usando a função por player](#matchmaking-using-per-player-role) seções deste documento.

## <a name="querying-joinable-networks"></a>Redes permite junções de consulta

Enquanto o emparelhamento é uma ótima maneira de conectar os jogadores juntos rapidamente, às vezes, é melhor permitir que os jogadores descobrir permite junções redes usando critérios de pesquisa personalizado e, em seguida, selecione a rede que desejam ingressar. Isso pode ser particularmente vantajoso quando uma sessão de jogo pode ter um grande conjunto de regras configuráveis de jogos e preferências de player. Para fazer isso, uma rede existente deve primeiro ser feita passível de consulta, permitindo `xim_allowed_player_joins::queried` joinability e configurando as informações de rede disponíveis para outras pessoas fora da rede por meio de uma chamada para `xim::set_network_configuration()`.

O exemplo a seguir habilita `xim_allowed_player_joins::queried` joinability, rede de conjuntos de configuração com uma configuração de equipe que permite um total de jogadores de 1 a 8 juntos em 1 equipe, um uint64_t constantes do modo de jogo específico do aplicativo definido pelo valor GAME_MODE_BRAWL, uma descrição "cat e conversão boxing do Ovelha corresponderem", um uint32_t constante de índice específico do aplicativo de mapa definido pelo valor MAP_KITCHEN e inclui spectatorallowed"marcas"chatrequired","simples",":

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

Outros jogadores fora da rede, em seguida, podem encontrar a rede chamando `xim::start_joinable_network_query()` com um conjunto de filtros que corresponde às informações de rede anteriormente na `xim::set_network_configuration()` chamar. O exemplo a seguir inicia uma consulta de rede permite junções com a opção de filtro do modo de jogo que consultará apenas para redes que usam o modo de jogo específico do aplicativo definido pelo valor GAME_MODE_BRAWL:

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

Aqui está outro exemplo que usa a opção de filtros de marca de consulta para redes com a marca "simples" e "spectatorallowed" na configuração pública que podem ser consultada:

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Opções de filtro diferentes também podem ser combinadas. O exemplo a seguir que usa a opção de filtro do modo de jogo e a marca de opção de filtro para iniciar uma consulta para redes que têm o GAME_MODE_BRAWL constante de modo de jogo específico do aplicativo e a marca "simples":

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Se a operação de consulta for bem-sucedida, o aplicativo receberá um `xim_start_joinable_network_query_completed_state_change` do qual o aplicativo pode recuperar uma lista de redes permite junções. O aplicativo receberá também continuamente `xim_joinable_network_query_updated_state_change` para redes permite junções adicionais ou quaisquer alterações que ocorrem com a lista retornada de redes permite junções até que ela seja interrompida manualmente ou automaticamente. A consulta em andamento pode ser interrompida manualmente chamando `xim::stop_joinable_network_query()`. Ele será interrompido automaticamente ao chamar `xim::start_joinable_network_query()` para iniciar uma nova consulta.

O aplicativo pode tentar ingressar em uma rede na lista de redes permite junções chamando `xim::move_to_network_using_joinable_network_information()`. O exemplo a seguir pressupõe que você está tentando ingressar um `xim_joinable_network_information` apontado pelo ponteiro 'selectedNetwork' não é protegida por uma senha (de modo que estamos passando nullptr para o segundo parâmetro):

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

Ao habilitar a consulta de rede com um xim_team_configuration que declara duas ou mais equipes, jogadores Unido chamando `xim::move_to_network_using_joinable_network_information()` terá um valor de índice de equipe padrão de 0.

> [!NOTE]
> Se o aplicativo tiver especificado a vários usuários locais e ingresse em uma rede com menos espaço do que o número de usuários locais, ainda pode ter êxito, a junção. Mas apenas o número permitido de usuários locais pode se associar à rede.