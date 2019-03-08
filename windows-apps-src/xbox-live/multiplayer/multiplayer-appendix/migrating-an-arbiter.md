---
title: Migrando um arbitrador
description: Saiba como migrar um arbitrador no Xbox Live Multiplayer 2015
ms.assetid: 9fb5d2c0-d548-4a22-b64e-6b215f78d22e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arbiter, com vários participantes de 2015
ms.localizationpriority: medium
ms.openlocfilehash: 7e250841fb0731a903c0e9c5bdeeecd42f07ac84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629701"
---
# <a name="migrating-an-arbiter"></a>Migrando um arbitrador

Em algum momento durante uma sessão completa, você precisa selecionar um arbitrador nova usando a migração do arbiter. Há dois tipos de migração:

-   **Migração gradual do arbiter**
-   **Migração do arbiter de failover**

O fluxograma a seguir ilustra como migrar um arbitrador.

![](../../images/multiplayer/Multiplayer_2015_HostMigration.png)

## <a name="graceful-arbiter-migration"></a>Migração gradual do Arbiter

Em migração gradual do arbiter, o arbitrador saído pode ajudá-lo a tarefa de migração e determinar um arbitrador novo. Esse tipo de migração usa a configuração de um arbitrador conforme descrito em [como: Definir um arbitrador para uma sessão MPSD](multiplayer-how-tos.md).


## <a name="failover-arbiter-migration"></a>Migração do Arbiter de failover

Em uma migração do arbiter de failover, a conexão para o arbitrador anterior é perdido e os computadores restantes devem determinar um arbitrador novo para a sessão. Migração do arbiter failover também define o host de token do dispositivo e lida com códigos de status HTTP/412 migração apenas como normal do arbiter faz. No entanto, há várias abordagens para a seleção de um arbitrador novo durante uma migração do arbiter de failover.
## <a name="select-arbiter-using-the-host-candidate-list"></a>Selecione o arbitrador usando a lista de candidatos do Host

Você pode configurar MPSD para fornecer uma lista de candidatos host ordenada com base em métricas de QoS que são medidos durante determinadas operações de emparelhamento. O cliente pode usar essa lista para determinar um arbitrador novo. Para tirar proveito dessa lista durante a migração do arbiter, cada par possível:

1.  Identifica a posição da lista do arbitrador anterior.
2.  Avalie o console próximo na lista.
3.  Se o console é um console local, use-o como novo o arbitrador.
4.  Se o console não está mais presente na sessão para múltiplos jogadores, ou foi desconectado de seus colegas, passe para a candidata seguinte na lista e avaliá-lo como nas etapas anteriores.
5.  Se atingir o final da lista com nenhum arbitrador novo selecionado, use uma abordagem greedy a seleção do arbiter, que pode interromper a conectividade. Consulte "Usar seleção de arbitrador Greedy".

| Observação                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Não é recomendável criar uma lista de candidatos do host do jogo depois de relacionamento de pessoas por meio de investigações de QoS explícitas no título. Se esse mecanismo é absolutamente necessário, ter seu cliente para usar o token de dispositivo de host em vez de informações do usuário, por exemplo, ID de usuário do Xbox, para determinar os candidatos arbitrador. |


### <a name="select-arbiter-using-peer-voting"></a>Selecione o arbitrador usando o par de votação

Se existe conectividade total entre todos os pontos, eles podem usar mensagens de ponto a ponto para votar e selecionar um novo arbitrador. O novo arbitrador, em seguida, atualiza o token de dispositivo de host da sessão usando uma atualização sincronizada. Consulte [como: Atualizar uma sessão para múltiplos jogadores](multiplayer-how-tos.md).


### <a name="use-greedy-arbiter-selection"></a>Use seleção arbitrador Greedy

Às vezes, nenhuma lista de candidatos do host está disponível ou conectividade QoS não é necessária, por exemplo, para as responsabilidades do arbiter puro. Nesses casos, seu cliente pode usar uma abordagem de seleção do arbiter greedy. Nesse caso, um par deve definir o novo arbitrador assim que ele detecta que o arbitrador original deixou a sessão de jogo, conforme relatado pelo **MultiplayerSessionChanged** eventos. Todos os outros pares ver um código de status HTTP/412 ao tentar definir o host de token do dispositivo, supondo que nenhuma outra alteração para a sessão é feitas neste momento. Apenas um ponto a ponto foi bem-sucedido no selecionando o novo arbitrador.


## <a name="see-also"></a>Consulte também

[Detalhes da sessão MPSD](mpsd-session-details.md)
