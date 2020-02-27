---
title: Início rápido do processo a Trace-.NET TraceProcessing
description: Neste guia de início rápido, saiba como acessar dados em um rastreamento ETW.
author: maiak
ms.author: maiak
ms.date: 02/20/2020
ms.topic: quickstart
ms.openlocfilehash: 162646baff9b2d08f6fc0ea4862802216cff9619
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629097"
---
# <a name="quickstart-process-your-first-trace"></a>Início rápido: processar seu primeiro rastreamento

Experimente o TraceProcessor para acessar dados em um rastreamento de ETW (rastreamento de eventos para Windows). O TraceProcessor permite que você acesse dados de rastreamento ETW como objetos .NET.

Neste início rápido, você aprenderá a:

1. Instale o pacote NuGet do TraceProcessing.
2. Crie um TraceProcessor.
3. Use TraceProcessor para acessar as linhas de comando do processo contidas no rastreamento.

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Visual Studio 2019

## <a name="install-the-traceprocessing-nuget-package"></a>Instalar o pacote NuGet do TraceProcessing

O .NET TraceProcessing está disponível no [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) com a seguinte ID de pacote:

Microsoft. Windows. EventTracing. Processing. All

Você pode usar esse pacote em um aplicativo de console para listar as linhas de comando de processo contidas em um rastreamento ETW (arquivo. ETL).

1. Crie um novo aplicativo de console .NET Core. No Visual Studio, selecione Arquivo, novo, projeto... e escolha o modelo aplicativo de console (.NET Core) para C#o.

    Insira um nome de projeto, por exemplo, TraceProcessorQuickstart, e escolha criar.

2. Em Gerenciador de Soluções, clique com o botão direito do mouse em dependências e escolha gerenciar pacotes NuGet... e alterne para a guia procurar.

3. Na caixa de pesquisa, insira Microsoft. Windows. EventTracing. Processing. All e Search.

    Selecione instalar no pacote NuGet com esse nome e feche a janela do NuGet.

## <a name="create-a-traceprocessor"></a>Criar um TraceProcessor

1. Altere Program.cs para o seguinte conteúdo:

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                // TODO: call trace.Use...

                trace.Process();

                Console.WriteLine("TODO: Access data from the trace");
            }
        }
    }
    ```

2. Forneça um nome de rastreamento a ser usado ao executar o projeto.

    Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e escolha Propriedades. Alterne para a guia Depurar e insira o caminho para um rastreamento (arquivo. ETL) em argumentos do aplicativo.

    Se você ainda não tiver um arquivo de rastreamento, poderá usar o [gravador de desempenho do Windows](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) para criar um.

3. Execute o aplicativo.

    Escolha Depurar, iniciar sem depuração para executar seu código.

## <a name="use-traceprocessor-to-access-process-command-lines-contained-in-the-trace"></a>Usar TraceProcessor para acessar as linhas de comando do processo contidas no rastreamento

1. Altere Program.cs para o seguinte conteúdo:

    ```csharp
    using Microsoft.Windows.EventTracing;
    using Microsoft.Windows.EventTracing.Processes;
    using System;

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
                IPendingResult<IProcessDataSource> pendingProcessData = trace.UseProcesses();

                trace.Process();

                IProcessDataSource processData = pendingProcessData.Result;

                foreach (IProcess process in processData.Processes)
                {
                    Console.WriteLine(process.CommandLine);
                }
            }
        }
    }
    ```

2. Execute o aplicativo novamente.

    Desta vez, você verá uma lista de linhas de comando de todos os processos que estavam em execução enquanto o rastreamento estava sendo gravado.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Neste guia de início rápido, você criou um aplicativo de console, instalou o TraceProcessor e o utilizou para acessar as linhas de comando do processo de um rastreamento ETW. Agora você tem um aplicativo que acessa dados de rastreamento.

As informações do processo são apenas um dos muitos tipos de dados armazenados em um rastreamento ETW que seu aplicativo pode acessar.

A próxima etapa é [examinar mais de perto o TraceProcessor](tutorial.md) e as outras fontes de dados que ele pode acessar.
