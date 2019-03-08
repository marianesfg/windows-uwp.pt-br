---
title: Reproduzir um jogo usando SmartMatch
description: Saiba como usar o Xbox Live para múltiplos jogadores manager para permitir que um jogador encontrar um jogo usando SmartMatch.
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes, Gerenciador de vários jogadores, fluxograma, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 0c0ba897f23eb690c3044b00cdb4ce3bea975e71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655951"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>Encontrar um jogo usando SmartMatch

Às vezes, um jogador pode não ter suficiente amigos online quando eles desejam jogar um jogo ou apenas deseja jogar contra pessoas aleatórias online. Você pode usar o serviço Xbox Live SmartMatch para localizar outros jogadores do Xbox Live

Esta página aborda as etapas básicas que você precisa implementar SmartMatch emparelhamento usando o Gerenciador de com vários participantes.

## <a name="find-a-match"></a>Localizar uma correspondência

Há quatro etapas envolvidas ao usar o Gerenciador de com vários participantes para enviar um convite para amigo do usuário e, em seguida, para esse amigo para o jogo em andamento:

1. [Inicializar o Gerenciador de vários jogadores](#initialize-multiplayer-manager)
2. [Criar a sessão de lobby pela adição de usuários locais](#create-lobby)
3. [Enviar convites para amigos](#send-invites)
4. [Aceitar convites](#accept-invites)
5. [Localizar uma correspondência](#find-match)

As etapas 1, 2, 3 e 5 são feitas no dispositivo executando o convite.  Etapa 4 seria iniciada normalmente na máquina do convidado após a inicialização do aplicativo por meio da ativação de protocolo.

Você pode ver um fluxograma do processo aqui: [Fluxograma - Play um jogo usando emparelhamento SmartMatch](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md).

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>1) inicializar o Gerenciador de vários jogadores <a name="initialize-multiplayer-manager">

| em conferência | Evento disparado |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | N/D |

O objeto de sessão lobby é criado automaticamente ao inicializar o Gerenciador de vários jogadores, supondo que um nome de modelo de sessão válida (configurado na configuração de serviço) é especificado. Observe que isso não cria a instância de sessão lobby no serviço.

**Exemplo:**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>2) criar a sessão de lobby pela adição de usuários locais<a name="create-lobby">

| Método | Evento disparado |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Aqui, você adiciona assinado localmente no Xbox Live de usuários para a sessão de corredor. Isso hospeda um corredor novo quando o primeiro usuário é adicionado. Para todos os outros usuários, eles serão adicionados ao lobby existente como usuários secundários. Essa API também anunciará lobby no shell para os amigos ingressar. Você pode enviar convites, definir propriedades de corredor, acessar membros de lobby via lobby() somente depois de adicionar o usuário local.

Depois de ingressar lobby, a Microsoft recomenda a configuração de endereço de conexão do membro, bem como todas as propriedades personalizadas para o membro.

Você deve repetir esse processo para todos os usuários conectado localmente.

**Exemplo: (único usuário local)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**Exemplo: (vários usuários locais)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


As alterações são divididas em lotes na próxima `do_work()` chamar.  
Com vários participantes manager dispara uma `user_added` evento cada vez que um usuário é adicionado à sessão lobby. Recomendamos que você inspecione o código de erro do evento para ver se esse usuário foi adicionado com êxito. Em caso de falha, uma mensagem de erro será fornecida detalhando os motivos da falha.

**Funções executadas pelo Gerenciador de vários jogadores**

* Registrar atividade em tempo Real e assinaturas de vários jogadores com o serviço Xbox Live para vários jogadores
* Criar sessão Lobby
* Junte-se todos os jogadores locais como o Active Directory
* Carregar SDA
* Membro do conjunto de propriedades
* Registre-se para eventos de alteração de sessão
* Definir a sessão de Lobby como sessão ativa

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>3) enviar convites para os amigos (opcionais) <a name="send-invites">

| Método | Evento disparado |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

Em seguida, você desejará exibir a UI Xbox padrão para os amigos que está convidando. Isso exibe uma interface do usuário que permite que o jogador selecionar amigos ou jogadores recentes para convidar para o jogo. Depois de confirmar as ocorrências de player, o Gerenciador de Multiplayer envia os convites para os jogadores selecionados.

Jogos também podem usar o `invite_users()` método para enviar convites para um conjunto de pessoas definidos por suas Ids de usuário do Xbox Live. Isso é útil se você preferir usar sua própria interface do usuário no jogo, em vez do estoque UI Xbox.

**Exemplo:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**Funções executadas pelo Gerenciador de vários jogadores**

* Traz o Xbox estoque do título da interface do usuário que pode ser chamado (TCUI)
* Envia o convite diretamente para os jogadores selecionados

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>4) aceitar convites (opcionais) <a name="accept-invites">

| Método | Evento disparado |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Quando um player de convidado aceita um convite de jogo ou associados a um jogo de amigos por meio de um shell de interface do usuário, o jogo é iniciado em seu dispositivo usando a ativação de protocolo. Uma vez o jogo inicia, com vários participantes Manager pode usar os argumentos de evento do protocolo ativado para unir corredor. Opcionalmente, se você ainda não adicionou os usuários locais por meio de lobby_session()::add_local_user(), você pode passar na lista de usuários por meio de join_lobby() API. Se o usuário convidado não é adicionado por meio de lobby_session()::add_local_user() ou join_lobby(), e em seguida, join_lobby() falhará e fornecer o que o convite foi enviado para como parte do join_lobby_completed_event_args() invited_xbox_user_id().

Depois de ingressar lobby, a Microsoft recomenda a configuração de endereço de conexão do membro, bem como todas as propriedades personalizadas para o membro. Você também pode definir o host por meio de set_synchronized_host se ele existir.

Por fim, o Gerenciador de com vários participantes serão automaticamente o usuário para a sessão de jogo de junção se um jogo já está em andamento e tem espaço para o convidado. O título será notificado por meio do evento join_game_completed fornecendo um código de erro apropriado e uma mensagem.

**Exemplo:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

```

Erro/sucesso é manipulado por meio de `join_lobby_completed` eventos

**Funções executadas pelo Gerenciador de vários jogadores**

* Registrar RTA & assinaturas com vários participantes
* Junte-se a sessão de Lobby
 * Limpeza de estado Lobby existente
 * Junte-se todos os jogadores locais como o Active Directory
 * Carregar SDA
 * Membro do conjunto de propriedades
* Registre-se para eventos de alteração de sessão
* Definir a sessão de Lobby como sessão ativa
* Junte-se a sessão de jogo (se existir)
 * Identificador de transferência de usos


### <a name="5-find-match-a-namefind-match"></a>5) localizar correspondência <a name="find-match">

| em conferência | Evento disparado |
|-----|----------------|
| `multiplayer_manager::find_match()` | Muitos eventos, conforme descrito em ```match_status``` (por exemplo: ```searching```, ```found```, ```measuring```, etc) |

Depois de convites, se houver, foram aceitos, e o host está pronto para começar a jogar o jogo, você pode usar SmartMatch para qualquer um dos encontrar um jogo existente que tem suficiente abrir player slots para todos os membros na sessão lobby chamando `find_match()`, ou crie um novo jogo se ssão que inclui todos os membros da sessão lobby e preenchimento de pontos de acesso abertos com outras pessoas procurando uma correspondência do mesmo tipo de jogo, chamando `join_game_from_lobby()` seguido pelo `set_auto_fill_members_during_matchmaking()`.

Antes de chamar `find_match()`, primeiro você deve configurar hoppers em sua configuração de serviço. Um hopper define as regras que SmartMatch usa para corresponder os jogadores.

**Exemplo:**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**Funções executadas pelo Gerenciador de vários jogadores**

* Crie um tíquete de correspondência
* Lidar com todos os estágios de QoS
* Manipular as alterações da lista
 * Reenviar (se necessário)
* Sessão de jogo de destino de junções
* Anunciar o jogo por meio da sessão do corredor
