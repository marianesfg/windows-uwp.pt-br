---
title: Iluminação especular
description: A iluminação especular identifica os realces especulares brilhantes que ocorrem quando a luz atinge uma superfície de objeto e reflete volta em direção da câmera.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Iluminação especular
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 283ea63d118f9a61fe745dd3eb60b68594c32279
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570642"
---
# <a name="specular-lighting"></a>Iluminação especular


*A Iluminação especular* identifica os realces especulares brilhantes que ocorrem quando a luz atinge a superfície de um objeto e reflete em direção à câmera. A iluminação especular é mais intensa do que a luz difusa e incide mais rapidamente na superfície do objeto. Demora mais tempo para calcular a iluminação especular que a iluminação difusa, no entanto, a vantagem de usá-lo é que ele adiciona detalhes significativos a uma superfície.

Modelar a reflexão especular requer que o sistema saiba em que direção a luz está viajando e a direção para os olhos do observador. O sistema usa uma versão simplificada do modelo de reflexo especular Phong, que utiliza um vetor de metade para aproximar a intensidade da reflexão especular.

O estado de iluminação padrão não calcula os realces especulares.

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Equação de Iluminação Especular


A iluminação especular é descrita pela seguinte equação.

|                                                                             |
|-----------------------------------------------------------------------------|
| Iluminação especular = Cₛ \ * sum\ [Lₛ \ * (N · H)<sup>P</sup> \ * Atten \ * Spot\] |

 

As variáveis, seus tipos e seus intervalos são:

| Parâmetro    | Valor padrão | Tipo                                                             | Descrição                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | Transparência de vermelho, verde, azul e alfa (valores de ponto flutuante) | Cor especular.                                                                                        |
| soma          | N/D           | N/D                                                              | Referência do componente especular cada da luz.                                                          |
| N            | N/D           | Vetor 3D (x, y e valores de ponto flutuante de z)                    | Normal de vértice.                                                                                         |
| H            | N/D           | Vetor 3D (x, y e valores de ponto flutuante de z)                    | Vetor de metade. Consulte a seção sobre o vetor de metade.                                                |
| <sup>P</sup> | 0.0           | Ponto flutuante                                                   | Potência de reflexão especular. Intervalo é 0 até + infinito                                                     |
| Lₛ           | (0,0,0,0)     | Transparência de vermelho, verde, azul e alfa (valores de ponto flutuante) | Cor especular clara.                                                                                  |
| Atten        | N/D           | Ponto flutuante                                                   | Valor de atenuação clara. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md). |
| Ponto         | N/D           | Ponto flutuante                                                   | Fator de destaque. Consulte [Fator de atenuação e destaque](attenuation-and-spotlight-factor.md).        |

 

O valor de Cₛ é:

-   cor do vértice 1, se a fonte de material especular for cor de vértice difusa, e a primeira cor do vértice for fornecida na declaração de vértice.
-   cor do vértice 2, se a origem de material especular for a cor do vértice especular, e a segunda cor do vértice for fornecido na declaração de vértice.
-   cor especular do material

**Observação**  se qualquer uma das opções de fonte de material especular é usado e a cor do vértice não for fornecida, a cor especular do material é usada.

 

Componentes especulares são vinculados para estar entre 0 e 255, depois que todas as luzes são processadas e interpoladas separadamente.

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>O vetor de metade


O vetor de metade (H) existe situado entre dois vetores: o vetor de um vértice de objeto para a fonte de luz e o vetor de um vértice de objeto para a posição da câmera. O Direct3D fornece duas maneiras de calcular o vetor de metade. Quando os realces especulares relativos à câmera estiverem habilitados (em vez de realces especulares ortogonais), o sistema calcula o vetor de metade usando a posição da câmera e a posição do vértice, juntamente com o vetor de direção da luz. A fórmula a seguir mostra isso.

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| Parâmetro       | Valor padrão | Tipo                                          | Descrição                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₚ              | N/D           | Vetor 3D (x, y e valores de ponto flutuante de z) | Posição da câmera.                                             |
| Vₚ              | N/D           | Vetor 3D (x, y e valores de ponto flutuante de z) | Posição do vértice.                                             |
| L<sub>dir</sub> | N/D           | Vetor 3D (x, y e valores de ponto flutuante de z) | Vetor de direção da posição de vértice para a posição da luz. |

 

Determinar o vetor de metade dessa maneira pode ser computacionalmente intensa. Como alternativa, usar realces especulares ortogonais (em vez de realces especulares relativos à câmera) instrui o sistema a agir como se o ponto de vista fosse infinitamente distante no eixo z. Isso se reflete na seguinte fórmula.

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

Essa configuração é computacionalmente menos intensa, mas muito menos precisa, portanto, é melhor usada por aplicativos que usam a projeção ortogonal.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemplo


Neste exemplo, o objeto é colorido usando a cor da luz especular da cena e uma cor especular de material.

De acordo com a equação, a cor resultante para os vértices do objeto é uma combinação da cor do material e da cor da luz.

A ilustração a seguir mostra a cor do material especular, que é cinza, e a cor da luz especular, que é branca.

![ilustração de uma esfera cinza](images/amb1.jpg)![ilustração de um círculo branco](images/lightwhite.jpg)

O destaque especular resultante é mostrado na ilustração a seguir.

![ilustração do realce especular](images/lights.jpg)

Combinar o realce especular com a iluminação ambiente e difusa produz a ilustração a seguir. Com todos os três tipos de iluminação aplicada, isso mais claramente se parece com um objeto realista.

![ilustração da combinação do realce especular, iluminação ambiente e iluminação difusa](images/lightads.jpg)

A iluminação especular é mais intensa para se calcular do que a iluminação difusa. Ela normalmente é usada para fornecer dicas visuais sobre o material da superfície. O realce especular varia em tamanho e cor com o material da superfície.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Matemática de iluminação](mathematics-of-lighting.md)

 

 




