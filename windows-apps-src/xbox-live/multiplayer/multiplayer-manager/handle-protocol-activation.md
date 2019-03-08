---
title: Tratar ativação de protocolos
description: Saiba como usar o Xbox Live para múltiplos jogadores manager para manipular a ativação de protocolo.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, Gerenciador de vários jogadores, ativação de protocolo
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655831"
---
# <a name="handle-protocol-activation"></a>Tratar ativação de protocolos

Ativação de protocolo é quando o sistema é iniciado automaticamente um jogo em resposta a outra ação, normalmente quando um player aceita um convite para jogo de outro jogador.

O título pode obter o protocolo ativado por meio das seguintes maneiras:

* Quando um usuário aceita um convite para jogo
* Quando um usuário selecionar "Ingressar Game" do jogador de um jogador.

Este cenário aborda como lidar com a ativação de protocolo, quando o título é iniciado e Junte-se o jogo e lobby (se houver).

Você pode ver um fluxograma do processo aqui: [Fluxograma — o player de ativação de protocolo do identificador](mpm-flowcharts/mpm-on-protocol-activation.md).

| Método | Evento disparado |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Quando um player aceita um convite de jogo ou une um jogo de um amigo por meio de cartão de jogador do player, o jogo é iniciado em seu dispositivo usando a ativação de protocolo. Uma vez o jogo inicia, com vários participantes Manager pode usar os argumentos de evento do protocolo ativado para unir corredor. Opcionalmente, se você ainda não adicionou os usuários locais por meio `lobby_session()::add_local_user()`, você pode passar na lista de usuários por meio de `join_lobby()` API. Se o usuário convidado não é adicionado, ou se o convite foi para outro usuário que o usuário que foi adicionado, `join_lobby()` falhará e forneça a `invited_xbox_user_id()` que o convite foi enviado para como parte do `join_lobby_completed_event_args`.

Depois de ingressar lobby, a Microsoft recomenda a configuração de endereço de conexão do membro, bem como todas as propriedades personalizadas para o membro. Você também pode definir o host por meio de `set_synchronized_host` se ele existir.

Por fim, o Gerenciador de com vários participantes serão automaticamente o usuário para a sessão de jogo de junção se um jogo já está em andamento e tem espaço para o convidado. O título será notificado por meio de `join_game_completed` fornecendo um código de erro apropriado e uma mensagem de evento.

**Exemplo:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
