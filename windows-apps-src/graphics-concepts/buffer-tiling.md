---
title: Buffers colocados lado a lado
description: Um recurso de buffer é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Buffers colocados lado a lado
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f3e5e117e05cef478ede508240a6b1d1022dea70
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7982725"
---
# <a name="buffer-tiling"></a>Buffers colocados lado a lado


Um recurso de [buffer](introduction-to-buffers.md) é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.

Buffers estruturados não devem ter nenhuma restrição sobre a distância lado a lado. Mas as otimizações de desempenho possível no hardware para o uso de [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) pode ser sacrificada, tornando-os lado a lado em primeiro lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como uma área de recurso de streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




