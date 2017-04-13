---
title: Mapas de luzes especulares
description: Quando iluminado por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares.
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords: Mapas de luzes especulares
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 4e2037bff175484aab10d8c650647fd26ed5e6fd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="specular-light-maps"></a>Mapas de luzes especulares


Quando iluminado por uma fonte de luz, os objetos brilhantes que usam materiais altamente refletores recebem realces especulares. Às vezes, você pode obter realces mais precisos aplicando mapas de luz especular aos primitivos, em vez de usar os realces especulares produzidos pelo módulo de iluminação.

Para executar o mapeamento de luz especular, adicione o mapa de luz especular à textura do primitivo e module (multiplique o resultado por) o mapa de luz RGB.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamento de luzes com texturas](light-mapping-with-textures.md)

 

 




