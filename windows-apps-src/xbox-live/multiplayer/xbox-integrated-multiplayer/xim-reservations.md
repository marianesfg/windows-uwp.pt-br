---
title: Reservas de Out-of-band XIM
description: Descreve como usar o Xbox integrado com vários participantes (XIM) como uma solução de bate-papo dedicado por meio de out-of-band reservas.
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.date: 01/28/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes integrado do xbox, xim, bate-papo
ms.localizationpriority: medium
ms.openlocfilehash: 82b38b8d0d94e4cccf501a101faee0e28a6b43c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660831"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Usando o XIM como uma solução de bate-papo dedicada através de reservas fora de banda

A maioria dos aplicativos usa o XIM para manipular todos os aspectos de reunião de jogadores. Afinal, um foco sobre a montagem de todos os recursos necessários para dar suporte a cenários completos de multijogador comuns é o motivo pelo qual que ele é chamado "Xbox Integrated Multiplayer". No entanto, alguns aplicativos que implementam suas próprias soluções com vários participantes usando servidores dedicados da Internet também gostaria que as vantagens do estabelecimento de latência baixa e confiável, comunicação de bate-papo de baixo custo peer-to-peer. XIM reconhece essa necessidade e dá suporte a um modo de extensão que tira proveito da comunicação de par simplificada do XIM para ampliar o gerenciamento de player externo acontecendo fora a API XIM. Em vez de mover em uma rede XIM através de meios sociais ou cruzamento, jogadores mover usando "reservas" espaços reservados garantidos para usuários específicos que são trocadas "out-of-band" por meio do mecanismo de rendezvous player externo do aplicativo.

Além do processo de movimentação, redes XIM gerenciadas usando reservas de out-of-band são efetivamente o mesmo que nenhuma outra rede XIM. Todas as funções de comunicação funcionam de forma idêntica. No entanto, para que haja correspondência e métodos de API de descoberta de redes sociais necessariamente estão desabilitados para redes XIM gerenciadas usando reservas de out-of-band, pois eles entraria em conflito com a implementação de externa do aplicativo. Você não pode enviar convites de tal rede XIM, por exemplo.

>XIM é otimizado para fornecer uma solução de ponta a ponta simples. Portanto, nem todas as topologias complexas ou cenários podem ser um ajuste perfeito para out-of-band reservas. Se você tiver dúvidas sobre a possibilidade ou como aproveitar recursos de comunicação do XIM, entre em contato com seu representante da Microsoft.

Os tópicos subsequentes descrevem como aproveitar as reservas de out-of-band em XIM. Por causa de relativamente poucas diferenças de "standard" uso XIM descritos nas seções anteriores, alguma discussão é abreviado. Familiaridade com [XIM usando](using-xim.md) é recomendado.


## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>Mover para uma nova rede XIM out-of-band reserva

Para começar a usar reservas de out-of-band, um dos seus participantes reunidos deve mover para uma nova rede XIM criada nesse modo. A seleção de quais par participante dispositivo cabe a você. Talvez você já tenha um conceito de jogo host ou servidor, que é uma opção natural para iniciar o processo, mas isso não é necessário. É recomendável escolher um dispositivo que reporta a um tipo de acesso de rede de "Abrir" para obter o melhor tempo de configuração de conectividade. Consulte o `Windows::Networking::XboxLive` documentação da plataforma para obter mais informações.

Mover para um XIM gerenciada por meio de reservas de fora de banda de rede é feito Inicializando XIM e declarando as IDs de usuário local Xbox pretendido, como visto no passo a passo de uso XIM padrão, mas em vez de chamar um método, como `xim::move_to_new_network()`, chame `xim::move_to_network_using_out_of_band_reservation()` com um cadeia de caracteres nula de reserva. Por exemplo:

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

