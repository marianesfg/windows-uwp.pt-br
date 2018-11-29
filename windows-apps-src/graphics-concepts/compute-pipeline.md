---
title: Calcular o pipeline
description: O pipeline de computação do Direct3D foi projetado para tratar os cálculos que podem ser feitos principalmente em paralelo com o pipeline de gráficos.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209391"
---
# <a name="compute-pipeline"></a>Calcular o pipeline


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]


O pipeline de computação do Direct3D foi projetado para tratar os cálculos que podem ser feitos principalmente em paralelo com o pipeline de gráficos. Há apenas algumas etapas no pipeline de computação, com dados de fluxo de entrada para saída por meio do estágio do sombreador de computação programável.

| | |
|-|-|
|Finalidade|Como outros sombreadores programáveis, o [Estágio de Sombreador de Computação (CS)](compute-shader-stage--cs-.md) foi projetado e implementado com HLSL. Um sombreador de computação fornece a computação alta velocidade de finalidade geral e tira proveito de um grande número de processadores paralelos na unidade de processamento gráfico (GPU). O estágio do sombreador de cálculo fornece compartilhamento de memória e sincronização de thread para permitir métodos mais eficazes de programação paralelos.|
|Entrada|Ao contrário de outros sombreadores programáveis, a definição de entrada é abstrata. A entrada pode ter uma, duas ou três dimensões por natureza, determinando o número de invocações de sombreador de computação que serão executadas. É possível definir dados compartilhados para que um conjunto de invocações o leia.|
|Saída|Os dados de saída do sombreador de computação, que pode ser altamente variado, podem ser sincronizados com o pipeline de renderização de gráficos quando os dados calculados são obrigatórios.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Guia de aprendizagem de Gráficos do Direct3D](index.md)

 

 
