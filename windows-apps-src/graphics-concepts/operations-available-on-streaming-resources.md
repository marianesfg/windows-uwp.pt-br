---
title: Operações disponíveis em recursos de streaming
description: Esta seção lista operações que podem ser executadas em recursos de streaming.
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords:
- Operações disponíveis em recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1289b01e7ffb780c7e3faa52585eb5f002cf519c
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7119356"
---
# <a name="operations-available-on-streaming-resources"></a>Operações disponíveis em recursos de streaming


Esta seção lista operações que podem ser executadas em recursos de streaming.

-   Operações Atualizar mapeamentos de blocos de atualização e Copiar mapeamentos de blocos que retornam vazio: essas operações apontam locais de blocos em um recurso de streaming para locais em pools de blocos, ou como NULO ou ambos. Essas operações podem atualizar um subconjunto separado dos ponteiros de blocos.
-   Operações de Copiar e Atualizar: todas as APIs que podem copiar dados de e para uma superfície de pool padrão funcionam para recursos de streaming. Leitura de blocos não mapeados produz 0 e as gravações em blocos não mapeados são ignoradas.
-   Operações de cópia e atualização de blocos: essas operações existem para copiar blocos com granularidade de 64 KB de e para qualquer recurso de streaming e um recurso de buffer em um layout de memória canônico. O driver de vídeo e o hardware executam qualquer "conversão" de memória necessária para o recurso de streaming.
-   As vinculações de pipeline do Direct3D e exibição/vinculações de criações que funcionariam em recursos sem streaming também funcionam em recursos de streaming.

Os controles de blocos estão disponíveis em contextos de imediatos ou adiados (assim como atualizações de recursos típicos), e durante a execução afetam os acessos subsequentes aos blocos de (operações não enviadas anteriormente).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




