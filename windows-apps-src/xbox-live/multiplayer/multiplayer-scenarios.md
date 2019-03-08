---
title: Cenários com vários participantes
description: Saiba mais sobre os diferentes cenários com vários participantes e como o Xbox Live dá suporte a esses cenários.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 338c6e65846ca0ce1307e1e5843f37fe63ecf70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618291"
---
# <a name="multiplayer-scenarios"></a>Cenários com vários participantes
Há muitos tipos diferentes de cenários com vários jogadores e escolher o cenário certo pode aumentar o envolvimento do jogador e o player de base para o seu jogo, que por sua vez ajuda a estender a vida Active Directory de seu jogo para o maior tempo possível.

Até mesmo jogos são principalmente as experiências de player único podem aproveitar recursos competitivos Xbox Live, como placares de líderes, estatísticas ou elementos Social.

A lista a seguir descreve alguns dos cenários mais comuns com vários participantes que você pode implementar com o Xbox Live. A lista é ordenada em termos de complexidade e a quantidade de trabalho necessária para implementar e testar, e você deve considerar estes fatores ao decidir sobre cenários de tipo que você deseja que seu jogo para dar suporte.

## <a name="comparative-indirect-play"></a>Comparativa Play (indireto)
Nesse cenário, os jogadores estão competindo entre si indiretamente, sem jogabilidade direta na mesma instância de um jogo. Ele é adequado para jogos que são orientados para uma experiência de jogador, mas contêm alguns aspectos de concorrência que permitem que os jogadores comparar como eles fizeram a outros jogadores.

A funcionalidade de exemplo para este cenário inclui:

* Placar de líderes, em que um jogador tenta obter a melhor "pontuação" em uma categoria em relação a outras partes envolvidas. Isso pode incluir um sistema de placar de líderes social para competir com amigos apenas.
* Realizações e estatísticas, em que um jogador deseja ser capaz de comparar seu progresso e desempenho e de seus amigos e possivelmente gabarmos bater uma medalha particularmente desafiador.
* "Espelhamento de em" ou "virtual com vários participantes", em que o jogador pode competir com uma gravação de outro do jogador (ou seus próprios) desempenho anterior, como uma visão geral em um jogo de corrida.

Esse tipo de vários jogadores pode ser obtido usando os seguintes serviços do Xbox Live:

* Presença
* Estatísticas
* Gerenciador de social
* Conquistas
* Placares de líderes
* Armazenamento conectado

Esse tipo de vários jogadores não exige qualquer um dos serviços específicos do Xbox Live para múltiplos jogadores. Testar este cenário requer contas do Xbox Live vários apenas.

## <a name="local-play-living-room-play"></a>Local Play (reproduzir sala)
Esse tipo de com vários participantes se baseia em dois ou mais players jogar um jogo juntos (versus uns aos outros, ou cooperativamente) em um único dispositivo. Um título pode usar uma única tela para todos os jogadores ou uma experiência de tela de divisão para cada jogador. Como alternativa, em vez com base em jogos, você pode usar uma abordagem de "estação quente" em que cada jogador assume o controle do jogo durante sua vez.

No console do Xbox One vários jogadores podem entrar em um único console. Cada jogador está vinculado a um controlador. Atualmente, os dispositivos Windows 10 somente dão suporte a entrada de uma única conta do Xbox Live, mas a Microsoft está investigando a essa alteração em uma atualização futura.

Embora seja possível criar um estilo somente de execução local de vários jogadores através dos serviços do Xbox Live para múltiplos jogadores, talvez seja melhor considerar esse cenário como um subconjunto de um cenário com vários participantes mais expansivo que incorpora as experiências online, como o adicionais investimento necessário é mínimo em comparação com o retorno potencial de expandir o cenário com vários participantes.

Esse tipo de vários jogadores pode usar serviços semelhantes do cenário anterior:

* Presença
* Estatísticas
* Gerenciador de social
* Conquistas
* Placares de líderes
* Armazenamento conectado

