---
title: Acessar dados de rastreamento-.NET TraceProcessing
description: Neste tutorial, saiba como acessar os dados de rastreamento usando o TraceProcessor.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: tutorial
ms.openlocfilehash: 170a8c3084e180714a319d67dca2b6a5756ea474
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629101"
---
# <a name="access-trace-data"></a>Acessar dados de rastreamento

O .NET TraceProcessing está disponível no [NuGet](https://www.nuget.org/packages/Microsoft.Windows.EventTracing.Processing.All) com a seguinte ID de pacote:

Microsoft. Windows. EventTracing. Processing. All

Esse pacote permite que você acesse dados em um arquivo de rastreamento. Se você ainda não tiver um arquivo de rastreamento, poderá usar o [gravador de desempenho do Windows](https://docs.microsoft.com/windows-hardware/test/wpt/start-a-recording) para criar um.

O exemplo de aplicativo de console a seguir mostra como acessar as linhas de comando de todos os processos contidos no rastreamento:

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

## <a name="using-traceprocessor"></a>Usando TraceProcessor

Para processar um rastreamento, chame [TraceProcessor. Create](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor.create). A interface principal é [ITraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itraceprocessor)e o uso dessa interface envolve o seguinte padrão:

1. Primeiro, informe ao processador quais dados você deseja usar em um rastreamento
2. Segundo, processar o rastreamento; e
3. Por fim, acesse os resultados.

Informando ao processador que tipos de dados você deseja antecipadamente significa que você não precisa gastar tempo processando grandes volumes de todos os tipos possíveis de dados de rastreamento. Em vez disso, [TraceProcessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor) apenas o trabalho necessário para fornecer os tipos específicos de dados que você solicita.

## <a name="recommended-project-settings"></a>Configurações de projeto recomendadas

Há algumas configurações de projeto que recomendamos usar com TraceProcessor:

1. Recomendamos a execução de exes como 64 bits.

    O padrão do Visual Studio para um C# novo aplicativo de console .NET Framework é qualquer CPU com preferência de 32 bits verificada. O padrão para o .NET Core pode já ter a configuração recomendada.

    O processamento de rastreamento pode ser de uso intensivo de memória, especialmente com rastreamentos maiores, e é recomendável alterar o destino da plataforma para x64 (ou desmarcar preferir o 32 bits) em exes que usam TraceProcessor. Para alterar essas configurações, consulte a guia compilar em Propriedades do projeto. Para alterar essas configurações para todas as configurações, verifique se a lista suspensa configuração está definida como todas as configurações, em vez do padrão somente da configuração atual.

2. Sugerimos usar o NuGet com o modo de PackageReference de estilo mais recente em vez do modo Packages. config mais antigo.

    Para alterar o padrão para novos projetos, consulte ferramentas, Gerenciador de pacotes NuGet, configurações do Gerenciador de pacotes, Gerenciamento de Pacotes, formato de gerenciamento de pacotes padrão.

## <a name="built-in-data-sources"></a>Fontes de dados internas

Um arquivo. etl pode capturar muitos tipos de dados em um rastreamento. Observe que os dados estão em um arquivo. etl depende de quais provedores foram habilitados quando o rastreamento foi capturado. A lista a seguir mostra os tipos de dados de rastreamento disponíveis em TraceProcessor:

| Código                                      | Descrição                                                                                                                | Itens de WPA relacionados                                                    |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| rastreou. UseClassicEvents()                  | Fornece eventos ETW clássicos de um rastreamento, que não incluem informações de esquema.                                         | Tabela de eventos genéricos (quando o tipo de evento é clássico ou WPP)             |
| rastreou. UseConnectedStandbyData()           | Fornece dados de um rastreamento sobre o sistema entrando e saindo do modo de espera conectado.                                        | Tabela de resumo do CS                                                     |
| rastreou. UseCpuIdleStates()                  | Fornece dados de um rastreamento sobre a CPU C-States.                                                                             | Tabela de Estados ociosos de CPU (quando o tipo é real)                          |
| rastreou. UseCpuSamplingData()                | Fornece dados de um rastreamento sobre o uso da CPU com base na amostragem periódica do ponteiro de instrução.                          | Tabela de uso da CPU (amostrada)                                            |
| rastreou. UseCpuSchedulingData()              | Fornece dados de um rastreamento sobre o agendamento de thread de CPU, incluindo alternâncias de contexto e eventos de thread prontos.                | Tabela de uso da CPU (preciso)                                            |
| rastreou. UseDevicePowerData()                | Fornece dados de um rastreamento sobre o dispositivo D-Estados.                                                                          | Tabela de dispositivos DState                                                  |
| rastreou. UseDirectXData()                    | Fornece dados de um rastreamento sobre a atividade do DirectX.                                                                         | Tabela de utilização de GPU                                                |
| traceUseDiskIOData()                      | Fornece dados de um rastreamento sobre a atividade de e/s de disco.                                                                        | Tabela de uso do disco                                                     |
| rastreou. UseEnergyEstimationData()           | Fornece dados de um rastreamento sobre o uso de energia estimado por processo do mecanismo de estimativa de energia.                         | Tabela de resumo do mecanismo de estimativa de energia (por processo)                  |
| rastreou. UseEnergyMeterData()                | Fornece dados de um rastreamento sobre o uso de energia medido da interface de medição de energia (EMI).                                  | Tabela do mecanismo de estimativa de energia (por EMI)                              |
| rastreou. UseFileIOData()                     | Fornece dados de um rastreamento sobre a atividade de e/s de arquivo.                                                                        | Tabela de e/s de arquivo                                                       |
| rastreou. UseGenericEvents()                  | Fornece eventos manifestados e TraceLogging de um rastreamento.                                                                  | Tabela de eventos genéricos (quando o tipo de evento é manifestado ou TraceLogging) |
| rastreou. UseHandles()                        | Fornece dados parciais de um rastreamento sobre identificadores de kernel ativos.                                                            | Tabela de identificadores                                                        |
| rastreou. UseHardFaults()                     | Fornece dados de um rastreamento sobre falhas de página de hardware.                                                                         | Tabela de falhas de hardware                                                    |
| rastreou. UseHeapSnapshots()                  | Fornece dados de um rastreamento sobre o uso do heap de processo.                                                                       | Tabela de instantâneo de heap                                                  |
| rastreou. UseHypercalls()                     | Fornece dados sobre hiperchamadas do Hyper-V que ocorreu durante um rastreamento.                                                        |                                                                      |
| rastreou. UseImageSections()                  | Fornece dados de um rastreamento sobre as seções de uma imagem.                                                                 | Coluna de nome da seção da tabela de uso da CPU (amostrada)                 |
| rastreou. UseInterruptHandlingData()          | Fornece dados de um rastreamento sobre ISR (rotina de serviço de interrupção) e atividade de DPC (chamada de procedimento adiado).               | Tabela DPC/ISR                                                        |
| rastreou. UseMarks()                          | Fornece as marcas (rótulos de carimbo de data/hora) de um rastreamento.                                                                      | Tabela de marcas                                                          |
| rastreou. UseMemoryUtilizationData()          | Fornece dados de um rastreamento sobre a utilização total da memória do sistema.                                                          | Tabela de utilização de memória                                             |
| rastreou. UseMetadata()                       | Fornece metadados de rastreamento disponíveis sem processamento adicional.                                                              | Configuração do sistema, rastreamentos e geral                             |
| rastreou. UsePlatformIdleStates()             | Fornece dados de um rastreamento sobre o destino e os Estados de ociosidade da plataforma real de um sistema.                                   | Tabela de estado ocioso da plataforma                                            |
| rastreou. UsePoolAllocations()                | Fornece dados de um rastreamento sobre o uso de memória do pool de kernel.                                                                 | Tabela de resumo do pool                                                   |
| rastreou. UsePowerConfigurationData()         | Fornece dados de um rastreamento sobre a configuração de energia do sistema.                                                               | Configuração do sistema, configurações de energia                                 |
| rastreou. UsePowerDependencyCoordinatorData() | Fornece dados de um rastreamento sobre as fases do coordenador de dependências de energia ativas.                                               | Tabela de resumo da fase de notificação                                     |
| rastreou. UseProcesses()                      | Fornece dados sobre processos ativos durante um rastreamento, bem como suas imagens e PDBs.                                      | Tabela de processos; Tabela de imagens; Hub de símbolos                           |
| rastreou. UseProcessorCounters()              | Fornece dados de um rastreamento sobre os valores do contador de desempenho do processador do monitor de contador de processador (PCM).                |                                                                      |
| rastreou. UseProcessorFrequencyData()         | Fornece dados de um rastreamento sobre a frequência na qual os processadores foram executados.                                                    | Tabela de frequência do processador (quando o tipo é real)                      |
| rastreou. UseProcessorProfileData()           | Fornece dados de um rastreamento sobre o perfil de energia do processador ativo.                                                       | Tabela de perfis de processador                                             |
| rastreou. UseProcessorParkingData()           | Fornece dados de um rastreamento sobre quais processadores foram estacionados ou com estacionamento anulado.                                                 | Tabela de estado de estacionamento do processador                                        |
| rastreou. UseProcessorParkingLimits()         | Fornece dados de um rastreamento sobre o número máximo permitido de processadores não estacionados.                                        | Tabela de estado do limite de estacionamento principal                                         |
| rastreou. UseProcessorQualityOfServiceData()  | Fornece dados de um rastreamento sobre a qualidade do nível de serviço para cada processador.                                          | Tabela de classes QoS de processador                                            |
| rastreou. UseProcessorThrottlingData()        | Fornece dados de um rastreamento sobre a limitação de frequência máxima do processador.                                                   | Tabela de restrições de processador                                          |
| rastreou. UseReadyBootData()                  | Fornece dados de um rastreamento sobre a atividade de pré-busca de inicialização da inicialização pronta.                                                | Tabela de eventos de inicialização pronta                                              |
| rastreou. UseReferenceSetData()               | Fornece dados de um rastreamento sobre páginas da memória virtual usada por cada processo.                                             | Tabela de conjuntos de referência                                                  |
| rastreou. UseRegionsOfInterest()              | Fornece regiões nomeadas de intervalos de interesse de um rastreamento, conforme especificado em um arquivo de configuração XML.                       | Regiões da tabela de interesse                                            |
| rastreou. UseRegistryData()                   | Fornece dados sobre a atividade do registro durante um rastreamento.                                                                      | Tabela do registro                                                       |
| rastreou. UseResidentSetData()                | Fornece dados de um rastreamento sobre as páginas de memória virtual para cada processo que estava residente na memória física.       | Tabela de conjunto residente                                                   |
| rastreou. UseRundownData()                    | Fornece dados de um rastreamento sobre intervalos durante os quais ocorre a coleta de dados de Resumo de rastreamento.                            | Regiões sombreadas na linha do tempo do grafo                                 |
| rastreou. UseScheduledTasks()                 | Fornece dados sobre as tarefas agendadas que foram executadas durante um rastreamento.                                                               | Tabela de tarefas agendadas                                                |
| rastreou. UseServices()                       | Fornece dados sobre serviços que estavam ativos ou que tiveram seu estado capturado durante um rastreamento.                                  | Tabela de serviços; Configuração do sistema, serviços                       |
| rastreou. UseStacks()                         | Fornece dados sobre pilhas registradas durante um rastreamento.                                                                        |                                                                      |
| rastreou. UseStackEvents()                    | Fornece dados sobre eventos associados a pilhas registradas durante um rastreamento.                                                 | Tabela de pilhas                                                         |
| rastreou. UseStackTags()                      | Fornece um mapeador que agrupa pilhas de um rastreamento em marcas de pilha, conforme especificado em um arquivo de configuração XML.               | Colunas como marcação de pilha e pilha (marcas de quadros)                     |
| rastreou. UseSymbols()                        | Fornece a capacidade de carregar símbolos para um rastreamento.                                                                          | Configurar caminhos de símbolo; Carregar símbolos                                 |
| rastreou. UseSyscalls()                       | Fornece dados sobre syscalls que ocorreram durante um rastreamento.                                                                 | Tabela syscalls                                                       |
| rastreou. UseSystemMetadata()                 | Fornece metadados gerais de todo o sistema de um rastreamento.                                                                       | Configuração do sistema                                                 |
| rastreou. UseSystemPowerSourceData()          | Fornece dados de um rastreamento sobre a fonte de energia do sistema ativo (AC vs DC).                                                | Tabela de origem de energia do sistema                                            |
| rastreou. UseSystemSleepData()                | Fornece dados de um rastreamento sobre o estado geral de energia do sistema.                                                               | Tabela de transição de energia                                               |
| rastreou. UseTargetCpuIdleStates()            | Fornece dados de um rastreamento sobre a CPU de destino C-States.                                                                      | Tabela de Estados ociosos da CPU (quando o tipo é destino)                          |
| rastreou. UseTargetProcessorFrequencyData()   | Fornece dados de um rastreamento sobre as frequências de processador de destino.                                                             | Tabela de frequência do processador (quando o tipo é destino)                      |
| rastreou. UseThreads()                        | Fornece dados sobre threads ativos durante um rastreamento.                                                                         | Tabela de tempos de vida de thread                                               |
| rastreou. UseTraceStatistics()                | Fornece estatísticas sobre os eventos em um rastreamento.                                                                           | Configuração do sistema, estatísticas de rastreamento                               |
| rastreou. UseUtcData()                        | Fornece dados de um rastreamento sobre a atividade de telemetria da Microsoft usando o cliente de telemetria universal (UTC).                      | Tabela UTC                                                            |
| rastreou. UseWindowInFocus()                  | Fornece dados de um rastreamento sobre alterações na janela da interface do usuário ativa em foco.                                                 | Janela na tabela de foco                                                |
| rastreou. UseWindowsTracePreprocessorEvents() | Fornece eventos de WPP (pré-processador de rastreamento de software) do Windows a partir de um rastreamento.                                                    | Tabela de rastreamento de WPP; Tabela de eventos genéricos (quando o tipo de evento é WPP)       |
| rastreou. UseWinINetData()                    | Fornece dados de um rastreamento sobre a atividade da Internet por meio do Windows Internet (WinINet).                                         | Baixar tabela de detalhes                                               |
| rastreou. UseWorkingSetData()                 | Fornece dados de um rastreamento sobre páginas de memória virtual que estavam no conjunto de trabalho para cada categoria de processo ou kernel. | Tabela de instantâneos de memória virtual                                       |

Consulte também os métodos de extensão em [ITraceSource](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.itracesource) para todos os dados de rastreamento disponíveis ou examine o método disponível em "Trace". mostrado pelo IntelliSense.

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

Nesta visão geral, você aprendeu a acessar os dados de rastreamento usando o TraceProcessor e as fontes de dados internas que ele pode acessar.

A próxima etapa é aprender a [estender](extensibility.md) o TraceProcessor para acessar dados de rastreamento personalizados.
