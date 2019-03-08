---
title: Gerenciador de vários jogadores
description: Saiba mais sobre o Xbox Live para múltiplos jogadores manager, uma API de alto nível projetada para torná-lo mais fácil de implementar com vários participantes.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594711"
---
# <a name="multiplayer-manager"></a>Gerenciador de vários jogadores

Xbox Live fornece suporte extensivo para adicionar a funcionalidade com vários participantes para os títulos, permitindo que seu jogo conectar-se membros do Xbox Live em todo o mundo.  Isso inclui cenários para que haja correspondência avançada, a capacidade de um player participar de jogo um amigo em andamento e muito mais. No entanto, a implementação o Xbox Live para múltiplos jogadores usando as APIs de 2015 Multiplayer diretamente pode ser uma tarefa complexa, que exigem um alto grau de design e teste para verificar se você está seguindo as práticas recomendadas e atender aos requisitos de certificação.

Com vários participantes Manager torna mais fácil adicionar funcionalidade para múltiplos jogadores ao seu jogo ao gerenciar sessões e o relacionamento de pessoas e fornecendo um estado e o modelo de programação baseado em evento. É um conjunto de APIs projetados para facilitar a implementar cenários com vários participantes para o seu Xbox Live jogo. Ele fornece uma API que é orientada em torno de cenários comuns de com vários participantes, como jogos com vários participantes com amigos, manipulação de jogo convida, manipulação de junção em andamento, compatibilidade e muito mais. Ele dá suporte a vários usuários locais e torna mais fácil para o título integrar com o diretório de sessão com vários participantes, se você estiver usando um serviço de cruzamento de terceiros. Muitos desses cenários podem ser feitos com apenas algumas chamadas à API.

## <a name="key-features"></a>Recursos Principais
Estes são os principais recursos da API do Gerenciador de Multiplayer:

* Gerenciamento de sessão fácil e relacionamento de pessoas do Xbox Live
* Estado e eventos com base em modelo de programação.
* Garante o Xbox Live práticas recomendadas, além de ser Multiplayer XR em conformidade.
* Dá suporte a títulos Xbox um XDK e UWP.
* Implementa [Multiplayer 2015 fluxogramas](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx).
* Funciona com as APIs de 2015 tradicional com vários participantes.

>**Importante** -seu jogo ainda deve implementar os eventos necessários para o on-line com vários participantes para passar na certificação.

## <a name="overview"></a>Visão geral
Gerenciador de vários jogadores é orientado a alguns conceitos-chave:
* `lobby_session` : Uma sessão persistente usada para gerenciar os usuários que são locais para esse dispositivo e seus amigos convidados que deseja jogar juntos. O grupo pode desempenhar várias rodadas, mapas, níveis, etc., do jogo, e a sessão lobby controla esse grupo de núcleo de amigos (incluindo players de locais para o dispositivo). Normalmente, esse grupo é formado enquanto o host pode navegar por meio dos menus, bate-papo com os membros do grupo para decidir qual modo de jogo que desejam reproduzir.

* `game_session` : Jogadores de faixas que estão executando uma instância específica do jogo. Por exemplo, uma corrida, um mapa ou um nível. Você pode criar uma nova sessão do jogo por meio de `join_game_from_lobby` que inclui os membros na sessão lobby.  Quando um membro aceita um convite, eles serão adicionados ao lobby e à sessão de jogo (se houver espaço). Players adicionais podem ser adicionados à sessão do jogo se emparelhamento está habilitado, mas esses players adicionais não são adicionados à sessão lobby. Isso significa que depois que o jogo termina, os jogadores na sessão lobby são ainda juntos, enquanto os jogadores extras de relacionamento de pessoas não são.

* `multiplayer_member` : Representa um usuário individual entrado em um dispositivo local ou remoto.

* `do_work` : Garante que as atualizações de estado apropriado do jogo são mantidas entre o título e o serviço Xbox Live com vários participantes. Para garantir o melhor desempenho, do_work() deve ser chamado com frequência, como uma vez por quadro. Ele fornece uma lista de `multiplayer_event` eventos de retorno de chamada para lidar com o jogo.

