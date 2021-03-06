---
title: Recursos de streaming
description: Os recursos de streaming são enormes recursos lógicos que utilizam pequenas quantias de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming anteriormente foram chamados de recursos lado a lado.
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Recursos de streaming
- recursos, streaming
- recursos, lado a lado
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c15c8a82219109a96d0a9ca192c4dfff5d86c9aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598231"
---
# <a name="streaming-resources"></a>Recursos de streaming


Os *recursos de streaming* são enormes recursos lógicos que utilizam pequenas quantias de memória física. Em vez de transferir um recurso grande inteiro, pequenas partes do recurso são transmitidas conforme necessário. Recursos de streaming eram anteriormente chamados de *recursos em bloco*.

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
<td align="left"><p><a href="the-need-for-streaming-resources.md">A necessidade de recursos de streaming</a></p></td>
<td align="left"><p>Os recursos de streaming são necessários para que a memória da GPU não seja desperdiçada armazenando regiões de superfícies que não serão acessadas, e para informar o hardware sobre como fazer a filtragem entre os blocos adjacentes.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">Criar recursos de streaming</a></p></td>
<td align="left"><p>Os recursos de streaming criados especificando um sinalizador quando você cria um recurso, indicando que o recurso é um recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">Pipeline de acesso a recursos de streaming</a></p></td>
<td align="left"><p>Os recursos de streaming podem ser usados nos modos de exibição de recurso de sombreador (SRV), modos de exibição de destino (RTV), modos de exibição de estêncil de profundidade (DSV) e modos de exibição de acesso não ordenado (UAV), bem como alguns pontos de vinculação onde os modos de exibição não são usados, como associações de buffer de vértice de renderização.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">As camadas de recursos de recursos de streaming</a></p></td>
<td align="left"><p>O Direct3D dá suporte a recursos de streaming em três níveis de recursos.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizado de gráficos do Direct3D](index.md)

[Recursos](resources.md)

 

 




