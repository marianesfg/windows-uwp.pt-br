---
title: Extensibilidade-.NET TraceProcessing
description: Neste tutorial, saiba como estender o .NET TraceProcessing.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629137"
---
# <a name="extend-traceprocessor"></a>Estender TraceProcessor

Muitos tipos de dados de rastreamento têm suporte interno em [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor), mas se você tiver seus outros provedores que deseja analisar (incluindo seus próprios provedores personalizados), esses dados também estarão disponíveis no rastreamento ao vivo enquanto o processamento ocorre.

Por exemplo, aqui está uma maneira simples de obter a lista de IDs de provedores em um rastreamento:

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

O exemplo a seguir mostra uma fonte de dados personalizada simplificada:

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
