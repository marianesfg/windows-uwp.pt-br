---
title: Texturas
description: "Texturas são uma ferramenta poderosa para criar realismo em imagens 3D geradas por computador. O Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização."
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords: Texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: ef5c72f3c667c63cb48c469349ae26c364050c19
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="textures"></a>Texturas


Texturas são uma ferramenta poderosa para criar realismo em imagens 3D geradas por computador. O Direct3D dá suporte a um amplo conjunto de recursos texturização, fornecendo aos desenvolvedores acesso fácil a técnicas avançadas de texturização.

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
<td align="left"><p>[Introdução a texturas](introduction-to-textures.md)</p></td>
<td align="left"><p>Um recurso de textura é uma estrutura de dados para armazenar texels, que são a unidade menor de uma textura que pode ser lida ou gravada. Quando a textura é lida por um sombreador, ela pode ser filtrada por amostras de texturas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Conceitos básicos de texturização](basic-texturing-concepts.md)</p></td>
<td align="left"><p>As imagens 3D geradas por computador de antigamente, embora geralmente avançadas para o tempo, tendiam a ter uma aparência de plástico brilhante. Eles não tinham os tipos de marcações como arranhões, rachaduras, impressões digitais e manchas da tela que dão aos objetos 3D uma complexidade visual realística. As texturas tornaram-se populares por aprimorarem o realismo das imagens 3D geradas por computador.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modos de endereçamento de textura](texture-addressing-modes.md)</p></td>
<td align="left"><p>Seu aplicativo Direct3D pode atribuir coordenadas de textura a qualquer vértice de qualquer primitiva. Normalmente, as coordenadas de textura u e v que você atribui a um vértice estão no intervalo de 0.0 a 1.0. No entanto, atribuindo coordenadas de textura fora desse intervalo, você pode criar certos efeitos especiais de texturização.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Filtragem de textura](texture-filtering.md)</p></td>
<td align="left"><p>A filtragem de textura produz uma cor de cada pixel na imagem renderizada em 2D da primitiva quando uma primitiva é renderizada por meio do mapeamento de uma primitiva 3D em uma tela 2D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de textura](texture-resources.md)</p></td>
<td align="left"><p>Texturas são um tipo de recurso usado para renderização.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Encapsulamento de textura](texture-wrapping.md)</p></td>
<td align="left"><p>O encapsulamento de textura muda a maneira básica com que o Direct3D rasteriza polígonos texturizados usando as coordenadas de textura especificadas para cada vértice. Durante a rasterização de um polígono, o sistema faz a interpolação entre as coordenadas de textura em cada um dos vértices do polígono para determinar os texels que devem ser usados para cada pixel do polígono.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Mesclagem de texturas](texture-blending.md)</p></td>
<td align="left"><p>O Direct3D pode mesclar até oito texturas em primitivas em uma única passagem. O uso da mesclagem de várias texturas pode aumentar profundamente a taxa de quadros de um aplicativo Direct3D. Um aplicativo emprega a mesclagem de várias texturas para aplicar texturas, sombras, iluminação especular, iluminação difusa e outros efeitos especiais em uma única passagem.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapeamento de luzes com texturas](light-mapping-with-textures.md)</p></td>
<td align="left"><p>Um mapa de luz é uma textura ou um grupo de texturas que contém informações sobre iluminação em uma cena 3D. Os mapas de luz mapeiam áreas de luz e sombra em primitivos. A passagem múltipla e a mesclagem de várias texturas permitem que seu aplicativo renderize cenas com uma aparência mais realista do que as técnicas de sombreamento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de textura compactada](compressed-texture-resources.md)</p></td>
<td align="left"><p>Mapas de texturas são imagens digitalizadas desenhadas em formas tridimensionais para adicionar detalhes visuais. Elas são mapeadas para essas formas durante a rasterização, e o processo pode consumir grandes quantidades de memória e barramento de sistema. Para reduzir a quantidade de memória consumida pelas texturas, o Direct3D dá suporte à compactação de superfícies de textura. Alguns dispositivos Direct3D dão suporte nativo para superfícies de texturas compactadas.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

 

 




