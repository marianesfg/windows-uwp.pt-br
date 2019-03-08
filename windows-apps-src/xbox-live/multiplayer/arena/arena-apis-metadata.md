---
title: Metadados de APIs da interface do usuário da arena
description: Contém uma lista abrangente dos metadados de interface do usuário para as APIs de Arena do Xbox.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio, experiência do usuário
ms.localizationpriority: medium
ms.openlocfilehash: 63151d2c584218090f98a3c5f8089bf24bdb9b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659761"
---
# <a name="arena-apis-a-comprehensive-list-of-ui-metadata"></a>APIs da arena: Uma lista abrangente dos metadados de interface do usuário

As APIs da Arena retorna os metadados para identificando os detalhes do torneio, correspondência e player dentro de um jogo. Aqui está uma lista completa.

DETALHES DO TORNEIO  | DETALHES DE CORRESPONDÊNCIA | DETALHES DA EQUIPE  | DETALHES DO REGISTRO
--- | --- | --- | ---
Nome do organizador | Hora de término — a data/hora em que este torneio tenha atingido o estado de "Cancelado" ou "Concluído". Isso é definido automaticamente quando um torneio é atualizado. | Correspondência anterior (resultados de jogos do torneio afirma, classificação, hora de término, hora de início da correspondência, é um Bye, descrição, opostos IDs de equipe) | Estado do registro (desconhecido, pendente, retiradas, rejeitada, registrado, concluído)
Nome do torneio (máximo de 128 char) | É um Bye   | Estado da equipe (desconhecida, registrado, lista de espera, em espera, check-in, execução, concluído) | Motivo do registro (membro desconhecido, fechado, já registrado, completa do torneio, equipe eliminado, torneio concluído)
Descrição do torneio (1000 caracteres no máximo) | Torneio resultado jogo estados (nenhum concurso/cancelada, win, perda, draw, classificação, sem mostrar) | A equipe está em um estado concluído de motivo (rejeitadas, removidos, eliminado, concluído) | Número mínimo e máximo de equipes registrados
Hora de início/término do registro | Estado de arbitragem: Concluído, none, cancelado, não há resultados, resultados parciais | Data de registro – a data e hora em uma equipe foi registrada. |
Fazer check-in a hora de início/término | Corresponder descritivo 'Rótulo' – ("final, 1 de calor") | Posição da equipe | O registro é aberto
Hora de início/término de execução | Hora de início | Nome da equipe | É fazer Check-in aberto
Tem um prêmio | Ids de equipe adversária | Classificação de final de equipe | Hora de início/término do registro
Tamanho da equipe Mín/Máx | | URI de continuação – leva os membros da equipe de volta à interface do usuário a Arena | Fazer check-in a hora de início/término
Modo de jogo | | Atual correspondem aos metadados (Descrição, hora de início, é Bye opostos IDs de equipe) | Contagem de equipes registrado
Estilo de torneio (eliminação simples, round robin) | | Resumo de equipe (estado de equipe, classificação) |
O registro é aberto | | Gamertags |
É fazer Check-in aberto | | |
Contagem de equipes registrada | | |
Está em execução aberto | | |
