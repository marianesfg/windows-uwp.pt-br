---
title: Mapas de luzes especulares
description: Quando iluminado por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares.
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords: Mapas de luzes especulares
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6da5292396a334e50c61fb4638334aae27581d7d
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="specular-light-maps"></a>Mapas de luzes especulares


Quando iluminado por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares. Às vezes, você pode obter realces mais precisos aplicando mapas de luz especular aos primitivos, em vez de usar os realces especulares produzidos pelo módulo de iluminação.

Para executar o mapeamento de luz especular, adicione o mapa de luz especular à textura do primitivo e module (multiplique o resultado por) o mapa de luz RGB.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamento de luzes com texturas](light-mapping-with-textures.md)

 

 




