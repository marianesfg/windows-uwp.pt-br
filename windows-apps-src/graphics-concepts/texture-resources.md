---
title: Recursos de textura
description: Texturas são um tipo de recurso usado para renderização.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- Recursos de textura
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72f58521e01d46437ba44453b94d12a82bb3e639
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436765"
---
# <a name="texture-resources"></a>Recursos de textura


Texturas são um tipo de recurso usado para renderização.

## <a name="span-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Renderizando com recursos de textura


O Direct3D dá suporte à mesclagem de várias texturas pelo conceito de estágios de textura. Cada estágio de textura contém uma textura e as operações que podem ser executadas na textura. As texturas nos estágios de textura formam o conjunto de texturas atuais. Consulte [Mesclagem de texturas](texture-blending.md). O estado de cada textura é encapsulado em seu estágio de textura.

Seu aplicativo também pode definir a perspectiva da textura e os estados de filtragem de textura. Consulte [Filtragem de textura](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




