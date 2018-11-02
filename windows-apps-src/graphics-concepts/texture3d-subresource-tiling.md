---
title: Sub-recursos lado a lado de Texture3D
description: Esta tabela mostra como os sub-recursos de Texture3D são colocados lado a lado.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Sub-recursos lado a lado de Texture3D
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 17970d509fa2bf6b80431e1c07b5d135c7dcb112
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5932022"
---
# <a name="texture3d-subresource-tiling"></a>Sub-recursos lado a lado de Texture3D


Esta tabela mostra como os sub-recursos de [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) são colocados lado a lado. Os valores nesta tabela não contam a compactação MIP final.

Esta tabela pega o agrupamento lado a lado do [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) e divide cada uma das dimensões x/y por 4 e adiciona 16 camadas de profundidade. Todos os blocos do primeiro plano (plano 2D de blocos definindo as 16 primeiras camadas de profundidade) aparecem antes dos planos subsequentes.

**Observação** Suporte de [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) nos recursos de streaming não é exposto na implementação inicial de recursos de streaming, mas as formas de bloco desejadas estão listadas aqui para um possível suporte em uma versão futura.

 

| Bits/Pixel (1 amostra/pixel) | Dimensões do bloco (Pixels, LxAxP) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

Contagens de bits de formato sem suporte com recursos de streaming: formatos de 96 bpp, formatos de vídeo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM e DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como uma área de recurso de streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




