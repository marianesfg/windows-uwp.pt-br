---
title: Matemática de iluminação
description: O modelo de luz do Direct3D abrange a iluminação ambiente, difusa, especular e de emissão. Isso é flexibilidade suficiente para solucionar problemas de uma diversas situações de iluminação. A quantidade total de luz em uma cena é chamada de iluminação global.
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- Matemática de iluminação
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d21f60694c55edacc7a5723e7ed470c37af992ab
ms.sourcegitcommit: 9031a51f9731f0b675769e097aa4d914b4854e9e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58618423"
---
# <a name="mathematics-of-lighting"></a>Matemática de iluminação

O modelo de luz do Direct3D abrange a iluminação ambiente, difusa, especular e de emissão. Isso é flexibilidade suficiente para solucionar problemas de uma diversas situações de iluminação. A quantidade total de luz em uma cena é chamada de *iluminação global*.

A iluminação global é calculada da seguinte maneira:

```cpp
global_illumination = ambient_lighting + diffuse_lighting + specular_lighting + emissive_lighting;
```

[Iluminação ambiente](ambient-lighting.md) é a iluminação constante. A iluminação ambiente é constante em todas as direções e fornece cor de forma igual para todos os pixels de um objeto. Ela é rápida de calcular, mas deixa os objetos com uma aparência simples e irreal.

A [iluminação difusa](diffuse-lighting.md) depende da direção da luz e da normal da superfície do objeto. A iluminação difusa varia entre a superfície de um objeto como resultado da alteração da direção da luz e da alteração do vetor numeral da superfície. É necessário mais tempo para calcular a iluminação difusa porque ela é alterada para cada vértice de objeto, mas a vantagem de usá-la é que ela sombreia objetos e dá a eles profundidade tridimensional (3D).

[A Iluminação especular](specular-lighting.md) identifica os realces especulares brilhantes que ocorrem quando a luz atinge a superfície de um objeto e reflete em direção à câmera. A iluminação especular é mais intensa do que a luz difusa e incide mais rapidamente na superfície do objeto. O cálculo da iluminação especular demora mais do que o da iluminação difusa, no entanto, a vantagem de usá-lo é que ele adiciona detalhes significativas à superfície.

A [iluminação emissiva](emissive-lighting.md) é emitido por um objeto; por exemplo, um brilho. A emissão faz com que um objeto renderizado pareça ter iluminação própria. A emissão afeta a cor de um objeto e pode, por exemplo, tornar um material escuro mais claro e adotar parte da cor emitida.

A iluminação realista pode ser realizada ao aplicar cada um desses tipos de iluminação a uma cena 3D. Os valores calculados para componentes de ambiente, emissão e difusa são reproduzidos como a cor do vértice difusa; o valor do componente de iluminação especular é reproduzido como a cor do vértice especular. Os valores de luz ambiente, difusa e especular podem ser afetados pelo fator de atenuação e destaque da luz específica. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md).

Para obter um efeito de iluminação mais realista, é possível adicionar mais luzes; no entanto, a cena leva mais tempo para renderizar. Para obter todos os efeitos desejados por um designer, alguns jogos usam mais processamento da CPU do que está normalmente disponível. Nesse caso, é comum reduzir o número de cálculos de iluminação ao mínimo usando mapas de luz e ambiente para adicionar a iluminação à cena com mapas de textura.

A iluminação é calculada no espaço da câmera. Consulte [Transformações de espaço de câmera](camera-space-transformations.md). A iluminação otimizada pode ser calculada no espaço do modelo mediante condições especiais: os vetores normais já estão normalizados, a mistura de vértice não é necessária e as matrizes de transformação são ortogonais.

Todos os cálculos de iluminação são feitos no espaço do modelo ao transformar a posição e a direção da fonte de luz, juntamente com a posição da câmera, para o espaço de modelo usando o inverso da matriz de mundo. Como resultado, se as matrizes de mundo ou modo de exibição apresentam dimensionamento não uniforme, a iluminação resultante pode ser imprecisa.

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
<td align="left"><p><a href="ambient-lighting.md">Luz ambiente</a></p></td>
<td align="left"><p>A iluminação ambiente fornece iluminação constante para uma cena. Ele destaca todos os vértices de objeto da mesma forma porque ela não depende de quaisquer outros fatores de iluminação como normais de vértice, direção da luz, posição da luz, intervalo ou atenuação. A iluminação ambiente é constante em todas as direções e fornece cor de forma igual para todos os pixels de um objeto. Ela é rápida de calcular, mas deixa os objetos com uma aparência simples e irreal.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">Difuso iluminação</a></p></td>
<td align="left"><p>A <em>iluminação difusa</em> depende da direção da luz e da normal da superfície do objeto. A iluminação difusa varia entre a superfície de um objeto como resultado da alteração da direção da luz e da alteração do vetor numeral da superfície. É necessário mais tempo para calcular a iluminação difusa porque ela é alterada para cada vértice de objeto, mas a vantagem de usá-la é que ela sombreia objetos e dá a eles profundidade tridimensional (3D).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">Iluminação especular</a></p></td>
<td align="left"><p><em>A Iluminação especular</em> identifica os realces especulares brilhantes que ocorrem quando a luz atinge a superfície de um objeto e reflete em direção à câmera. A iluminação especular é mais intensa do que a luz difusa e incide mais rapidamente na superfície do objeto. O cálculo da iluminação especular demora mais do que o da iluminação difusa, no entanto, a vantagem de usá-lo é que ele adiciona detalhes significativas à superfície.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">Iluminação emissiva</a></p></td>
<td align="left"><p>A <em>iluminação emissiva</em> é emitido por um objeto; por exemplo, um brilho. A emissão faz com que um objeto renderizado pareça ter iluminação própria. A emissão afeta a cor de um objeto e pode, por exemplo, tornar um material escuro mais claro e adotar parte da cor emitida.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">Transformações de espaço de câmera</a></p></td>
<td align="left"><p>Os vértices no espaço da câmera são calculados transformando os vértices de objeto com a matriz de visualização do mundo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">Fator de atenuação e destaque</a></p></td>
<td align="left"><p>Os componentes de iluminação especular e difusa da equação de iluminação global contêm termos que descrevem a atenuação de luz e o cone em destaque.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Materiais e luzes](lights-and-materials.md)

 

 




