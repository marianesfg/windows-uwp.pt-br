---
title: "Níveis de recursos de streaming"
description: "O Direct3D dá suporte a recursos de streaming em três níveis de recursos."
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords: "Níveis de recursos de streaming"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 671c679938860057a5fe8f083eebfe47dc518e05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="streaming-resources-features-tiers"></a>Níveis de recursos de streaming


O Direct3D dá suporte a recursos de streaming em três níveis de recursos.

O nível 1 oferece recursos básicos para recursos de streaming.

O nível 2 adiciona funcionalidades além do nível 1, como a garantia de um mipmap de textura não empacotada quando o tamanho é pelo menos uma forma de bloco padrão; instruções de sombreador para fixação de nível de detalhe (LOD) e para a obtenção de status sobre a operação de sombreador; Além disso, a leitura de blocos mapeados em NULL trata o valor de amostragem como zero.

O nível 3 acrescenta recursos de Texture3D, além do nível 2.

As funções de consulta estão disponíveis nas versões do Direct3D, para validar o suporte de driver e hardware para recursos de streaming, e em que nível de camada.

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
<td align="left"><p>[Nível 1](tier-1.md)</p></td>
<td align="left"><p>Esta seção descreve o suporte de nível 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Nível 2](tier-2.md)</p></td>
<td align="left"><p>O suporte do nível 2 a recursos de streaming adiciona funcionalidades além do nível 1, como a garantia de um mipmap de textura não empacotada quando o tamanho é pelo menos uma forma de bloco padrão; instruções de sombreador para fixação de nível de detalhe (LOD) e para a obtenção de status sobre a operação de sombreador; Além disso, a leitura de blocos mapeados em NULL trata o valor de amostragem como zero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Nível 3](tier-3.md)</p></td>
<td align="left"><p>O nível 3 adiciona suporte para Texture3D para recursos de streaming, além dos recursos do [nível 2](tier-2.md).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




