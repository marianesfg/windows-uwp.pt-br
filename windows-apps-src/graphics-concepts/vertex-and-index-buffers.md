---
title: Buffers de vértice e índice
description: Os buffers de vértice são buffers de memória que contêm dados de vértice; vértices em um buffer de vértice são processados para executar a transformação, iluminação e corte.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- Buffers de vértice e índice
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2ea6ce4060957ade5dd1007389be51176440f04
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593571"
---
# <a name="vertex-and-index-buffers"></a>Buffers de vértice e índice


Os *buffers de vértice* são buffers de memória que contêm dados de vértice; vértices em um buffer de vértice são processados para executar a transformação, iluminação e corte. *Buffers de índice* são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice usados para renderizar primitivas.

Os buffers de vértice podem conter qualquer tipo de vértice - transformado ou não transformado, aceso ou apagado - que pode ser renderizado. Você pode processar os vértices de um buffer de vértice para realizar operações como transformação, iluminação ou gerar sinalizadores de recorte. A transformação sempre é executada.

A flexibilidade dos buffers de vértice os torna pontos de preparo ideais para a reutilização da geometria transformada. Você pode criar um buffer de vértice único, transformar, iluminar e recordar os vértices nele, e renderizar o modelo na cena quantas vezes forem necessárias sem transformá-lo novamente, mesmo com alterações de estado de renderização intercaladas. Isso é útil ao renderizar modelos que usam várias texturas: a geometria é transformada apenas uma vez e, em seguida, partes dela podem ser renderizadas conforme necessário, intercaladas com as alterações necessárias de textura. As alterações de estado de renderização feitas após os vértices serem processados entram em vigor na próxima vez em que os vértices forem processados.

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
<td align="left"><p><a href="introduction-to-buffers.md">Introdução aos buffers</a></p></td>
<td align="left"><p>Um recurso de buffer é uma coleção de dados completamente tipados, agrupados em elementos. Os buffers armazenam dados, como as coordenadas de textura em um <em>buffer de vértice</em>, indexa em um <em>buffer de índice</em>, dados de constantes de sombreador em um <em>buffer constante</em>, vetores de posição, vetores normais ou estado do dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">Buffers de índice</a></p></td>
<td align="left"><p><em>Buffers de índice</em> são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice usados para renderizar primitivas.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizado de gráficos do Direct3D](index.md)

 

 




