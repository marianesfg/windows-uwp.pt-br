---
title: Iluminação difusa
description: A iluminação difusa depende da direção da luz e da normal da superfície do objeto.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- Iluminação difusa
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1785b06aa2217e8ec15aeaa560bd98a65522df2e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8197651"
---
# <a name="diffuse-lighting"></a>Iluminação difusa


A *iluminação difusa* depende da direção da luz e da normal da superfície do objeto. A iluminação difusa varia entre a superfície de um objeto como resultado da alteração da direção da luz e da alteração do vetor numeral da superfície. É necessário mais tempo para calcular a iluminação difusa porque ela é alterada para cada vértice de objeto, mas a vantagem de usá-la é que ela sombreia objetos e dá a eles profundidade tridimensional (3D).

Depois de ajustar a intensidade de luz para efeitos de atenuação, o mecanismo de iluminação calcula quanto da luz restante reflete de um vértice, dado o ângulo da normal do vértice e a direção da luz incidente. O mecanismo de iluminação ignora essa etapa para luzes direcionais, porque elas não são atenuadas com a distância. O sistema considera dois tipos de reflexão, difusa e especular, e usa uma fórmula diferente para determinar a quantidade de luz que é refletida para cada um.

Depois que calcular os valores de luz refletida, o Direct3D aplica esses novos valores às propriedades de reflexão difusa e especular do material atual. Os valores de cor resultantes são os componentes difusos e especulares que o rasterizador usa para produzir o sombreamento Gouraud e o realce especular.

A iluminação difusa é descrita pela seguinte equação.

Iluminação difusa = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| Parâmetro       | Valor padrão | Tipo          | Descrição                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| soma             | N/A           | N/D           | Referência de cada componente difuso da luz.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Cor difusa.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Cor difusa clara.                                                                             |
| N               | N/D           | D3DVECTOR     | Normal de vértice                                                                                    |
| L<sub>dir</sub> | N/D           | D3DVECTOR     | Vetor de direção do vértice de objeto à luz.                                                |
| Atten           | N/D           | FLUTUANTE         | Atenuação de luz. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md). |
| Ponto            | N/D           | FLUTUANTE         | Fator de destaque. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md).  |

 

Para calcular a atenuação (Atten) ou as características de destaque (ponto), consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md).

Componentes difusos são vinculados para estar entre 0 e 255, depois que todas as luzes são processadas e interpoladas separadamente. O valor de iluminação difusa resultante é uma combinação dos valores de luz ambiente, difusa e emissiva.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Neste exemplo, o objeto é colorido usando a cor difusa clara e uma cor difusa de material.

De acordo com a equação, a cor resultante para os vértices do objeto é uma combinação da cor do material e da cor da luz.

As duas ilustrações a seguir mostram a cor de material, que é cinza, e a cor de luz, que é vermelho brilhante.

![ilustração de uma esfera cinza](images/amb1.jpg)![ilustração de uma esfera vermelha](images/lightred.jpg)

A cena resultante é mostrada na ilustração a seguir. O único objeto na cena é uma esfera. O cálculo de iluminação difusa considera a cor difusa da luz e do material e a modifica pelo ângulo entre a direção da luz e a normal do vértice usando o produto de ponto. Como resultado, o verso da esfera obtém fica escuro à medida que a superfície das curvas da esfera se afasta da luz.

![ilustração de uma esfera com iluminação difusa](images/lightd.jpg)

Combinar a iluminação difusa com a iluminação ambiente dos exemplos anteriores sobreará toda a superfície do objeto. A luz ambiente sombreia toda a superfície e a luz difusa ajuda a revelar a forma do objeto 3D, conforme mostrado na ilustração a seguir.

![Ilustração de uma esfera com iluminação difusa e iluminação ambiente](images/lightad.jpg)

A iluminação difusa é mais intensa para se calcular do que a iluminação ambiente. Como ela depende das normais de vértice e da direção da luz, você pode ver a geometria de objetos no espaço 3D, o que produz uma iluminação mais realística do que a iluminação ambiente. Você pode usar realces especulares para obter uma aparência mais realista.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Matemática de iluminação](mathematics-of-lighting.md)

 

 




