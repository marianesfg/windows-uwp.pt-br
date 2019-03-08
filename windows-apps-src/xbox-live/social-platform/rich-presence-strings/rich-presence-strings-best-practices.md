---
title: Práticas recomendadas de presença avançada
description: Conheça as práticas recomendadas para usar a presença de Rich do Xbox Live.
ms.assetid: 51a84137-37e4-4f98-b3d3-5ae70e27753d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, recursos avançados de presença, práticas recomendadas
ms.localizationpriority: medium
ms.openlocfilehash: 75268575dd9dce59141d8909a59909bc973edbec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615661"
---
# <a name="rich-presence-best-practices"></a>Práticas recomendadas de presença avançada

As dicas a seguir ajudará você a obter o máximo de presença avançada em seu jogo. Lembre-se de que a presença avançada mais cadeias de caracteres você definir, quanto mais avançado a experiência de outros jogadores que descobrem as pessoas o jogo.

-   Use suas estatísticas em cadeias de caracteres, para que você possa definir sua cadeia de caracteres e, em seguida, não se preocupe.

    Se sua cadeia de caracteres tem o nome do mapa nele e você estiver usando o stat CurrentMap para preencher o espaço em branco, em seguida, o serviço atualizará sua cadeia de caracteres apropriadamente, conforme os jogadores viajam de um mapa para o mapa no jogo. Essa abordagem permite que você não preocupar em manter a cadeia de caracteres atualizado, desde que o título está enviando os eventos adequados para a plataforma de dados.

    O título deve definir a cadeia de caracteres de base de presença avançada com o serviço de presença periodicamente, para garantir que as informações de presença avançada para um usuário sejam precisas e o serviço está usando a cadeia de caracteres de base correta.

-   Use a presença avançada para abrir novas oportunidades de conversa. Crie cadeias de caracteres que tendem a gerar interesse em um jogo para novos jogadores, bem como os jogadores casuais, que podem ter perdido a um recurso especial.

-   Crie cadeias de caracteres de presença avançada que motivam players para agir. Por exemplo, em vez de dizer "Reprodução no Mausoleum," Diga, "solicitar assistência; Defender Mausoleum." Use o estado de presença avançada para habilitar cenários interessantes, como ingressar em um jogo em andamento. Em seguida, outro jogador pode participe e ajude.

-   Crie cadeias de caracteres de presença avançada que capacitam os jogadores para mostrar suas realizações, como a conclusão dos níveis ou detecção de áreas de segredo.

-   Localize suas cadeias de caracteres de presença avançada e seus parâmetros associados, para que os jogadores do Xbox em todo o mundo podem fazer parte da comunidade que promovendo a você.

-   Alguns parâmetros alterados rapidamente, mas pode levar tempo para novos dados de presença avançada sejam exibidos para um amigo. Se sua cadeia de caracteres contém "arma atual" e o jogador tem a capacidade de alternar entre seus pistol e rifle, sua cadeia de caracteres de presença avançada pode não ser completamente precisa em um determinado momento. No entanto, em alguns casos, isso não é um problema. Se sua cadeia de caracteres de presença avançada contém o valor de total inimigos pôs fim e o valor está desativado por 1 ou 2 por alguns segundos, que pode ser okey para seu cenário.
