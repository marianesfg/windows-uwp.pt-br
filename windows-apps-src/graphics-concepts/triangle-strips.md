---
title: Listas de triângulos
description: Uma faixa de triângulos é uma série de triângulos conectados. Como os triângulos são conectados, o aplicativo não precisa especificar repetidamente todos os três vértices de cada triângulo.
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- Listas de triângulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62ad93fa480f0515c4ed6df2d73a745454197ac6
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291634"
---
# <a name="triangle-strips"></a>Listas de triângulos

Uma faixa de triângulos é uma série de triângulos conectados. Como os triângulos são conectados, o aplicativo não precisa especificar repetidamente todos os três vértices de cada triângulo. Por exemplo, você precisa de apenas sete vértices para definir a faixa de triângulos a seguir.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo

![ilustração de uma faixa de triângulos com sete vértices](images/tristrip.png)

O sistema usa os vértices v1, v2 e v3 para desenhar o primeiro triângulo; v2, v4 e v3 para desenhar o segundo triângulo; v3, v4 e v5 para desenhar o terceiro; V4 e v6 v5 para desenhar o quarto; e assim por diante. Observe que os vértices do segundo e do quarto triângulo estão fora de ordem; isso é necessário para garantir que todos os triângulos sejam desenhados em uma orientação no sentido horário.

A maioria dos objetos em cenas 3D são compostos por faixas de triângulos. Isso ocorre porque as faixas de triângulos podem ser usadas para especificar objetos complexos de maneira a fazer uso eficiente da memória e do tempo de processamento.

A ilustração a seguir ilustra uma faixa de triângulos renderizado.

![ilustração de uma faixa de triângulos renderizada](images/tstrip2.png)

O código a seguir mostra como criar vértices para essa faixa de triângulos.

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

O exemplo de código abaixo mostra como renderizar essa faixa de triângulos no Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>Polígonos


Muitas vezes, as faixas de triângulo são usadas para criar polígonos. Um polígono é uma figura 3D fechada delineada por pelo menos três vértices. O polígono mais simples é um triângulo. O Microsoft Direct3D usa triângulos para compor a maior parte de seus polígonos porque é garantido que todos os três vértices de um triângulo serão coplanares. Renderizar vértices que não são planares é ineficiente. Você pode combinar triângulos para formar malhas e polígonos grandes e complexos.

A ilustração a seguir mostra um cubo. Dois triângulos formam cada face do cubo. Todo o conjunto de triângulos constitui um primitivo cúbico. Você pode aplicar texturas nas superfícies de primitivos para torná-los parecidos com um único formato sólido. Para obter detalhes, consulte [Texturas](textures.md).

![ilustração de um cubo com dois triângulos em cada face](images/cube3d.png)

Você também pode usar triângulos para criar primitivos cujas superfícies parecem ser curvas suaves. A ilustração a seguir mostra como uma esfera pode ser simulada com triângulos. Depois que um material é aplicado, a esfera pode ser projetada para parecer curvada quando for renderizada.

![ilustração de uma esfera simulada usando triângulos](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Primitivos](primitives.md)

 

 




