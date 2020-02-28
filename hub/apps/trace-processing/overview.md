---
title: O que é .NET TraceProcessing-.NET TraceProcessing
description: Nesta visão geral, saiba o que é o .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bc2ec3035e097a8cab15b556d1b1793385b8aeb9
ms.sourcegitcommit: 7f1b64f62bc3a82ebcd3807c809363df46919195
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77705751"
---
# <a name="process-etw-traces-in-net"></a>Processar rastreamentos ETW no .NET

O [ETW (rastreamento de eventos para Windows)](https://docs.microsoft.com/windows/win32/etw/event-tracing-portal) é um sistema de coleta de rastreamento avançado integrado ao sistema operacional Windows. O Windows tem integração profunda com o ETW, incluindo dados sobre o comportamento do sistema até o kernel para eventos como alternâncias de contexto, alocação de memória, processo de criação e saída e muito mais. Os dados de todo o sistema disponíveis no ETW o tornam uma boa opção para a análise de desempenho de ponta a ponta ou outras perguntas que exigem observar a interação entre vários componentes em todo o sistema.

Ao contrário do log de texto, o ETW fornece eventos estruturados projetados para o processamento de dados automatizado. A Microsoft criou ferramentas poderosas sobre esses eventos estruturados, incluindo o [Windows Performance Analyzer (WPA)](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer), que fornece uma interface gráfica para visualizar e explorar os dados de rastreamento capturados em um arquivo de rastreamento ETW (. ETL).

Dentro da Microsoft, usamos intensamente rastreamentos ETW para medir o desempenho de novas compilações do Windows. Dado o volume de dados produzido pelo sistema de engenharia do Windows, a análise automatizada é essencial. Para nossa análise de rastreamento automatizada, usamos C# muito o .net e, portanto, criamos a [API .net TraceProcessing](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) para acessar muitos tipos de dados de rastreamento ETW. Essa tecnologia também é usada no analisador de desempenho do Windows para alimentar várias de suas tabelas.

Os pacotes NuGet do .NET TraceProcessing permitem que você analise seus próprios aplicativos e sistemas com as mesmas ferramentas que a Microsoft usa para analisar o Windows.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Nesta visão geral, você aprendeu o que é o .NET TraceProcessing.

A próxima etapa é [processar seu primeiro rastreamento](quickstart.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Acessar dados de rastreamento](tutorial.md)
* [Estender TraceProcessor](extensibility.md)
* [Carregar símbolos](symbols.md)
* [Usar Streaming](streaming.md)
* [Amostras](https://github.com/microsoft/eventtracing-processing-samples)
* [Referência de API](reference.md)
