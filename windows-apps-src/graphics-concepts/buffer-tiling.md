---
title: Buffers colocados lado a lado
description: Um recurso de buffer é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Buffers colocados lado a lado
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1817f501962ccae4cfaf9c0ce075724abd5e7672
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5840208"
---
# <a name="buffer-tiling"></a>Buffers colocados lado a lado


Um recurso de [buffer](introduction-to-buffers.md) é dividido em blocos de 64KB, com algum espaço vazio no último bloco se o tamanho não é um múltiplo de 64KB.

Buffers estruturados não devem ter nenhuma restrição sobre a distância lado a lado. Mas as otimizações de desempenho possível no hardware para o uso de [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) pode ser sacrificada, tornando-os lado a lado em primeiro lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como uma área de recurso de streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




