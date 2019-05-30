---
title: Sub-recursos lado a lado de Texture3D
description: Esta tabela mostra como os sub-recursos de Texture3D são colocados lado a lado.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Sub-recursos lado a lado de Texture3D
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b63fdeeffd4b95afab6556b6f0318732ff988b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370901"
---
# <a name="texture3d-subresource-tiling"></a>Sub-recursos lado a lado de Texture3D


Esta tabela mostra como os sub-recursos de [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) são colocados lado a lado. Os valores nesta tabela não contam a compactação MIP final.

Esta tabela pega o agrupamento lado a lado do [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) e divide cada uma das dimensões x/y por 4 e adiciona 16 camadas de profundidade. Todos os blocos do primeiro plano (plano 2D de blocos definindo as 16 primeiras camadas de profundidade) aparecem antes dos planos subsequentes.

**Observação**  O suporte para   [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) nos recursos de streaming não é exposto na implementação inicial de recursos de streaming, mas as formas de bloco desejadas estão listadas aqui para um possível suporte em uma versão futura.

 

| Bits/Pixel (1 amostra/pixel) | Dimensões do bloco (Pixels, LxAxP) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1,4                       | 128x64x16                       |
| BC2,3,5,6,7                 | 64x64x16                        |

 

Contagens de bit de formato não tem suportadas com recursos de streaming são formatos de bpp 96, formatos de vídeo, DXGI\_formato\_R1\_UNORM, DXGI\_formato\_R8G8\_B8G8\_UNORM, e DXGI\_formato\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como área do recurso streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




