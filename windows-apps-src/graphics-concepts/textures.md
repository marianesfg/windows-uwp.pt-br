---
title: Texturas
description: Texturas são uma ferramenta poderosa para criar realismo em imagens 3D geradas por computador. Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização.
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords:
- Texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81d77b262cc77c23d859cf76227a34bc72b15b96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593001"
---
# <a name="textures"></a>Texturas


Texturas são uma ferramenta poderosa para criar realismo em imagens 3D geradas por computador. Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização.

Para melhorar o desempenho, é recomendável usar texturas dinâmicas. Uma textura dinâmica pode ser bloqueada, gravada e desbloqueada em cada quadro.

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
<td align="left"><p><a href="introduction-to-textures.md">Introdução às texturas</a></p></td>
<td align="left"><p>Um recurso de textura é uma estrutura de dados para armazenar texels, que é a menor unidade de uma textura que pode ser lida ou gravada. Quando a textura é lida por um sombreador, é possível filtrá-la por amostras de texturas.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="basic-texturing-concepts.md">Conceitos básicos de texturização</a></p></td>
<td align="left"><p>As imagens 3D geradas por computador antigamente, embora geralmente avançadas para o tempo, tendiam a ter uma aparência de plástico brilhante. Eles não tinham os tipos de marcações como arranhões, rachaduras, impressões digitais e manchas da tela que dão aos objetos 3D uma complexidade visual realística. As texturas se tornaram populares para o aprimoramento do realismo das imagens 3D geradas por computador.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-addressing-modes.md">Modos de endereçamento de textura</a></p></td>
<td align="left"><p>Seu aplicativo Direct3D pode atribuir coordenadas de textura a qualquer vértice de qualquer primitiva. Normalmente, as coordenadas de textura u e v que você atribui a um vértice estão no intervalo de 0.0 a 1.0. No entanto, atribuindo coordenadas de textura fora desse intervalo, você pode criar certos efeitos especiais de texturização.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering.md">Filtragem de textura</a></p></td>
<td align="left"><p>A filtragem de textura produz uma cor de cada pixel na imagem renderizada em 2D da primitiva quando uma primitiva é renderizada por meio do mapeamento de uma primitiva 3D em uma tela 2D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-resources.md">Recursos de textura</a></p></td>
<td align="left"><p>Texturas são um tipo de recurso usado para renderização.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-wrapping.md">Quebra automática de textura</a></p></td>
<td align="left"><p>O encapsulamento de textura muda a maneira básica com que o Direct3D rasteriza polígonos texturizados usando as coordenadas de textura especificadas para cada vértice. Durante a rasterização de um polígono, o sistema faz a interpolação entre as coordenadas de textura em cada um dos vértices do polígono para determinar os texels que devem ser usados para cada pixel do polígono.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="texture-blending.md">A combinação de textura</a></p></td>
<td align="left"><p>O Direct3D pode mesclar até oito texturas em primitivas em uma única passagem. O uso da mesclagem de várias texturas pode aumentar profundamente a taxa de quadros de um aplicativo Direct3D. Um aplicativo emprega a mesclagem de várias texturas para aplicar texturas, sombras, iluminação especular, iluminação difusa e outros efeitos especiais em uma única passagem.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="light-mapping-with-textures.md">Mapeamento de luz com texturas</a></p></td>
<td align="left"><p>Um mapa de luz é uma textura ou um grupo de texturas que contém informações sobre a iluminação em uma cena 3D. Os mapas de luz mapeiam áreas de luz e sombra em primitivos. A passagem múltipla e a mesclagem de textura múltipla permitem que o app renderize cenas com uma aparência mais realista em comparação às técnicas de sombreamento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compressed-texture-resources.md">Recursos de textura compactada</a></p></td>
<td align="left"><p>Os mapas de textura são imagens digitalizadas desenhadas em formas tridimensionais para adicionar detalhes visuais. Elas são mapeadas para essas formas durante a rasterização, e o processo pode consumir grandes quantidades de memória e barramento de sistema. Para reduzir a quantidade de memória consumida pelas texturas, o Direct3D dá suporte à compactação de superfícies de textura. Alguns dispositivos Direct3D têm suporte nativo às superfícies de textura compactadas.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizado de gráficos do Direct3D](index.md)

 

 




