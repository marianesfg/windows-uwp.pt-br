---
title: Mapas de luzes especulares
description: Quando iluminado por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares.
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords:
- Mapas de luzes especulares
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6bd9db0afa914ef7a56dbd55c938129b86a43743
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5755317"
---
# <a name="specular-light-maps"></a>Mapas de luzes especulares


Quando iluminados por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares. Às vezes, você pode obter realces mais precisos aplicando mapas de luz especular aos primitivos, em vez de usar os realces especulares produzidos pelo módulo de iluminação.

Para executar o mapeamento de luz especular, adicione o mapa de luz especular à textura do primitivo e module (multiplique o resultado por) o mapa de luz RGB.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamento de luzes com texturas](light-mapping-with-textures.md)

 

 




