---
title: "Retângulos"
description: "Pela programação do Direct3D e do Windows, objetos na tela são referidos em termos de retângulos delimitadores."
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords: "Retângulos"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 2128dd8fa6ff22e20cd8a25dea0fd44431c1fae2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="rectangles"></a>Retângulos


Pela programação do Direct3D e do Windows, objetos na tela são referidos em termos de retângulos delimitadores. Os lados de um retângulo delimitador sempre são paralelos às laterais da tela, então o retângulo pode ser descrito por dois pontos: o canto superior esquerdo e o canto inferior direito.

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

 

 



