---
title: Usar streaming-.NET TraceProcessing
description: Neste tutorial, saiba como usar o streaming para acessar dados de rastreamento imediatamente e usando menos memória.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: e04f306a6a5c03d1f502b9cfb6c2cbb737e0098f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629070"
---
# <a name="use-streaming-with-traceprocessor"></a>Usar streaming com TraceProcessor

Por padrão, o TraceProcessor acessa os dados carregando-os na memória à medida que o rastreamento é processado. Essa abordagem de buffer é fácil de usar, mas pode ser cara em termos de uso de memória.

TraceProcessor também fornece rastreamento. UseStreaming (), que dá suporte ao acesso a vários tipos de dados de rastreamento de forma contínua (processando dados conforme eles são lidos do arquivo de rastreamento, em vez de armazenar em buffer esses dados na memória). Por exemplo, um rastreamento syscalls pode ser muito grande e o armazenamento em buffer de toda a lista de syscalls em um rastreamento pode ser bastante caro.

## <a name="accessing-buffered-data"></a>Acessando dados armazenados em buffer

O código a seguir mostra o acesso a dados syscall na maneira normal e em buffer por meio do rastreamento. UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<ISyscallDataSource> pendingSyscallData = trace.UseSyscalls();

            trace.Process();

            ISyscallDataSource syscallData = pendingSyscallData.Result;

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            foreach (ISyscall syscall in syscallData.Syscalls)
            {
                IProcess process = syscall.Thread?.Process;

                if (process == null)
                {
                    continue;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            }

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="accessing-streaming-data"></a>Acessando dados de streaming

Com um rastreamento syscalls grande, a tentativa de armazenar em buffer os dados syscall na memória pode ser bastante dispendiosa ou nem mesmo ser possível. O código a seguir mostra como acessar os mesmos dados syscall de forma contínua, substituindo o rastreamento. UseSyscalls () com Trace. UseStreaming(). UseSyscalls():

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Processes;
using Microsoft.Windows.EventTracing.Syscalls;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 1)
        {
            Console.Error.WriteLine("Usage: <trace.etl>");
            return;
        }

        using (ITraceProcessor trace = TraceProcessor.Create(args[0]))
        {
            IPendingResult<IThreadDataSource> pendingThreadData = trace.UseThreads();

            Dictionary<IProcess, int> syscallsPerCommandLine = new Dictionary<IProcess, int>();

            trace.UseStreaming().UseSyscalls(ConsumerSchedule.SecondPass, context =>
            {
                Syscall syscall = context.Data;
                IProcess process = syscall.GetThread(pendingThreadData.Result)?.Process;

                if (process == null)
                {
                    return;
                }

                if (!syscallsPerCommandLine.ContainsKey(process))
                {
                    syscallsPerCommandLine.Add(process, 0);
                }

                ++syscallsPerCommandLine[process];
            });

            trace.Process();

            Console.WriteLine("Process Command Line: Syscalls Count");

            foreach (IProcess process in syscallsPerCommandLine.Keys)
            {
                Console.WriteLine($"{process.CommandLine}: {syscallsPerCommandLine[process]}");
            }
        }
    }
}
```

## <a name="how-streaming-works"></a>Como funciona a transmissão

Por padrão, todos os dados de streaming são fornecidos durante a primeira passagem pelo rastreamento e os dados armazenados em buffer de outras fontes não estão disponíveis. O exemplo acima mostra como combinar o streaming com o buffer – os dados de thread são armazenados em buffer antes de os dados syscall serem transmitidos. Como resultado, o rastreamento deve ser lido duas vezes – uma vez para obter dados de thread em buffer e uma segunda vez para acessar dados syscall de streaming com os dados de thread em buffer agora disponíveis. Para combinar streaming e buffer dessa forma, o exemplo passa ConsumerSchedule. SecondPass para Trace. UseStreaming(). UseSyscalls (), que faz com que o processamento de syscall aconteça em uma segunda passagem pelo rastreamento. Ao executar em uma segunda passagem, o retorno de chamada syscall pode acessar o resultado pendente do rastreamento. UseThreads () quando processa cada syscall. Sem esse argumento opcional, o streaming syscall teria sido executado na primeira passagem pelo rastreamento (haveria apenas uma passagem) e o resultado pendente do rastreamento. UseThreads () ainda não estaria disponível. Nesse caso, o retorno de chamada ainda teria acesso ao ThreadId da syscall, mas ele não teria acesso ao processo para o thread (porque o thread para processar dados de vinculação é fornecido por meio de outros eventos que talvez ainda não tenham sido processados).

Algumas diferenças importantes no uso entre o armazenamento em buffer e o streaming:

1. O armazenamento em buffer retorna um [IPendingResult&lt;t&gt;](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.ipendingresult-1)e o resultado que ele contém está disponível somente antes de o rastreamento ter sido processado. Depois que o rastreamento foi processado, os resultados podem ser enumerados usando técnicas como ForEach e LINQ.
2. O streaming retorna void e, em vez disso, usa um argumento de retorno de chamada. Ele chama o retorno de chamada uma vez, à medida que cada item fica disponível. Como os dados não são armazenados em buffer, nunca há uma lista de resultados a serem enumerados com foreach ou LINQ – o retorno de chamada de streaming precisa armazenar em buffer qualquer parte dos dados que deseja salvar para uso após a conclusão do processamento.
3. O código para processar dados armazenados em buffer aparece após a chamada para Trace. Process (), quando os resultados pendentes estiverem disponíveis.
4. O código para processar dados de streaming aparece antes da chamada para Trace. Process (), como um retorno de chamada para o rastreamento. UseStreaming. Use... ().
5. Um consumidor de streaming pode optar por processar apenas parte do fluxo e cancelar retornos de chamada futuros chamando o contexto. Cancel (). Um consumidor de buffer sempre recebe uma lista completa e armazenada em buffer.

## <a name="correlated-streaming-data"></a>Dados de streaming correlacionados

Às vezes, os dados de rastreamento vêm em uma sequência de eventos – por exemplo, syscalls são registrados por meio de eventos de inserção e saída separados, mas os dados combinados de ambos os eventos podem ser mais úteis. O método Trace. UseStreaming(). UseSyscalls () correlaciona os dados de ambos os eventos e fornece-os à medida que o par se torna disponível. Alguns tipos de dados correlacionados estão disponíveis por meio de rastreamento. UseStreaming():

| Código                                        | Descrição                                                                                                                                     |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| rastreou. UseStreaming(). UseContextSwitchData() | Transmite dados de alternância de contexto correlacionados (de eventos compactados e não compactados, com SwitchInThreadIds mais precisos do que eventos não compactados brutos). |
| rastreou. UseStreaming(). UseScheduledTasks()    | Transmite dados de tarefa agendados correlacionados.                                                                                                         |
| rastreou. UseStreaming(). UseSyscalls()          | Transmite dados de chamada do sistema correlacionados.                                                                                                            |
| rastreou. UseStreaming(). UseWindowInFocus()     | Transmite dados correlacionados à janela em foco.                                                                                                        |

## <a name="standalone-streaming-events"></a>Eventos de streaming autônomos

Além disso, Trace. UseStreaming () fornece eventos analisados para vários tipos de eventos autônomos diferentes:

| Código                                               | Descrição                                     |
|----------------------------------------------------|-------------------------------------------------|
| rastreou. UseStreaming(). UseLastBranchRecordEvents()   | Streams analisados pela última vez eventos de registro de ramificação (LBR). |
| rastreou. UseStreaming(). UseReadyThreadEvents()        | Fluxos de eventos de thread prontos analisados.             |
| rastreou. UseStreaming(). UseThreadCreateEvents()       | Fluxos de criação de eventos de thread analisados.            |
| rastreou. UseStreaming(). UseThreadExitEvents()         | Transmite eventos de saída de thread analisados.              |
| rastreou. UseStreaming(). UseThreadRundownStartEvents() | Fluxos de eventos de início de encerramento de thread analisados.     |
| rastreou. UseStreaming(). UseThreadRundownStopEvents()  | Eventos de parada de encerramento de thread analisados por fluxos.      |
| rastreou. UseStreaming(). UseThreadSetNameEvents()      | Fluxos de eventos de nome de conjunto de threads analisados.          |

## <a name="underlying-streaming-events-for-correlated-data"></a>Eventos de streaming subjacentes para dados correlacionados

Por fim, Trace. UseStreaming () também fornece os eventos subjacentes usados para correlacionar dados na lista acima. Esses eventos subjacentes são:

| Código                                                        | Descrição                                                                                | Incluída em                                 |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------|---------------------------------------------|
| rastreou. UseStreaming(). UseCompactContextSwitchEvents()        | Fluxos de eventos de alternância de contexto compacto analisados.                                              | rastreou. UseStreaming(). UseContextSwitchData() |
| rastreou. UseStreaming(). UseContextSwitchEvents()               | Fluxos de eventos de alternância de contexto analisados. SwitchInThreadIds pode não ser preciso em alguns casos. | rastreou. UseStreaming(). UseContextSwitchData() |
| rastreou. UseStreaming(). UseFocusChangeEvents()                 | Fluxos de eventos de alteração de foco da janela analisada.                                                 | rastreou. UseStreaming(). UseWindowInFocus()     |
| rastreou. UseStreaming(). UseScheduledTaskStartEvents()          | Fluxos de eventos de início da tarefa agendada analisada.                                                | rastreou. UseStreaming(). UseScheduledTasks()    |
| rastreou. UseStreaming(). UseScheduledTaskStopEvents()           | Eventos de interrupção de tarefa agendada analisados por fluxos.                                                 | rastreou. UseStreaming(). UseScheduledTasks()    |
| rastreou. UseStreaming(). UseScheduledTaskTriggerEvents()        | Fluxos de eventos de gatilho de tarefa agendada analisados.                                              | rastreou. UseStreaming(). UseScheduledTasks()    |
| rastreou. UseStreaming(). UseSessionLayerSetActiveWindowEvents() | Fluxos de eventos da janela ativa do conjunto de camadas de sessão analisada.                                     | rastreou. UseStreaming(). UseWindowInFocus()     |
| rastreou. UseStreaming(). UseSyscallEnterEvents()                | A syscall analisada transmite eventos de entrada.                                                       | rastreou. UseStreaming(). UseSyscalls()          |
| rastreou. UseStreaming(). UseSyscallExitEvents()                 | Eventos de saída syscall analisados por fluxos.                                                        | rastreou. UseStreaming(). UseSyscalls()          |

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você aprendeu a usar o streaming para acessar dados de rastreamento imediatamente e usando menos memória.

A próxima etapa é procurar acessar os dados desejados em seus rastreamentos. Examine as [amostras](https://github.com/microsoft/eventtracing-processing-samples) para ver algumas ideias. Observe que nem todos os rastreamentos incluem todos os tipos de dados com suporte.
