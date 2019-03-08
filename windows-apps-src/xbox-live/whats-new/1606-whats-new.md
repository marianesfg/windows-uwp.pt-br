---
title: O que há de novo para o Xbox Live SDK – junho de 2016
description: O que há de novo para o Xbox Live SDK – junho de 2016
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595281"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>O que há de novo para o Xbox Live SDK – junho de 2016

Consulte a [What's New - abril de 2016](1604-whats-new.md) artigo para o que foi adicionado na versão de abril de 2016.

> **Observação:** Se você estiver usando o Kit de desenvolvedores Xbox (XDK), em seguida, a *What's New - abril de 2016* abordará as novas funcionalidades adicionais que foi adicionada desde a versão de março XDK.

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="contextual-search"></a>Pesquisa Contextual
As novas classes a seguir foram adicionadas para o `contextual_search` API para recuperar os clipes de jogos:

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

Consulte a documentação de referência para obter mais informações.

## <a name="networking"></a>Rede
O Xbox Live SDK agora inclui uma nova declaração que detecta se websockets 5 ou mais são criados por usuário em uma instância única do título.

É possível desabilitar isso assert chamando `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **Observação:** Manager social e com vários participantes Manager usam um único websocket combinado se você usar esses recursos em seu título.

## <a name="tools"></a>Ferramentas
* Agora, o Xbox Live resiliência plug-in para o Fiddler está incluído no SDK do Xbox Live.  
É um complemento do Fiddler que permite aos desenvolvedores bloquear seletivamente as chamadas para o Xbox Live.
Usando esse plug-in, os desenvolvedores possam testar resiliência em interrupções de serviço parcial em seus títulos de jogos.
A ferramenta inclui um número Xbox Live serviço pontos de extremidade internos e diferentes de tipos de erro para o teste.
Há suporte para títulos de todos os Xbox One e UWP.
