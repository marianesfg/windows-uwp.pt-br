---
title: Estágio do rasterizador (RS)
description: O rasterizador recorta primitivas que não estão exibidas, prepara-as para o estágio do sombreador de pixel (PS) e determina como invocar os sombreadores de pixel.
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- Estágio do rasterizador (RS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 499d361bddbecef50ee8cdf44b56530a98cfccd1
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8323590"
---
# <a name="rasterizer-rs-stage"></a>Estágio do rasterizador (RS)


O rasterizador recorta primitivas que não estão exibidas, prepara-as para o [estágio do sombreador de pixel (PS)](pixel-shader-stage--ps-.md) e determina como invocar os sombreadores de pixel. O estágio de rasterização converte as informações de vetor (compostas de formas ou primitivas) em uma imagem de rasterizador (composta de pixels) para fins de exibição de gráficos 3D em tempo real.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


Durante a rasterização, cada primitiva é convertida em pixels, enquanto interpola valores por vértice entre cada primitiva. A rasterização inclui o recorte de vértices para o tronco de exibição, execução de uma divisão por z para fornecer perspectiva, mapeamento de primitivas para um visor 2D e determinação de como invocar o sombreador de pixel. Embora o uso de um sombreador de pixel seja opcional, o estágio do rasterizador sempre realiza recorte, uma divisão de perspectiva para transformar os pontos em espaço homogêneo e mapeia os vértices para o visor.

Você pode desabilitar a rasterização informando o pipeline de que não há sombreador de pixel (defina o [estágio do sombreador de pixel (PS)](pixel-shader-stage--ps-.md) para **NULL** e desabilite o teste de profundidade e de estêncil). Enquanto estão desabilitados, os contadores do pipeline relacionados à rasterização não serão atualizados.

No hardware que implementa as otimizações de buffer de Z hierárquicas, você pode habilitar o pré-carregamento de buffer de z definindo o estágio de sombreador de pixel (PS) como **NULL** e permitindo teste de profundidade e de estêncil.

Consulte [Regras de rasterização](rasterization-rules.md).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Presume-se que vértices (x, y, z, w) chegando ao estágio de rasterizador estejam em espaço de recorte homogêneo. Nesse espaço de coordenadas, o eixo X aponta para a direita, o Y aponta para cima e Z aponta para longe da câmera.

O estágio de rasterizador (RS) de função fixa é alimentado pelo estágio de saída de fluxo (SO) e/ou pelo estágio de pipeline anterior, como o [estágio do sombreador de geometria (GS)](geometry-shader-stage--gs-.md). Se GS não for usado, RS será alimentado pelo [estágio do sombreador de domínio (DS)](domain-shader-stage--ds-.md). Se DS também não for usado, RS será alimentado pelo [estágio do sombreador de vértice (VS)](vertex-shader-stage--vs-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


Usar o estágio de sombreador de pixel (PS) é opcional. O estágio do rasterizador pode gerar saída diretamente para o [estágio de fusão de saída (OM)](output-merger-stage--om-.md) em vez disso.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Regras de rasterização](rasterization-rules.md)

[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




