---
title: Não há suporte para formatos de estêncil com recursos de streaming
description: Formatos que contêm estêncil não são compatíveis com recursos de streaming.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Não há suporte para formatos de estêncil com recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35813a6242abd555e87329c25a413285d1d948
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660981"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Não há suporte para formatos de estêncil com recursos de streaming


Formatos que contêm estêncil não são compatíveis com recursos de streaming.

Os formatos que contêm o estêncil incluem DXGI\_formato\_D24\_UNORM\_S8\_UINT (e relacionados formatos na família R24G8) e DXGI\_formato\_D32\_FLUTUAR\_S8X24\_UINT (e relacionados formatos na família R32G8X24).

Algumas implementações armazenam profundidade e estêncil em alocações separadas enquanto outras os armazenam juntos. O gerenciamento de blocos para os dois esquemas precisaria ser diferente, e nenhuma API única pode abstrair ou racionalizar as diferenças. É recomendável que o hardware futuro tenha suporte a superfícies de profundidade e estêncil independentes, cada uma colocada lado a lado de forma independente.

A profundidade de 32 bits teria blocos de 128 x 128, e o estêncil de 8 bits teria blocos de 256 x 256. Portanto, os aplicativos precisam conviver com o desalinhamento do formato de bloco entre a profundidade e o estêncil. Mas o mesmo problema já existe com formatos de superfície de destino de renderização diferentes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Fluxo de recurso entre processos e compartilhamento de dispositivo](streaming-resource-cross-process-and-device-sharing.md)

 

 




