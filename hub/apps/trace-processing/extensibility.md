---
title: Extensibilidade-.NET TraceProcessing
description: Neste tutorial, saiba como estender o .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: 59722f1f31364c464a8a763d28f3d15ef13609a8
ms.sourcegitcommit: cfba95a96202c4250de845115d1b99361412a779
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77903283"
---
# <a name="extend-traceprocessor"></a>Estender TraceProcessor

Muitos tipos de dados de rastreamento têm suporte interno em [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor), mas se você tiver seus outros provedores que deseja analisar (incluindo seus próprios provedores personalizados), esses dados também estarão disponíveis no rastreamento ao vivo enquanto o processamento ocorre.

> [!NOTE]
> Esta parte da API está em versão prévia e em desenvolvimento ativo. Ele pode ser alterado em versões futuras.

Por exemplo, aqui está uma maneira simples de obter a lista de IDs de provedores em um rastreamento.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

O exemplo a seguir mostra uma fonte de dados personalizada simplificada.

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você aprendeu a estender o TraceProcessor.

A próxima etapa é aprender a [carregar símbolos](symbols.md) para rastreamentos.
