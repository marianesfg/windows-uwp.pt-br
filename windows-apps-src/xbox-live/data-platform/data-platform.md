---
title: Plataforma de dados dinâmicos do Xbox
description: Visão geral do Xbox Live Data Platform, que consiste em serviços para gerenciar as conquistas, estatísticas de player e placares de líderes.
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, estatísticas, conquistas, placares de líderes, plataforma de dados
ms.localizationpriority: medium
ms.openlocfilehash: f9a14c508e265cdfc510896eb621466d03c66776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634831"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Placar de líderes de plataforma – estatísticas, de dados, conquistas do Xbox Live

Gravar dados de jogos para a plataforma de dados do Xbox Live permite que o título a ser executado como um serviço. Além disso, a plataforma de dados do Xbox Live unidades engajamento do usuário com o seu título usando estatísticas, placares de líderes e conquistas e superfícies de estatísticas no shell do console e Xbox App em destaque.

Um exemplo de uma estatística para um jogo de corrida pode ser que as total corridas executar no modo de faixa de arrastar, que pode ser usado para criar um placar de líderes para comparar sua pontuação contra a comunidade e pode ser refletido em sua cadeia de caracteres de presença avançada em tempo real (por exemplo , "GoTeamEmily está reproduzindo modo arrastar Strip: 523 corridas concluídas"). Corridas total no modo de faixa de arrasto também pode ser usadas como um critério para o emparelhamento, garantindo seu players com experiência semelhante têm maior probabilidade da ser correspondido em conjunto.

O título pode exibir o placar de líderes com base nas estatísticas de player. Por exemplo, o placar de líderes pode ser uma classificação global de corridas concluída. Você chamar esses serviços usando as APIs do Xbox Live diretamente ou wrappers em um mecanismo de jogo, como o Unity.

Você pode designar determinados stats estatísticas como em destaque. Estatísticas em destaque são mostradas na GameHub do seu título e comparam os jogadores em relação a seus amigos.

Realizações não são ativadas pelas estatísticas e seu título decide quando uma realização está desbloqueada. Por exemplo, o título pode ter uma medalha para a conclusão de uma corrida em menos de um minuto. O título da controla os parâmetros necessários para desbloquear a realização de. Neste exemplo, seria até seu título para medir quanto a corrida levou e, em seguida, a realização de prêmio. Normalmente, jogos recebeu o prêmio, juntamente com a conclusão da realizações. Cabe a você decidir a quantidade de jogos para cada conquista.

## <a name="features"></a>Recursos ##
Os tópicos a seguir fornecem mais informações sobre os recursos de plataforma de dados do Xbox Live:

* [Estatísticas de Player](../leaderboards-and-stats-2017/player-stats.md)
* [Realizações](../achievements-2017/achievements.md)
* [Placares](../leaderboards-and-stats-2017/leaderboards.md)
