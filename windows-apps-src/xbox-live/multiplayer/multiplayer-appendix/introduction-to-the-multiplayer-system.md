---
title: Introdução ao sistema Multijogador
description: Fornece uma introdução de alto nível para o sistema de 2015 do Xbox Live com vários participantes.
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, com vários participantes de 2015
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629411"
---
# <a name="introduction-to-the-multiplayer-system"></a>Introdução ao sistema Multijogador

| Observação                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Este artigo é para uso avançado de API.  Como ponto de partida, dê uma olhada na [API do Gerenciador de vários jogadores](../multiplayer-manager.md) que simplifica significativamente o desenvolvimento.  Informe sua mãe se você encontrar um cenário sem suporte no Gerenciador de com vários participantes. |

Este documento contém as seções a seguir
* Sobre o sistema com vários participantes
* Arquiteturas, Interfaces e os componentes com vários participantes
* Partes compatíveis com vários participantes de 2015
* Terminologia para múltiplos jogadores
* Quais são as novidades em 2015 Multiplayer
* Diferenças entre o Xbox 360 e Xbox MPSD uma sessão funções

## <a name="about-the-multiplayer-system"></a>Sobre o sistema com vários participantes


2015 multiplayer otimiza o uso direto de MPSD e sessões de jogos. Ele fornece melhor suporte para várias sessões simultâneas para um único título ou o usuário e atualiza a API de serviços Xbox (XSAPI) para ativar os títulos para:

-   Anuncie a disponibilidade para junções e a atividade atual de um usuário.
-   Envie convites para sessões, juntamente com cadeias de caracteres de contexto visível pelo usuário (o título especificado).
-   Descubra e ingressar em sessões por meio de código do título.
-   Manter as conexões de soquete da web para MPSD para que eles possam receber notificações breves (shoulder taps) na sessão alterações, por exemplo, as atualizações que refletem as assinaturas para alterar os eventos e alterações de estado de conexão. MPSD também usa conexões de soquete da web para detectar rapidamente e agir sobre desconexão do cliente.
-   Use o cruzamento de SmartMatch.

## <a name="multiplayer-components-interfaces-and-architectures"></a>Arquiteturas, Interfaces e os componentes com vários participantes

### <a name="components-of-2015-multiplayer"></a>Componentes com vários participantes de 2015

Com vários participantes é um sistema que consiste em vários componentes. Ele é flexível o suficiente para permitir que outros componentes, como servidores dedicados e sistemas externos para que haja correspondência.
#### <a name="multiplayer-session-directory-mpsd"></a>Diretório de sessão multijogador (MPSD)


Diretório de sessão para múltiplos jogadores (MPSD) é um serviço que mantém uma coleção de sessões. Uma sessão é definida como um documento seguro que residem na nuvem e que representa um grupo de pessoas a jogar um jogo. Para obter detalhes MPSD, consulte [diretório de sessão com vários participantes (MPSD)](multiplayer-session-directory.md).


#### <a name="multiplayer-apis"></a>APIs com vários participantes

Multiplayer 2015 fornece uma API de WinRT de com vários participantes, implementado por meio de **Microsoft.Xbox.Services.Multiplayer Namespace**. Seus componentes incluem o **MultiplayerService classe**, definir um serviço web que encapsula MPSD.

Também há uma API de REST com vários participantes. Ele define os objetos de URIs e JSON que são chamados pelos métodos de API do WinRT. A funcionalidade REST pode ser usada em chamadas diretas de seus títulos, mas você é incentivado a acessar MPSD indiretamente por meio da API do WinRT. Para obter mais informações, consulte **MPSD chamando**.

#### <a name="xbox-party-system"></a>Sistema de terceiros do Xbox

Com vários participantes de 2015, o sistema de terceiros do Xbox oferece suporte a apenas as partes de bate-papo como entidades externas. Títulos podem interagir com o sistema de terceiros para descobrir a associação de partes de bate-papo. Para obter mais informações, consulte **partes compatíveis com 2015 Multiplayer**.

O sistema de terceiros agora dá suporte a jogos diretamente por meio da sessão MPSD, em vez de usar uma parte do jogo. É responsabilidade do título para usar sessões para habilitar operações como a interação de membro, efetuar pull de jogadores em um jogo como espaço se torna disponível e envolvimento do usuário durante os períodos de espera.


### <a name="2015-multiplayer-interfaces"></a>Interfaces com vários participantes de 2015

