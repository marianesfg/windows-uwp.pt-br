---
title: Modo de exibição de recurso de sombreador (SRV) e modo de exibição de acesso não ordenado (UAV)
description: Os modos de exibição de recurso de sombreador geralmente envolvem as texturas em um formato que pode ser acessado pelos sombreadores. Um modo de exibição de acesso não ordenado fornece funcionalidade semelhante, mas permite a leitura e a gravação na textura (ou outro recurso) em qualquer ordem.
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- Exibição de recurso de sombreador (SRV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd6cca08c0845e2da5420fc1f040cc285c87953d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636041"
---
# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>Modo de exibição de recurso de sombreador (SRV) e modo de exibição de acesso não ordenado (UAV)


Os modos de exibição de recurso de sombreador geralmente envolvem as texturas em um formato que pode ser acessado pelos sombreadores. Um modo de exibição de acesso não ordenado fornece funcionalidade semelhante, mas permite a leitura e a gravação na textura (ou outro recurso) em qualquer ordem.

A quebra automática de uma textura é provavelmente a forma mais simples de exibição de recurso de sombreador. Exemplos mais complexos seriam uma coleção de sub-recursos (matrizes individuais, planos ou cores de uma textura mipmapped), texturas 3D, gradientes de cor de textura 1D e assim por diante.

Os modos de exibição de acesso não ordenados são um pouco mais dispendiosos em termos de desempenho, mas permitem, por exemplo, que uma textura seja gravada ao mesmo tempo em que é lida. Isso permite que a textura atualizada seja reutilizada pelo pipeline de elementos gráficos para alguma outra finalidade. Os modos de exibição de recurso de sombreador são somente para leitura (que é o uso mais comum de recursos).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Exibições](views.md)

 

 




