---
title: Transformações
description: A parte do Direct3D que leva a geometria através do pipeline de geometria de funções fixas é o mecanismo de transformação.
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- Transformações
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9eb129609b2fa8564b04d128c3cb06251b044360
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5704595"
---
# <a name="transforms"></a>Transformações


A parte do Direct3D que leva a geometria através do pipeline de geometria de funções fixas é o mecanismo de transformação. Ele localiza o modelo e o visualizador do mundo, projeta vértices para a exibição na tela e corta os vértices no visor. O mecanismo de transformação também executa cálculos de iluminação para determinar os componentes difusos e especulares em cada vértice.

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
<td align="left"><p><a href="transform-overview.md">Visão geral da transformação</a></p></td>
<td align="left"><p>As transformações de matriz lidam com muita a matemática de nível inferior dos gráficos 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="world-transform.md">Transformação do mundo</a></p></td>
<td align="left"><p>Uma transformação de mundo muda as coordenadas do espaço do modelo, onde os vértices são definidos em relação à origem local de um modelo, para o espaço do mundo. No espaço do mundo, os vértices são definidos em relação a uma origem comum a todos os objetos em uma cena. A transformação do mundo coloca um modelo no mundo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="view-transform.md">Transformação da exibição</a></p></td>
<td align="left"><p>Uma <em>transformação de exibição</em> localiza o visualizador no espaço do mundo, transformando vértices em espaço da câmera. No espaço de câmera, a câmera ou visualizador está na origem, olhando na direção z positiva. A matriz de exibição realoca os objetos no mundo ao redor de uma posição de câmera - a origem do espaço da câmera - e orientação.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="projection-transform.md">Transformação da projeção</a></p></td>
<td align="left"><p>A <em>transformação da projeção</em> controla as partes internas da câmera, como escolher uma lente de uma câmera. Este é o mais complicado dos três tipos de transformação.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas e geometria](coordinate-systems-and-geometry.md)

 

 




