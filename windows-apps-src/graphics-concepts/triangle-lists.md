---
title: Listas de triângulos
description: Uma lista de triângulos é uma lista de triângulos isolados. Os triângulos isolados podem ou não estar próximos uns dos outros. Uma lista de triângulos deve ter pelo menos três vértices e o número total de vértices deve ser divisível por três.
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- Listas de triângulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 53fd2b132fda018030b7555a9cdac718ec1f1cc4
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291554"
---
# <a name="triangle-lists"></a>Listas de triângulos

Uma lista de triângulos é uma lista de triângulos isolados. Os triângulos isolados podem ou não estar próximos uns dos outros. Uma lista de triângulos deve ter pelo menos três vértices e o número total de vértices deve ser divisível por três.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Use listas de triângulos para criar um objeto que é composto de peças separadas. Por exemplo, uma maneira de criar uma parede de campo de força em um jogo 3D é especificando uma grande lista de triângulos pequenos, desconectados. Em seguida, aplique um material e textura que parece emitir luz à lista de triângulos. Cada triângulo na parede parece brilhar. A cena por trás da parede fica parcialmente visível por meio dos espaços entre os triângulos, da forma que um jogador pode esperar ao olhar para um campo de força.

As listas de triângulo também são úteis para criar primitivos que têm bordas afiadas e são sombreados com sombreamento Gouraud. Consulte [Face e vetores normais de vértice](face-and-vertex-normal-vectors.md).

A ilustração a seguir ilustra uma lista de triângulos renderizada.

![Ilustração de uma lista de triângulos renderizada](images/trilist.png)

O código a seguir mostra como criar vértices para essa lista de triângulos.

```cpp
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

O exemplo de código abaixo mostra como renderizar essa lista de triângulos no Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Primitivos](primitives.md)

 

 




