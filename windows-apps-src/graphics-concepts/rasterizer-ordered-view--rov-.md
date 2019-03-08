---
title: Modo de exibição organizado rasterizado (ROV)
description: Rasterizar modos de exibição ordenados permite que algumas das limitações de um buffer de profundidade sejam abordadas, especialmente tendo várias texturas que contém transparência sendo aplicadas aos mesmos pixels.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- Modo de exibição organizado rasterizado (ROV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ed49ba81c44a799e4d827d636f601c77d01333b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603591"
---
# <a name="rasterizer-ordered-view-rov"></a>Modo de exibição organizado rasterizado (ROV)


Rasterizar modos de exibição ordenados permite que algumas das limitações de um buffer de profundidade sejam abordadas, especialmente tendo várias texturas que contém transparência sendo aplicadas aos mesmos pixels.

Modos de exibição ordenados por rasterizador permitem que algoritmos de "Transparência independente de ordem" (OIT) sejam aplicadas à renderização de pixels. Um buffer de profundidade permite que apenas um pixel seja desenhado ou obstruído, não há conceito de obstrução parcial por meio de transparência. Algoritmos OIT aplicam texturas transparentes na ordem correta, portanto, se, por exemplo, um objeto de vidro transparente deve aparecer atrás de uma janela de vidro que fica atrás de vegetação que use texturas transparentes, então o resultado final será desenhado previsivelmente correto. Sem algoritmos ROVs e OIT, a ordem em que esses objetos transparentes seriam desenhados seria imprevisível e uma cena renderizada simplesmente poderia ficar confusa e errada.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Modos de exibição](views.md)

 

 