2015 multiplayer usa várias interfaces para outros componentes do Xbox.
#### <a name="xbox-secure-sockets"></a>Xbox Secure Sockets

2015 multiplayer dá suporte à comunicação de rede de baixo nível entre os dispositivos usando soquetes seguros do Xbox e o Winsock. A funcionalidade de rede usa o Internet Protocol Security (IPSec) para permitir que os títulos fornecer associação de dispositivo seguro. Ver **visões gerais de rede** para obter detalhes sobre o Xbox proteger o recurso de soquetes.


#### <a name="xbox-live-real-time-activity-service"></a>Serviço de atividade em tempo real ao vivo do Xbox

2015 multiplayer usa o **serviço de atividade em tempo real** permitir que os títulos assinar as alterações de sessão MPSD e habilitar a detecção automática do cliente se desconecta. Mais informações são fornecidas em [serviço de atividade em tempo real (RTA)](../../real-time-activity-service/real-time-activity-service.md).


#### <a name="xbox-live-matchmaking-service"></a>Serviço de cruzamento de ao vivo do Xbox

O **serviço de cruzamento** grupos de players, dependendo de suas preferências e dados e informações fornecidas durante a SmartMatch cruzamento de formulários. Para obter detalhes de vários jogadores uso desse serviço, consulte [SmartMatch cruzamento](smartmatch-matchmaking.md).


#### <a name="xbox-live-reputation-service"></a>Serviço de reputação em tempo real do Xbox

O *serviço de reputação* gerencia as estatísticas do usuário sobre reputação e durante a reputação filtrada para que haja correspondência. Ele é usado pelo Multiplayer 2015 por meio do emparelhamento SmartMatch. Para obter mais informações, consulte [reputação](../../social-platform/people-system/reputation.md).


#### <a name="xbox-live-compute"></a>Computação de Xbox Live

Xbox Live Compute fornece a capacidade para títulos de 2015 Multiplayer de processamento na nuvem. Para obter mais informações sobre como usar o Xbox Live Compute, consulte [usando o Xbox Live Compute em Multiplayer](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer).


### <a name="typical-2015-multiplayer-architectures"></a>Arquiteturas com vários participantes típicas de 2015

Esta seção apresenta as arquiteturas de vários jogadores 2015 mais comuns.
#### <a name="peer-to-peer-architecture"></a>Arquitetura ponto a ponto

Arquitetura ponto a ponto, o título usa o cruzamento MPSD e SmartMatch para descobrir os endereços de ponto a ponto. Os endereços, em seguida, são usados para conectar-se os pares usando soquetes seguros do Xbox. Para obter mais informações, consulte *Introdução ao Winsock no Xbox One*.

