---
title: "Filtragem anisotrópica de textura"
description: "Anisotropia é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida."
ms.assetid: 58923809-EF76-4C16-BCE7-922A66425F83
keywords:
- "Filtragem anisotrópica de textura"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4946281fd754f267b37ee0a069d5101f8169deff
ms.lasthandoff: 02/07/2017

---

# <a name="anisotropic-texture-filtering"></a>Filtragem anisotrópica de textura


*Anisotropia* é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida. O Direct3D mede a anisotropia de um pixel como o prolongamento - ou seja, comprimento dividido pela largura - de um pixel de tela é mapeado inverso no espaço de textura.

Você pode usar a filtragem de textura anisotrópica em conjunto com filtragem de textura linear ou a filtragem de textura mipmap para melhorar os resultados da renderização.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 





