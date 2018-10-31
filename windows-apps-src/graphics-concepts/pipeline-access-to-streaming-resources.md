---
title: Acesso pipeline aos recursos de streaming
description: Recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino de renderização (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenados (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como vinculações de buffer de vértice.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Acesso pipeline aos recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f6f777669959721405fc5c77ef134e3726291b9c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814738"
---
# <a name="pipeline-access-to-streaming-resources"></a>Acesso pipeline aos recursos de streaming


Recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino de renderização (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenados (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como vinculações de buffer de vértice. Para obter a lista de vinculações suportadas, consulte [Parâmetros de criação de recursos de streaming](streaming-resource-creation-parameters.md). As várias operações de cópia do D3D também funcionam em recursos de streaming.

Se várias coordenadas de bloco em um ou mais modos de exibição estiverem vinculadas ao mesmo local de memória, leituras e gravações de diferentes caminhos para a mesma memória ocorrerão em uma ordem de acesso de memória não determinística e não repetida.

Se todos os blocos por trás de um volume de acesso de memória de um sombreador estiverem mapeados para blocos exclusivos, o comportamento será idêntico em todas as implementações para superfícies com o mesmo conteúdo da memória de maneira sem blocos.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Nesta seção


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportamento de SRV com blocos não mapeados</a></p></td>
<td align="left"><p>O comportamento de leituras de modo de exibição de recurso de sombreador (SRV) que envolvem blocos não mapeados depende do nível de suporte de hardware.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportamento de UAV com blocos não mapeados</a></p></td>
<td align="left"><p>O comportamento de leituras e gravações de modo de exibição de acesso não ordenado (UAV) depende do nível de suporte de hardware.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportamento de rasterizador com blocos não mapeados</a></p></td>
<td align="left"><p>Esta seção descreve o comportamento de rasterizador com blocos não mapeados.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Limitações de acesso de bloco com mapeamentos duplicados</a></p></td>
<td align="left"><p>Há limitações em acesso de bloco com mapeamentos duplicados, como ao copiar recursos de streaming com origem e destino sobrepostos, ou ao renderizar para blocos compartilhados dentro da área de renderização.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Recursos de exemplo de textura de recursos de streaming</a></p></td>
<td align="left"><p>As funções de amostragem de textura de recursos de streaming incluem obter feedback de status de sombreador sobre áreas mapeadas, verificar se todos os dados acessados estão mapeados no recurso, vinculação para ajudar sombreadores a evitar áreas em recursos de streaming com mipmap que se sabe serem não mapeados e descobrir qual será o LOD mínimo totalmente mapeado para um volume de filtro de textura inteiro.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposição de recursos de streaming HLSL</a></p></td>
<td align="left"><p>Uma sintaxe específica da Microsoft HLSL High Level Shader Language (HLSL) é necessária para dar suporte a recursos de streaming no <a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">Modelo de sombreador 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