## <a name="state-machine"></a>Máquina de estado
O `do_work()` chamada é necessária para garantir que seu estado é mantido atualizado.  Para vários jogadores Manager para fazer seu trabalho, os desenvolvedores devem chamar o `do_work()` método regularmente. A maneira mais confiável para fazer isso é chamar pelo menos uma vez por quadro. `do_work()` Retorna rapidamente quando ele não tem nenhum trabalho a fazer, portanto, não há nenhum motivo de preocupação sobre chamá-lo com muita frequência.

Todos os objetos retornados pela API do Gerenciador de com vários participantes não devem ser considerados thread-safe. No entanto, ele permite que você controle a sincronização de thread se você estiver chamando-o por vários threads. A biblioteca tem proteção multithreading interna, mas você ainda precisará implementar seu próprio bloqueio, se você precisar de um thread acesse todos os valores – por exemplo, percorrer a lista de members() enquanto outro thread pode invocar `do_work()`.

O Gerenciador de Multiplayer mantém um modelo de estado com base Atualizando as sessões em segundo plano, como players de join, deixem ou quando as sessões são atualizadas. Para ajudar a evitar problemas de sincronização de thread entre o thread de interface do usuário e seu jogo thread, com vários participantes Manager não atualiza o estado visível do aplicativo das sessões até que você chame o `do_work()` método. Tradicionalmente deseja receber notificações sobre eventos como alterações de sessão em um thread em segundo plano e, em seguida, precisa sincronizá-lo com um thread de interface do usuário para exibir essas alterações. Com o Gerenciador de vários jogadores, esse trabalho é feito para você em segundo plano.  Você pode chamar `do_work()` em seu thread principal no momento de sua escolha, para obter o instantâneo mais recente do estado de quais Multiplayer Manager is Adaptive Response buffering para você em segundo plano.

## <a name="events-and-notifications"></a>Eventos e notificações
Com vários participantes Manager define um conjunto de eventos significativos (consulte `multiplayer_event_type`) e notifica o título por meio de `do_work()` método quando elas ocorrem. Eventos como jogadores remotos ingressar ou sair, alterando propriedades do membro ou a alteração de estado de sessão. Todas as APIs do Gerenciador de com vários participantes são síncronos. O `do_work()` método retorna uma lista de eventos quando essas operações assíncronas são concluídas. O título deve lidar com esses eventos como você ver apropriado. Consulte o `multiplayer_event` documentação para obter mais detalhes da classe.

Para cada evento, dependendo do tipo de evento, você deve converter o `event_args` à classe arg evento apropriado para obter as propriedades de evento. O exemplo a seguir demonstra como usar `do_work()` para manipular eventos:

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>Cenários

Nesta seção, passaremos por meio de alguns cenários comuns e as APIs que você chamaria em cada cenário.  Algumas informações em segundo plano está fazendo com vários participantes Manager também são fornecidas.

* [Brincar com amigos](multiplayer-manager/play-multiplayer-with-friends.md)
* [Localizar uma correspondência](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [Enviar convites do jogos](multiplayer-manager/send-game-invites.md)
* [Manipular a ativação de protocolo](multiplayer-manager/handle-protocol-activation.md)

Uma visão geral de alto nível da API pode ser encontrada em [visão geral da API do Gerenciador de vários jogadores](multiplayer-manager/multiplayer-manager-api-overview.md).

## <a name="what-multiplayer-manager-does-not-do"></a>O que Multiplayer Manager não faz
Embora Multiplayer Manager facilita muito a implementação de cenários com vários participantes e abstrai alguns dos dados do desenvolvedor, há algumas coisas que ele não trata ou não é mais adequada para.

* Jogos para o servidor on-line persistente, como MMOs ou outros tipos de jogos que exigem sessões grandes (mais de 100 jogadores em uma sessão).
* Servidor de gerenciamento de sessão de servidor

>Gerenciador de vários jogadores não está vinculado a qualquer tecnologia de rede específico e devem funcionar com qualquer camada de rede.

## <a name="next-steps"></a>Próximas etapas

Consulte o C++ ou WinRT *Multiplayer* exemplo para obter um exemplo de funcionamento da API.

A documentação da API pode ser encontrada nos guias de C++ ou WinRT no namespace Microsoft::Xbox::Services::Multiplayer::Manager.  Você também pode ver o `multiplayer_manager.h` cabeçalho.

Se você tiver dúvidas, comentários, ou tiver algum problema usando o Gerenciador de vários jogadores, entre em contato com sua mãe ou postar um thread de suporte a fóruns em [ https://forums.xboxlive.com ](https://forums.xboxlive.com).
