---
title: Níveis de recursos de streaming
description: O Direct3D dá suporte a recursos de streaming em três níveis de recursos.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Níveis de recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bc7d04a826bd8be0947e8d84ab8dbff6f67e8027
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287787"
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
<td align="left"><p><a href="tier-1.md">Nível 1</a></p></td>
<td align="left"><p>Esta seção descreve o suporte de nível 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Nível 2</a></p></td>
<td align="left"><p>O suporte do nível 2 a recursos de streaming adiciona funcionalidades além do nível 1, como a garantia de um mipmap de textura não empacotada quando o tamanho é pelo menos uma forma de bloco padrão; instruções de sombreador para fixação de nível de detalhe (LOD) e para a obtenção de status sobre a operação de sombreador; Além disso, a leitura de blocos mapeados em NULL trata o valor de amostragem como zero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Nível 3</a></p></td>
<td align="left"><p>O nível 3 adiciona suporte para Texture3D para recursos de streaming, além dos recursos do <a href="tier-2.md">nível 2</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




