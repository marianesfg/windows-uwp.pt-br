---
title: Criação de pool de blocos
description: Os apps podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de blocos é restrito ao limite de tamanho de recursos do Direct3D11, que é aproximadamente 1/4 da unidade de processamento gráfico (GPU) RAM.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Criação de pool de blocos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cbb8b61c8eeef1a842a7c6b61d09670f056bb409
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5940636"
---
# <a name="tile-pool-creation"></a>Criação de pool de blocos


Os apps podem criar um ou mais pools de blocos por dispositivo Direct3D. O tamanho total de cada pool de blocos é restrito ao limite de tamanho de recursos do Direct3D11, que é aproximadamente 1/4 da unidade de processamento gráfico (GPU) RAM.

Um pool de blocos é composto por blocos de 64 KB, mas o sistema operacional (driver de vídeo) gerencia o pool inteiro como uma ou mais alocações em segundo plano – a divisão não fica visível para os aplicativos. Os recursos de streaming definem o conteúdo apontando para blocos em um pool de blocos. O desmapeamento de um bloco de um recurso de streaming é feito apontando o bloco para **NULL**. Esses blocos desmapeados têm regras sobre o comportamento de leituras ou gravações; consulte [Controle de risco versus recursos de pool de blocos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamentos estão em um pool de blocos](mappings-are-into-a-tile-pool.md)

 

 




