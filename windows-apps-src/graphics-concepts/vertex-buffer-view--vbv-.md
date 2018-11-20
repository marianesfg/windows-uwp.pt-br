---
title: Modo de exibição de buffer de vértice (VBV) e modo de exibição de buffer de índice (IBV)
description: Um buffer de vértices contém dados para uma lista de vértices.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Modo de exibição de buffer de vértice (VBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7956c7a03256da04e98a5b8f9a33951d7e0216d3
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7445835"
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

 

 




