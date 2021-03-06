---
title: Estágio de saída de fluxo(SO)
description: O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Estágio de saída de fluxo(SO)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3614b7bde3a87c8f5fa6fdc0eada560fd7bbcdc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370961"
---
# <a name="stream-output-so-stage"></a>Estágio de saída de fluxo(SO)


O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


![diagrama da localização do estágio de saída de fluxo no pipeline](images/d3d10-pipeline-stages-so.png)

O estágio de saída de fluxo transmite os dados primitivos do pipeline para a memória em seu caminho para o rasterizador. Dados do estágio anterior podem ser transmitidos para a memória e/ou passados para o rasterizador. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.

Os dados transmitidos para a memória podem ser lidos de volta no pipeline em uma passagem de renderização subsequente, ou podem ser copiados para um recurso de preparo (para que possam ser lidos pela CPU). A quantidade de dados transmitidos pode variar; o Direct3D foi projetado para tratar os dados sem a necessidade de consulta (GPU) sobre a quantidade de dados gravados.--&gt;

Há duas maneiras de fornecer dados de saída de fluxo para o pipeline:

-   Os dados da saída de fluxo podem ser transmitidos de volta para o estágio de Assembler de Entrada (IA).
-   Os dados da saída de fluxo podem ser lidos pelos sombreadores programáveis usando as funções [Load](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>entrada


Dados de vértice de um estágio de sombreador anterior.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior, como o estágio de Sombreador de Geometria (GS), para um ou mais buffers na memória. Se o estágio Geometry Shader (GS) estiver inativo, o estágio Stream saída (SO) continuamente gera dados de vértice do estágio de sombreador de domínio (DS) para buffers de memória (ou se o DS também está inativo, do estágio de sombreador de vértice (VS)).

Quando uma faixa de triângulos ou linha for vinculada ao estágio de Assembler de Entrada (IA), cada faixa é convertida em uma lista antes de ser transmitida. Os vértices sempre são gravados como primitivos completas (por exemplo, 3 vértices por vez para triângulos); primitivos incompletos nunca são transmitidos. Os tipos primitivos com adjacência descartam os dados de adjacência antes de transmitir os dados.

O estágio de saída de fluxo dá suporte a até 4 buffers simultaneamente.

-   Se você estiver transmitindo dados em vários buffers, cada buffer pode capturar apenas um único elemento (até 4 componentes) de dados por vértice, com uma distância de dados implícita igual à largura do elemento em cada buffer (compatível com a maneira como os buffers de elemento único podem ser vinculados para a entrada em estágios de sombreador). Além disso, se os buffers tiverem tamanhos diferentes, a escrita é interrompida assim que qualquer um dos buffers estiver cheio.
-   Se você estiver transmitindo dados em um único buffer, o buffer pode capturar até 64 componentes escalares de dados por vértice (256 bytes ou menos) ou a distância de vértice pode ser de até 2048 bytes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




