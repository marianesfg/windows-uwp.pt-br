---
title: Projetando experiências do Xbox Live
description: Saiba como criar excelentes experiências para membros do Xbox Live pelo planejamento de estatísticas de player, placares de líderes e conquistas do título.
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, estatísticas, conquistas, placares de líderes e design
ms.localizationpriority: medium
ms.openlocfilehash: 0f080593727ec6d7ddd529b2ce976708174250ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600781"
---
# <a name="designing-xbox-live-experiences"></a>Projetando experiências do Xbox Live

Este tópico orienta você por meio de um processo de amostra de pensar e adicionando os placares de líderes, conquistas e estatísticas do usuário para seu jogo usando os recursos de plataforma de dados do Xbox Live.

## <a name="example"></a>Exemplo
Vamos configurar um jogo fictício que mostra como a plataforma de dados pode ajudar você iluminar experiências incríveis do Xbox Live no painel do Xbox One, o aplicativo do Xbox no Windows 10 e no seu jogo. Para este cenário, trabalharemos com um imaginário racing jogo chamado _corridas 2015_.

Para obter o máximo de valor dessas experiências, vamos seguir estas recomendadas fases de planejamento:
1. Projetar suas experiências XBL
2. As estatísticas necessárias para potencializar seus cenários de design
3. Configure os placares de líderes, conquistas ou estatísticas em destaque conforme necessário


## <a name="1-design-your-xbox-live-experiences"></a>1. Design de que experiências do Xbox Live
Queremos _corrida 2015_ para obter o máximo proveito do Xbox Live para manter os usuários envolvidos dentro e fora do jogo. As primeira perguntas a serem feitas são:

1. O que fazer queremos que a experiência de guia do nosso GameHub realizações como? (Estatísticas em destaque e placares de líderes Social)
2. Quais metas e as motivações queremos dar aos jogadores a fazer no jogo? (Conquistas)
3. Quais as pontuações ou estatísticas, queremos que os jogadores a usar para classificar em si contra outros jogadores no jogo? (Placares de líderes)


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>GameHubs - social placares de líderes e estatísticas em destaque
GameHubs estiver descarregando experiências em que os usuários possam saber tudo sobre um jogo. Como um desenvolvedor e/ou editor, isso é o lugar perfeito para você _showcase_ como excelente e rica seu jogo é para os usuários do Xbox que não tiverem comprado o jogo ainda. GameHubs também são projetados para mostrar métricas envolventes e progresso aos jogadores que já possuem o título. Sob a guia conquistas, os usuários podem encontrar o andamento do jogo e conquistas, Hero estatísticas e placares de líderes social.

Objetivo desta seção é capturar as métricas mais atraentes para o jogo e expô-los para fornecer uma imagem exclusiva da experiência do jogadores com o jogo e classificá-las em relação a seus amigos em um placar de líderes social. Por exemplo, na guia Forza 6 medalha poderia ser semelhante a:

![Imagem de Gamehub](../images/omega/forza_gamehub.png)


Você perceberá que algumas das estatísticas, como o controlado por milhas e primeiro lugar conclui as decorações de bronze, Prata ou ouro que ilustram onde o jogador classifica em relação a seus amigos. Interagir com qualquer uma dessas estatísticas mostrará um placar de líderes completa como este:

![Placar de líderes](../images/omega/progress_gamehub_lb.png)

 Para _corrida 2015_, acreditamos que o seguinte é um bom conjunto de estatísticas que representam a experiência de um jogador com o título, bem como boa métricas de classificação:
 * Obter uma visão mais rápida
 * Wins mais 1º de local
 * Milhas controlado por


### <a name="achievements"></a>Conquistas
Realizações são um mecanismo de todo o sistema para dirigir e recompensando ações de jogos dos usuários de maneira consistente em todos os jogos. Projetar isso corretamente ajuda a orientar os usuários para experimentar o jogo com o seu limite e estende o tempo de vida do jogo.

Para _corridas 2015_, aqui está um subconjunto das realizações que queremos configurar:
* Concluir 1 lap em menos de 60 segundos
* Concluir 10 corridas em 1º lugar
* Conclua as faixas de corrida pelo menos 1 em cada
* Próprios 5 carros
* Unidade de 10.000 milhas
* ...


###  <a name="leaderboards"></a>Placares de líderes
Placar de líderes fornece uma maneira para jogadores classificar em si contra outros jogadores para ações específicas que eles podem obter no jogo. Além dos placares de líderes social no GameHubs, podemos configurar placares de líderes globais a ser usado no jogo. Estes são alguns dos placares de líderes que queremos classificar todos os nossos usuários em:

* Tempo de visão geral mais rápido
 * Metadados: em que isso aconteceu de faixa
 * Metadados: usado de carro
 * Metadados: condições de tempo
* Wins mais 1º de local
* A maioria dos carros coletados

## <a name="next-steps"></a>Próximas etapas
Agora que você sabe como criar efetivamente estatísticas, placares de líderes e conquistas, você agora pode começar a começar a implementar em seu título.  As próximas seções descreve o processo de ponta a ponta, começando com a configuração no Partner Center.

Consulte [estatísticas de Player](../leaderboards-and-stats-2017/player-stats.md)
