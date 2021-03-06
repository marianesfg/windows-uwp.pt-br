---
title: Sub-recursos lado a lado de Texture2D e Texture2DArray
description: Estas tabelas mostram como sub-recursos Texture2D e Texture2DArray são agrupados lado a lado.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Sub-recursos lado a lado de Texture2D e Texture2DArray
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e193ab7bce31c1f13cb40f04902922c6ff21056
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370910"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Sub-recursos lado a lado de Texture2D e Texture2DArray


Estas tabelas mostram como sub-recursos [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) e [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) são agrupados lado a lado. Os valores nestas tabelas não contam a compactação MIP final.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Sub-recursos com contagens de multisample 1


Esta tabela mostra como os sub-recursos [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) e [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) com contagens multisample de 1 são agrupados lado a lado.

| Bits/Pixel (1 amostra/pixel) | Dimensões do bloco (Pixels, LxA) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64x64                         |
| BC1,4                       | 512x256                       |
| BC2,3,5,6,7                 | 256x256                       |

 

Contagens de bit de formato não tem suportadas com recursos de streaming são formatos de bpp 96, formatos de vídeo, DXGI\_formato\_R1\_UNORM, DXGI\_formato\_R8G8\_B8G8\_UNORM, e DXGI\_formato\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Sub-recursos com várias contagens de multisample


Esta tabela mostra como os sub-recursos [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d) e [**Texture2DArray**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2darray) com várias contagens multisample são agrupados lado a lado.

| Bits/Pixel (1 amostra/pixel) | Dimensões do bloco (Pixels, LxA) |
|-----------------------------|-------------------------------|
| 1                           | 1x1                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4x2                           |
| 16                          | 4x4                           |

 

Somente as contagens de amostra 1 e 4 são necessárias (e permitidas) têm suporte com recursos de streaming. Os recursos de streaming no momento não têm suporte para 2, 8 e 16 mesmo que sejam mostrados.

As implementações podem optar por dar suporte ao modo de suavização multisample (MSAA) de 2, 8 e/ou 16 amostras para recursos de não streaming mesmo que o recurso de streaming não tenha suporte para eles.

Recursos de streaming com contagens de amostras maiores que 1 não podem usar formatos 128 bpp.

As restrições de formatos e contagens de amostras com suporte se devem às inconsistências de hardware da especificação.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Como área do recurso streaming é colocada lado a lado](how-a-streaming-resource-s-area-is-tiled.md)

 

 




