---
title: Formatos de estêncil sem suporte com recursos de streaming
description: Formatos que contêm estêncil não são compatíveis com recursos de streaming.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Não há suporte para formatos de estêncil com recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db6476afe265ea4a2556a5f787a14daa2e212e61
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735131"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Não há suporte para formatos de estêncil com recursos de streaming


Formatos que contêm estêncil não são compatíveis com recursos de streaming.

Os formatos que contêm o estêncil incluem o formato de\_DXGI\_D24\_UNORM\_S8\_UINT (e os formatos relacionados na família R24G8) e o formato DXGI\_\_D32\_FLOAT\_S8X24\_UINT (e os formatos relacionados na família R32G8X24).

Algumas implementações armazenam profundidade e estêncil em alocações separadas enquanto outras os armazenam juntos. O gerenciamento de blocos para os dois esquemas precisaria ser diferente, e nenhuma API única pode abstrair ou racionalizar as diferenças. É recomendável que o hardware futuro tenha suporte a superfícies de profundidade e estêncil independentes, cada uma colocada lado a lado de forma independente.

A profundidade de 32 bits teria blocos de 128 x 128, e o estêncil de 8 bits teria blocos de 256 x 256. Portanto, os aplicativos precisam conviver com o desalinhamento do formato de bloco entre a profundidade e o estêncil. Mas o mesmo problema já existe com formatos de superfície de destino de renderização diferentes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Recurso de streaming entre processos e compartilhamento de dispositivos](streaming-resource-cross-process-and-device-sharing.md)

 

 




