---
title: Ferramentas de diagnóstico de gráficos
description: Saiba como obter e usar os recursos de diagnóstico de elementos gráficos, incluindo depuração de elementos gráficos, análise do quadro de elementos gráficos e uso da GPU no Visual Studio.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.date: 11/20/2019
ms.topic: article
keywords: windows 10, uwp, jogos, elementos gráficos, diagnóstico, ferramentas e directx
ms.localizationpriority: medium
ms.openlocfilehash: 14711a356f00ac027bf990554c90547017b9cc4d
ms.sourcegitcommit: f464e5157ca22cfe31075f2f1219b8227587bf03
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263093"
---
# <a name="graphics-diagnostics-tools"></a>Ferramentas de diagnóstico de gráficos

As ferramentas de diagnóstico de gráficos estão disponíveis no Windows 10 como um recurso opcional. Para usar os recursos de diagnóstico de gráficos (fornecidos no tempo de execução e o Visual Studio) para desenvolver aplicativos ou jogos do DirectX, instale o recurso ferramentas gráficas opcionais.

1. Acesse **configurações** > **aplicativos** > **aplicativos & recursos/recursos opcionais**.
2. Se as **ferramentas de gráficos** já estiverem listadas em **recursos instalados**, você estará pronto. Caso contrário, clique em **Adicionar um recurso**.
3. Procure e/ou selecione **ferramentas de gráficos**e, em seguida, clique em **instalar**.

Recursos de diagnóstico de elementos gráficos incluem a capacidade de criar dispositivos de depuração do Direct3D (por meio de camadas Direct3D SDK) no tempo de execução do DirectX, além de depuração de elementos gráficos, análise de quadros e uso da GPU.

-   A depuração de elementos gráficos permite rastrear chamadas Direct3D feitas pelo seu aplicativo. Você pode então repetir essas chamadas, inspecionar parâmetros, depurar e experimentar com sombreadores e visualizar ativos gráficos para diagnosticar problemas de renderização. Você pode fazer logs em computadores Windows, simuladores ou dispositivos e reproduzi-los novamente em hardwares diferentes.
-   Análise de Quadros de Gráficos no Visual Studio é executado em um log de depuração de gráficos e coleta o tempo de linha de base para as chamadas do Direct3D Draw. Em seguida, ele executa um conjunto de experimentos modificando várias configurações de gráficos e produz uma tabela de resultados de tempo. Você pode usar esses dados para compreender os problemas de desempenho de elementos gráficos em seu aplicativo e analisar os resultados dos vários experimentos para identificar oportunidades de melhoria no desempenho.
-   O uso da GPU no Visual Studio permite que você monitore o uso da GPU em tempo real. Ele coleta e analisa os dados de tempo das cargas de trabalho que estão sendo tratadas pela CPU e pela GPU, para que você possa determinar onde estão os gargalos.

## <a name="related-topics"></a>Tópicos relacionados

[Visão geral do Diagnóstico de Gráficos no Visual Studio](/visualstudio/debugger/overview-of-visual-studio-graphics-diagnostics?view=vs-2015)