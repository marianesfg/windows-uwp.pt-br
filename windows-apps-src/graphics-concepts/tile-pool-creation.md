---
title: Criação de pool de blocos
description: Os apps podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de bloco é restrito ao limite de tamanho de recursos do Direct3D 11, que é de aproximadamente 1 e 4 da unidade de processamento gráfico (GPU) de RAM.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Criação de pool de blocos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612801"
---
# <a name="tile-pool-creation"></a>Criação de pool de blocos


Os apps podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de bloco é restrito ao limite de tamanho de recursos do Direct3D 11, que é de aproximadamente 1 e 4 da unidade de processamento gráfico (GPU) de RAM.

Um pool de blocos é composto por blocos de 64 KB, mas o sistema operacional (driver de vídeo) gerencia o pool inteiro como uma ou mais alocações em segundo plano – a divisão não fica visível para os aplicativos. Os recursos de streaming definem o conteúdo apontando para blocos em um pool de blocos. O desmapeamento de um bloco de um recurso de streaming é feito apontando o bloco para **NULL**. Esses blocos desmapeados têm regras sobre o comportamento de leituras ou gravações; consulte [Controle de risco versus recursos de pool de blocos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamentos estiverem em um pool de bloco](mappings-are-into-a-tile-pool.md)

 

 




