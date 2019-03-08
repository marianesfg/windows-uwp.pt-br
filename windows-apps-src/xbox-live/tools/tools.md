---
title: Ferramentas de desenvolvimento do Xbox Live
description: Saiba mais sobre as ferramentas que são fornecidas para ajudar a desenvolver e testar seu Xbox Live habilitado título.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, ferramentas, a redefinição de player, live analisador de rastreamento, LTA, ferramenta de conta ao vivo do xbox,
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623511"
---
# <a name="development-tools-for-xbox-live"></a>Ferramentas de desenvolvimento do Xbox Live

Esta seção aborda várias ferramentas que você pode usar para ajudá-lo a desenvolver para o Xbox Live. Muitas das ferramentas estão disponíveis sobre o [GitHub de ferramentas de desenvolvedor Xbox Live](https://github.com/Microsoft/xbox-live-developer-tools) repositório. Você também pode usar o [biblioteca de ferramentas de desenvolvimento](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) para criar suas próprias ferramentas personalizadas. Todas as ferramentas de desenvolvedor autônomo podem ser baixadas em [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

> [!NOTE]
> As ferramentas de MatchSim e XboxLiveCompute incluídas no download só podem ser usadas pelos parceiros gerenciados ou registrados de parceiros na [ ID@Xbox ](https://www.xbox.com/Developers/id) programa. Para saber mais sobre os programas de desenvolvedor disponíveis, consulte o [visão geral do programa de desenvolvedor](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview). 

## <a name="global-storage"></a>Armazenamento global
Armazenamento de título global é usado para armazenar dados que todos podem ler, como listas, mapas, desafios ou recursos de arte. Ele é um tipo de [título armazenamento](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md). A ferramenta de armazenamento Global é usada para gerenciar o armazenamento de título global em áreas restritas de teste. Dados ainda devem ser publicados para o varejo por meio do Partner Center ou o Portal de desenvolvedor do Xbox (XDP). A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip. Ferramentas personalizadas podem ser criadas com o [biblioteca de ferramentas de desenvolvimento](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="multiplayer-session-history-viewer"></a>Visualizador de histórico de sessão para múltiplos jogadores
Visualizador de histórico de sessão com vários participantes lhe dá a capacidade de exibir uma linha do tempo histórica de todas as alterações ao longo da sessão para múltiplos jogadores histórico de um documento (incluindo documentos excluídos). Usando essa ferramenta lhe dará uma compreensão mais profunda do que está acontecendo com os documentos de sessão MPSD conforme eles são alterados ao longo do tempo. Ele está disponível como uma ferramenta autônoma na [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip.

## <a name="player-data-reset"></a>Redefinição de dados do Player
A ferramenta de redefinição de dados do Player pode ser usada para redefinir os dados de um jogador em áreas restritas de teste. Você pode redefinir dados como; conquistas, placares de líderes, estatísticas e histórico de título. A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip. Ferramentas personalizadas podem ser criadas com o [biblioteca de ferramentas de desenvolvimento](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="xbox-live-developer-account"></a>Conta de desenvolvedor ao vivo do Xbox
A ferramenta de conta de desenvolvedor do Xbox Live é usada para gerenciar a autenticação de uma conta de desenvolvedor. Ele é necessário para interagir com outras ferramentas de desenvolvedor que exigem uma credencial de desenvolvedor, como redefinição de Player e o armazenamento Global. A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-trace-analyzer"></a>Analisador de rastreamento em tempo real do Xbox
Usando o [analisador de rastreamento do Xbox Live](analyze-service-calls.md), você pode capturar todas as chamadas de serviço e, em seguida, analisá-los offline para quaisquer violações nos padrões de chamada. O rastreamento de chamadas de serviço pode ser ativado usando a ferramenta de linha de comando xbtrace ou cenários avançado, por meio da ativação de protocolo para obter mais informações. Também há suporte para a ativação de chamada de serviço de controle diretamente no código do título. A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-account-tool"></a>Ferramenta do Xbox Live conta  
O [ferramenta de conta do Xbox Live](xbox-live-account-tool.md) foi projetado para ajudá-lo a configurar as contas de teste existente para testar cenários de jogos. Por exemplo, você pode usar a ferramenta de conta do Xbox Live para alterar o nome de jogador da conta ou adicionar rapidamente os seguidores de 1000 a lista de amigos da conta. A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip.

## <a name="config-as-source"></a>Configuração como código-fonte
[A configuração como código-fonte](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) é um conjunto de ferramentas que a Microsoft desenvolveu para acomodar usuários avançados, fornecendo ferramentas oficialmente suportadas e APIs para integrar em nossos serviços de configuração. Esses serviços do Xbox Live normalmente são configurados para o título no Partner Center, incluindo os serviços que variam de placares de líderes, conquistas, serviços web e terceiras partes confiáveis. Para muitos desenvolvedores de jogos, usar o Partner Center é suficiente. Para usuários avançados, no entanto, há um desejo de integrar tarefas comuns de configuração em seus próprios processos e ferramentas.  Configuração como código-fonte destina-se para dar suporte a esses cenários, fornecendo ferramentas de linha de comando e novas APIs para dar suporte à integração personalizada em seus fluxos de trabalho existentes e pipelines. A ferramenta está disponível por meio da linha de comando como parte do [ferramentas de desenvolvimento](https://aka.ms/xboxliveuwptools) zip.
