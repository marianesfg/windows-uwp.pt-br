---
title: Conceitos de vários jogadores Xbox Live
description: Saiba mais sobre conceitos para múltiplos jogadores comuns usados por sistemas com vários participantes Xbox Live.
ms.assetid: 1e765f19-1530-4464-b5cf-b00259807fd3
ms.date: 08/25/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes
ms.localizationpriority: medium
ms.openlocfilehash: 0e2ba32b2eaff757cfa6401952567f6b0163f632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623791"
---
# <a name="xbox-live-multiplayer-concepts"></a>Conceitos de vários jogadores Xbox Live

Este tópico discute um número de termos com vários participantes importantes e conceitos que são usados com frequência na documentação do Xbox Live. Ter uma boa compreensão desses conceitos ajudará você a entender como funciona a Xbox Live para múltiplos jogadores.

## <a name="multiplayer-session"></a>Sessão para múltiplos jogadores

Uma sessão para múltiplos jogadores representa um grupo de usuários do Xbox Live e propriedades associadas a eles. A sessão é criada e mantida pelo títulos e é representado como um documento JSON seguro que reside na nuvem Xbox Live. O documento de sessão em si contém informações sobre os usuários do Xbox Live que estão conectados à sessão, quantos pontos de acesso estão disponíveis e personalizados de metadados (para a sessão, bem como para cada membro de sessão) e outras informações relacionadas à sessão de jogo.

Cada sessão se baseia em um modelo de sessão, que são definidos pelo desenvolvedor de jogos e são configurados na configuração do serviço Xbox Live para uma instância de título.

Enquanto um título pode criar e atualizar uma sessão, ele não é possível excluir diretamente uma sessão.  Em vez disso, depois que todos os jogadores forem removidos de uma sessão, o serviço Xbox Live para vários jogadores excluirá automaticamente a sessão após um tempo limite especificado. Para obter informações detalhadas sobre as sessões, consulte [detalhes da sessão MPSD](multiplayer-appendix/mpsd-session-details.md).

Títulos podem optar por usar várias sessões, mas uma implementação típica de vários jogadores usará duas sessões:

* Lobby sessão - essa é uma sessão que representa um grupo de amigos que deseja manter juntos em várias rodadas, níveis, mapas, etc., do jogo.
* Sessão de jogo - Isso é uma sessão que representa as pessoas que estão executando em uma instância de sessão específica de um jogo, como um round correspondência, nível, etc. Esta sessão pode incluir membros de várias sessões de lobby que ingressaram a instância de sessão juntos, normalmente por meio de um serviço de cruzamento.

Aqui está um exemplo de cenário: Sally quer jogar alguns multiplayer com seus amigos, John e Lisa. Sally é iniciado um jogo e convida José e Lisa em seu jogo. Depois que eles se associar, Sally, John e Lisa estão todos em uma sessão de corredor. Nesta sessão, eles decidem reproduzir em uma correspondência on-line com outras pessoas. O jogo cria uma sessão de jogo e usa o serviço de cruzamento Xbox Live para preencher os slots de player restantes com outros jogadores do Xbox Live.

Digamos que Bob e Joe são correspondidos com eles e cinco deles reproduzir o arredondamento em conjunto. Após o término de arredondamento, Sally, John e Lisa deixe a sessão de jogo, mas são ainda juntos na sessão lobby (sem Bob e Joe) e podem optar por executar outra round ou alternar para modo de jogo diferente.

### <a name="session-member"></a>Membro de sessão

Um membro de sessão é um usuário do Xbox Live que faz parte de uma sessão.

### <a name="arbiter"></a>Arbiter

Um arbitrador é um console ou um dispositivo que gerencia o estado da sessão para um jogo. Por exemplo, o arbitrador seria responsável por uma sessão de jogo para o emparelhamento de publicidade para encontrar mais jogadores.

O arbitrador é definido pelo título. Ele pode ser o mesmo que o host do jogo, mas não precisa ser o mesmo.

### <a name="session-host"></a>Host de sessão

O host da sessão é o console ou dispositivo que executa o jogo executar a simulação para títulos criada em uma arquitetura de rede de ponto a ponto baseada em host. Esse console ou o dispositivo é normalmente o mesmo que o arbitrador, mas ele não precisa ser o mesmo.

## <a name="multiplayer-service-session-directory"></a>Diretório de sessões de serviços para múltiplos jogadores