![Diagrama da arquitetura ponto a ponto](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>Arquitetura cliente-servidor

Na arquitetura cliente-servidor com vários participantes, o título usa o cruzamento MPSD e SmartMatch para descobrir os endereços de servidor dedicado. Em seguida, eles são usados para se conectar ao servidor dedicado usando soquetes seguros do Xbox. Para obter mais informações, consulte *Introdução ao Winsock no Xbox One*.

| Observação                                                                         |
|-------------------------------------------------------------------------------------------|
| Instâncias de computação do Xbox Live podem ser usadas como os servidores a arquitetura cliente-servidor. |

![Diagrama da arquitetura cliente-servidor](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>Partes compatíveis com vários participantes de 2015
2015 com vários participantes para Xbox One não expõem a "entidade de jogo" como uma construção de nível de sistema. No entanto, ele dá suporte a "entidade de bate-papo" em um nível de sistema, como no Multiplayer de 2014.

| Observação                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| Os títulos ainda podem implementar experiências de usuário semelhantes àquelas implementado usando a parte do jogo, mas usando sessões MPSD em vez disso. |


### <a name="chat-party"></a>Bate-papo terceiros

Um participante de bate-papo é um grupo de pessoas que bate-papo entre si, gerenciado pelo usuário por meio de um aplicativo de terceiros o Xbox. Os usuários podem ser em uma parte de bate-papo quando estiver em execução em uma sessão de jogo ou durante a execução de outra atividade de console. No entanto, não há nenhum vínculo entre os usuários do grupo de bate-papo e outras atividades para esses usuários.

A parte de bate-papo é exposta usando o *PartyChat classe*, que permite que o título examinar o estado da parte de bate-papo. Por exemplo, um título pode enumerar os membros da parte de bate-papo usando o *PartyChat.GetPartyChatViewAsync método*.


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>Implementando recursos semelhantes para aqueles relacionados a parte do jogo

Em 2014 Multiplayer, a parte do jogo servido várias finalidades. Permissão para ela um título ao:

-   Anunciar a disponibilidade para junções e a atividade atual de um usuário
-   Enviar convites (convites) para as sessões
-   Descobrir e ingressar em sessões
-   Receber notificações sobre determinadas alterações para as sessões registradas para a parte
-   Use o cruzamento de SmartMatch
-   Manter um grupo de pessoas juntos em várias sessões do jogos

Multiplayer 2015 oferece suporte a todos os recursos acima diretamente por meio das sessões MPSD.

## <a name="multiplayer-terminology"></a>Terminologia para múltiplos jogadores


| Termo                                 | Descrição|
| --- | --- |
| Player de Active Directory                        | Um player que foi definido para o estado ativo dentro da sessão. O título define um player para esse estado quando o player faz parte de um jogo. Para obter mais informações, consulte [estados de sessão do usuário](mpsd-session-details.md).                                                                                                                                                                                                                                                                                          |
| Arbiter                              | O console único em uma sessão de jogo que gerencia o estado da sessão para múltiplos jogadores sessão diretório (MPSD) para um jogo, por exemplo, em que a sessão de jogo para o emparelhamento, para localizar mais jogadores de publicidade. O arbitrador é definido pelo título. O arbitrador nem sempre é o host do jogo. Ver [sessão arbitrador](mpsd-session-details.md).                                                                                                                                                                            |
| Jogo organizado                        | Um tipo de jogo que é criado por meio de um jogador convidando outros participantes para ingressar, sem qualquer envolvimento da compatibilidade.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Bate-papo terceiros                           | Um grupo de pessoas que estão conversando juntos. As pessoas podem ser envolvidas na mesma atividade ou eles podem ser envolvidos em atividades diferentes, como jogos, música ou aplicativos. Ver **partes compatíveis com 2015 Multiplayer**.                                                                                                                                                                                                                                                                                 |
| Convite para jogo                          | Um convite para ingressar em uma sessão de jogo.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Sessão de jogo                         | Uma sessão em que os usuários, na verdade, estão jogando em conjunto. Todos os cenários com vários participantes, por exemplo, para que haja correspondência ou ingressar em um corredor, em última análise terminam em uma sessão de jogo. Geralmente, a sessão é anunciada como as atividades atuais dos usuários para habilitar associações. Ele também é usado para criar o recente lista de jogadores. Ver [detalhes da sessão MPSD](mpsd-session-details.md).                                                                                                                                                           |
| Host de sessão de jogo                    | O console em execução o jogo executar simulação para títulos criada em uma arquitetura de rede de ponto a ponto baseada em host. Esse console normalmente é o mesmo que o arbitrador, mas ele não precisa ser o mesmo.                                                                                                                                                                                                                                                                                                                            |
| Alça (ou o identificador de sessão)           | Uma referência a uma sessão MPSD que tem estado adicional e o comportamento associado a ele. Ver [MPSD manipula a sessões](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                    |
| Player inativo                      | Um player que foi definido para o estado inativo dentro da sessão. O título define um player para esse estado quando um jogo é suspenso ou não está inativo, conforme definido pelo título. Em alguns casos, MPSD também pode definir um player como inativo, mas é basicamente a responsabilidade do título para fazê-lo. Para obter mais informações, consulte [estados de sessão do usuário](mpsd-session-details.md).                                                                                                                           |
| Hopper                               | Um Hopper é uma coleção orientada a lógica de tíquetes de correspondência. Um título pode ter vários hoppers, mas só os tíquetes dentro do mesmo hopper podem ser correspondidos. Por exemplo, um título pode criar um hopper para qual player habilidade é o item mais importante para correspondência. Ele pode usar outro hopper em que os jogadores são correspondidos somente se tiver adquirido o mesmo conteúdo que pode ser baixado. Para obter mais informações sobre onde hoppers se encaixa no fluxo de trabalho SmartMatch, consulte [SmartMatch operações de tempo de execução](smartmatch-matchmaking.md) |
| Junte-se em andamento                     | O conceito de ingressar em outro player's jogo após o início do jogo. Players podem participar de jogo de um amigo por meio de cartão de jogador do amigo. O título, em seguida, pode mover esses players para a sessão de jogo no momento apropriado.                                                                                                                                                                                                                                                                                                      |
| Sessão lobby                        | Uma sessão de auxiliar para players convidados que estão aguardando para ingressar em uma sessão de jogo. Ver [detalhes da sessão MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                       |
| Sessão de destino da correspondência                 | Uma sessão de correspondência Configure durante SmartMatch cruzamento para representar a correspondência. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                              |
| Sessão de tíquete de correspondência                 | Uma sessão de correspondência preliminar configurar durante SmartMatch emparelhamento. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                                         |
| Sessão MPSD                         | Um documento seguro que reside no diretório de sessão para múltiplos jogadores (MPSD) dentro da nuvem do Xbox Live. Ele contém um grupo de usuários que podem estar conectadas durante a execução de um título no Xbox One, junto com os metadados sobre os usuários e seus jogos. Ver [detalhes da sessão MPSD](mpsd-session-details.md).                                                                                                                                                                                                                  |
| Diretório de sessão multijogador (MPSD) | O serviço funcionando na nuvem que o sistema com vários participantes usa para armazenar e recuperar as sessões. Ver [diretório de sessão para múltiplos jogadores (MPSD)](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                |
| Aplicativo de terceiros                            | Um sistema Xbox One ajustar o aplicativo que permite aos usuários exibir e gerenciar suas partes.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Sessão de servidor                       | Uma sessão de jogo criada pelo processamento do Xbox Live Compute. Ver [detalhes da sessão MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                                            |
| Toque shoulder                         | Uma notificação de MPSD para um título que ocorreu uma alteração potencialmente interessante no serviço. O toque shoulder é rápido lembrete, geralmente menos informativo que uma notificação regular. Ver [MPSD alterar o tratamento de notificação e desconectar detecção](multiplayer-session-directory.md).                                                                                                                                                                                                                     |
| Organizar partida com o SmartMatch               | Uma Xbox Live para que haja correspondência funcionalidade disponível para Xbox One títulos, implementados pelo serviço para que haja correspondência. Usando MPSD e para que haja correspondência, o título faz uma solicitação a ser correspondida e será notificado depois que um grupo correspondente foi encontrado. Ver [SmartMatch cruzamento](smartmatch-matchmaking.md).                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>Quais são as novidades em 2015 Multiplayer

| Cuidado                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ao usar com vários participantes de 2015, é importante observar que as classes relacionadas à parte em 2014 Multiplayer não devem ser mais usadas. Faz com que o comportamento incoerentes uma combinação de funcionalidade para múltiplos jogadores de 2015 com as classes de terceiros e nunca deve ser tentada. |


### <a name="new-concepts-in-2015-multiplayer"></a>Novos conceitos no 2015 Multiplayer


#### <a name="web-socket-connections-to-mpsd"></a>Conexões de soquete da Web para MPSD

MPSD agora permite que os títulos manter as conexões de soquete da web com ele. Essas conexões permitem que os clientes recebam notificações quando as sessões são alterados. Para obter mais informações, consulte [detecção de desconectar e tratamento de notificação de alteração de MPSD](multiplayer-session-directory.md).


#### <a name="mpsd-session-handles"></a>Identificadores de sessão MPSD

2015 multiplayer adiciona suporte para identificadores de sessão MPSD, que fazem referências às sessões que podem incluir dados digitados. Para obter mais informações, consulte [MPSD manipula a sessões](multiplayer-session-directory.md).


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>Resumo da nova funcionalidade de API do WinRT para múltiplos jogadores de 2015

Nova funcionalidade de API do WinRT para múltiplos jogadores se baseia o XSAPI existente, para ajudar a facilitar que a transição para títulos já usando o Multiplayer 2014 em combinação com a API de jogo de terceiros.

2015 multiplayer adiciona o *MultiplayerActivityDetails classe*, que representa os detalhes da atividade atual do usuário, por exemplo, a sessão na qual o usuário é permite junções.

Nova funcionalidade foi adicionada para o *MultiplayerService classe*. Exemplos são métodos e propriedades de lidar com atividades de usuários e grupos de redes sociais, recuperação de sessões usando vários filtros e identificadores, envio de convites do jogos e a sessão de leituras e gravações a usando alças.

O *MultiplayerSession classe* adiciona funcionalidade para trabalhar com tipos de alteração de sessão, as assinaturas para notificações, comparação de sessão e definir uma sessão como fechado.

O *classe MultiplayerSessionReference* é alterado para herdar **IMultiplayerSessionReference**, para dar suporte a chamadas de namespace entre. A classe também tem um novo caminho URI, métodos de análise.

| Observação                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Para dar suporte a eventos e assinaturas para notificações, 2015 Multiplayer adiciona a funcionalidade do *Microsoft.Xbox.Services.RealTimeActivity Namespace*. Também é incluído na nova funcionalidade está *SystemUI.ShowSendGameInvitesAsync método*, usado para mostrar o convite do jogo da interface do usuário para vários jogadores de 2015. |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Diferenças entre o Xbox 360 e Xbox MPSD uma sessão funções

| Função | Xbox 360 | Xbox One |
|---|---|---|
| **Obter informações de sessão de jogo** | XSessionGetDetails, XSessionSearchByID ou título faz o acompanhamento. | Informações de sessão de solicitações de título do MPSD. |
|**Migrar o host** | Quando necessário, o título chama XSessionMigrateHost. | Dependendo da causa da migração, o título pode ser capaz de atribuir um novo host da sessão ou pode criar uma nova sessão MPSD. |
| **Várias sessões do player** | Difícil de lidar com mais de uma sessão por vez, por exemplo, XNetReplaceKey versus XNetUnregisterKey. | Sessão baseada em serviço torna o gerenciamento de uma sessão mais limpo e simplifica a manipulação de várias sessões. |
| **Signouts e desconecta** | Título tem que lidar com desconecta e signout diferentemente com XCloseHandle ou XSessionDelete. | MPSD simplifica signouts e desconectar o tratamento e tempos limite definido em configuração do jogo. |
| **Para que haja correspondência** | Consultas com base no cliente para que haja correspondência | Compatibilidade com base no serviço que permite melhor correspondência de qualidade e mais fáceis de cruzamento de plano de fundo dentro do título. |


### <a name="sessions"></a>Sessions

O Xbox 360, uma sessão representado uma instância do jogo. Os usuários pesquisados para sessões em que o serviço de cruzamento e informado estatísticas no final de uma sessão.

No Xbox One, uma sessão é mais genérica e representa um grupo de jogadores. Uma sessão é necessária para qualquer conectividade de rede entre os consoles e contém informações que devem ser compartilhadas entre todos os usuários na sessão. Alguns exemplos dessas informações incluem o número de jogadores permitidos na sessão, o endereço de seguro de cada console na sessão e dados de jogos personalizados.


### <a name="xbox-matchmaking"></a>Para que haja correspondência Xbox

No Xbox 360, títulos executada para que haja correspondência Configurando um esquema de atributos e um conjunto de consultas para pesquisar por meio desses atributos. Em tempo de execução, o título optou por qualquer host uma sessão ou procure um.

No Xbox One, para que haja correspondência é baseada em servidor e players e títulos não decidir se deseja hospedar ou pesquisar. Em vez disso, cada grupo previamente formado de jogadores cria uma sessão de "permissão" e envia essa sessão para o serviço de cruzamento. O serviço, em seguida, localiza a outras sessões e combina os grupos para formar uma nova sessão de "target". Os clientes são notificados sobre a correspondência e executam a qualidade de serviço (QoS) para validar a conectividade com outros membros da sessão antes de iniciar o jogo.


### <a name="xbox-live-compute-service"></a>Serviço de computação ao vivo do Xbox

O serviço Xbox Live Compute é uma nova oferta no Xbox One. Ele permite aos desenvolvedores aproveitar o poder de computação elástica da nuvem e permite maiores cenários com vários participantes que eram possíveis em uma rede ponto a ponto. Para obter mais informações sobre o serviço Xbox Live Compute, consulte a documentação do Xbox Live Compute nos documentos do XDK.


## <a name="see-also"></a>Consulte também

[Diretório de sessão para múltiplos jogadores (MPSD)](multiplayer-session-directory.md)

[Para que haja correspondência SmartMatch](smartmatch-matchmaking.md)

[Serviço de atividade em tempo real (RTA)](../../real-time-activity-service/real-time-activity-service.md)

[Reputação](../../social-platform/people-system/reputation.md)

[Usando o Xbox Live Compute em com vários participantes (requer acesso de parceiro gerenciado)](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
