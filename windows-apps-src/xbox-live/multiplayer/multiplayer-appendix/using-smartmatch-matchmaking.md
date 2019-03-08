---
title: Usando o emparelhamento de SmartMatch
description: Saiba como usar o Xbox Live SmartMatch para corresponder os jogadores em um jogo com vários participantes.
ms.assetid: 10b6413e-51d9-4fec-9110-5e258d291040
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox one, com vários participantes, para que haja correspondência, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 89f33768efcd649987866fd0798c222aa97f7ff8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592091"
---
# <a name="using-smartmatch-matchmaking"></a>Usando o emparelhamento de SmartMatch

O fluxograma a seguir ilustra o processo de emparelhamento de SmartMatch.

![](../../images/multiplayer/Multiplayer_2015_SmartMatch_Matchmaking.png)

## <a name="creating-a-match-ticket-session-and-a-match-ticket"></a>Criação de uma sessão de tíquete de correspondência e um tíquete de correspondência

Antes de iniciar o emparelhamento, um "scout" para que haja correspondência configura uma sessão de tíquete de correspondência para representar o grupo de pessoas que gostariam de inserir o cruzamento juntos. Todos os usuários neste grupo Junte-se a sessão usando o **MultiplayerSession.Join método (String, Boolean, Boolean)**.

Depois que a sessão de tíquete foi criada e populada com os jogadores, o título envia a sessão para o serviço de compatibilidade usando o **MatchmakingService.CreateMatchTicketAsync método**. Esse método cria um tíquete de correspondência que representa a sessão de tíquete e atualiza o campo /servers/matchmaking/properties/system/status da sessão de tíquete para "Pesquisar". Para obter mais informações, consulte [como: Crie um tíquete de correspondência](multiplayer-how-tos.md).

A resposta do que o método de criação do tíquete de correspondência é uma **CreateMatchTicketResponse classe** objeto. A resposta contém a ID de tíquete de correspondência, um GUID que pode ser usado pode ser usado para cancelar o relacionamento de pessoas, excluindo o tíquete. A resposta também contém um tempo de espera médio para o hopper, que pode ser usado para definir as expectativas dos usuários.


## <a name="setting-matchmaking-attributes-on-the-session-and-players"></a>Definindo atributos para que haja correspondência na sessão e Players

Ao enviar uma sessão de emparelhamento, o título pode definir atributos que o serviço de cruzamento usa para a sessão com outras sessões de grupo. Atributos podem ser especificados no nível de permissão ou no nível por membro.


### <a name="setting-matchmaking-attributes-at-the-match-ticket-level"></a>Definindo atributos de compatibilidade no nível de permissão de correspondência

O título envia atributos no nível de permissão de correspondência na *ticketAttributesJson* parâmetro do **MatchmakingService.CreateMatchTicketAsync** método. Atributos especificados no nível do tíquete substituem os mesmos atributos especificados no nível por membro.


### <a name="setting-matchmaking-attributes-at-the-per-member-level"></a>Definindo atributos de compatibilidade no nível por membro

O título Especifica atributos por membro em cada membro dentro da sessão de tíquete de correspondência. Elas são definidas por meio da chamada a **MultiplayerSession.SetCurrentUserMemberCustomPropertyJson método**, usando um nome de propriedade de "matchAttrs". Essa chamada coloca os atributos em /members/ {index} / propriedades/personalizado/matchAttrs campo em cada jogador dentro da sessão de tíquete.

