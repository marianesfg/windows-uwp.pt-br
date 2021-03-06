---
title: Exemplo
description: A amostragem é o processo de leitura de valores de entrada de uma textura ou outros recursos. A \ 0034;amostra \ 0034; é qualquer objeto que lê a partir de recursos.
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- Exemplo
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e80e160e1225e510ab95f52cbd9f21049c890457
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663651"
---
# <a name="sampler"></a>Exemplo


A amostragem é o processo de leitura de valores de entrada de uma textura ou outros recursos. Uma "amostra" é qualquer objeto que lê a partir de recursos.

Há muitos problemas e artefatos da amostragem de uma textura e renderização para uma área da tela. Por exemplo, se a área a ser renderizado para for de 50 por 50 pixels e a textura for de 16 x 16 pixels ou 256 por 256 pixels, então um alongamento ou encolhimento considerável da textura precisará ser aplicado. Como essa incompatibilidade de tamanho leva a artefatos indesejados, técnicas de filtragem são usadas para atenuar esses artefatos. Uma abordagem comum de filtragem ao uso de texturas pequenas para áreas maiores de renderização é a filtragem "bilinear".

Outro problema ocorre quando a área que está sendo renderizada está em um ângulo muito oblíquo ao modo de exibição (por exemplo, uma textura de 256 por 256 sendo renderizada para uma área de 100 pixels, mas com profundidade de apenas 5 pixels devido ao ângulo de visualização). Neste caso, a filtragem "anisotrópica" geralmente é aplicada. A filtragem anisotrópica fornece melhor qualidade de imagem que a filtragem bilinear, à medida que ela remove os efeitos de suavização sem desfoque excessivo, mas é mais dispendiosa em termos computacionais.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Exibições](views.md)

 

 




