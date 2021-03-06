---
title: Mistura de textura com passagem múltipla
description: Os apps Direct3D podem conseguir vários efeitos especiais ao aplicar diversas texturas a um primitivo durante múltiplas passagens de renderização.
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- Mistura de textura com passagem múltipla
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b1e8958874ede50a18f2d2446c8f156361210e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589891"
---
# <a name="multipass-texture-blending"></a>Mistura de textura com passagem múltipla


Os apps Direct3D podem conseguir vários efeitos especiais ao aplicar diversas texturas a um primitivo durante múltiplas passagens de renderização. O termo comum para isso é *mesclagem de texturas de passagem múltipla*. O uso típico da mesclagem de textura com passagens múltiplas é emular os efeitos de iluminação complexos e os modelos de sombreamento ao aplicar várias cores de diversas texturas diferentes. Uma dessas aplicações é chamada de *mapeamento suave*. Consulte [Mapeamento de luz com texturas](light-mapping-with-textures.md).

**Observação**    alguns dispositivos são capazes de aplicar várias texturas a primitivos em uma única passagem. Consulte [Mistura de textura](texture-blending.md).

 

Se o hardware do usuário não oferecer suporte à mistura de textura múltipla, o app pode usar a mistura de textura com passagem múltipla para obter os mesmo efeitos visuais. No entanto, o app não pode manter as taxas de quadros que são possíveis ao usar a mistura de textura múltipla.

Para realizar uma mistura de textura com passagem múltipla em um app C/C++:

1.  Defina uma textura no estágio de textura 0.
2.  Selecione a cor desejada, além dos argumentos e das operações de mistura de alfa. As configurações padrão são adequadas para a mistura de textura com passagem múltipla.
3.  Renderize os objetos adequados na cena.
4.  Defina a textura seguinte no estágio de textura 0.
5.  Defina os estados de renderização para ajustar os fatores de mistura de origem e destino conforme necessário. O sistema combina as novas texturas aos pixels existentes na superfície de destino de renderização de acordo com esses parâmetros.
6.  Repita as etapas 3, 4 e 5 com a quantidade de texturas necessária.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[A combinação de textura](texture-blending.md)

 

 




