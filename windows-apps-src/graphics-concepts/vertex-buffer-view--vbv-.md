---
title: "Modo de exibição de buffer de vértice (VBV) e modo de exibição de buffer de índice (IBV)"
description: "Um buffer de vértices contém dados para uma lista de vértices."
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords: "Modo de exibição de buffer de vértice (VBV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 502b0e4816e31ebace93d3250f7da335d2540272
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Modo de exibição de buffer de vértice (VBV) e modo de exibição de buffer de índice (IBV)


Um buffer de vértices contém dados para uma lista de vértices. Os dados de cada vértice podem incluir posição, cor, vetor normal, coordenadas de textura e assim por diante. Um buffer de índice mantém os índices de números inteiros (deslocamentos) em um buffer de vértice e é usado para definir e renderizar um objeto composto por um subconjunto da lista completa de vértices.

A definição de um único vértice geralmente é definida pelo aplicativo, como:

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

A definição de CUSTOMVERTEX seria então passada para o driver de gráficos ao criar buffers de vértice.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




