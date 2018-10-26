---
title: Conceitos básicos de texturização
description: As imagens 3D geradas por computador antigamente, embora geralmente avançadas para o tempo, tendiam a ter uma aparência de plástico brilhante.
ms.assetid: 3CA3905D-E837-48EB-A81F-319AA1C6537E
keywords:
- Conceitos básicos de texturização
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c48b7b007c1af1eaf6013d5f6e99468713d50745
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562267"
---
# <a name="basic-texturing-concepts"></a>Conceitos básicos de texturização


As imagens 3D geradas por computador de antigamente, embora geralmente avançadas para o tempo, tendiam a ter uma aparência de plástico brilhante. Eles não tinham os tipos de marcações como arranhões, rachaduras, impressões digitais e manchas da tela que dão aos objetos 3D uma complexidade visual realística. As texturas se tornaram populares para o aprimoramento do realismo das imagens 3D geradas por computador.

No seu uso diário, a textura word se refere à suavidade ou aspereza de um objeto. Em gráficos de computador, no entanto, uma textura será um bitmap de cores de pixels que dá a aparência de textura de um objeto.

Como as texturas do Direct3D são bitmaps, qualquer bitmap pode ser aplicado a uma primitiva do Direct3D. Por exemplo, os apps podem criar e tratar os objetos que aparecem com um padrão de madeira neles. Grama, sujeira e rochas podem ser aplicadas a um conjunto de primitivas 3D que formam uma colina. O resultado é uma colina realista. Você também pode usar a textura para criar efeitos como sinais ao longo de um acostamento, estratos de rocha em um penhasco, ou a aparência de mármore no chão.

Além disso, o Direct3D dá suporte a técnicas mais avançadas de texturização como a mesclagem de textura com ou sem transparência e o mapeamento de luz. Consulte [Mistura de textura](texture-blending.md) e [Mapeamento de luz com texturas](light-mapping-with-textures.md).

Se seu app criar um dispositivo de camada de abstração de hardware (HAL) ou um dispositivo de software, ele pode usar texturas de 8, 16, 24, 32, 64 ou 128 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




