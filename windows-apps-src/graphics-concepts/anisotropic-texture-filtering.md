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
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6f4b74d64b7a3f9768697b5b6f495a322686c59
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1042595"
---
# <a name="anisotropic-texture-filtering"></a>Filtragem anisotrópica de textura


*Anisotropia* é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida. O Direct3D mede a anisotropia de um pixel como o prolongamento - ou seja, comprimento dividido pela largura - de um pixel de tela é mapeado inverso no espaço de textura.

Você pode usar a filtragem de textura anisotrópica em conjunto com filtragem de textura linear ou a filtragem de textura mipmap para melhorar os resultados da renderização.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Filtragem de textura](texture-filtering.md)

 

 



