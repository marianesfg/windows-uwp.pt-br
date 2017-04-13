---
title: Recursos de streaming
description: "Os recursos de streaming são enormes recursos lógicos que utilizam pequenas quantias de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming anteriormente foram chamados de recursos lado a lado."
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Recursos de streaming
- recursos, streaming
- recursos, lado a lado
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: f45c1d955d901366b0160d148cef88ce015aeaa2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resources"></a>Recursos de streaming


Os *recursos de streaming* são enormes recursos lógicos que utilizam pequenas quantias de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming anteriormente foram chamados de *recursos lado a lado*.

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
<td align="left"><p>[A necessidade de recursos de streaming](the-need-for-streaming-resources.md)</p></td>
<td align="left"><p>Os recursos de streaming são necessários para que a memória da GPU não seja desperdiçada armazenando regiões de superfícies que não serão acessadas, e para informar o hardware sobre como fazer a filtragem entre os blocos adjacentes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Criar recursos de streaming](creating-streaming-resources.md)</p></td>
<td align="left"><p>Os recursos de streaming criados especificando um sinalizador quando você cria um recurso, indicando que o recurso é um recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Acesso pipeline aos recursos de streaming](pipeline-access-to-streaming-resources.md)</p></td>
<td align="left"><p>Os recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenado (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como associações de buffer de vértice de renderização.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Níveis de recursos de streaming](streaming-resources-features-tiers.md)</p></td>
<td align="left"><p>O Direct3D dá suporte a recursos de streaming em três níveis de recursos.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

[Recursos](resources.md)

 

 