O processo de emparelhamento "nivela" por membro cada em um único atributo de nível de permissão, com base no método flatten especificado para o atributo na configuração do Xbox Live para o hopper. Isso pode ser configurado no [XDP](https://xdp.xboxlive.com) ou [Partner Center](https://partner.microsoft.com/dashboard).


## <a name="making-the-match"></a>Fazendo a correspondência

Com a sessão de tíquete e a correspondência tíquete configurada, as correspondências de serviço para que haja correspondência a sessão de tíquete representados com outras sessões de permissão que representa outros grupos e cria ou identifica uma sessão de destino da correspondência. O serviço também cria as reservas para os jogadores correspondentes na sessão de destino e marca as sessões de tíquete conforme correspondido. MPSD notifica o título dessa alteração para a sessão de tíquete.

Agora o título deve, em seguida, tomar medidas para inicializar a sessão de destino para confirmar que suficiente players apareceram e executar a qualidade de serviço (QoS) verifica para garantir que eles possam se conectar entre si com êxito. Se a inicialização e/ou QoS falhar, o título marca a sessão de tíquete para reenvio ao relacionamento de pessoas para que o outro grupo que pode ser encontrado. Para obter mais informações sobre os processos, consulte [inicialização da sessão de destino e QoS](smartmatch-matchmaking.md).

Durante a atividade de correspondência, as seguintes alterações são feitas aos objetos JSON para a sessão:

-   campo /Servers/matchmaking/Properties/System/status definido como "encontrado"
-   campo de /Servers/matchmaking/Properties/System/targetSessionRef definido para a sessão de destino
-   /Members/ {index} / propriedades/personalizado/campo matchAttrs para cada sessão de tíquete é copiado para o /members/ {index} / constantes/personalizado/matchmakingResult/playerAttrs campo
-   Para cada jogador, do tíquete atributos copiados do campo no tíquete correspondência ticketAttributes /members/ {index} / constantes/personalizado/matchmakingResult/ticketAttrs campo


## <a name="maintaining-the-match-ticket"></a>Manter o tíquete de correspondência

O serviço de cruzamento usa um instantâneo da sessão de tíquete no momento quando o tíquete de correspondência é criado para a sessão. Portanto, se qualquer players ingressar ou sair da sessão de tíquete, o título deve usar o serviço de cruzamento excluir e recriar o tíquete de correspondência.


## <a name="reusing-the-game-session-as-a-match-ticket-session"></a>Reutilizando o sessão de jogo como uma sessão de tíquete de correspondência

| Importante                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| É importante perceber que duas sessões com *preserveSession* conjunto como sempre não poderá corresponder entre si, uma vez que eles não podem ser combinados. O fluxo para que haja correspondência utilizado pelo título deve levar esse fato em consideração. |

Um título pode reutilizar uma sessão de jogo existente como uma sessão de tíquete de correspondência localizar mais jogadores para ingressar em um jogo que já está em andamento. Para habilitar isso, o título deve criar o tíquete de correspondência chamando **CreateMatchTicketAsync** com o *preserveSession* parâmetro definido como **PreserveSessionMode.Always** . O serviço de cruzamento, em seguida, garante que a sessão existente usada para o tíquete é preservada durante todo o processo de emparelhamento e torna-se a sessão de destino resultante.


## <a name="deleting-the-match-ticket"></a>Excluindo o tíquete de correspondência

Para excluir o tíquete de correspondência, as chamadas de título **MatchmakingService.DeleteMatchTicketAsync método**. Exclusão do tíquete:

1.  Interrompe o relacionamento de pessoas para os usuários na sessão de tíquete.
2.  Atualizações de campo /servers/matchmaking/properties/system/status na sessão de tíquete para "cancelado".


## <a name="performing-matchmaking-for-games-using-xbox-live-compute"></a>Realizar o emparelhamento para jogos usando computação em tempo real do Xbox

Aqui estão as etapas de alto nível que ocorrem para obter um usuário matchmade em um jogo com base no Xbox Live Compute. Um fluxo semelhante deve aplicar de jogos hospedados por terceiros.
1.  O scout cria uma sessão de tíquete para representar o grupo. Esta sessão contém uma lista de possíveis datacenters, localizados na configuração da sessão em /constants/system/measurementServerAddresses. Ele vem de ambos o modelo de sessão se a lista de datacenter é estática ou de cliente gravando no momento da criação de sessão após obtendo-o do Xbox Live Compute primeiro. Esta sessão contém também gsiSetId, gameVariantId e maxAllowedPlayers valores no objeto personalizado/targetSessionConstants/gameServerPlatform.
2.  Todos os outros clientes no grupo de ingressarem na sessão de tíquete.
3.  Todos os membros do grupo de baixar os valores de measurementServerAddresses do objeto /constants/system para a sessão de tíquete, execute ping-los usando a API da plataforma e carregar uma lista ordenada de datacenters preferencial para a sessão, conforme definido em /members/ {index} /Properties/System/serverMeasurements.

| Observação                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O título pode definir e recuperar os valores de measurementServerAddresses da sessão usando o **MultiplayerSession.SetMeasurementServerAddresses** método e o  **Propriedade MultiplayerSessionConstants.MeasurementServerAddressesJson**. |

4.  As chamadas do scout **CreateMatchTicketAsync**, passando uma referência para a sessão de tíquete.

| Observação                                                                                                                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se os objetos de sessão de tíquete tiverem constantes incompatíveis, o método de tíquete create talvez não seja bem-sucedida. Isso pode ser evitado com a adição de uma necessidade regra para o hopper, para evitar a correspondência com jogadores que têm incompatíveis constantes. |

Se o **MatchTicketDetailsResponse.PreserveSession propriedade** é definido como nunca, o serviço de cruzamento copia as medidas do servidor de cada membro para a representação interna do tíquete. Ele nivela as medidas de servidor dos membros do tíquete em uma coleção de medidas de servidor único para o tíquete, armazenado na representação interna do tíquete como um atributo do tíquete "especial".

Se **MatchTicketDetailsResponse.PreserveSession** é definida como sempre, o servidor de medidas não são usadas. Em vez disso, o serviço de cruzamento o valor de /properties/system/matchmaking/serverConnectionString copiado para a sessão para a representação interna de tíquete, como uma coleção de serverMeasurements de tamanho 1 com latência zero.

5.  O serviço de cruzamento corresponde a sessão de tíquete com outras pessoas que representa outros grupos, colocar o servidor de coleções de medição em conta. Ele tenta corresponder o grupo com outros grupos que têm os mesmos datacenters preferenciais altamente.
6.  Depois que um grupo correspondente foi encontrado, o serviço de cruzamento cria ou identifica uma sessão de destino e adiciona todos os jogadores de sessões de tíquete que são correspondidas juntos. O serviço grava as medidas de servidor nivelado final para o grupo correspondente no /properties/system/serverConnectionStringCandidates. Ele grava as medidas de servidor enviada originalmente para cada membro adicionado recentemente na sessão de destino /members/ {index} / constantes/sistema/matchmakingResult/serverMeasurements.
7.  Todos os clientes executam inicialização na sessão de destino, conforme discutido acima. No entanto, como os clientes se conectarem ao Xbox Live Compute, eles não fazem QoS uns com os outros para confirmar a conectividade.
8.  Algumas ou todas as chamadas de clientes do **GameServerPlatformService.AllocateClusterAsync método**. O primeiro deles dispara a alocação, enquanto os outros recebem somente uma confirmação. O método obtém a sessão de destino do MPSD e escolhe um data center com base nas preferências de datacenter na sessão de destino, bem como carga e outros dados de conhecimento específico de computação Xbox Live.
9.  Reproduzir todos os clientes.


## <a name="see-also"></a>Consulte também

[Operações de tempo de execução SmartMatch](smartmatch-matchmaking.md)

[Para que haja correspondência SmartMatch](smartmatch-matchmaking.md)

**Namespace Microsoft.Xbox.Services.Matchmaking**

**Namespace Microsoft.Xbox.Services.Multiplayer**
