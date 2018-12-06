---
title: Vetores, vértices e quatérnios
description: Em todo o Direct3D, os vértices descrevem a posição e a orientação. Cada vértice em um primitivo é descrito por um vetor que oferece suas coordenadas de textura, cor e posição, e um vetor normal que oferece a sua orientação.
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- Vetores, vértices e quatérnios
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8942e53b7372e2e8b3cf4ed05f89b4187bdfc4be
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8746113"
---
# <a name="vectors-vertices-and-quaternions"></a>Vetores, vértices e quatérnios


Em todo o Direct3D, os vértices descrevem a posição e a orientação. Cada vértice em um primitivo é descrito por um vetor que oferece suas coordenadas de textura, cor e posição, e um vetor normal que oferece a sua orientação.

Os quatérnios adicionam um quarto elemento ao valores \[x, y, z\] que definem um vetor de três componentes. Os quatérnios são uma alternativa aos métodos de matriz que são normalmente usados para rotações 3D. Um quatérnio representa um eixo no espaço 3D e uma rotação em torno do eixo em questão. Por exemplo, um quatérnio pode representar um eixo (1,1,2) e uma rotação de 1 radiano. Os quatérnios transportam informações úteis, mas seu verdadeiro valor é proveniente das duas operações que você pode executar neles: composição e interpolação.

Realizar a composição em quatérnios é semelhante a combiná-los. A composição de dois quatérnios é representada como a ilustração a seguir.

![Ilustração de notação de quatérnio](images/quateq.png)

A composição de dois quatérnios aplicados a uma geometria significa "girar a geometria ao redor do eixo₂ com rotação₂ e, em seguida, girá-la ao redor do eixo₁ com rotação₁." Nesse caso, Q representa uma rotação em torno de um eixo único que é o resultado da aplicação de q₂, e, em seguida, q₁ à geometria.

Usando a interpolação de quatérnio, um aplicativo pode calcular um caminho suave e razoável de um eixo e orientação para outro. Portanto, a interpolação entre q₁ e q₂ fornece uma maneira simples de fazer a animação de uma orientação para outra.

Quando você usa composição e interpolação juntos, eles fornecem uma maneira simples de manipular uma geometria de maneira que se pareça complexa. Por exemplo, imagine que você tenha uma geometria que você deseja girar para uma determinada orientação. Você sabe que você deseja girá-la r₂ graus em torno do eixo₂, em seguida, girá-la r₁ graus em torno do eixo₁, mas você não sabe o quatérnio final. Usando a composição, você pode combinar as duas rotações na geometria para obter um único quaternion que é o resultado. Em seguida, você pode interpolar do original para o quaternion composto para atingir uma transição suave de um para outro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