O serviço Xbox Live Multiplayer opera na nuvem Xbox Live e centraliza os metadados do sistema com vários participantes de um jogo em vários clientes. O sistema que acompanha este metadados é conhecido como o diretório de sessão com vários participantes, ou MPSD de forma abreviada. Você pode pensar MPSD como uma biblioteca de sessões ativas de jogos. Seu jogo pode adicionar, procurar, modificar ou remover as sessões ativas relacionadas ao seu título. O MPSD também gerencia o estado de sessão e atualiza as sessões quando necessário.

MPSD permite que os títulos compartilhar as informações básicas necessárias para se conectar a um grupo de usuários. Isso garante que a funcionalidade de sessão é sincronizado e consistente. Ele coordena com o shell e o sistema de operacional do console Xbox One no envio/aceitando convites e no que estão sendo unidas por meio de cartão de jogador.

### <a name="session-handles"></a>Identificadores de sessão

Uma sessão é identificada exclusivamente no MPSD por uma combinação de partes de dados:

* A configuração do serviço (SCID) ID do título
* O nome do modelo de sessão que foi usado para criar a sessão
* O nome da sessão

Um identificador de sessão é um objeto JSON que contém uma referência a uma sessão específica que existe em MPSD. Identificadores de sessão permitem que os membros Xbox Live ingressar em sessões existentes.

Cada identificador de sessão inclui um guid que identifica exclusivamente o identificador, que permite que os títulos fazer referência a sessão por meio de um único guid.

Há vários tipos de identificadores de sessão:

* Identificador de convidar
* Identificador de pesquisa
* Identificador de atividade
* Identificador de correlação
* Identificador de transferência

#### <a name="invite-handle"></a>Identificador de convidar

Um identificador de convite é passado para um membro quando eles são convidados a participar de um jogo. O identificador de convite contém informações que permitem que o ingresso de jogo do membro convidado a sessão correta.

#### <a name="search-handle"></a>Identificador de pesquisa

Um identificador de pesquisa inclui metadados adicionais sobre a sessão e permite que os títulos pesquisar as sessões que atendem aos critérios selecionados.

#### <a name="activity-handle"></a>Identificador de atividade

Um identificador de atividade permite que membros de ver o que estão executando outros membros em sua rede social e podem ser usados participar de jogo de um amigo.

#### <a name="correlation-handle"></a>Identificador de correlação

Um identificador de correlação efetivamente funciona como um alias para uma sessão, permitindo que um jogo para se referir a uma sessão usando apenas a id do identificador de correlação.

### <a name="transfer-handle"></a>Identificador de transferência

Um identificador de transferência é usado para mover os jogadores de uma sessão para outra sessão.

### <a name="invites"></a>Convites

Xbox Live fornece um sistema de convite é compatível com o serviço com vários participantes. Ele permite que os jogadores convidar outros jogadores suas sessões do jogos. Players convidados recebem um convite para jogo e um título usa essas informações para ingressar em uma sessão existente e a experiência com vários participantes. Títulos de controlam o fluxo de convite e quando os convites podem ser enviadas.

Convites podem ser enviados por meio do shell pelo usuário ou diretamente do título. O texto de notificação para um convite pode ser definido dinamicamente por um título para fornecer mais informações para o player de convidado. Convites do também podem incluir dados adicionais para o título que não é visível para o player e podem ser usados para transmitir informações adicionais.

### <a name="join-in-progress"></a>Junção em andamento

Além de convites, Xbox Live também fornece uma opção com shell para players ingressar em uma sessão de ativos de jogo de amigos ou outros jogadores conhecidos. Isso permite que o outro caminho em uma sessão ativa do jogo e também é orientado pelo MPSD. Títulos de controlam quando as sessões são permite junções e qual sessão expor para associação em andamento.

### <a name="protocol-activation"></a>Ativação de protocolos

Se Sally envia um convite para Lisa para ingressar seu jogo, Lisa recebe uma notificação em seu dispositivo que ela pode optar por aceitar ou recusar.

Se ela aceita o convite, o sistema operacional tenta iniciar o jogo, se o jogo não já está em execução e aciona um evento de ativação que contém informações sobre o motivo pelo qual o jogo foi ativado e qualquer detalhe adicional (no caso de um convite, por exemplo, o detalhes incluem a ID do player de convidado, bem como a sessão que foi convidado para o membro).

O processo de lidar com esse evento é conhecido como a ativação de protocolo e indica que o jogo automaticamente deve ir para um estado específico, que é detalhado nos argumentos de evento de ativação. Se o membro estiver ingressando em um jogo, a id do identificador de sessão é especificada como um dos argumentos.

No caso de Lisa, aceitar o convite deve automaticamente inicia o jogo (se necessário) e ingressar à mesma sessão jogo como Sally, sem necessidade de realizar uma ação adicional de Lisa.

