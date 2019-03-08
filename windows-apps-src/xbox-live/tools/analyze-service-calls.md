---
title: Analisador de rastreamento em tempo real do Xbox
description: Saiba como usar o analisador de rastreamento do Xbox Live para examinar as chamadas de serviço feitas pelo seu título.
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, uma, chamadas de serviço, testes, o analisador de rastreamento
ms.localizationpriority: medium
ms.openlocfilehash: fa8ca37842edfbeaab0063cd953f3a34358a82da
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623461"
---
# <a name="xbox-live-trace-analyzer"></a>Analisador de rastreamento em tempo real do Xbox

A API de serviços do Xbox Live agora permite que os desenvolvedores de título capturar todas as chamadas de serviço e, em seguida, analisá-los offline para quaisquer violações nos padrões de chamada. O rastreamento de chamadas de serviço pode ser ativado usando novas funcionalidades disponíveis na ferramenta de linha de comando xbtrace ou por meio da ativação de protocolo para cenários mais avançados. Também há suporte para a ativação de chamada de serviço de controle diretamente no código do título. A ferramenta de análise offline, chamada Xbox Live rastreamento Analyzer (XBLTraceAnalyzer.exe) pode ser encontrada como parte do pacote do Xbox Live Tools [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).


## <a name="gather-logs-and-analyze-the-service-calls"></a>Coletar logs e analisar as chamadas de serviço

As etapas a seguir são necessárias para coletar os logs que contêm o registro de suas chamadas de serviço e analisá-los usando o analisador de rastreamento do Xbox Live.

1.  Crie seu título usando a versão do Xbox Live API dos serviços que está incluído em julho de 2015 ou versão mais recente do Xbox Developer Kit (XDK).
2.  Modificar o título para habilitar o rastreamento conforme descrito abaixo.
3.  Implante seu título.
4.  Inicie o título e faça pelo menos uma chamada para serviços do Xbox Live para inicializar a API de serviços do Xbox Live.
5.  Inicie o rastreamento no ponto em seu título que você deseja analisar.
6.  Pare o rastreamento.
7.  Execute a ferramenta Analisador de rastreamento do Xbox Live em seu computador de desenvolvimento e exibir a saída.

## <a name="starting-and-stopping-tracing"></a>Iniciando e parando rastreamento

Há três maneiras de iniciar e parar o rastreamento:

1.  Você pode chamar um conjunto de APIs de serviços do Xbox Live diretamente do seu título.
2.  Você pode usar o *xbtrace* ferramenta de linha de comando.
3.  Você pode usar a ativação de protocolo por meio de *gerenciamento de aplicativos (xbapp.exe)* ferramenta de linha de comando.


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>Iniciando e parando rastreamento diretamente do seu título

Para iniciar o rastreamento diretamente do seu título, faça o seguinte:

1.  No `Microsoft::Xbox::Services::Experimental` namespace, defina a `EnableServiceCallTracking` propriedade do `ServiceCallTrackerSettings` classe como true.
2.  Chamar `StartServiceCallTracking()` para iniciar o rastreamento de chamadas de serviço.
3.  Chamar `StopServiceCallTracking()` para parar o rastreamento de chamadas de serviço.
4.  Depois que o rastreamento é interrompido, copiar o arquivo de rastreamento resultante de na unidade temporária do desenvolvedor no console para o seu PC usando o *cópia de arquivo (xbcp.exe)* ou o *Xbox uma vizinhança* para analisá-los usando o analisador de rastreamento do Xbox Live.

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>Iniciando e parando rastreamento usando a ferramenta de linha de comando xbtrace

A maneira mais conveniente e direta para iniciar o rastreamento é usar a ferramenta de linha de comando xbtrace com o tipo de rastreamento xboxliveservices. Quando você usa xbTrace, o arquivo de rastreamento resultante é copiado para o seu PC para você.

Iniciando e parando rastreamentos usando xbtrace depende a ativação de protocolo. Antes de usar xbtrace para iniciar e parar o rastreamento, você deve inicializar a ativação de protocolo por meio da chamada a `RegisterForProtcolActivation` método no `ServiceCallTrackerSettings` classe.

O exemplo a seguir mostra como iniciar e parar um rastreamento de serviços do Xbox Live usando xbTrace:

    xbtrace start xboxliveservices
    xbtrace stop


Lembre-se de que o título deve estar em execução e ativação de protocolo deve ser inicializada antes de iniciar e parar o rastreamento com xbtrace. Depois que o rastreamento é interrompido, xbtrace copia o arquivo de rastreamento para seu computador de desenvolvimento e o coloca em um diretório cujo nome inclui "xbtrace" e um carimbo de hora. O nome desse diretório pode ser substituído usando \[etlfile\] opção para xbtrace.

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>Iniciando e parando rastreamento usando a ativação de protocolo
----------------------------------------------------------
O rastreamento também pode ser controlado usando os recursos de ativação de protocolo de "lançamento xbApp". Você deve saber titleid do seu título para iniciar e parar o rastreamento por meio da ativação de protocolo. Você pode encontrar sua id de título no arquivo de manifesto do seu título. O rastreamento é controlado por meio de URIs que contêm o parâmetro "serviceCallTracking". Os exemplos a seguir mostram como iniciar e parar o rastreamento para um título cujo id título é 12345678:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

Quando você usa a ativação de protocolo, o arquivo de rastreamento resultante é armazenado na unidade de rascunho do desenvolvedor no console. Você precisará copiar o arquivo de volta para seu PC usando xbcp ou o Xbox uma vizinhança. O arquivo não é copiado automaticamente para o PC encontram ao usar xbtrace.

Ativação de protocolo permite que você defina os parâmetros de rastreamento adicionais, como um nível de detalhes. Há suporte para quatro níveis de detalhamento: quiet, diagnóstico, detalhadas e mínimo. O exemplo a seguir mostra como definir um nível de detalhamento:

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>Analisar o arquivo de rastreamento

Depois que o arquivo de rastreamento tiver sido copiado para o seu PC, você pode usar a ferramenta Analisador de rastreamento do Xbox Live em GNDP para analisar o uso do seu título dos serviços do Xbox Live. Consulte a documentação incluída com a ferramenta Analisador de rastreamento do Xbox Live no jogo Developer Network para obter uma descrição de como invocar a ferramenta e interpretar a saída. Você também pode executar XBLTraceAnalyzer.exe com a opção de linha de comando? ou -h para exibir a Ajuda de linha de comando.
