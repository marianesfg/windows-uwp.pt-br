---
title: Comportamento de UAV com blocos não mapeados
description: O comportamento das leituras e gravações de modo de exibição de acesso não ordenado (UAV) depende do nível de suporte do hardware.
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords:
- Comportamento de UAV com blocos não mapeados
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c1429579ddd896d5c717968f509ddf578a79f2e0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043675"
---
# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>Comportamento de UAV com blocos não mapeados


O comportamento das leituras e gravações de modo de exibição de acesso não ordenado (UAV) depende do nível de suporte do hardware. Para uma análise dos requisitos, veja o comportamento geral de leitura e gravação para [Recursos de streaming que apresentam faixas](streaming-resources-features-tiers.md). Esta seção resume o comportamento ideal.

As operações de sombreador que lêem a partir de um bloco não mapeado em um UAV retornam 0 em todos os componentes sem perder o formato e o padrão para componentes que estão faltando.

As operações de sombreador que tentam gravar em um bloco não mapeado fazem com que nada seja gravado na área não mapeado (enquanto as gravações em uma área mapeada prosseguem). Essa definição ideal para o tratamento de gravação não é exigida pelo [nível 2](tier-2.md); as gravações em blocos não mapeados podem acabar em um cache que pode ser coletado pelas leituras subsequentes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 



