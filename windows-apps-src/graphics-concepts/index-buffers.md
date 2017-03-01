---
title: "Buffers de índice"
description: "Buffers de índice são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice, usados para renderizar primitivas."
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- "Buffers de índice"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6aa62a7506b37314b1952a6687920a2cdf3deca3
ms.lasthandoff: 02/07/2017

---

# <a name="index-buffers"></a>Buffers de índice


*Buffers de índice* são buffers de memória que contêm dados de índice, que são deslocamentos de inteiro em buffers de vértice usados para renderizar primitivas.

Os buffers de índice são buffers de memória que contêm dados de índice. Dados de índice, ou índices, são deslocamentos de inteiro em buffers de vértice e são usados para renderizar primitivas.

Um buffer de vértices contém vértices. Portanto, você pode desenhar um buffer de vértice com ou sem primitivas indexadas. No entanto, como um buffer de índice contém índices, você não pode usar um buffer de índice sem um buffer de vértice correspondente.

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>Descrição de buffer de índice


Um buffer de índice é descrito em termos de suas funcionalidades, como onde ele existe na memória, se suporta leitura e gravação, e o tipo e o número de índices que contém.

Descrições de buffer de índice informam ao seu app como um buffer existente foi criado. Você fornece uma estrutura de descrição vazia para o sistema preencher com os recursos de um buffer de índice criado anteriormente.

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>Requisitos de processamento de índice


O desempenho das operações de processamento de índice depende muito onde o buffer de índice existe na memória e qual tipo de dispositivo de renderização que está sendo usado. Os apps controlam a alocação de memória para buffers de índice quando eles são criados.

O app pode gravar diretamente índices em um buffer de índice alocado na memória ideal de driver. Essa técnica impede uma operação de cópia redundante mais tarde. Essa técnica não funciona bem se o app lê dados de um buffer de índice, pois operações de leitura feitas pelo host da memória ideal de driver podem ser muito lentas. Portanto, se seu app precisar ler durante o processamento ou gravar dados no buffer de modo irregular, um buffer de índice de memória do sistema é a melhor opção.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Buffers de vértice e índice](vertex-and-index-buffers.md)

 

 





