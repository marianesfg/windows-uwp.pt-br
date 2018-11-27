---
title: Lidar com espaço disponível para recursos de streaming
description: Esta seção especifica o espaço de endereço virtual que está disponível para recursos de streaming.
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- Lidar com espaço disponível para recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 35a591d805870df97ee03169b20e664316e094a7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7826462"
---
# <a name="address-space-available-for-streaming-resources"></a>Lidar com espaço disponível para recursos de streaming


Esta seção especifica o espaço de endereço virtual que está disponível para recursos de streaming.

Em sistemas operacionais de 64 bits, pelo menos 40 bits de espaço de endereço virtual (1 TB) está disponível.

Para sistemas operacionais de 32 bits, o espaço de endereço é de 32 bits (4 GB). Para sistemas ARM de 32 bits, a criação de recursos de streaming individuais poderá falhar se a alocação usar mais de 27 bits de espaço de endereço (128 MB). Isso inclui qualquer preenchimento oculto no espaço de endereço que do hardware pode usar para mipmaps, preenchimento de bloco empacotado e, possivelmente, preenchimento de dimensões de superfície para potências de 2.

Em sistemas de elementos gráficos com uma tabela de página separada para a unidade de processamento gráfico (GPU), a maior parte desse espaço de endereço estará disponível para os recursos GPU feitos pelo app, embora alocações de GPU feitas pelo driver de vídeo cabem no mesmo espaço.

Em sistemas futuros com uma tabela de página compartilhada entre a CPU e GPU, o espaço de endereço disponível é compartilhado entre todos os CPU e pela GPU alocações em um processo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Parâmetros de criação de recursos de streaming](streaming-resource-creation-parameters.md)

 

 




