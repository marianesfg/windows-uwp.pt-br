---
title: Apêndice do sistema Multijogador
description: Fornece links para saber mais sobre o serviço Xbox Live para múltiplos jogadores 2015.
ms.assetid: 412ae5f4-6975-4a8b-9cc2-9747e093ec4d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 703e48c0f200c59fa6f9181905756ef834aabe4a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643381"
---
# <a name="multiplayer-appendix"></a>Apêndice do sistema Multijogador

Permite que o sistema com vários participantes no Xbox One jogo e o assembly de jogadores em grupos. O sistema é seguro, fácil de usar e flexível, permitindo que você não apenas para criar recursos simples rapidamente, mas também compilar recursos mais complexos e conecte seus próprios serviços.

> **Observação**  
Este artigo é para uso avançado de API.  Como ponto de partida, dê uma olhada na [API do Gerenciador de vários jogadores](../multiplayer-manager.md) que simplifica significativamente o desenvolvimento.  Informe sua mãe se você encontrar um cenário sem suporte no Gerenciador de com vários participantes.

A versão atual do sistema com vários participantes é Multiplayer de 2015. Esta versão abstrai o conceito de "entidade de jogo" e usa o diretório de sessão para múltiplos jogadores (MPSD) para controlar sessões de jogos.

> **Observação**  
A versão anterior do sistema com vários participantes é Multiplayer de 2014. Essa versão é baseada no conceito da parte do jogo e participação em jogos por meio de partes. Esta versão foi preterida, embora o código-fonte para que ele ainda é fornecido com o XDK. A documentação para múltiplos jogadores de 2014 não está mais incluída com o XDK. Se você precisar desta documentação, use uma versão 2014 do XDK.


## <a name="in-this-section"></a>Nesta seção

[Introdução](introduction-to-the-multiplayer-system.md)  
Apresenta o sistema com vários participantes.

[Diretório de sessão com vários participantes (mpsd)](multiplayer-session-directory.md)  
Descreve o diretório de sessão para múltiplos jogadores (MPSD), que centraliza os metadados de API com vários participantes de um jogo em todos os títulos e gerencia as sessões de jogos.

[Detalhes da sessão MPSD](mpsd-session-details.md)  
Fornece detalhes de uma sessão MPSD.

[Problemas comuns ao adaptar seus títulos para vários jogadores de 2015](common-issues-when-adapting-multiplayer.md)  
Descreve problemas comuns a serem considerados quando você adaptar títulos para operar com vários participantes de 2015.

[Para que haja correspondência SmartMatch](smartmatch-matchmaking.md)  
Descreve o sistema para que haja correspondência usado pelo serviço Xbox Live.

[Migrando um arbitrador](migrating-an-arbiter.md)  
Descreve o processo MPSD para a migração do arbiter.

[Usando o emparelhamento de SmartMatch](using-smartmatch-matchmaking.md)  
Descreve como usar o emparelhamento de SmartMatch.

[Como com vários participantes do](multiplayer-how-tos.md)  
Fornece procedimentos para usar com vários participantes de 2015 em títulos.

[Códigos de Status de sessão para múltiplos jogadores](multiplayer-session-status-codes.md)  
Define os códigos de status de sessão para múltiplos jogadores do Xbox One.

[Perguntas frequentes para múltiplos jogadores de 2015 e solução de problemas](multiplayer-2015-faq.md)  
Define as perguntas frequentes e solução de problemas para vários jogadores.

[Diretório de sessão para múltiplos jogadores Xbox One](xbox-one-multiplayer-session-directory.md) fornece uma visão geral da criação da sessão para múltiplos jogadores usando o novo serviço Xbox um com vários participantes sessão diretório (MPSD).

[Fluxos de jogo com vários participantes convida](flows-for-multiplayer-game-invites.md) descreve o fluxo de experiências envolvidos em convidar outro jogador para um jogo com vários participantes.

[Sessão de jogo e o jogo de terceiros visibilidade e joinability](game-session-and-game-party-visibility-and-joinability.md) descreve as diferenças entre a visibilidade e joinability já está relacionado a uma sessão de jogo com vários participantes.
