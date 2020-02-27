---
title: Carregar símbolos-.NET TraceProcessing
description: Neste tutorial, saiba como carregar símbolos ao processar rastreamentos.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: a6954538159c6fffb3185aa8b3137af26e17b32f
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629087"
---
# <a name="use-symbols-in-net-traceprocessing"></a>Usar símbolos no .NET TraceProcessing

O [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) dá suporte ao carregamento de símbolos e à obtenção de pilhas de várias fontes de dados. O aplicativo de console a seguir examina exemplos de CPU e gera a duração estimada em que uma função específica estava em execução (com base na amostragem estatística do rastreamento de uso da CPU).

```csharp
using Microsoft.Windows.EventTracing;
using Microsoft.Windows.EventTracing.Cpu;
using Microsoft.Windows.EventTracing.Symbols;
using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.Error.WriteLine("Usage: GetCpuSampleDuration.exe <trace.etl> <imageName> <functionName>");
            return;
        }

        string tracePath = args[0];
        string imageName = args[1];
        string functionName = args[2];
        Dictionary<string, Duration> matchDurationByCommandLine = new Dictionary<string, Duration>();

        using (ITraceProcessor trace = TraceProcessor.Create(tracePath))
        {
            IPendingResult<ISymbolDataSource> pendingSymbolData = trace.UseSymbols();
            IPendingResult<ICpuSampleDataSource> pendingCpuSamplingData = trace.UseCpuSamplingData();

            trace.Process();

            ISymbolDataSource symbolData = pendingSymbolData.Result;
            ICpuSampleDataSource cpuSamplingData = pendingCpuSamplingData.Result;

            symbolData.LoadSymbolsForConsoleAsync(SymCachePath.Automatic, SymbolPath.Automatic).GetAwaiter().GetResult();

            Console.WriteLine();
            IThreadStackPattern pattern = AnalyzerThreadStackPattern.Parse($"{imageName}!{functionName}");

            foreach (ICpuSample sample in cpuSamplingData.Samples)
            {
                if (sample.Stack != null && sample.Stack.Matches(pattern))
                {
                    string commandLine = sample.Process.CommandLine;

                    if (!matchDurationByCommandLine.ContainsKey(commandLine))
                    {
                        matchDurationByCommandLine.Add(commandLine, Duration.Zero);
                    }

                    matchDurationByCommandLine[commandLine] += sample.Weight;
                }
            }

            foreach (string commandLine in matchDurationByCommandLine.Keys)
            {
                Console.WriteLine($"{commandLine}: {matchDurationByCommandLine[commandLine]}");
            }
        }
    }
}
```

A execução desse programa produz uma saída semelhante à seguinte:

```shell
C:\GetCpuSampleDuration\bin\Debug\> GetCpuSampleDuration.exe C:\boot.etl user32.dll LoadImageInternal
0.0% (0 of 1165; 0 loaded)
<snip>
100.0% (1165 of 1165; 791 loaded)
wininit.exe: 15.99 ms
C:\Windows\Explorer.EXE: 5 ms
winlogon.exe: 20.15 ms
"C:\Users\AdminUAC\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background: 2.09 ms
```

(Os detalhes da saída variam de acordo com o rastreamento).

## <a name="symbols-format"></a>Formato de símbolos

Internamente, o TraceProcessor usa o formato [SymCache](https://docs.microsoft.com/windows-hardware/test/wpt/loading-symbols#symcache-path) , que é um cache de alguns dos dados armazenados em um PDB. Ao carregar símbolos, o TraceProcessor requer a especificação de um local a ser usado para esses arquivos SymCache (um caminho SymCache) e dá suporte à especificação opcional de um SymbolPath para acessar PDBs. Quando um SymbolPath é fornecido, o TraceProcessor criará arquivos SymCache a partir dos arquivos PDB, conforme necessário, e o processamento subsequente dos mesmos dados poderá usar os arquivos SymCache diretamente para melhorar o desempenho.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste tutorial, você aprendeu a carregar símbolos ao processar rastreamentos.

A próxima etapa é aprender a usar o [streaming](streaming.md) para acessar dados de rastreamento sem armazenar em buffer tudo na memória.
