---
title: "Mapas de luzes monocromáticas"
description: "O mapeamento de luzes monocromáticas permite que os adaptadores mais antigos misturem a textura de passagem múltipla, quando uma placa de aceleradora 3D mais antiga não oferece suporte à mesclagem de textura com o valor de alfa do pixel de destino."
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords: "Mapas de luzes monocromáticas"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67029443f59d87c7f3e38560595dca1b7b399632
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="monochrome-light-maps"></a>Mapas de luzes monocromáticas


O mapeamento de luzes monocromáticas permite que os adaptadores mais antigos misturem a textura de passagem múltipla, quando uma placa de aceleradora 3D mais antiga não oferece suporte à mesclagem de textura com o valor de alfa do pixel de destino.

Algumas placas aceleradoras 3D mais antigas não oferecem suporte à mesclagem de textura com o valor de alfa do pixel de destino. Em geral, esses adaptadores também não oferecem suporte à mistura de múltipla textura. Se o app for executado em um adaptador desse tipo, ele pode usar a mistura da textura de passagem múltipla para executar o mapeamento de luzes monocromáticas.

Para executar o mapeamento de luzes monocromáticas, um app armazena as informações de iluminação nos dados de alfa das texturas de mapa de luzes. O app usa os recursos de filtragem de textura do Direct3D para realizar um mapeamento de cada pixel na imagem do primitivo para um texel correspondente no mapa de luz. Ele define o fator de mesclagem de origem para o valor de alfa do texel correspondente.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Mapeamento de luzes com texturas](light-mapping-with-textures.md)

 

 




