---
title: Acesso pipeline aos recursos de streaming
description: Os recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenado (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como associações de buffer de vértice de renderização.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Acesso pipeline aos recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b10ba23db301a675bf102fd8fb6e278dbba11da8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371019"
---
# <a name="pipeline-access-to-streaming-resources"></a>Acesso pipeline aos recursos de streaming


Os recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenado (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como associações de buffer de vértice de renderização. Para obter a lista de vinculações suportadas, consulte [Parâmetros de criação de recursos de streaming](streaming-resource-creation-parameters.md). As várias operações de cópia do D3D também funcionam em recursos de streaming.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportamento SRV com blocos não mapeados</a></p></td>
<td align="left"><p>O comportamento de leituras de modo de exibição (SRV) de recurso de sombreador que envolve blocos não mapeados depende do nível de suporte de hardware.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportamento UAV com blocos não mapeados</a></p></td>
<td align="left"><p>O comportamento das leituras e gravações de modo de exibição de acesso não ordenado (UAV) depende do nível de suporte do hardware.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportamento de rasterizador com blocos não mapeados</a></p></td>
<td align="left"><p>Esta seção descreve o comportamento de rasterizador com blocos não mapeados.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Limitações de acesso de lado a lado com mapeamentos duplicados</a></p></td>
<td align="left"><p>Há limitações em acesso de bloco com mapeamentos duplicados, como ao copiar recursos de streaming com origem e destino sobrepostos, ou ao renderizar para blocos compartilhados dentro da área de renderização.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Recursos de amostragem de textura de recursos de streaming</a></p></td>
<td align="left"><p>Os recursos de amostragem de textura de recursos de streaming incluem obter feedback de status do sombreador sobre as áreas mapeadas, verificar se todos os dados que estão sendo acessados foram mapeados no recurso, fixação para ajudar os sombreadores a evitar áreas em recursos de streaming mipmapped que são conhecidos por não serem mapeados, e descobrir o que será o LED mínimo que está totalmente mapeado para um volume de filtro de textura inteiro.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposição de recursos de streaming de HLSL</a></p></td>
<td align="left"><p>Uma sintaxe específica da Microsoft HLSL High Level Shader Language (HLSL) é necessária para dar suporte a recursos de streaming no <a href="https://docs.microsoft.com/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">Modelo de sombreador 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




