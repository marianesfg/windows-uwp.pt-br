---
title: "Comportamento de rasterizador com blocos não mapeados"
description: "Esta seção descreve o comportamento de rasterizador com blocos não mapeados."
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords: "Comportamento de rasterizador com blocos não mapeados"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: b4f85689af4d1f4d69c7f50f094fc8487e4b59e6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Comportamento de rasterizador com blocos não mapeados


Esta seção descreve o comportamento de rasterizador com blocos não mapeados.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


O comportamento de leituras e gravações do modo de exibição de estêncil de profundidade (DSV) depende do nível de suporte de hardware. Para uma análise dos requisitos, consulte o comportamento de leitura e gravação geral para [Níveis de recursos de streaming](streaming-resources-features-tiers.md).

Aqui está o comportamento ideal:

Se um bloco não estiver mapeado no DepthStencilView, o valor de retorno de profundidade de leitura será 0, o que será enviado, em seguida, para todas as operações configuradas para o valor de leitura de profundidade. Gravações para blocos de profundidade ausentes serão ignoradas. Essa definição ideal para o tratamento da gravação não é exigida pelo [Nível 2](tier-2.md); gravações para blocos não mapeados podem acabar em um cache que leituras subsequentes podem coletar.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


O comportamento leituras e gravações de modo de exibição de destino de renderização (RTV) depende do nível de suporte de hardware. Para uma análise dos requisitos, consulte o comportamento de leitura e gravação geral para [Níveis de recursos de streaming](streaming-resources-features-tiers.md).

Em todas as implementações, diferentes RTVs (e DSV) associados ao mesmo tempo podem ter diferentes áreas mapeadas versus não mapeadas e podem ter diferentes formatos de superfície dimensionados (o que significa formas de bloco diferentes).

Aqui está o comportamento ideal:

Leituras de RTVs retornam 0 em blocos ausentes e gravações são ignoradas. Essa definição ideal para o tratamento da gravação não é exigida pelo [Nível 2](tier-2.md); gravações para blocos não mapeados podem acabar em um cache que leituras subsequentes podem coletar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 



