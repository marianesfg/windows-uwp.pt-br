---
title: Operações disponíveis em pools de blocos
description: As operações em pools de blocos incluem o redimensionamento de um pool de blocos, o que oferece recursos (memória temporariamente para o sistema e o pool de blocos inteiro) e recuperação de recursos.
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- Operações disponíveis em pools de blocos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec546d467f338bbbe2f4dbf89015a4487e001718
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864310"
---
# <a name="operations-available-on-tile-pools"></a>Operações disponíveis em pools de blocos


As operações em pools de blocos incluem o redimensionamento de um pool de blocos, o que oferece recursos (memória temporariamente para o sistema e o pool de blocos inteiro) e recuperação de recursos.

-   O tempo dos pools de blocos funciona como qualquer outro recurso do Direct3D, com o suporte da contagem de referência, incluindo nesse caso o acompanhamento do mapeamentos de recursos de streaming. Quando o app não faz referência a um pool de blocos e todos os mapeamentos de blocos na memória são apagados e os acessos à unidade de processamento gráfico (GPU) são concluídos, o sistema operacional desalocará o pool de blocos.
-   As APIs relacionadas ao compartilhamento de superfície e ao trabalho de sincronização funcionam para pools de blocos (mas não diretamente em recursos de streaming). Semelhante ao comportamento dos pools de blocos oferecidos, os comandos do Direct3D que acessam recursos de streaming que apontam para um pool de blocos são ignorados se o pool de blocos foi compartilhado e atualmente é adquirido por outro processo e dispositivo.
-   Redimensionamento do pool de blocos.
-   Oferta e recuperação de recursos: essas operações de produção de memória temporária para o sistema operam no pool de blocos inteiro (e não estão disponíveis para recursos de streaming individuais). Se um recurso de streaming aponta para um bloco em um pool de blocos oferecido, o recurso de streaming se comporta como se foi oferecido (por exemplo, o tempo de execução ignora comandos que fazem referência a ele).

Os dados não podem ser copiados de e para a memória diretamente do pool de blocos. Os acessos à memória são sempre feitos por meio de recursos de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Tópicos relacionados


[Criar recursos de streaming](creating-streaming-resources.md)

 

 




