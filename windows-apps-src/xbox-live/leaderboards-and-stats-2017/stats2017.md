---
title: Estatísticas dos jogadores
description: Introdução ao 2017 de estatísticas
ms.date: 07/02/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, estatísticas de player, placares de líderes, estatísticas de 2017
ms.localizationpriority: medium
ms.openlocfilehash: eabcc655afa5a9d58ca8ef45569d099e503ebc32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650861"
---
# <a name="stats-2017"></a>Estatísticas de 2017

Estatísticas 2017 permite que os desenvolvedores configurem as estatísticas de jogadores individuais indicando o progresso e habilidade no uso do jogo. Essas estatísticas são uma ferramenta social que permitirá que os jogadores seja mais competitiva com seus amigos e o título maior da comunidade, bem como mostrando alguns dos recursos e desafios que seu título tem a oferecer. Se você tiver um jogo de corrida com estatísticas em destaque, como descompasso mais longo e o melhor tempo de travamento, você pode se comunicar o tipo de jogo de corrida que os jogadores podem esperar. Vendo como pilha de jogadores em relação a seus amigos e a comunidade dará a elas mais incentivo para comprar e executar seu título. Os jogadores verá estatísticas em destaque no GameHub do título. Estatísticas em destaque periodicamente também podem aparecer em blocos fixados de conteúdo que os usuários podem adicionar ao modo de exibição inicial.

Estatísticas de 2017 opera aceitando stat valores como pares chave-valor do seu título de um player e armazenar esse valor stat para que ele pode ser exibido em um momento posterior. Estatísticas de 2017 destina-se para dar suporte a cenários de placar de líderes Xbox Live, salvando estatísticas sobre a jogadores individuais para que possam ser comparados e classificados em um placar de líderes mais tarde. Estatísticas de 2017 aceita valores enviados a ele com pouca ou nenhuma validação, então, cabe em seu título para lidar com toda a lógica que determina o valor correto para um estado.

## <a name="configured-stats-and-featured-leaderboards"></a>Estatísticas configuradas e placares de líderes em destaque

Estatísticas são configuradas na [painel da Central de desenvolvimento do Windows](https://developer.microsoft.com/en-us/dashboard/windows/overview). Para configurar as estatísticas que você já deve ter um título configurado. Se você não tiver um título configurado você pode aprender como fazer isso lendo [criar e testar o título de um novo criador](../get-started-with-creators/create-and-test-a-new-creators-title.md).  As estatísticas que você configurar no Partner Center aparecerão em GameHub do seu título conforme a Figura:

O *recurso de estatísticas* são realçados em amarelo na imagem abaixo.
![Placar de líderes oficial clube Social de página](../images/omega/gamehub_featuredstats.png)


No Xbox One, os usuários conseguem pin jogos diretamente à sua tela inicial para localizar rapidamente informações relevantes, conteúdo controlado pela comunidade e postagens de desenvolvedor. As estatísticas que você configure também podem aparecer como parte do conteúdo de Home fixado. Estatísticas mostram o jogador, juntamente com um amigo com classificação um pouco mais alto, encorajando-os para reproduzir seu caminho ao subir placar de líderes. Placar de líderes no conteúdo fixado apareceria conforme realçado na imagem a seguir.

![O Halo 5 fixado placar de líderes](../images/stats/Halo_5_Pinned_Leaderboard.png)

Comparam estatísticas em destaque em andamento de jogo com base nas estatísticas configuradas com outros amigos que também desempenham o título. Isso incentiva mais tempo de reprodução do título quando amigos competem por pontuação alta ou conectar-se simplesmente via uma experiência compartilhada. Stat placares de líderes em destaque são mensal placares de líderes que são redefinidas no primeiro dia do mês.

Os desenvolvedores são limitados a ter não mais do que 20 estatísticas em destaque para seu título, mas não há nenhum requisito para que os desenvolvedores incluem estatísticas de destaque em seu título.

## <a name="further-reading"></a>Leitura adicional
Saiba como configurar estatísticas com [configuração de estatísticas em destaque e placares de líderes de 2017](../configure-xbl/dev-center/featured-stats-and-leaderboards.md) Saiba como atualizar valores stat com [Atualizando estatísticas de 2017](player-stats-updating.md)