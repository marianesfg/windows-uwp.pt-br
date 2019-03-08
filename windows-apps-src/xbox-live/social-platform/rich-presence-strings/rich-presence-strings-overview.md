---
title: Presença avançada
description: Saiba como o Xbox Live Rich presença pode ajudar a promover seu título.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, recursos avançados de presença
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604111"
---
# <a name="rich-presence"></a>Presença avançada

Usando a presença avançada, seu jogo pode anunciar o que um player está fazendo agora. Por exemplo, o seu jogo pode usar cadeias de caracteres de presença avançada para mostrar todos os jogadores o status de jogadores do seu jogo, como *ausente*. Informações de presença avançada são visíveis para os jogadores conectados ao Xbox Live. O ideal é que uma cadeia de caracteres de presença avançada informa outros jogadores do Xbox Live o que está fazendo um player, e onde no seu jogo o player é fazê-lo. O conceito de cadeias de caracteres de presença avançada é o mesmo no Xbox One, como era no Xbox 360, mas a nova implementação segue o entretenimento como uma iniciativa de serviço. Os tópicos nesta seção descrevem como configurar suas cadeias de caracteres de presença avançada e, em seguida, como definir a cadeia de caracteres para um usuário reproduzir seu título.


## <a name="definitions"></a>Definições

**Enumerações**  
Uma enumeração é uma lista de alguns dimensão no jogo. Exemplos dessas dimensões do jogo são classes de caracteres, armas, mapas e assim por diante. Queremos ver uma lista das armas possíveis em seu jogo, uma lista de todas as classes de caracteres possíveis ou mapas e assim por diante.

**Par de cadeia de caracteres de localidade**  
Cada cadeia de caracteres possíveis presença avançada deve ter uma localidade associada a ele para especificar em quais localidades a cadeia de caracteres pode/deve ser usada. Cada enumeração também terá um conjunto de pares de cadeia de caracteres de localidade também.

**String-set**  
Um conjunto de cadeia de caracteres é composto de um grupo de pares de cadeia de caracteres de localidade. Esse conjunto define os valores possíveis de uma cadeia de caracteres de presença avançada para todas as localidades possíveis ou os valores possíveis para uma enumeração de todos os locais possíveis.

**Nomes amigáveis**  
Há dois tipos de nomes amigáveis:

**Cadeia de caracteres de presença avançada**  
O nome amigável para um conjunto de cadeia de caracteres é um identificador exclusivo na forma de uma cadeia de caracteres usada para fazer referência a um conjunto de cadeia de caracteres.

**Enumeração**  
Esses nomes amigáveis são utilizados para identificar exclusivamente uma enumeração específica, como a enumeração de armas ou a enumeração da classe de caractere.


## <a name="in-this-section"></a>Nesta seção

[Configuração de presença avançada](rich-presence-strings-configuration.md)  
Como configurar a presença avançada para uso em seu título.

[Presença avançada Atualizando cadeias de caracteres](rich-presence-strings-updating-strings.md)  
Como atualizar cadeias de caracteres de presença avançada do seu título.

[Práticas recomendadas de presença avançada](rich-presence-strings-best-practices.md)  
Práticas recomendadas para usam de presença avançada em seu título.

[Limitações e as políticas de presença avançada](rich-presence-strings-policies-and-limitations.md)  
As políticas sobre como usar a presença avançada em seu título.

[Apêndice de presença avançada](rich-presence-strings-appendix.md)  
Exemplos adicionais e detalhes sobre a plataforma de dados relevantes para a presença avançada.

[Programação Xbox Live presença avançada](programming-rich-presence.md)  
Demonstra como usar a presença avançada com o Xbox Live.
