---
title: Recursos
description: Um recurso é uma área na memória que pode ser acessada pelo pipeline de Direct3D.
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- Recursos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a56b03de29e110a2768ebe71f4e61d8099ca1cf8
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5827839"
---
# <a name="resources"></a>Recursos


Um recurso é uma área na memória que pode ser acessada pelo pipeline de Direct3D. Para que o pipeline acesse a memória com eficiência, os dados fornecidos ao pipeline (como geometria de entrada, recursos de sombreador e texturas) devem ser armazenados em um recurso. Existem dois tipos de recursos a partir dos quais todos os recursos de Direct3D são derivados: um buffer ou uma textura. Até 128 recursos podem estar ativos para cada estágio do pipeline.

Cada app normalmente criará muitos recursos. Exemplos de recursos incluem: buffers de vértice, buffer de índice, buffer constante, texturas e recursos de sombreador. Existem várias opções que determinam como os recursos podem ser usados. Você pode criar recursos com tipo forte ou sem tipo; você pode controlar se os recursos têm tanto acesso de leitura quanto de gravação; você pode disponibilizar recursos somente para CPU, GPU ou ambas. Naturalmente, haverá compensação entre velocidade versus funcionalidade: quanto maior a funcionalidade que você permitir a um recurso, o menor será o desempenho esperado.

Como um app geralmente usa muitas texturas, o Direct3D também tem o conceito de uma matriz de textura para simplificar o gerenciamento de textura. Uma matriz de textura contém uma ou mais texturas (todas do mesmo tipo e dimensões) que podem ser indexadas de dentro de um app ou por meio de sombreadores. Matrizes de textura permitem que você use uma única interface com vários índices para acessar muitas texturas. Você pode criar quantas matrizes de textura precisar para gerenciar tipos diferentes de textura.

Depois de criar os recursos que seu app usará, conecte ou associe cada recurso aos estágios de pipeline que farão uso deles. Isso é feito chamando uma API de vinculação, que leva um ponteiro para o recurso. Como mais de um estágio de pipeline pode precisar de acesso ao mesmo recurso, o Direct3D tem o conceito de uma exibição de recurso. Um modo de exibição identifica a parte de um recurso que pode ser acessada. Você pode criar *m* modos de exibição, ou um recurso, e associá-los a *n* estágios, pressupondo que você siga as regras de associação para recurso compartilhado (o tempo de execução irá gerar erros no momento da compilação se você não fizer isso).

Uma exibição de recurso fornece um modelo geral para acesso a um recurso (como texturas ou buffers). Como você pode usar um modo de exibição para informar o tempo de execução sobre quais dados acessar e como acessá-los, exibições de recursos permitem criar recursos sem tipo. Ou seja, você pode criar recursos para um determinado tamanho no momento da compilação e, em seguida, declarar o tipo de dados dentro do recurso quando o recurso for vinculado ao pipeline. Modos de exibição expõem muitas funções para o uso de recursos, como a capacidade de ler superfícies de profundidade/estêncil de fundo no sombreador, gerar cubemaps dinâmicos em uma única passagem e renderizar simultaneamente várias fatias de um volume.

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
<td align="left"><p><a href="resource-types.md">Tipos de recurso</a></p></td>
<td align="left"><p>Diferentes tipos de recursos têm um layout distinto (ou volume de memória). Todos os recursos usados pelo pipeline Direct3D derivam de dois tipos de recurso básicos: <a href="resource-types.md#buffer-resources">buffers</a> e <a href="resource-types.md#texture-resources">texturas</a>. Um buffer é uma coleção de dados brutos (elementos); uma textura é uma coleção de texels (elementos de textura).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">Escolhendo um recurso</a></p></td>
<td align="left"><p>Um recurso é uma coleção de dados que são usados pelo pipeline de 3D. Criar recursos e definir seu comportamento é o primeiro passo para programar seu app. Este guia aborda tópicos básicos para escolher os recursos exigidos pelo seu app.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">Copiando e acessando dados de recursos</a></p></td>
<td align="left"><p>Sinalizadores de uso indicam como o app pretende usar os dados de recurso para colocar os recursos na área mais eficiente de memória possível. Dados de recursos são copiados em todos os recursos para que a CPU ou a GPU possa acessá-los sem afetar o desempenho.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">Exibições de textura</a></p></td>
<td align="left"><p>No Direct3D, os recursos de textura são acessados com um modo de exibição, que é um mecanismo para interpretação de hardware de um recurso na memória. Um modo de exibição permite que um estágio de pipeline específico acesse somente os <a href="resource-types.md">sub-recursos</a> de que precisa, na representação desejada pelo app.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Sistemas de coordenadas](coordinate-systems.md)

[Guia de aprendizagem de Gráficos do Direct3D](index.md)

[Regras de ponto flutuante](floating-point-rules.md)

[Conversão de tipo de dados](data-type-conversion.md)
