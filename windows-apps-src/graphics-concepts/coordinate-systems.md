---
title: Sistemas de coordenadas
description: Normalmente, os apps de elementos gráficos 3D usam um dos dois tipos de sistemas de coordenadas cartesianas de esquerda e direita. Em ambos os sistemas de coordenadas, o eixo x positivo aponta para a direita e o eixo y positivo aponta para cima.
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- Sistemas de coordenadas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f85bf490bd1dd68e2d0ba31335f2fc0f89fe27b0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655791"
---
# <a name="coordinate-systems"></a>Sistemas de coordenadas


Normalmente, os apps de elementos gráficos 3D usam um dos dois tipos de sistemas de coordenadas cartesianas de esquerda e direita. Em ambos os sistemas de coordenadas, o eixo x positivo aponta para a direita e o eixo y positivo aponta para cima.

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>À esquerda e à direita serão entregues de coordenadas


Você pode lembrar para qual direção o eixo z positivo aponta, apontando os dedos de sua mão direita ou esquerda na direção x positiva e curvando-os na direção y positiva. A direção do seu polegar aponta em sua direção ou para longe de você, é a direção em que o eixo z positivo aponta para esse sistema de coordenadas. A ilustração a seguir mostra esses dois sistemas de coordenadas.

![ilustração dos sistemas de coordenadas cartesianas à esquerda e à direita](images/leftrght.png)

O Direct3D usa um sistema de coordenadas à esquerda. Apesar das coordenadas de esquerda e direita serem os sistemas mais comuns, há uma variedade de outros sistemas de coordenadas usados no software 3D. Por exemplo, não é incomum para apps de modelagem 3D usar um sistema de coordenadas no qual o eixo y aponta para o visualizador ou para longe dele, e o eixo z aponta para cima.

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>Vértices e vetores


Dado o sistema de coordenadas, uma coordenada x, y e z pode definir um ponto no espaço (um "vértice") ou uma direção 3D (um "vetor").

Um conjunto de vértices pode ser usado para definir linhas e formas. Os objetos mais simples que podem ser definidos por vértices são chamados de [Primitivas](primitives.md), e um objeto mais complexo definido por um conjunto de primitivas é chamado de "malha".

As operações essenciais realizadas em malhas definidas em um sistema de coordenadas 3D são conversão, rotação e dimensionamento. Você pode combinar essas transformações básicas para criar uma matriz de transformação. Para obter detalhes, consulte [Transformações](transforms.md).

Quando você combina essas operações, os resultados não são comutativos; a ordem em que você faz multiplicação das matrizes é importante.

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>Portabilidade de um sistema de coordenadas destro


Se você estiver fazendo a portabilidade de um app que se baseia em um sistema de coordenadas à direita, você deve fazer duas alterações nos dados passados para o Direct3D:

-   Inverter a ordem dos vértices de triângulos, de modo que o sistema os percorra em sentido horário, a partir da frente. Em outras palavras, se os vértices forem v0, v1, v2, passe-os para o Direct3D como v0, v2, v1.
-   Use a matriz de visualização para dimensionar o espaço do mundo em -1 na direção z. Para fazer isso, inverter o sinal do \_31 \_32, \_33, e \_34 membro da estrutura de matriz que você usa para sua matriz de exibição.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




