---
title: Criar recursos de streaming
description: Os recursos de streaming criados especificando um sinalizador quando você cria um recurso, indicando que o recurso é um recurso de streaming.
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- Criar recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a796897aa786283499c25b0f405e302feeb5f938
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5867873"
---
# <a name="creating-streaming-resources"></a>Criar recursos de streaming


Os recursos de streaming criados especificando um sinalizador quando você cria um recurso, indicando que o recurso é um recurso de streaming.

As restrições sobre quando você pode criar um recurso como um recurso de streaming estão descritas em [parâmetros de criação de recursos de Streaming](streaming-resource-creation-parameters.md).

O armazenamento de um recurso não streaming é alocado no sistema de elementos gráficos quando o recurso é criado, como a alocação para uma matriz de texturas 2D.

Quando um recurso de streaming é criado, o sistema de elementos gráficos não aloca o armazenamento para o conteúdo do recurso. Em vez disso, quando um app cria um recurso de streaming, o sistema de elementos gráficos cria uma reserva de espaço de endereço apenas para a área da superfície lado a lado e, em seguida, permite que o mapeamento dos blocos seja controlado pelo app. O "mapeamento" de um bloco é simplesmente a localização física na memória para a qual um bloco lógico em um recurso aponta (ou **NULL** para um bloco não mapeado).

Não confunda esse conceito com a noção de mapeamento de um recurso do Direct3D para acesso de CPU, que apesar de usar o mesmo nome é completamente independente. Você poderá definir e alterar o mapeamento de cada bloco individualmente, conforme necessário, sabendo que todos os blocos para uma superfície não precisam ser mapeadas de uma só vez, fazendo assim o uso eficaz da quantidade de memória disponível.

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
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">Os mapeamentos estão em um pool de blocos</a></p></td>
<td align="left"><p>Quando um recurso é criado como um recurso de streaming, os blocos que o compõem são provenientes da indicação de locais em um pool de blocos. Um pool de blocos é um pool de memória (sustentado por uma ou mais alocações nos bastidores - nunca vistos pelo app).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">Parâmetros de criação de recursos de streaming</a></p></td>
<td align="left"><p>Há algumas restrições sobre o tipo de recursos do Direct3D que você pode criar como um recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">Parâmetros de criação de pool de blocos</a></p></td>
<td align="left"><p>Use os parâmetros nesta seção para definir pools de bloco ao criar um buffer.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">Processo cruzado de recursos de streaming e compartilhamento de dispositivos</a></p></td>
<td align="left"><p>Os pools de bloco podem ser compartilhados com outros processos como recursos tradicionais. O streaming de recursos que faz referência a pools de bloco não pode ser compartilhado entre dispositivos e processos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">Operações disponíveis em recursos de streaming</a></p></td>
<td align="left"><p>Esta seção lista operações que podem ser executadas em recursos de streaming.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">Operações disponíveis em pools de blocos</a></p></td>
<td align="left"><p>As operações em pools de blocos incluem o redimensionamento de um pool de blocos, a oferta de recursos (concedendo memória temporariamente para o sistema para todo o pool de blocos) e a recuperação de recursos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">Como uma área de recurso de streaming é colocada lado a lado</a></p></td>
<td align="left"><p>Quando você cria um recurso de streaming, as dimensões, o tamanho do elemento de formato e o número de mipmaps e/ou fatias de matriz (se aplicável) determinam o número de blocos que são obrigatórios para toda a área de superfície.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recursos de streaming](streaming-resources.md)

 

 




