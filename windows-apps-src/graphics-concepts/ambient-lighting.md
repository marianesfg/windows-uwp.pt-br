---
title: Iluminação ambiente
description: A iluminação ambiente fornece iluminação constante para uma cena.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- Iluminação ambiente
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ac958a93fcafbb33a9025196b49398e2e3269e55
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291834"
---
# <a name="ambient-lighting"></a>Iluminação ambiente

A iluminação ambiente fornece iluminação constante para uma cena. Ele destaca todos os vértices de objeto da mesma forma porque ela não depende de quaisquer outros fatores de iluminação como normais de vértice, direção da luz, posição da luz, intervalo ou atenuação. A iluminação ambiente é constante em todas as direções e fornece cor de forma igual para todos os pixels de um objeto. Ela é rápida de calcular, mas deixa os objetos com uma aparência simples e irreal.

A iluminação ambiente é o tipo mais rápido de iluminação, mas produz resultados menos realistas. O Direct3D contém uma única propriedade de luz ambiente global que você pode usar sem criar qualquer luz. Como alternativa, você pode configurar qualquer objeto de luz para fornecer iluminação ambiente.

A iluminação ambiente para uma cena é descrita pela seguinte equação.

Luz ambiente = Cₐ\*\[Gₐ + sum (Atten<sub>eu</sub>\*especiais<sub>eu</sub>\*L<sub>ai</sub>)\]

Onde:

| Parâmetro         | Valor padrão | Tipo          | Descrição                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | Cor ambiente do material                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | Cor ambiente global                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | Atenuação de luz da luz ith. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md). |
| Ponto<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | Fator de destaque da luz ith. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md).  |
| soma               | N/D           | N/D           | Soma da luz ambiente                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | Cor ambiente da luz da luz ith                                                                              |

 

O valor de Cₐ é:

-   vértice color1, se AMBIENTMATERIALSOURCE = D3DMCS\_COLOR1 e a primeira cor do vértice é fornecido na declaração de vértice.
-   vértice cor2, se AMBIENTMATERIALSOURCE = D3DMCS\_cor2 e a segunda cor de vértice é fornecido na declaração de vértice.
-   cor ambiente do material.

**Observação**    se qualquer opção AMBIENTMATERIALSOURCE for usada e a cor do vértice não for fornecida, a cor ambiente material é usada.

 

Para usar a cor ambiente do material, use SetMaterial, conforme mostrado no exemplo de código abaixo.

Gₐ é a cor ambiente global. Ele é definido usando SetRenderState (D3DRS\_ambientes). Há uma cor ambiente global em uma cena do Direct3D. Este parâmetro não está associado um objeto de luz do Direct3D.

L<sub>ai</sub> é a cor ambiente da luz ith na cena. Cada luz do Direct3D tem um conjunto de propriedades, uma delas é a cor ambiente. O termo, sum(L<sub>ai</sub>) é a soma de todas as cores ambiente na cena.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Neste exemplo, o objeto é colorido usando a luz ambiente da cena e uma cor ambiente de material.

```cpp
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

De acordo com a equação, a cor resultante para os vértices do objeto é uma combinação da cor do material e da cor da luz.

As duas ilustrações a seguir mostram a cor de material, que é cinza, e a cor de luz, que é vermelho brilhante.

![ilustração de uma esfera cinza](images/amb1.jpg)![ilustração de uma esfera vermelha](images/lightred.jpg)

A cena resultante é mostrada na ilustração a seguir. O único objeto na cena é uma esfera. A luz ambiente destaca todos os vértices do objeto com a mesma cor. Não é dependente da normal de vértice ou da direção da luz. Como resultado, o círculo se parece com um círculo 2D porque não há nenhuma diferença na sombreamento ao redor a superfície do objeto.

![ilustração de uma esfera com iluminação](images/lighta.jpg)

Para dar uma aparência mais realista aos objetos, aplique iluminação especular ou difusa além de iluminação.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Cálculos de iluminação](mathematics-of-lighting.md)

 

 