O padrão `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, e `xim_move_to_network_succeeded_state_change` , em seguida, será fornecido ao longo do tempo ao processar as alterações de estado no típico `xim::start_processing_state_changes()` e `xim::finish_processing_state_changes()` loop. Por exemplo:

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
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Depois que o dispositivo inicial processou as alterações de estado e tinha seus jogadores movido com êxito à rede XIM, ele deve criar reservas para os usuários adicionais.

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>Adicionar os jogadores a uma rede XIM gerenciados usando reservas de out-of-band

Redes XIM que são gerenciados usando reservas de out-of-band sempre relatam um valor de `xim_allowed_player_joins::out_of_band_reservation` do `xim::allowed_player_joins()` método; eles estão fechados para todos os jogadores, exceto aqueles com pontos de reservados para suas IDs de usuário do Xbox chamando `xim::create_out_of_band_reservation()`. `xim::create_out_of_band_reservation()` Obtém uma matriz de usuários, para que possa criar tais reservas para os jogadores externamente reunidas uma só vez ou ao longo do tempo. Além disso, os usuários que já têm os jogadores que participam da rede XIM são ignorados, portanto, você também pode fornecer as IDs de usuário adicionais Xbox como um conjunto de substituição completa ou delta que as alterações, o que for conveniente. O exemplo a seguir pressupõe que você já tenha seu conjunto totalmente reunido dos ponteiros de cadeia de caracteres de IDs de usuário do Xbox em uma matriz variável 'xboxUserIds' com 'xboxUserIdCount' número de elementos:

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

Isso inicia o processo assíncrono de criação de um reservas para as IDs de usuário especificado do Xbox. Quando a operação for concluída, XIM será fornecido um `xim_create_out_of_band_reservation_completed_state_change` que relata o êxito ou falha. Se for bem-sucedido, uma cadeia de caracteres de reserva será disponibilizada para o seu sistema fornecer a essas IDs de usuário do Xbox fornecido para a operação. Cadeias de caracteres de reserva criadas com êxito são válidas por apenas um determinado período de tempo. Esse tempo é retornado dentro de `xim_create_out_of_band_reservation_completed_state_change`.

Com uma cadeia de caracteres válida de reserva, seu mecanismo de externo de "out-of-band" usado para reunir os jogadores fora XIM pode ser usado para distribuir a cadeia de caracteres para esses enumerado. Por exemplo, se você estiver usando o serviço de diretório de sessão com vários participantes (MPSD) Xbox Live, você pode escrever essa cadeia de caracteres como uma propriedade de sessão compartilhada personalizada no documento de sessão (Observação: a cadeia de caracteres de reserva sempre conterá apenas um conjunto limitado de caracteres que são seguro para uso em JSON sem a necessidade de codificação de Base64 ou escape).

Depois que os outros usuários recuperar suas cadeias de caracteres de reserva da propriedade de documento de sessão compartilhada, eles podem, em seguida, começar a mover para a rede XIM usá-lo como o parâmetro para `xim::move_to_network_using_out_of_band_reservation()`. O exemplo a seguir pressupõe que a cadeia de caracteres de reserva foi extraída para uma variável chamada 'reservationString'.

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

As operações de movimentação executa de forma assíncrona assim como para o dispositivo inicial que especificou um ponteiro nulo para a cadeia de caracteres de reserva. Alterações de estado `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, e `xim_move_to_network_succeeded_state_change` serão geradas pela mudança. Quando a movimentação for bem-sucedida, players de locais e remotas serão adicionados. Os dispositivos existentes na rede XIM serão fornecidos um `xim_player_joined_state_change` para esses novos jogadores. Neste ponto, a comunicação de bate-papo de voz e texto é habilitada automaticamente entre jogadores nesses dispositivos diferentes nessa rede XIM (no qual política de privacidade e permitem).

A operação de movimentação deve falhar devido a uma cadeia de caracteres de reserva inválido, XIM retornará um `xim_network_exited_state_change` com o motivo pelo qual `xim_network_exit_reason::invalid_reservation`. Isso pode ocorrer por várias razões:

1. O título tenta chamar `xim::move_to_network_using_out_of_band_reservation()` no dispositivo remoto antes do `xim_create_out_of_band_reservation_completed_state_change` é retornado no dispositivo de host
1. O título tenta chamar `xim::move_to_network_using_out_of_band_reservation()` no dispositivo remoto depois que a reserva expirar.
1. O título tenta chamar `xim::move_to_network_using_out_of_band_reservation()` no dispositivo remoto várias vezes ao chamar somente `xim::create_out_of_band_reservation()` depois.

Independentemente do êxito ou falha, as reservas são consumidas por operações de movimentação que usá-los. Portanto, após cada tentativa de uso, o host deve gerar uma nova cadeia de caracteres de reserva chamando `xim::create_out_of_band_reservation()` novamente.

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>Configurar destinos de bate-papo em uma rede XIM gerenciados usando reservas de out-of-band

