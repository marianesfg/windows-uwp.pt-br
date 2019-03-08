---
title: Enviar convites para jogos
description: Saiba como usar o Xbox Live para múltiplos jogadores manager para permitir que um jogador enviar convites do jogos.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes, Gerenciador para múltiplos jogadores, fluxograma, jogo convite
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645991"
---
# <a name="send-game-invites"></a>Enviar convites para jogos

Um dos cenários com vários participantes mais simples é permitir que um jogador para seu jogo online com amigos. Este cenário aborda as etapas que necessárias para enviar convites para outro jogador para participar do jogo.

Depois que você tiver [inicializado o Gerenciador de vários jogadores](play-multiplayer-with-friends.md)e ter [criou uma sessão de Lobby pela adição de usuários locais](play-multiplayer-with-friends.md), você deve aguardar até que você receba o `user_added` evento antes que você pode começar a enviar convites.

Há duas maneiras de enviar um convite:

1. [Convite de plataforma Xbox TCUI](#xbox-platform-invite-tcui)
2. [Título implementado a interface do usuário personalizada](#title-implemented-custom-ui)

Você pode ver um fluxograma do processo aqui: [Fluxograma - Enviar convite para outro jogador](mpm-flowcharts/mpm-send-invites.md).

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) Xbox plataforma convite TCUI <a name="xbox-platform-invite-tcui">

| Método | Evento disparado |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

Chamar `invite_friends()` abrirá a UI Xbox padrão para os amigos que está convidando. Isso exibe uma interface do usuário que permite que o jogador selecionar amigos ou jogadores recentes para convidar para o jogo. Depois de confirmar as ocorrências de player, o Gerenciador de Multiplayer envia os convites para os jogadores selecionados.

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) título implementado a interface do usuário personalizada<a name="title-implemented-custom-ui">

| Método | Evento disparado |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

O título pode implementar um TCUI personalizado para exibição online com seus amigos e convidando-os. Jogos podem usar o `invite_users()` método para enviar convites para um conjunto de pessoas definidos por suas Ids de usuário do Xbox Live. Isso é útil se você preferir usar sua própria interface do usuário no jogo, em vez do estoque UI Xbox.

**Exemplo:**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**Funções executadas pelo Gerenciador de vários jogadores**

* Envia o convite diretamente para os jogadores selecionados
