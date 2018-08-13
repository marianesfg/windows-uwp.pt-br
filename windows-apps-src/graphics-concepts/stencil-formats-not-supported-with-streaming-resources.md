---
title: Não há suporte para formatos de estêncil com recursos de streaming
description: Formatos que contêm estêncil não são compatíveis com recursos de streaming.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Não há suporte para formatos de estêncil com recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 330b768c0f00f36ce8ce539b9ce41c7ac812e6fc
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043225"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Não há suporte para formatos de estêncil com recursos de streaming


Formatos que contêm estêncil não são compatíveis com recursos de streaming.

Os formatos que contêm estêncil incluem DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (e formatos relacionados na família R24G8) e DXGI\_FORMAT\_D32\_FLOAT\_S8X24\_UINT (e formatos relacionados na família R32G8X24).

Algumas implementações armazenam profundidade e estêncil em alocações separadas enquanto outras os armazenam juntos. O gerenciamento de blocos para os dois esquemas precisaria ser diferente, e nenhuma API única pode abstrair ou racionalizar as diferenças. É recomendável que o hardware futuro tenha suporte a superfícies de profundidade e estêncil independentes, cada uma colocada lado a lado de forma independente.

A profundidade de 32 bits teria blocos de 128 x 128, e o estêncil de 8 bits teria blocos de 256 x 256. Portanto, os aplicativos precisam conviver com o desalinhamento do formato de bloco entre a profundidade e o estêncil. Mas o mesmo problema já existe com formatos de superfície de destino de renderização diferentes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Processo cruzado de recursos de streaming e compartilhamento de dispositivos](streaming-resource-cross-process-and-device-sharing.md)

 

 



