---
title: Filtragem anisotrópica de textura
description: Anisotropia é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida.
ms.assetid: 58923809-EF76-4C16-BCE7-922A66425F83
keywords:
- Filtragem anisotrópica de textura
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6e91c707b31de859d61ae926518c40812758320e
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597444"
---
# <a name="anisotropic-texture-filtering"></a>Filtragem anisotrópica de textura


*Anisotropia* é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida. O Direct3D mede a anisotropia de um pixel como o prolongamento - ou seja, comprimento dividido pela largura - de um pixel de tela é mapeado inverso no espaço de textura.

Você pode usar a filtragem de textura anisotrópica em conjunto com filtragem de textura linear ou a filtragem de textura mipmap para melhorar os resultados da renderização.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 




