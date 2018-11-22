---
title: Objetos de estado
description: O estado do dispositivo é agrupado em objetos de estado que reduzem enormemente o custo de alterações de estado. Há vários objetos de estado, e cada um deles é projetado para inicializar um conjunto de estado para um estágio de pipeline específico. Objetos de estado variam de acordo com a versão do Direct3D.
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- Objetos de estado
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3b0333d77e635961d51d3d5bb45cdf2def646fb8
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7573190"
---
# <a name="state-objects"></a>Objetos de estado


O estado do dispositivo é agrupado em objetos de estado que reduzem enormemente o custo de alterações de estado. Há vários objetos de estado, e cada um deles é projetado para inicializar um conjunto de estado para um estágio de pipeline específico. Objetos de estado variam de acordo com a versão do Direct3D.

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>Estado de layout de entrada


Esse grupo de estado determina como o [estágio do Assembler de Entrada (IA)](input-assembler-stage--ia-.md) lê os dados fora dos buffer de entrada e os monta para serem usados pelo sombreador de vértice. Isso inclui o estado, como o número de elementos no buffer de entrada e a assinatura dos dados de entrada. O estágio do Assembler de Entrada (IA) flui os primitivos da memória para o pipeline.

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>Estado do rasterizador


Esse grupo de estado inicializa o [estágio do Rasterizador (RS)](rasterizer-stage--rs-.md). Esse objeto inclui o estado como modos preenchimento ou eliminação, permitindo o recorte de um retângulo de corte e a configuração de parâmetros de várias amostras. Este estágio rasteriza os primitivos em pixels, realizando operações como recorte e mapeamento de primitivos no visor.

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>Estado do estêncil de profundidade


Esse grupo de estado inicializa a parte do estêncil de profundidade do [estágio de Fusão de Saída (OM)](output-merger-stage--om-.md). Mais especificamente, esse objeto inicializa os testes de estêncil e profundidade.

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>Estado de mesclagem


Esse grupo de estado inicializa a parte de mesclagem do [estágio de Fusão de Saída (OM)](output-merger-stage--om-.md).

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>Estado de amostra


Esse grupo de estado inicializa um objeto de amostra. Um objeto de amostra é usado pelos estágios de sombreador para filtrar texturas na memória.

No Direct3D, o objeto de amostra não está vinculado a uma textura específica, ele simplesmente descreve como fazer a filtragem dado qualquer recurso anexado.

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Considerações sobre desempenho


Projetar a API para usar objetos de estado cria várias vantagens de desempenho. Isso inclui validação de estado no momento da criação de objeto, habilitação do cache de objetos de estado no hardware e redução considerável da quantidade de estado que é passada durante uma chamada de API de configuração de estado (passando um manipulador para o objeto de estado em vez do estado).

Para obter essas melhorias de desempenho, é necessário criar os objetos de estado quando seu aplicativo for inicializado, logo antes do seu loop de renderização. Os objetos de estado são imutáveis, ou seja, depois que são criados, não podem ser alterados. Em vez disso, você deve destruí-los ou recriá-los.

Você pode criar vários objetos de amostra com várias combinações de estado de amostra. A alteração do estado de amostra é então realizada chamando a API de "Configuração" apropriada, que por sua vez passa um manipulador para o objeto (em vez do estado de amostra). Isso reduz significativamente a quantidade de sobrecarga durante cada quadro de renderização para alterar o estado, uma vez que o número de chamadas e a quantidade de dados são consideravelmente reduzidos.

Como alternativa, você pode optar por usar o sistema de efeito que gerenciará automaticamente a criação e destruição eficientes de objetos de estado para seu aplicativo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

[Modos de exibição](views.md)

 

 




