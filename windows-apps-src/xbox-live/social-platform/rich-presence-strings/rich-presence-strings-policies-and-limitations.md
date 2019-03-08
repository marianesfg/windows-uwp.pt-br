---
title: Limitações e as políticas de presença avançada
description: Saiba mais sobre as políticas e limitações do sistema de presença de Rich do Xbox Live.
ms.assetid: 0ad21a75-0524-45a8-8d8a-0dec0f7d6d6f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, recursos avançados de presença, políticas
ms.localizationpriority: medium
ms.openlocfilehash: f85974c0ccb38f3c33fb214ddaf24b98dd8dcdd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643591"
---
# <a name="rich-presence-policies-and-limitations"></a>Limitações e as políticas de presença avançada

Quando você implementa presença avançada para seu título, devem cumprir as políticas e os limites a seguir.

-   Cada título deve ter pelo menos 1 de conjunto de cadeia de caracteres, mas não há nenhum limite superior em quantos conjuntos de cadeias de caracteres você pode ter.
-   Você deve definir uma cadeia de caracteres padrão, bem como a cultura neutra cadeias de caracteres para cada enumeração em cada cadeia de caracteres de presença avançada.
-   Você pode usar numéricos ou estatísticas para preencher os parâmetros em suas cadeias de caracteres da cadeia de caracteres. Você não pode usar as estatísticas de data e hora.
-   Se você estiver usando as estatísticas em suas cadeias de caracteres de presença avançada, essas estatísticas (incluindo qualquer enumerações para estatísticas) devem estar disponíveis no mesmo SCID & área restrita.
-   Você tem 1 linha 44 caracteres total (inclusive os valores dos parâmetros). Isso é semelhante aos limites de presença de Rich do Xbox 360. Podemos estará trabalhando com o cliente para ver se o comprimento da cadeia de caracteres pode crescer. Haverá um aviso se a cadeia de caracteres pode ser maior.
    -   Caracteres Unicode são necessários e devem ser capazes de trabalhar com codificação UTF-8 para exibição.
-   Seus nomes amigáveis devem seguir estas regras:
    -   Os caracteres permitidos são 'A' a 'Z', 'a' a 'z', sublinhado ('\_') e ' 0' até ' 9'.

        Não há nenhum limite de caractere.

-   Nenhuma verificação de cadeia de caracteres é feita em cadeias de caracteres; Você deve fazer qualquer verificação de cadeia de caracteres, como verificação ortográfica e verificar que a cadeia de caracteres tenha sido localizada corretamente.
    -   Se sentimos que um conjunto de cadeia de caracteres é inadequado (por exemplo, a linguagem abusiva ou ofensiva), Microsoft impede que os títulos do uso de presença avançada até que as cadeias de caracteres foram atualizadas para a nossa satisfação.
-   Se o título não é a integração com a plataforma de dados, não há nenhuma opção para o uso de estatísticas como parâmetros em suas cadeias de caracteres.
    -   Todas as cadeias de caracteres devem ser completamente predefinidas nesse caso (não há tokens são permitidos).
-   Nomes de enumeração devem ser exclusivos entre todas as enumerações e devem ser exclusivos para nomes de estatística.
-   Se uma linha excede o número de caracteres que podem ser mostrados, e há uma quebra de linha, a linha será truncada automaticamente.
