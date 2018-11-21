---
title: Filtragem de textura
description: A filtragem de textura produz uma cor de cada pixel na imagem renderizada em 2D da primitiva quando uma primitiva é renderizada por meio do mapeamento de uma primitiva 3D em uma tela 2D.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- Filtragem de textura
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce80bc0f64e1aba8328880203f22ea3909fdb3e3
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7427571"
---
# <a name="texture-filtering"></a>Filtragem de textura


A filtragem de textura produz uma cor de cada pixel na imagem renderizada em 2D da primitiva quando uma primitiva é renderizada por meio do mapeamento de uma primitiva 3D em uma tela 2D.

Quando o Direct3D renderiza uma primitiva, ele mapeia a primitiva 3D em uma tela 2D. Se a primitiva tem uma textura, o Direct3D deve usar essa textura para produzir uma cor para cada pixel na imagem renderizada em 2D da primitiva. Para cada pixel da imagem na tela da primitiva, ele deve obter um valor de cor da textura. Esse processo é chamado de *filtragem de textura*.

Quando uma operação de filtro de textura é executada, a textura que está sendo usada normalmente também está sendo ampliada ou reduzida. Em outras palavras, ela está sendo mapeada para uma imagem primitiva maior ou menor que ela mesma. A ampliação de uma textura pode resultar no mapeamento de muitos pixels para um texel. O resultado pode ser uma aparência robusta. A redução de uma textura geralmente significa que um único pixel é mapeado para muitos texels. A imagem resultante pode ficar desfocada ou distorcida. Para resolver esses problemas, a mesclagem das cores dos texels devem ser realizada para chegar a uma cor para o pixel.

O Direct3D simplifica o processo complexo de filtragem de textura. Ele fornece três tipos de filtragem de textura: filtragem linear, filtragem anisotrópica e filtragem mipmap. Se você não selecionar a filtragem de textura, o Direct3D usará uma técnica chamada de amostragem do ponto mais próximo.

Cada tipo de filtragem de textura possui vantagens e desvantagens. Por exemplo, a filtragem de textura linear pode produzir bordas irregulares ou uma aparência robusta na imagem final. No entanto, esse é um método de filtragem de textura computacionalmente de baixa sobrecarga. A filtragem com mipmaps geralmente produz os melhores resultados, especialmente quando combinada com uma filtragem anisotrópica. No entanto, ele exige o máximo de memória das técnicas que o Direct3D pode suportar.

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>Tipos de filtragem de textura


O Direct3D dá suporte às abordagens de filtragem de textura a seguir.

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
<td align="left"><p><a href="nearest-point-sampling.md">Amostragem do ponto mais próximo</a></p></td>
<td align="left"><p>Os aplicativos não precisam usar a filtragem de textura. O Direct3D pode ser configurado para calcular o endereço do texel, que geralmente não é avaliado como inteiro, e copia a cor de texel com o endereço do inteiro mais próximo. Esse processo é chamado de <em>amostragem do ponto mais próximo</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">Filtragem bilinear de textura</a></p></td>
<td align="left"><p>A <em>filtragem bilinear</em> calcula a média ponderada dos 4 texels mais próximos do ponto de amostragem. Essa abordagem de filtragem é mais precisa e comum que a filtragem por ponto mais próximo. Essa abordagem é eficiente porque é implementada em hardware de gráficos modernos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">Filtragem anisotrópica de textura</a></p></td>
<td align="left"><p><em>Anisotropia</em> é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Filtragem de textura com mipmaps</a></p></td>
<td align="left"><p>Um <em>mipmap</em> é uma sequência de texturas, cada uma delas é uma representação em resolução mais baixa progressivamente da mesma imagem. A altura e a largura de cada imagem ou nível, no mipmap é uma potência de dois menor do que o nível anterior.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




