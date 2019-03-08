---
title: Configurar estatísticas e placares de líderes de 2017
description: Saiba como configurar o Xbox Live em destaque Stats e placares de líderes no Partner Center com o 2017 da plataforma de dados.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598851"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>Configuração de estatísticas em destaque ou placares de líderes no Partner Center com a plataforma de dados de 2017

Com o 2017 de plataforma de dados, você só precisa configurar um stat nos dois casos:

* A stat é usado como base para um placar de líderes global, ou
* A stat é um stat de player em Destaque que será exibido na página de hub jogos.

Em ambos os casos, você deve configurar stats e placares de líderes em conjunto. Cada placar de líderes baseia-se em um estado, e você não pode configurar um stat sem configurar também um placar de líderes global associado, mesmo se você não planeja usar um placar de líderes de stat um player em destaque.

Você não precisa configurar estatísticas que não são estatísticas de player em destaque e não são usadas por um placar de líderes global.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>Configurar um placar de líderes global e um stat de player associado

Primeiro, vá para a seção de estatísticas de Player para seu título conforme mostrado abaixo.

![](../images/omega/dev_center_player_stats_creators.png)

Em seguida, clique o `Create your first featured stat/leaderboard` botão e, em seguida, preencha esta caixa de diálogo e clique em Salvar.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

O `Display name` campo é o que os usuários verão no GameHub.  Essa cadeia de caracteres pode ser localizada no `Localize strings` seção.  Clique em `Show Options` no `Localize strings` seção para ver detalhes sobre como localizar essas cadeias de caracteres.

O `ID` campo é o nome stat e será como você fará referência a seu stat ao atualizá-lo no seu código do título.   Consulte a [Atualizando estatísticas](player-stats-updating.md) seção abaixo para obter mais detalhes.

O `Format` é o formato de dados de stat.

O `Display Logic` oferece a escolha entre `Player progression`, `Personal best`, e `Counter`:
- Progressão do Player: Representa um jogador individual ou a progressão do jogo.  O último valor definido é o que os usuários verão.  Por exemplo, posicione em turno atual.
- Melhor pessoal: Representa a melhor pontuação atual que postou um player. O valor máximo ou mínimo definido dependendo da ordem de classificação é o que os usuários verão.  Por exemplo, colo mais rápido.
- Contador: Podem ser adicionadas para calcular um número cumulativo de outro jogador.  

O `Sort` campo permite que você altere a ordem de classificação de placar de líderes.

Você também pode optar por tornar esta estatística uma `Featured Stat`, mas clicar em `Feature on players' profiles`.  

## <a name="configure-featured-stats"></a>Configurar as estatísticas em destaque

Quando você define um stat de jogador, você tem a opção de verificação de `Featured Stat`.  Observe os seguintes requisitos

| Tipo de desenvolvedor | Requisito |
|----------------|-------------|
| Programa de Criadores do Xbox Live | Não há nenhum requisito para designar qualquer estatísticas como estatísticas em destaque.  No entanto, você está limitado a um máximo de 10 |
| ID@Xbox e parceiros da Microsoft | Você deve designar entre 3 e 10 estatísticas em destaque |

## <a name="next-steps"></a>Próximas etapas

Em seguida, você precisará atualizar a estatística de código do título.  Ver [Atualizando estatísticas](player-stats-updating.md) para obter mais detalhes.