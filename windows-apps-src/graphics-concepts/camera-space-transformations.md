---
title: "Transformações de espaço de câmera"
description: "Os vértices no espaço da câmera são calculados transformando os vértices de objeto com a matriz de visualização do mundo."
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords: "Transformações de espaço de câmera"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 907df69fdd0c785294283de858a0fcebd0c63513
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="camera-space-transformations"></a>Transformações de espaço de câmera


Os vértices no espaço da câmera são calculados transformando os vértices de objeto com a matriz de visualização do mundo.

V = V \* wvMatrix

As normais de vértice, no espaço da câmera, são calculadas transformando as normais de objeto com a transposição inversa da matriz de exibição do mundo. A matriz de visualização do mundo poderá ou não ser ortogonal.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

A inversão de matrizes e a transposição de matrizes operam em uma matriz de 4 x 4. A multiplicação combina a normal com a parte 3 x 3 da matriz 4 x 4 resultante.

Se o estado de renderização for definido para normalizar normais, os vetores normais de vértice são normalizados após a transformação do espaço da câmera da seguinte maneira:

N = norm(N)

A posição da luz no espaço da câmera é calculada transformando a posição da fonte de luz com a matriz de visualização.

Lₚ = Lₚ \* vMatrix

A direção para a luz no espaço da câmera para uma luz direcional é calculada multiplicando a direção da fonte de luz pela matriz de visualização, normalizando e anulando o resultado.

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

Para um ponto de luz e um destaque, a direção à luz é calculada da seguinte maneira:

L<sub>dir</sub> = norm(V \ * Lₚ), onde os parâmetros são definidos na tabela a seguir.

| Parâmetro       | Valor padrão | Tipo                                          | Descrição                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | N/A           | Vetor 3D (x, y e valores de ponto flutuante de z) | Vetor de direção do vértice de objeto à luz          |
| V               | N/A           | Vetor 3D (x, y e valores de ponto flutuante de z) | Posição do vértice no espaço da câmera                           |
| wvMatrix        | Identidade      | matriz 4 x 4 de valores de ponto flutuante           | Matriz composta contendo as transformções de modo de exibição e mundo |
| N               | N/A           | Vetor 3D (x, y e valores de ponto flutuante de z) | Normal de vértice                                             |
| Lₚ              | N/A           | Vetor 3D (x, y e valores de ponto flutuante de z) | Posição da luz no espaço da câmera                            |
| vMatrix         | Identidade      | matriz 4 x 4 de valores de ponto flutuante           | Matriz que contém a transformação de modo de exibição                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Matemática de iluminação](mathematics-of-lighting.md)

 

 



