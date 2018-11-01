---
title: Interpolação de triângulo
description: Durante a renderização, o pipeline interpola os dados de vértice em cada triângulo.
ms.assetid: 1A76DD78-CED7-42BE-BA81-B9050CD3AF9B
keywords:
- Interpolação de triângulo
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 56ce3520248a0fca25230d7ee2a822d827d842a3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876366"
---
# <a name="triangle-interpolation"></a>Interpolação de triângulo


Durante a renderização, o pipeline interpola os dados de vértice em cada triângulo. Os dados de vértice podem ser uma ampla variedade de dados e podem incluir (mas sem estarem limitados a isso): cor difusa, cor especular, alfa difuso (opacidade de triângulo), alfa especular, e fator de nevoeiro. Para o pipeline programável do vértice, o fator de nevoeiro é obtido a partir do registro de nevoeiro. Para o pipeline de funções fixas do vértice, o fator de nevoeiro é obtido a partir do alfa especular.

Para alguns dados de vértice, a interpolação é dependente do modo atual de sombreamento, da seguinte maneira:

| Modo de sombreamento | Descrição                                                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Plano         | Somente o fator de nevoeiro é interpolado no modo de sombreamento simples. Para todos os outros valores interpolados, a cor do primeiro vértice no triângulo é aplicada em toda a face. |
| Gouraud      | A interpolação linear é executada entre todos os três vértices.                                                                                                               |

 

As cores difusas e realces especulares são tratados de forma diferente, dependendo do modelo de cor. No modelo de cor RGB, o sistema usa os componentes de cor vermelha, verde e azul na interpolação.

O componente alfa de uma cor é tratado como um valor interpolado separado porque drivers de dispositivo podem implementar transparência de duas maneiras diferentes: utilizando uma mesclagem de textura ou usando textura pontilhada.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




