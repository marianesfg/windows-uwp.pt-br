---
title: O que há de novo para o Xbox Live SDK - abril de 2016
description: O que há de novo para o Xbox Live SDK - abril de 2016
ms.assetid: a6f26ffd-f136-4753-b0cd-92b0da522d93
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9ce63a0174fa0c4158764b8bca2443d58d0aefd9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660761"
---
# <a name="whats-new-for-the-xbox-live-sdk---april-2016"></a>O que há de novo para o Xbox Live SDK - abril de 2016

Consulte a [What's New - março de 2016](1603-whats-new.md) artigo para o que foi adicionado no 1603

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="tournaments"></a>Torneios
- A ferramenta de torneios do Xbox Live agora está incluído com o SDK.  Você poderá ver isso no diretório de ferramentas, juntamente com algumas informações sobre como usá-lo.
- As APIs para torneios também estão disponíveis.  Confira o namespace Xbox::Services::Tournaments
- Documentação também está disponível no guia de programação.

## <a name="documentation"></a>Documentação
- [Guia de solução de problemas de entrada](../using-xbox-live/troubleshooting/troubleshooting-sign-in.md) lista algumas estratégias gerais para depurar falhas de entrada, bem como as etapas a seguir com base no código de erro.
- O [Marketplace](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-marketplace/marketplace-and-downloadable-content) docs para desenvolvedores do Xbox One apenas agora podem ser encontradas no guia de programação.  Os desenvolvedores da UWP devem continuar a consultar o Partner Center para documentação sobre o armazenamento.
- Há um [XDK para portabilidade UWP guia](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) você pode consultar se você estiver interessado em levar um Xbox One título para a plataforma Universal do Windows.
- Consulte a [refinada a limitação de taxa](../using-xbox-live/best-practices/fine-grained-rate-limiting.md) artigo para obter uma descrição de como elas são impostas para vários pontos de extremidade de serviço do Xbox Live e cenários, bem como informações sobre quais são os limites.

## <a name="multiplayer-manager"></a>Gerenciador de vários jogadores
O [Multiplayer Manager](../multiplayer/multiplayer-manager.md) não está mais em experimental.  Nós incorporamos comentários de desenvolvedores que usam essa API e feitas algumas das APIs mais consistentes entre si.  Use o Multiplayer Gerenciador como ponto de partida ao fazer o desenvolvimento para múltiplos jogadores, pois ele fornece uma API mais simples que gerencia muitas complexidades da API de 2015 com vários participantes para você.

No abaixo de seções, listamos alguns dos novos recursos para a API, bem como um pequeno número de alterações significativas.

#### <a name="completed-events"></a>Eventos concluídos
Todas as APIs agora tem um correspondente``` _competed``` todos os eventos são disparados independentemente do êxito ou falha. O comportamento anterior era de que ele será acionado somente em caso de falha para o título agir em. E já que as chamadas são em lote, isso significa que após a conclusão, o título será obter vários ```_competed``` eventos.

| API | Evento retornado |
|-----|----------------|
| ```lobby_session()->set_local_member_properties``` |  ```local_member_property_write_completed ```
| ```lobby_session()->set_local_member_connection_address``` | ```local_member_connection_address_write_completed``` |
| ```lobby_session()->set_properties``` | ```session_property_write_completed``` |
| ```lobby_session()->set_synchronized_properties``` | ```session_synchronized_property_write_completed``` |
| ```lobby_session()->set_synchronized_host``` | ```synchronized_host_write_completed``` |

O mesmo se aplica a ```game_session()```.

#### <a name="application-defined-context"></a>Contexto de aplicativo definidos
Agora, você pode passar em um contexto opcional definido pelo aplicativo para cada métodos set _ * correlacionar o evento com vários participantes para a chamada inicial.
Por exemplo

```cpp
_XSAPIIMP xbox_live_result<void> set_properties(
        _In_ const string_t& name,
        _In_ const web::json::value& valueJson,
        _In_ context_t context = nullptr
       );
```

O contexto seria retornado, em seguida, volta para o título por meio de ```_completed``` eventos.

#### <a name="breaking-changes"></a>Alterações recentes

1.  Ambas as APIs de convidar (```invite_friends``` & ```invite_users```) agora são síncronos. Após a conclusão, ele retorna um evento invite_sent.

2.  ```write_synchronized_properties_and_commit``` foi renomeado para ```set_synchronized_properties```. Após a conclusão, ele retornará um ```session_synchronized_property_write_completed``` eventos.

3.  ```write_synchronized_host_and_commit``` foi renomeado para ```set_synchronized_host```. Após a conclusão, ele retornará um ```synchronized_host_write_completed``` eventos.

4.  Em ```lobby_session()```

  *Removido*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
```

  *Adicionado*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

5.  Em ```game_session()```

  *Removido*

```cpp
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_tournaments_server& tournaments_server() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services:: multiplayer::multiplayer_session_arbitration_server& arbitration_server() const;
```
  *Adicionado*

```cpp
_XSAPIIMP const std::unordered_map<string_t, multiplayer_session_reference>& tournament_teams() const;
_XSAPIIMP const std::unordered_map<string_t, xbox::services::tournaments::tournament_team_result>& tournament_team_results() const;
```

6.  Alterar tipos de evento:

  *Removido*

```cpp
write_pending_changes_failed,
tournament_property_changed,
arbitration_property_changed
```

  *Renomeado*

  ```write_synchronized_properties_completed``` renomeado para ```session_synchronized_property_write_completed```

  ```write_synchronized_host_completed``` renomeado para ```synchronized_host_write_completed```
