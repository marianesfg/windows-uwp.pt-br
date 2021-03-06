---
title: Faixas de linhas
description: Uma faixa de linhas é um primitivo composto de segmentos de linha conectados. O app pode usar faixas de linhas para criar polígonos não fechados. Um polígono fechado tem o último vértice conectado ao primeiro por um segmento de linhas.
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- Faixas de linhas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d17a79c14e981ab0c2c0414074aff17c90a0b478
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291624"
---
# <a name="line-strips"></a>Faixas de linhas

Uma faixa de linhas é um primitivo composto de segmentos de linha conectados. O app pode usar faixas de linhas para criar polígonos não fechados. Um polígono fechado tem o último vértice conectado ao primeiro por um segmento de linhas. Se o app faz polígonos com base em faixas alinhadas, os vértices não têm garantia de estar coplanares.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


A ilustração a seguir mostra uma faixa de linhas renderizada.

![Ilustração de uma faixa de linhas](images/linstrip.gif)

O código a seguir mostra como criar vértices para essa faixa de linhas.

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

O exemplo de código abaixo mostra como renderizar uma faixa de linhas no Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Primitivos](primitives.md)

 

 




