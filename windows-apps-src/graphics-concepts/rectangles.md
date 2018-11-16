---
title: Retângulos
description: Pela programação do Direct3D e do Windows, objetos na tela são referidos em termos de retângulos delimitadores.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Retângulos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cb870fa851e51773d95d23ebf9d31f76774924de
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6970742"
---
# <a name="rectangles"></a>Retângulos


Em toda a programação do Direct3D e do Windows, os objetos na tela são referidos em termos de retângulos delimitadores. Os lados de um retângulo delimitador sempre são paralelos às laterais da tela, então o retângulo pode ser descrito por dois pontos: o canto superior esquerdo e o canto inferior direito.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Retângulos delimitadores


A maioria dos apps usam a estrutura [**RECT**](https://msdn.microsoft.com/library/windows/desktop/dd162897) (ou um alias typedef para ela) para transmitir informações sobre um retângulo delimitador para usar ao transferir para a tela ou ao executar a detecção de visitas. No C++, a estrutura **RECT** tem a seguinte definição.

```
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

No exemplo anterior, os membros esquerdo e superior são as coordenadas x e y do canto superior esquerdo do retângulo delimitador. Da mesma forma, os membros direito e inferior compõem as coordenadas do canto inferior direito. A ilustração a seguir mostra como você pode visualizar esses valores.

![ilustração do retângulo delimitado pelos valores esquerdo, superior, direito e inferior](images/rect.png)

A coluna de pixels na borda direita e a linha de pixels na borda inferior não estão incluídas no RECT. Por exemplo, bloquear um RECT com membros {10, 10, 138, 138} resulta em um objeto com 128 pixels em largura e altura.

Para eficiência, consistência e a facilidade de uso, todas as funções de apresentação do Direct3D trabalham com retângulos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




