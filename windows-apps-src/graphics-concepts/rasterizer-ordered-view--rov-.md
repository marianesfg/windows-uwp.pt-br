---
title: Modo de exibição organizado rasterizado (ROV)
description: Modos de exibição ordenados por rasterizador permitem lidar com algumas das limitações de um buffer de profundidade, em particular ter várias texturas contendo transparência e todas aplicadas aos mesmos pixels.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- Modo de exibição organizado rasterizado (ROV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7327304e2b42ff5ff71be136220b58e99c6228d2
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996899"
---
# <a name="rasterizer-ordered-view-rov"></a>Modo de exibição organizado rasterizado (ROV)


Modos de exibição ordenados por rasterizador permitem lidar com algumas das limitações de um buffer de profundidade, em particular ter várias texturas contendo transparência e todas aplicadas aos mesmos pixels.

Modos de exibição ordenados por rasterizador permitem que algoritmos de "Transparência independente de ordem" (OIT) sejam aplicadas à renderização de pixels. Um buffer de profundidade permite que apenas um pixel seja desenhado ou obstruído, não há conceito de obstrução parcial por meio de transparência. Algoritmos OIT aplicam texturas transparentes na ordem correta, portanto, se, por exemplo, um objeto de vidro transparente deve aparecer atrás de uma janela de vidro que fica atrás de vegetação que use texturas transparentes, então o resultado final será desenhado previsivelmente correto. Sem algoritmos ROVs e OIT, a ordem em que esses objetos transparentes seriam desenhados seria imprevisível e uma cena renderizada simplesmente poderia ficar confusa e errada.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




