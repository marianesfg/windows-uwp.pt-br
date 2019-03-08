---
title: Visão geral da API do Gerenciador para múltiplos jogadores
description: Saiba mais sobre a API do Xbox Live Manager com vários participantes.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes, manager com vários participantes
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628021"
---
# <a name="multiplayer-manager-api-overview"></a>Visão geral da API do Gerenciador de vários jogadores

Esta página descreve uma ampla visão geral da Manager com vários participantes API e como eles são usados em um jogo. Ele chama as classes mais importantes e os métodos na API. Para obter informações detalhadas do API, consulte a documentação de referência. Para obter exemplos de como usar essas APIs em um aplicativo, consulte o exemplo com vários participantes.

## <a name="namespace"></a>Namespace
As classes com vários participantes Manager estão incluídas o namespace a seguir:

| language | namespace |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

Há listas a seguir descreve as classes principais que você deve compreender:

* [Classe de Gerenciador com vários participantes](#multiplayer-manager-class)
* [Classe de evento com vários participantes](#multiplayer-event-class)
* [Classe de membro de vários jogadores](#multiplayer-member-class)
* [Classe Multiplayer Lobby sessão](#multiplayer-lobby-session-class)
* [Classe da sessão de jogo Multiplayer](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>Classe de Gerenciador com vários participantes <a name="multiplayer-manager-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

Essa classe é a classe principal para interagir com o Gerenciador de com vários participantes. É uma classe singleton, portanto, você só precisa criar e inicializar a classe de uma vez.
Essa classe contém um objeto de sessão lobby único e um objeto de sessão única de jogo.

No mínimo, você deve chamar o `initialize()` e `do_work()` métodos nessa classe em ordem para vários jogadores Manager à função.

As tabelas a seguir descreve algumas, mas não todas, das mais comumente usados métodos e propriedades para esta classe. Para obter uma lista descritiva completa de membros de classe, consulte a referência.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| Initialize() | initialize() | Inicializa o Gerenciador de com vários participantes. Você deve chamar esse método antes de usar o Gerenciador de com vários participantes. |
| DoWork() | do_work() | Atualiza os estados de sessão visível do aplicativo. Você deve chamar esse método pelo menos uma vez por quadro, e seu jogo deve tratar os eventos com vários participantes que são retornados pelo método. |
| JoinLobby() | join_lobby() | Fornece uma maneira de junções de uma sessão de corredor do amigo por meio de um handleId que identifica exclusivamente o lobby você deseja ingressar ou quando o usuário aceitar um convite que faz com que o título obter o protocolo ativado. |
| JoinGameFromLobby() | join_game_from_lobby() | Una a sessão de jogo do corredor se houver um, e se houver espaço. Se a sessão não existir, ele cria uma nova sessão de jogo com os membros existentes do corredor. Isso não migrar propriedades de sessão lobby existentes para a sessão de jogo. Depois de ingressar, você pode definir as propriedades ou o host por meio de `game_session()::set_synchronized_*` APIs. O título é necessário para chamar essa API em todos os clientes que deseja ingressar na sessão de jogo.|
| JoinGame() | join_game() | Une uma sessão de jogo existente recebe um nome de sessão exclusivo globalmente, normalmente localizado por meio de serviços de cruzamento de terceiros. Você pode passar na lista de IDs que você deseja que façam parte do jogo de usuário do xbox.|
| FindMatch() | find_match() | Use o cruzamento do Xbox Live para localizar e ingressar em um jogo. |
| LeaveGame() | leave_game() | Deixa um jogo e retorna o membro e todos os membros locais ao lobby. |
| **Propriedades** | | |
| LobbySession | lobby_session() | Um identificador para um objeto que representa a sessão de corredor. |
| GameSession |  game_session() | Um identificador para um objeto que representa a sessão de jogo. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>Classe de evento com vários participantes <a name="multiplayer-event-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

Quando você chama `do_work()`, com vários participantes Manager retorna uma lista de eventos que representam as alterações para as sessões, pois chamado pela última vez `do_work()`. Esses eventos incluem alterações, como um membro adicionado a uma sessão, um membro tiver deixado a uma sessão, propriedades do membro foram alteradas, o cliente do host foi alterado, etc.

Para obter uma lista de todos os possíveis tipos de evento, consulte o `multiplayer_event_type` enumeração.

Cada retornado `multiplayer_event` inclui um `event_args`, que deve ser convertido para a classe de event_args apropriado para o tipo de evento. Por exemplo, se o `event_type` é `member_joined`, em seguida, você converteria o `event_args` fazem referência a um `member_joined_event_args` classe.

Seu jogo deve lidar com cada um dos eventos conforme necessário após a chamada `do_work()`.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>Classe de membro de vários jogadores <a name="multiplayer-member-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

Essa classe representa um player em um corredor ou sessão de jogo. A classe contém propriedades sobre o membro, incluindo o XboxUserID para o player, o endereço de conexão de rede para o player, as propriedades personalizadas para cada jogador e muito mais.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>Classe Multiplayer Lobby sessão <a name="multiplayer-lobby-session-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

Uma sessão persistente usada para gerenciar os usuários que são locais para esse dispositivo e seus amigos convidados que deseja jogar juntos. Uma sessão de lobby deve conter pelo menos um membro para o Gerenciador de vários jogadores tomar atitudes com vários participantes. Você inicialmente pode criar uma nova sessão lobby chamando o `add_local_user()` método.

As tabelas a seguir descreve algumas, mas não todas, das mais comumente usados métodos e propriedades para esta classe. Para obter uma lista descritiva completa de membros de classe, consulte a referência.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| AddLocalUser() | add_local_user() | Adiciona um usuário local (um player que entrou no dispositivo local) para a sessão de corredor. Se esse for o primeiro membro adicionado a uma sessão do corredor, em seguida, ele cria uma nova sessão de lobby. |
| RemoveLocalUser() | remove_local_user() | Remove um membro especificado do corredor e sessão de jogo. |
| InviteFriends() | invite_friends() | Abre um Xbox Live interface do usuário padrão que permite que o jogador selecionar as pessoas de sua lista de amigos e, em seguida, convites para os jogadores no jogo. |
| InviteUsers() | invite_users() | Convida usuários sepcified do Xbox Live no jogo. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | Define o endereço de rede para o membro local. Jogos podem usar esse endereço de rede para estabelecer comunicações de rede entre os membros. |
| SetLocalMemberProperties() | set_local_member_properties() | Define uma propriedade personalizada para um membro local. A propriedade é armazenada em uma cadeia de caracteres JSON. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | Remove uma propriedade personalizada para um membro local. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Define uma propriedade personalizada para a sessão de corredor. A propriedade é armazenada em uma cadeia de caracteres JSON. Se a propriedade é compartilhada entre os dispositivos e poderão ser atualizada por vários dispositivos ao mesmo tempo, use a versão sincronizada do método. |
| IsHost() | is_host() | Indica se o dispositivo atual está atuando como o host de corredor. |
| SetSynchronizedHost() | set_synchronized_host() | Define o host do corredor. |
| **Propriedades** | | |
| LocalMembers | local_members() | A coleção de membros que está conectado no dispositivo local. |
| Membros | Members() | A coleção de membros que estão na sessão lobby. |
| Propriedades | Properties) | Um objeto JSON que representa uma coleção de propriedades para a sessão de corredor. |
| Host | host() | O membro de host para corredor. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>Classe da sessão de jogo Multiplayer <a name="multiplayer-game-session-class">

| language | class |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

A sessão de jogo representa o grupo de membros do Xbox Live que estão participando em uma instância do jogo real. Isso pode incluir os jogadores que foram correspondidos por meio de serviços de emparelhamento.

Para iniciar uma nova sessão de jogo que inclui membros a partir de `lobby_session`, você pode chamar `multiplayer_manager::join_game_from_lobby()`. Se você quiser usar o cruzamento do Xbox Live, você pode chamar `multiplayer_manager::find_match()`. Se você estiver usando um serviço de cruzamento de terceiros, você pode chamar `multiplayer_manager::join_game()`.

As tabelas a seguir descreve algumas, mas não todas, das mais comumente usados métodos e propriedades para esta classe. Para obter uma lista descritiva completa de membros de classe, consulte a referência.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Define uma propriedade personalizada para a sessão de jogo. A propriedade é armazenada em uma cadeia de caracteres JSON. Se a propriedade é compartilhada entre os dispositivos e poderão ser atualizada por vários dispositivos ao mesmo tempo, use a versão sincronizada do método. |
| IsHost() | is_host() | Indica se o dispositivo atual está atuando como o host do jogo. |
| SetSynchronizedHost() | set_synchronized_host() | Define o host do jogo. |
| **Propriedades** | | |
| Membros | Members() | A coleção de membros que estão na sessão do jogo. |
| Propriedades | Properties) | Um objeto JSON que representa uma coleção de propriedades para a sessão de jogo. |
| Host | host() | O membro de host para o jogo. |
