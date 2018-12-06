---
title: Primitivos
description: Um primitivo 3D é uma coleção de vértices que forma uma única entidade 3D.
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- Primitivos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6c2bf3aa2f421efa7061f3003e6cab8c9f7b8c58
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8757293"
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
<td align="left"><p><a href="point-lists.md">Listas de pontos</a></p></td>
<td align="left"><p>Uma lista de ponto é uma coleção de vértices que são renderizados como pontos isolados. Seu app pode usar listas de ponto em cenas 3D para campos de estrelas, ou linhas pontilhadas na superfície de um polígono.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">Listas de linhas</a></p></td>
<td align="left"><p>Uma lista de linhas contém segmentos de linha reta isolados. As listas de linhas são úteis para tarefas como adicionar chuva com neve ou pesada a uma cena 3D. Os apps criam uma lista de linhas preenchendo uma matriz de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">Faixas de linhas</a></p></td>
<td align="left"><p>Uma faixa de linhas é um primitivo composto de segmentos de linha conectados. O app pode usar faixas de linhas para criar polígonos não fechados. Um polígono fechado tem o último vértice conectado ao primeiro por um segmento de linhas.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">Listas de triângulos</a></p></td>
<td align="left"><p>Uma lista de triângulos é uma lista de triângulos isolados. Os triângulos isolados podem ou não estar próximos uns dos outros. Uma lista de triângulos deve ter pelo menos três vértices e o número total de vértices deve ser divisível por três.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">Listas de triângulos</a></p></td>
<td align="left"><p>Uma faixa de triângulo é uma série de triângulos conectados. Como os triângulos estão conectados, o app não precisa especificar repetidamente todos os três vértices de cada triângulo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




