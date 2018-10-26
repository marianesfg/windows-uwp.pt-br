---
title: Sistemas de coordenadas e geometria
description: A programação de apps Direct3D requer familiaridade ao trabalhar com princípios geométricos 3D. Esta seção apresenta os conceitos geométricos mais importantes para a criação de cenas 3D.
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- Sistemas de coordenadas e geometria
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce3b1ebfc2f18a06ff4fa960c91749f61f5011d9
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5638049"
---
# <a name="coordinate-systems-and-geometry"></a>Sistemas de coordenadas e geometria


A programação de apps Direct3D requer familiaridade ao trabalhar com princípios geométricos 3D. Esta seção apresenta os conceitos geométricos mais importantes para a criação de cenas 3D.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="coordinate-systems.md">Sistemas de coordenadas</a></p></td>
<td align="left"><p>Normalmente, os apps de elementos gráficos 3D usam um dos dois tipos de sistemas de coordenadas cartesianas de esquerda e direita. Em ambos os sistemas de coordenadas, o eixo x positivo aponta para a direita e o eixo y positivo aponta para cima.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="primitives.md">Primitivas</a></p></td>
<td align="left"><p>Uma <em>primitiva</em> 3D é uma coleção de vértices que formam uma única entidade 3D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="face-and-vertex-normal-vectors.md">Face e vetores normais de vértice</a></p></td>
<td align="left"><p>Cada rosto em uma malha tem um vetor normal de unidade perpendicular. Direção do vetor é determinada pela ordem em que os vértices são definidos, e se o sistema de coordenadas é orientado à direita ou esquerda.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rectangles.md">Retângulos</a></p></td>
<td align="left"><p>Em toda a programação do Direct3D e do Windows, os objetos na tela são referidos em termos de retângulos delimitadores. Os lados de um retângulo delimitador sempre são paralelos às laterais da tela, para que o retângulo possa ser descrito por dois pontos, o canto superior esquerdo e o canto inferior direito.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-interpolation.md">Interpolação de triângulo</a></p></td>
<td align="left"><p>Durante a renderização, o pipeline interpola os dados de vértice em cada triângulo. Os dados de vértice podem ser uma ampla variedade de dados e podem incluir (mas sem estarem limitados a isso): cor difusa, cor especular, alfa difuso (opacidade de triângulo), alfa especular, e fator de nevoeiro.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vectors--vertices--and-quaternions.md">Vetores, vértices e quatérnios</a></p></td>
<td align="left"><p>Em todo o Direct3D, os vértices descrevem a posição e a orientação. Cada vértice em uma primitiva é descrito por um vetor que oferece suas coordenadas de textura, cor e posição, e um vetor normal que oferece a sua orientação.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="transforms.md">Transformações</a></p></td>
<td align="left"><p>A parte do Direct3D que leva a geometria através do pipeline de geometria de funções fixas é o mecanismo de transformação. Ele localiza o modelo e o visualizador do mundo, projeta vértices para a exibição na tela e corta os vértices no visor. O mecanismo de transformação também executa cálculos de iluminação para determinar os componentes difusos e especulares em cada vértice.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="viewports-and-clipping.md">Visores e recorte</a></p></td>
<td align="left"><p>O <em>visor</em> é um retângulo (2D) bidimensional no qual uma cena 3D é projetada. No Direct3D, o retângulo existe como coordenadas dentro de uma superfície do Direct3D que o sistema usa como um destino de renderização. A transformação da projeção converte os vértices no sistema de coordenadas usado para o visor. Um visor também é usado para especificar o intervalo de valores de profundidade em uma superfície de destino de renderização na qual uma cena será renderizada (geralmente 0.0 a 1.0).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

 

 