Testar este cenário requer várias contas do Xbox Live e vários controladores em um único dispositivo.

##  <a name="online-play-with-friends"></a>Reprodução online com amigos
Esse cenário é a experiência de vários jogadores online mais tradicional. Nesse cenário, um membro do Xbox Live quer jogar um jogo com amigos apenas, mas não quer jogar com estranhos. Amigos ou são convidados ou ingressar em um jogo em andamento porque ela está em andamento.

Para o controle dos pais um título para múltiplos jogadores mais amplo (como mostrado posteriormente) deve ter a capacidade de limitar multiplayer online a somente os amigos e fazer fallback para esse cenário. Controles dos pais que limitam as interações com estranhos também são aplicados por meio do serviço Xbox Live.

Esse tipo de vários jogadores pode ser obtido usando os seguintes serviços do Xbox Live:

* Gerenciador de vários jogadores
* Presença
* Estatísticas
* Gerenciador de social

Testar este cenário requer várias contas do Xbox Live e vários dispositivos.

## <a name="online-play-through-a-list-of-game-sessions"></a>Reprodução online por meio de uma lista de sessões de jogos
Nesse cenário, um jogador pode procurar uma lista de sessões de jogo permite junções em um jogo e, em seguida, selecione qual para ingressar. O reprodutor também tem a capacidade de criar instâncias de nova sessão de jogos ao hospedar um jogo localmente. Essas instâncias de jogos podem permitir que as preferências de personalizado (como o modo de jogo, nível ou regras de jogos). Dependendo do design do título, sessões de jogos podem dar suporte a restrições, como exigir uma senha para determinados níveis de habilidade/player ou de junção. Essas instâncias de sessão de jogo também podem ser totalmente público ou oculto, dependendo de como seu jogo implementa a sessão de navegação e junção.

Esse cenário permite que os jogadores selecionar automaticamente uma sessão de jogo. Isso oferece um controle para o jogador, mas não há nenhuma garantia de que uma sessão fornecerá uma boa experiência. Uma sessão não pode ser preenchida com o player correto definido com um nível de habilidade interessantes. Uma lista de sessão é mais eficaz quando jogos tem uma pequena comunidade com vários participantes ativa.

Esse tipo de vários jogadores pode usar serviços semelhantes do cenário anterior:

* Gerenciador de vários jogadores
* [Navegação de sessão](session-browse.md)
* Presença
* Estatísticas
* Gerenciador de social

Testar este cenário requer um grande conjunto de contas do Xbox Live e dispositivos para testar com precisão. Os desenvolvedores devem observar que dynamics player true para listas de sessão só pode ser testado com o player grandes bases.

## <a name="online-play-though-looking-for-group"></a>Jogo online, porém procurando por grupo
Esse cenário é semelhante ao cenário de lista de sessão, no entanto, ele divergirá em pontos importantes. Em vez de uma lista de jogo no título a plataforma fornece funcionalidade para a lista de sessões de jogos fora o jogo. Esses anúncios de aparência para o grupo têm como objetivo fornecer uma experiência mais social e incluir jogabilidade, habilidades e restrições de relação de redes sociais. Isso permite que um jogo fornecer uma experiência aprimorada ao longo da sessão de lista e também fornece o criador da sessão mais controle.

O player que criou a sessão "atraente para o grupo" pode aprovar ou negar solicitações para ingressarem em seu grupo de outros jogadores. Isso permite que os jogadores possam localizar outros jogadores que compartilham suas preferências de playstyle.

O serviço Xbox Live LFG é novo para 2016 e as APIs de visualização devem estar disponíveis para títulos em maio.

Esse tipo de vários jogadores pode usar serviços semelhantes do cenário anterior:

* Gerenciador de vários jogadores
* Procurando por grupo do Xbox
* Presença
* Estatísticas
* Gerenciador de social
* Serviço LFG

Testar este cenário requer um grande conjunto de dispositivos para testar e contas do Xbox Live.

