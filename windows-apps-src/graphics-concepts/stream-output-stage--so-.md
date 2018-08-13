---
title: Estágio de Saída de Fluxo(SO)
description: O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Estágio de Saída de Fluxo(SO)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30c3ed335360d7b259c045722b65bb08a71b6e0c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044035"
---
# <a name="stream-output-so-stage"></a>Estágio de Saída de Fluxo(SO)


O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior para um ou mais buffers na memória. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidade e usos


![diagrama da localização do estágio de saída de fluxo no pipeline](images/d3d10-pipeline-stages-so.png)

O estágio de saída de fluxo transmite os dados primitivos do pipeline para a memória em seu caminho para o rasterizador. Dados do estágio anterior podem ser transmitidos para a memória e/ou passados para o rasterizador. Os dados transmitidos para a memória podem ser reenviados para o pipeline como dados de entrada ou lidos pela CPU.

Os dados transmitidos para a memória podem ser lidos de volta no pipeline em uma passagem de renderização subsequente, ou podem ser copiados para um recurso de preparo (para que possam ser lidos pela CPU). A quantidade de dados transmitidos pode variar; o Direct3D foi projetado para tratar os dados sem a necessidade de consulta (GPU) sobre a quantidade de dados gravados.--&gt;

Há duas maneiras de fornecer dados de saída de fluxo para o pipeline:

-   Os dados da saída de fluxo podem ser transmitidos de volta para o estágio de Assembler de Entrada (IA).
-   Os dados da saída de fluxo podem ser lidos pelos sombreadores programáveis usando as funções [Load](https://msdn.microsoft.com/library/windows/desktop/bb509694).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Dados de vértice de um estágio de sombreador anterior.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Saída


O estágio de Saída de Fluxo (SO) emite continuamente (ou transmite) dados de vértice do estágio ativo anterior, como o estágio de Sombreador de Geometria (GS), para um ou mais buffers na memória. Se o estágio de Sombreador de Geometria (GS) estiver inativo, o estágio de Saída de Fluxo (SO) gera continuamente dados de vértice do estágio de Sombreador de Domínio (DS) para buffers na memória (ou se o DS também estiver inativo, do estágio do Sombreador de Vértice (VS)).

Quando uma faixa de triângulo ou linha estiver ligada para o estágio de entrada Assembler (IA), cada faixa é convertida em uma lista antes que eles são transmitidos check-out. Vértices sempre são gravados como primitivos completos (por exemplo, 3 vértices de uma vez para triângulos); primitivos incompletos nunca são transmitidos check-out. Tipos de primitivos com proximidade descartam os dados de proximidade antes de streaming dados check-out.

O estágio de saída de fluxo dá suporte a até 4 buffers simultaneamente.

-   Se você estiver transmitindo dados em vários buffers, cada buffer pode capturar apenas um único elemento (até 4 componentes) de dados por vértice, com uma distância de dados implícita igual à largura do elemento em cada buffer (compatível com a maneira como os buffers de elemento único podem ser vinculados para a entrada em estágios de sombreador). Além disso, se os buffers tiverem tamanhos diferentes, a escrita é interrompida assim que qualquer um dos buffers estiver cheio.
-   Se você estiver transmitindo dados em um único buffer, o buffer pode capturar até 64 componentes escalares de dados por vértice (256 bytes ou menos) ou a distância de vértice pode ser de até 2048 bytes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Pipeline de elementos gráficos](graphics-pipeline.md)

 

 




