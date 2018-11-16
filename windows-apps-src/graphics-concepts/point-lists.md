---
title: Listas de pontos
description: Uma lista de ponto é uma coleção de vértices que são renderizados como pontos isolados. Seu app pode usar listas de ponto em cenas 3D para campos de estrelas, ou linhas pontilhadas na superfície de um polígono.
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- Listas de pontos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b4c2e0f6e99099df4bdda9452521acf3b0cd2b8f
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6994845"
---
# <a name="point-lists"></a>Listas de pontos


Uma lista de ponto é uma coleção de vértices que são renderizados como pontos isolados. Seu app pode usar listas de ponto em cenas 3D para campos de estrelas, ou linhas pontilhadas na superfície de um polígono.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


A ilustração a seguir mostra uma lista de ponto renderizada.

![ilustração de uma lista de ponto](images/pointlst.png)

Seu app pode aplicar materiais e texturas a uma lista de ponto. As cores na textura ou no material aparecem apenas nos pontos desenhados e não entre os pontos.

O código a seguir mostra como criar vértices para essa lista de pontos.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

O exemplo de código abaixo mostra como renderizar essa lista de pontos no Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Primitivos](primitives.md)

 

 