## <a name="simple-matchmaking"></a>Para que haja correspondência Simple
Nesse cenário, um player (ou grupo de jogadores) está procurando por outros jogadores (que podem ou não ser conhecidos para o player) para um jogo online. Em vez de selecionar amigos, o jogo fornece um fluxo para que haja correspondência simples que permite que os players para ingressar em sessões de jogos com outros jogadores. O fluxo do emparelhamento neste cenário é simple: players pesquisar e localizar outros jogadores sem qualquer configuração de emparelhamento significativa. Esse cenário funciona melhor com um público maior de online.

Para corresponder os jogadores juntos adequadamente, qualquer relacionamento de pessoas deve tentar honrar reputação e bloquear listas no Xbox Live. O serviço Xbox Live SmartMatch lida automaticamente com essas restrições. Respeitar as restrições, como esses ajuda a garantir a mais segura e melhor experiência para os jogadores.

Uma parte importante de emparelhamento são verificações de rede de qualidade de serviço (QoS). Essas verificações Certifique-se de que a conectividade de rede entre dois jogadores é suficiente para uma experiência de jogo bom. Isso é diferente em Reproduzir Online com o cenário de amigos, pois é possível repetir o relacionamento de pessoas e localizar outros jogadores em caso de uma conexão de rede incorreta.

Esse tipo de vários jogadores pode usar serviços semelhantes do cenário anterior:

* Gerenciador de vários jogadores
* Presença
* Estatísticas
* Gerenciador de social
* Para que haja correspondência SmartMatch

Testar este cenário requer um grande conjunto de contas do Xbox Live e dispositivos para testar com precisão. Os desenvolvedores devem observar que dynamics player true para listas de sessão só pode ser testado com o player grandes bases. Ferramentas estão disponíveis para simplificar a condição de rede e SmartMatch testes.

## <a name="skill-based-matchmaking"></a>Para que haja correspondência baseado em habilidades
Relacionamento de pessoas em habilidade é um refinamento do cenário para que haja correspondência simples. Nesse cenário, que o serviço de cruzamento inclui mais avançadas conjuntos de regras, como habilidade, nível do player e outras propriedades específicas de jogo. O serviço de cruzamento usa regras que usam essas corresponder os parâmetros de encontrar uma sessão de mais alta qualidade para o player. Dependendo do projeto de jogo parâmetros de correspondência são configurados diretamente pelo player ou são definidos automaticamente pelo título.

Xbox Live SmartMatch fornece um conjunto de regras que podem ser usados para emparelhamento baseado em habilidades. O serviço de SmartMatch usa um recurso do Xbox Live chamado TrueSkill para ajudar a avaliar a habilidade relativa de um player.

Esse tipo de vários jogadores pode usar serviços semelhantes do cenário anterior:

* Gerenciador de vários jogadores
* Presença
* Estatísticas
* Gerenciador de social
* Para que haja correspondência SmartMatch

Testar este cenário requer um grande conjunto de contas do Xbox Live e dispositivos. Os desenvolvedores devem observar que dynamics player true para listas de sessão só pode ser testado com o player grandes bases. As ferramentas estão disponíveis para simplificar a condição de rede, TrueSkill e SmartMatch testes.

## <a name="tournaments"></a>Torneios
Torneios permitem que os jogadores capacitados competir uns com os outros em um formato organizado. Normalmente, torneios são hospedados e organizados por organizações terceirizadas independentes.

Xbox Live, você adicionou um novo recurso em 2016 para permitir que os títulos usar o Xbox Live para que haja correspondência em conjunto com organizadores de torneio de terceiros aprovados. Esse recurso permite que os títulos de integrar perfeitamente torneios com a experiência de shell do Xbox, sem a necessidade de participantes para se registrar nos sites externos.

Esse tipo de vários jogadores pode ser obtido usando os serviços do Xbox Live, como:

* Presença
* Estatísticas
* Redes sociais
* Reputação
* Multijogadores
* Para que haja correspondência SmartMatch
* Torneios

Testar este cenário requer um grande conjunto de contas do Xbox Live e dispositivos.
