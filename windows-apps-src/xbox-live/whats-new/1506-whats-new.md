---
title: O que há de novo para o Xbox Live SDK – junho de 2015
description: O que há de novo para o Xbox Live SDK – junho de 2015
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a42d0fb0a3cb457a60a0542bfc5966893d00f18b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627871"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>O que há de novo para o Xbox Live SDK – junho de 2015

A versão de junho do Xbox Live SDK inclui as seguintes atualizações:

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte ##
O Xbox Live SDK agora dá suporte a compilação mais recente do Insider do Windows 10 e o Visual Studio 2015 RC.

## <a name="title-callable-ui-apis"></a>APIs de interface do usuário que pode ser chamado de título

| Observação |
|------|
| Esta seção se aplica somente aos títulos UWP, títulos XDK devem se referir à classe Windows.Xbox.UI.SystemUI TCUI  |

O Xbox Live SDK agora contém o wrapper de APIs que dão suporte à interface do usuário que pode ser chamado do título (TCUI) para a exibição de estoque da interface do usuário em um dispositivo Windows 10 PC/móvel.

Essas APIs só estão disponíveis para aplicativos UWP.

Você pode chamar os wrappers TCUI do TitleCallableUI (WinRT) e classes title_callable_ui (C++).

O estoque de interfaces do usuário incluem:
* Um seletor de player da interface do usuário
* Um seletor de jogo de convite da interface do usuário
* Um cartão de perfil do player da interface do usuário
* Um amigo de adicionar ou remover da interface do usuário
* Um realizações de título do jogo mostrar interface do usuário

Ver o novo *TCUI* exemplo, por exemplo, uso das novas APIs. Você pode encontrar o exemplo em {*raiz do código-fonte do SDK*} \Samples\TCUI.

## <a name="new-authentication-model-for-uwp-apps"></a>Novo modelo de autenticação para aplicativos UWP
O Xbox Live SDK agora dá suporte ao uso de uma conta da Microsoft (MSA) para identificar o Xbox Live player em um dispositivo Windows 10 PC/móvel.

Agora você pode entrar em um usuário usando as classes de xbox_live_user (C++) e XboxLiveUser (WinRT).

## <a name="new-api-for-writing-events-in-uwp-apps"></a>Nova API para gravar eventos em aplicativos UWP

| Observação |
|------|
| Esta seção se aplica somente aos títulos UWP.  Os desenvolvedores do XDK devem fazer referência a [eventos de jogo](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events) artigo para obter informações específicas do XDK  |

As classes de events_service (C++) e o novo EventsService (WinRT) permitem que você gravar eventos de jogos que podem atualizar estatísticas do usuário, conquistas, placares de líderes, etc. Essas novas classes são apenas para aplicativos UWP.

## <a name="breaking-change-to-event-handlers"></a>Alteração significativa para manipuladores de eventos ##
Todos os manipuladores de eventos no SDK do C++ foram alterados de uma única `void set_*_handler()` método a um par de `function_context add_*_handler()` e `void remove_*_handler(function_context context)` métodos.

Cada `add_*_handler()` método retorna um `function_context` objeto. Quando você remove o manipulador de eventos, você deve passar o `function_context` objeto.

Por exemplo:
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>Novas APIs para gerenciar os cenários com vários participantes
O Xbox Live SDK agora inclui um conjunto de APIs de C++ para o gerenciamento de cenários comuns de vários participantes. As APIs são incluídas no namespace xbox.services.experimental.multiplayer.

Essas APIs estão no namespace experimental que significa que eles provavelmente mudarão com base nos comentários.  Incentivamos os desenvolvedores para usá-las e fornecer comentários.

Ver o novo *MultiplayerManager* por exemplo, exemplo de uso dessas novas APIs. Você pode encontrar o exemplo em {*raiz do código-fonte do SDK*} \Samples\MultiplayerManager.