Redes XIM que são gerenciados usando reservas de out-of-band terão comportamento idêntico para as redes XIM tradicionais em relação à comunicação de bate-papo. Para dar suporte a concorrência cenários, recém-criados redes XIM são automaticamente configurados para só há suporte para bate-papo com outros jogadores que são membros da equipe do mesmo; ou seja, o valor configurado relatado pelo `xim::chat_targets()` por padrão é `xim_chat_targets::same_team_index_only`. Isso pode ser alterado para permitir que todos se comunicar com qualquer outra pessoa (no qual política de privacidade e permitem) chamando `xim::set_chat_targets()`. O exemplo a seguir começa a configurar todas as pessoas na rede XIM para usar um `xim_chat_targets::all_players` valor:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Todos os participantes serão informados de que um novo destino de configuração em vigor quando elas são fornecidas uma `xim_chat_targets_changed_state_change`.

Cada jogador em redes XIM gerenciadas usando reservas de out-of-band é inicialmente configurado com o mesmo valor de índice de equipe, zero. Isso significa que uma configuração de `xim_chat_targets::same_team_index_only` é indistinguível de `xim_chat_targets::all_players` por padrão. No entanto, você pode alterar o índice de equipe do jogador um local a qualquer momento chamando `xim_player::xim_local::set_team_index()`. O exemplo a seguir configura um ponteiro de player localPlayer para ter um novo valor de índice da equipe de um:

```cpp
 localPlayer->local()->set_team_index(1);
```

Todos os dispositivos serão informados de que o jogador tem um novo valor de índice de equipe em vigor quando elas são fornecidas uma xim_player_team_index_changed_state_change para esse jogador. Se a configuração de destino de bate-papo atualmente é xim_chat_targets::same_team_index_only, outros jogadores com esse novo índice de equipe mesmo começará de voz de audição e que está sendo fornecido o chat de texto (privacidade e a política permitir) da alteração player e vice-versa. Os jogadores com o índice antigo de equipe deixará de troca de comunicação tais bate-papo. Se a configuração de destino de bate-papo atualmente é xim_chat_targets::all_players, em seguida, índice de equipe não tem impacto sobre quem pode bater papo com quem.

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>Removendo os jogadores de uma rede XIM gerenciados usando reservas de out-of-band

Você está gerenciando a lista de jogadores externamente ao usar o out-of-band reservas, então, naturalmente, que talvez você precise remover participantes da rede XIM. A maneira comum é para aplicativos aproveitar o mesmo host do jogo que foi usado para criar a rede XIM e reservas subsequentes originalmente para também gerenciar a remoção do player e fazer isso chamando `xim::kick_player()`. Isso remove o player especificado e todos os jogadores no mesmo dispositivo de rede XIM. O exemplo a seguir pressupõe que o aplicativo tiver determinado que deseja remover o `xim_player` objeto apontado pela variável 'playerToRemove':

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

O player aplicável (ou players) serão fornecidos qualquer necessária `xim_player_left_state_change` para cada jogador e um `xim_network_exited_state_change` indicando que eles foi executado da rede. Como alternativa, cada jogador pode chamar `xim::leave_network()` para sair em sua próprias para o mesmo efeito.

Lembre-se de que a comunicação peer-to-peer XIM pode fazer sua própria determinação a qualquer momento que um jogador saiu da rede XIM devido a dificuldades ambientais. Seu aplicativo deve estar preparado para lidar com um "não solicitado" `xim_player_left_state_change` e reconciliar qualquer discrepância entre o estado do XIM e seu esquema de gerenciamento do player externo de forma apropriada para seu aplicativo específico.

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>Limpando a uma rede XIM gerenciados usando reservas de out-of-band

Todos os jogadores que já não tiverem sido inicializados do XIM de rede e quiser retornar para o estado como se apenas xim::initialize() e `xim::set_intended_local_xbox_user_ids()` tivesse sido chamada, pode começar a fazer isso usando o `xim::leave_network()` método:

```cpp
xim::singleton_instance().leave_network();
```

Isso inicia a desconectar-se de forma assíncrona de outros participantes. Isso fará com que os dispositivos remotos a ser fornecido um `xim_player_left_state_change` para os jogadores local e o dispositivo local serão fornecido um `xim_player_left_state_change` para cada jogador, local ou remoto. Quando todos os desconexão normal foi concluído, um final `xim_network_exited_state_change` será fornecido. O aplicativo pode chamar `xim::cleanup()` para liberar todos os recursos e retornar para o estado não inicializado:

```cpp
 xim::singleton_instance().cleanup();
```

Invocando `xim::leave_network()` e aguardando a `xim_network_exited_state_change` para sair de um XIM rede normalmente é sempre recomendável quando um `xim_network_exited_state_change` já não foi fornecido. Chamar `xim::cleanup()` diretamente pode causar problemas de desempenho de comunicação para os demais participantes enquanto eles são forçados para mensagens de tempo limite para o dispositivo que simplesmente "desapareceu".
