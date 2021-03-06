---
title: Filtragem de textura
description: A filtragem de textura produz uma cor de cada pixel na imagem renderizada em 2D da primitiva quando uma primitiva é renderizada por meio do mapeamento de uma primitiva 3D em uma tela 2D.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- Filtragem de textura
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 449a31d92235efc50119bcd0db11b3532f523cd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633721"
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
<td align="left"><p><a href="nearest-point-sampling.md">Mais próximo ponto de amostragem</a></p></td>
<td align="left"><p>Os apps não são obrigados a usar filtragem de textura. O Direct3D pode ser definido para calcular o endereço de texel, que geralmente não avalia como inteiros e copia a cor de texel com o endereço inteiro mais próximo. Esse processo é chamado de <em>amostragem do ponto mais próximo</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">Filtragem de textura bilinear</a></p></td>
<td align="left"><p>A <em>filtragem bilinear</em> calcula a média ponderada dos 4 texels mais próximos do ponto de amostragem. Essa abordagem de filtragem é mais precisa e comum que a filtragem por ponto mais próximo. Essa abordagem é eficiente porque ela é implementado no hardware de gráficos moderno.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">Filtragem de textura anisotrópica</a></p></td>
<td align="left"><p><em>Anisotropia</em> é visível no texels a distorção de um objeto 3D cujo superfície é orientada com um ângulo em relação ao plano da tela. Quando um pixel de uma primitiva anisotrópica é mapeado para texels, sua forma é distorcida.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Filtragem com mipmaps de textura</a></p></td>
<td align="left"><p>Um <em>mipmap</em> é uma sequência de texturas, cada uma delas é uma representação em resolução mais baixa progressivamente da mesma imagem. A altura e a largura de cada imagem ou nível, no mipmap é uma potência de dois menor do que o nível anterior.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Texturas](textures.md)

 

 




