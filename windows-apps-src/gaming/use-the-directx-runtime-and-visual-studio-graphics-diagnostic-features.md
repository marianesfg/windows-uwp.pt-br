---
author: mtoepke
title: "Ferramentas de diagnóstico de elementos gráficos"
description: "Saiba como obter e usar os recursos de diagnóstico de elementos gráficos, incluindo depuração de elementos gráficos, análise do quadro de elementos gráficos e uso da GPU no Visual Studio."
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
translationtype: Human Translation
ms.sourcegitcommit: 765cde8afe32cef6515ee6ca9750365aa35984a2
ms.openlocfilehash: 5d8f7a8dbba2da76c6093d034c2ca8c2b4592c7e

---

# Ferramentas de diagnóstico de elementos gráficos


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Com o Windows 10, as ferramentas de diagnóstico de gráficos agora estão disponíveis dentro do Windows como recurso opcional. Para usar os recursos de diagnóstico de gráficos fornecidos em tempo de execução e no Visual Studio para desenvolver jogos ou aplicativos DirectX, instale o recurso opcional Ferramentas Gráficas:

1.  Vá para **Configurações**, selecione **Sistema**, selecione **Aplicativos e Recursos** e clique em **Gerenciar recursos opcionais**.
2.  Clique em **Adicionar um recurso**.   
3.  Na lista **Recursos opcionais**, selecione **Ferramentas de Gráficos** e clique em **Instalar**.

Recursos de diagnóstico de elementos gráficos incluem a capacidade de criar dispositivos de depuração do Direct3D (por meio de camadas Direct3D SDK) no tempo de execução do DirectX, além de depuração de elementos gráficos, análise de quadros e uso da GPU.

-   A depuração de elementos gráficos permite rastrear chamadas Direct3D feitas pelo seu aplicativo. Você pode então repetir essas chamadas, inspecionar parâmetros, depurar e experimentar com sombreadores e visualizar ativos gráficos para diagnosticar problemas de renderização. Os logs podem ser feitos em computadores Windows, simuladores ou dispositivos e reproduzidos em um hardware diferente.
-   A Análise de Quadros de Gráficos no Visual Studio é executada em um log de depuração de elementos gráficos e reúne o tempo de linha de base para as chamadas draw do Direct3D. Em seguida, ela executa um conjunto de experimentos modificando várias configurações de elementos gráficos e produz uma tabela de resultados de tempo. Você pode usar esses dados para compreender os problemas de desempenho de elementos gráficos em seu aplicativo e analisar os resultados dos vários experimentos para identificar oportunidades de melhoria no desempenho.
-   O uso da GPU no Visual Studio permite que você monitore o uso da GPU em tempo real. Ela coleta e analisa os dados de tempo das cargas de trabalho que estejam sendo manipuladas pela CPU e pela GPU, para que você possa determinar onde estão os gargalos.

## Tópicos relacionados


[Visão geral do diagnóstico de gráficos no Visual Studio](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 







<!--HONumber=Aug16_HO3-->