Podem de ativação de protocolo ser disparada por aceitar um convite, ingressar em outro membro do jogador por meio de seu cartão de perfil, ou clicando em um profundo vinculado conquista.

## <a name="smartmatch-matchmaking"></a>Para que haja correspondência SmartMatch

SmartMatch é o nome do serviço Xbox Live para emparelhamento anônimo. Esse serviço corresponde players do mesmo jogo com base em configuráveis a um conjunto de regras de correspondência.

O serviço de cruzamento trabalha em conjunto com o MPSD e usa sessões de emparelhamento de entrada e saída. Para que haja correspondência é executada no serviço, que permite que os títulos de fornecer facilmente outras experiências durante o fluxo de relacionamento de pessoas, por exemplo jogador dentro do título.

Indivíduos ou grupos que deseja inserir o cruzamento criar uma sessão de tíquete de correspondência e, em seguida, solicitar o serviço de cruzamento para localizar outros jogadores com quem configurar uma correspondência. Isso resulta na criação de um temporário "correspondência tíquete" que reside dentro do serviço para que haja correspondência (em hopper uma correspondência) para um período de tempo.

O serviço de cruzamento escolhe sessões para reproduzir em conjunto com base na configuração de regra, as estatísticas armazenadas para cada jogador e qualquer informação adicional fornecido no momento da solicitação de correspondência. O serviço, em seguida, cria uma sessão de destino de correspondência que contém todos os jogadores que tem sido correspondidos e notifica os títulos dos usuários da correspondência.

Quando a sessão de destino estiver pronta, títulos podem executar a qualidade das verificações de serviço (QoS) para confirmar que o grupo pode funcionam juntas, e/ou os jogadores Junte-se à sessão para iniciar o jogo. Durante a QoS processo e matchmade jogabilidade, títulos de manter o estado de sessão atualizados dentro de MPSD e recebem notificações de MPSD sobre alterações à sessão. Essas alterações incluem usuários ingressar ou sair e muda para o arbitrador de sessão.

### <a name="match-ticket-session"></a>Sessão de tíquete de correspondência

Uma sessão de tíquete de correspondência representa os clientes para os jogadores que desejam fazer uma correspondência. Ele geralmente é criado com base em um grupo de jogadores que estão em um corredor juntos ou em outros grupos específicos de título de jogadores. Em alguns casos, a sessão de tíquete pode ser uma sessão já está em andamento que está procurando mais jogadores de jogo.

### <a name="match-ticket"></a>Tíquete de correspondência

Enviar uma sessão de tíquete para emparelhamento resulta na criação de um tíquete de correspondência que controla a tentativa de emparelhamento. Atributos podem ser adicionados ao tíquete, por exemplo, o mapa de jogo ou o nível de player. Eles, juntamente com os atributos dos jogadores na sessão de tíquete, são usados para determinar a correspondência.

### <a name="hoppers"></a>Hoppers

Hoppers são lógicas locais onde os tíquetes de correspondência são coletados e especificados no início do relacionamento de pessoas. Só os tíquetes dentro do mesmo hopper podem ser correspondidos. Um título pode ter vários hoppers, mas só pode iniciar um relacionamento de pessoas em um de cada vez. Por exemplo, um título pode criar um hopper para qual player habilidade é o item mais importante para correspondência. Ele pode usar outro hopper em que os jogadores são correspondidos somente se tiver adquirido o mesmo conteúdo que pode ser baixado.

Você pode configurar hoppers para emparelhamento na configuração do serviço. TBD.

## <a name="quality-of-service-qos"></a>Qualidade de serviço (QoS)

Quando os jogadores joguem um jogo online, a qualidade do jogo é afetada pela qualidade da comunicação de rede entre os dispositivos que estão hospedando os jogos. Uma rede ruim pode resultar em experiências de jogos indesejáveis, como latência ou a conexão cair devido à largura de banda insuficiente ou a latência.

QoS refere-se para avaliar a força de uma conexão online (largura de banda e latência) entre os participantes para garantir que todos os jogadores possuem uma qualidade de conexão de rede suficiente. Isso é especificamente importante para os jogadores que são correspondidos durante o relacionamento de pessoas para garantir uma boa experiência devido a net. Ele é aplicável menos convites onde amigos funcionam juntas e são geralmente disposto a aceitar as consequências de uma conexão ruim.

Você pode configurar a sessão para lidar com QoS automaticamente com base em critérios específicos, ou seu jogo pode manipular medindo o QoS sempre que qualquer pessoa que une a sessão.
