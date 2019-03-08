---
title: O que há de novo para o Xbox Live SDK – fevereiro de 2016
description: O que há de novo para o Xbox Live SDK – fevereiro de 2016
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594731"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>O que há de novo para o Xbox Live SDK – fevereiro de 2016

Consulte a [What's New - outubro de 2015](1510-whats-new.md) artigo para o que foi adicionado na versão 1510

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="throttling"></a>a limitação
- Limitação refinado será em breve distribuído para a maioria dos serviços do Xbox Live.  API de serviço Xbox (XSAPI) será automaticamente manipular novas tentativas e informá-lo de chamadas que são limitadas durante o desenvolvimento.  Mais detalhes podem ser encontrados na [melhores práticas de chamada Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) artigo na documentação.

## <a name="leaderboards"></a>Placares de líderes
- Placar de líderes de multicolunas agora pode ser acessados pela API do GetLeaderboard. Se você fornecer um vetor de nomes de colunas adicionais, o vetor de colunas no resultado será preenchido se existirem nessas colunas.

## <a name="documentation"></a>Documentação
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) documentação está aqui.  Você pode usar o Application Insights com uma conta gratuita do Azure para exibir eventos de plataforma de dados em tempo quase real.  Essa funcionalidade só está disponível para aplicativos UWP em execução no Windows 10 na área de trabalho.
- Documentação atualizada da ferramenta de eventos comuns do Xbox para desenvolvedores UWP, discutindo como gerar wrappers para enviar eventos de plataforma de dados.  Observe que isso é opcional e você pode continuar a usar a API WriteInGameEvent se você preferir.
- Usando o Fiddler para depurar eventos de plataforma de dados e certifique-se de que eles estão sendo enviados corretamente.  Isso é apenas para eventos UWP.
- Obter informações sobre como coletar logs para a ferramenta Analisador de rastreamento ao vivo estão disponíveis.  Consulte a [analisar chamadas para serviços do Xbox Live](../tools/analyze-service-calls.md) artigo.
