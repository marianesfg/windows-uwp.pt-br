---
title: Fator de atenuação e destaque
description: Os componentes de iluminação especular e difusa da equação de iluminação global contêm termos que descrevem a atenuação de luz e o cone em destaque.
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- Fator de atenuação e destaque
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8126ac8fa738a2b8a9680d215179fe23f77c5d44
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659291"
---
# <a name="attenuation-and-spotlight-factor"></a>Fator de atenuação e destaque


Os componentes de iluminação especular e difusa da equação de iluminação global contêm termos que descrevem a atenuação de luz e o cone em destaque. Esses termos são descritos a seguir.

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>Atenuação


A atenuação de uma luz depende do tipo de luz e da distância entre a luz e a posição do vértice. Para calcular a atenuação, use uma das seguintes equações.

Atten = 1 / (att0<sub>eu</sub> + att1<sub>eu</sub> \* 1!d + att2<sub>eu</sub> \* d²)

Onde:

| Parâmetro        | Valor padrão | Tipo           | Descrição                                     | Intervalo          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | Ponto flutuante | Fator de atenuação constante                     | 0 para + infinito |
| att1<sub>i</sub> | 0.0           | Ponto flutuante | Fator de atenuação linear                       | 0 para + infinito |
| att2<sub>i</sub> | 0.0           | Ponto flutuante | Fator de atenuação quadrática                    | 0 para + infinito |
| d.                | N/D           | Ponto flutuante | Distância da posição de vértice até a posição da luz | N/D            |

 

-   Atten = 1, se a luz é uma luz direcional.
-   Atten = 0, se a distância entre a luz e o vértice exceder o intervalo da luz.

A distância entre a luz e a posição do vértice é sempre positiva.

d = | L<sub>dir</sub> |

Onde:

| Parâmetro       | Valor padrão | Tipo                                             | Descrição                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | N/D           | Vetor 3D com x, y e valores de ponto flutuante de z | Vetor de direção da posição de vértice para a posição da luz |

 

Se d é maior do que o intervalo da luz, o Direct3D não faz nenhum cálculo de atenuação adicional e não aplica nenhum efeito da luz ao vértice.

As constantes de atenuação atuam como coeficientes na fórmula - você poderá produzir uma variedade de curvas de atenuação fazendo ajustes simples nelas. Você pode definir Attenuation1 como 1.0 para criar uma luz sem atenuação, mas que ainda é limitada por intervalo, ou você pode fazer experiências com valores diferentes para obter vários efeitos de atenuação.

A atenuação no intervalo máximo da luz não é 0.0. Para impedir que luzes apareçam repentinamente quando estiverem no intervalo de luz, um app pode aumentar o intervalo de luz. Ou então, o app pode configurar constantes de atenuação para que o fator de atenuação fique próximo de 0.0 no intervalo de luz. O valor de atenuação é multiplicado pelos componentes em vermelho, verde e azul de cor da luz para dimensionar a intensidade da luz, à medida que um fator de luz de distância transita para um vértice.

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>Fator de destaque


A seguinte equação especifica o fator de destaque.

![equação de fator de destaque](images/dx8light9.png)

| Parâmetro         | Valor padrão | Tipo           | Descrição                              | Intervalo                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | N/D           | Ponto flutuante | cosseno(ângulo) para destaque i            | N/D                      |
| phi<sub>i</sub>   | 0.0           | Ponto flutuante | Ângulo de penumbra de destaque i em radianos | \[teta<sub>eu</sub>, pi) |
| theta<sub>i</sub> | 0.0           | Ponto flutuante | Ângulo de penumbra de destaque i em radianos    | \[0, pi)                 |
| queda           | 0.0           | Ponto flutuante | Fator de queda                           | (-infinito + infinito)   |

 

Onde:

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

e:

| Parâmetro       | Valor padrão | Tipo                                             | Descrição                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | N/D           | Vetor 3D com x, y e valores de ponto flutuante de z | O negativo da direção da luz no espaço da câmera         |
| L<sub>dir</sub> | N/D           | Vetor 3D com x, y e valores de ponto flutuante de z | Vetor de direção da posição de vértice para a posição da luz |

 

Depois de computar a atenuação da luz, para calcular os componentes especulares e difusos do vértice, o Direct3D também considera os efeitos de destaque, se aplicável, o ângulo que reflete a luz de uma superfície e a reflexão do material atual. Em [Tipos de luz](light-types.md), consulte "Em destaque".

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Cálculos de iluminação](mathematics-of-lighting.md)

 

 




