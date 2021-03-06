---
title: Comportamento de SRV com blocos não mapeados
description: O comportamento de leituras de modo de exibição (SRV) de recurso de sombreador que envolve blocos não mapeados depende do nível de suporte de hardware.
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords:
- Comportamento de SRV com blocos não mapeados
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d62a1d3158e13187f89277a1ba009bd56fc2b39a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659421"
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>Comportamento SRV com blocos não mapeados


O comportamento de leituras de modo de exibição (SRV) de recurso de sombreador que envolve blocos não mapeados depende do nível de suporte de hardware. Para uma análise dos requisitos, veja o comportamento de leitura para [Níveis de recursos de streaming](streaming-resources-features-tiers.md). Esta seção resume o comportamento ideal, que o [Nível 2](tier-2.md) requer.

Considere uma operação de filtro de textura que lê de um conjunto de texels em um SRV. Texels que se enquadram em blocos não mapeados contribuem com 0 em todos os componentes sem perder o formato (e o padrão para componentes que estão faltando) para a operação de filtro geral junto com contribuições de texels mapeadas. Os texels são todos ponderados e combinados juntos independentemente dos dados serem provenientes de blocos mapeados ou não mapeados.

Alguns hardwares de [Nível 2](tier-2.md) de primeira geração não atendem a esse requisito específico e retornam o 0 com os padrões descritos acima como resultado do filtro geral se qualquer texel (com espessura diferente de zero) se enquadrar em blocos não mapeadas. O requisito de incluir todos os texels no filtro (peso diferente de zero) não poderá faltar em nenhum outro hardware.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de acesso a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




