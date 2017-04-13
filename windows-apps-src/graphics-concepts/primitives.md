---
title: Primitivos
description: "Um primitivo 3D é uma coleção de vértices que forma uma única entidade 3D."
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords: Primitivos
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 22f143030d3973cf77cd3b0857cf9b67ff5503fb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="primitives"></a>Primitivos


Um *primitivo 3D* é uma coleção de vértices que forma uma única entidade 3D.

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
<td align="left"><p>[Listas de pontos](point-lists.md)</p></td>
<td align="left"><p>Uma lista de ponto é uma coleção de vértices que são renderizados como pontos isolados. Seu app pode usar listas de ponto em cenas 3D para campos de estrelas, ou linhas pontilhadas na superfície de um polígono.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Listas de linhas](line-lists.md)</p></td>
<td align="left"><p>Uma lista de linhas contém segmentos de linha reta isolados. As listas de linhas são úteis para tarefas como adicionar chuva com neve ou pesada a uma cena 3D. Os apps criam uma lista de linhas preenchendo uma matriz de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Faixas de linhas](line-strips.md)</p></td>
<td align="left"><p>Uma faixa de linhas é um primitivo composto de segmentos de linha conectados. O app pode usar faixas de linhas para criar polígonos não fechados. Um polígono fechado tem o último vértice conectado ao primeiro por um segmento de linhas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Listas de triângulos](triangle-lists.md)</p></td>
<td align="left"><p>Uma lista de triângulos é uma lista de triângulos isolados. Os triângulos isolados podem ou não estar próximos uns dos outros. Uma lista de triângulos deve ter pelo menos três vértices e o número total de vértices deve ser divisível por três.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Listas de triângulos](triangle-strips.md)</p></td>
<td align="left"><p>Uma faixa de triângulo é uma série de triângulos conectados. Como os triângulos estão conectados, o app não precisa especificar repetidamente todos os três vértices de cada triângulo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




