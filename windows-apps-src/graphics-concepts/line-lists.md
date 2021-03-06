---
title: Listas de linhas
description: Uma lista de linhas contém segmentos de linha reta isolados. As listas de linhas são úteis para tarefas como adicionar chuva com neve ou pesada a uma cena 3D. Os apps criam uma lista de linhas preenchendo uma matriz de vértices.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- Listas de linhas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac66066a4140ace5905ff6bc52a7b1290341beea
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291576"
---
# <a name="line-lists"></a>Listas de linhas

Uma lista de linhas contém segmentos de linha reta isolados. As listas de linhas são úteis para tarefas como adicionar chuva com neve ou pesada a uma cena 3D. Os apps criam uma lista de linhas preenchendo uma matriz de vértices. Observe que o número de vértices em uma lista de linhas deve ser um número par maior ou igual a dois.

-   [Exemplo](#example)
-   [Tópicos relacionados](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


A ilustração a seguir mostra uma lista de linha renderizada.

![Ilustração de uma lista de linhas](images/linelst.png)

Você pode aplicar materiais e texturas a uma lista de linhas. As cores no material ou na textura aparecem somente nas linhas desenhadas, não em qualquer ponto entre as linhas.

O código a seguir mostra como criar vértices para essa lista de linhas.

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

O exemplo de código abaixo mostra como renderizar uma lista de linhas no Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Primitivos](primitives.md)

 

 




