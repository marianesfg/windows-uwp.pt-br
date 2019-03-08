---
title: Serviço de atividade em tempo real
description: Saiba mais sobre o serviço de atividade do Xbox Live em tempo real.
ms.assetid: 50de262f-fc55-4301-83b5-0a8a30bc7852
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, serviço de atividade em tempo real.
ms.localizationpriority: medium
ms.openlocfilehash: 36389fac3bd6dea2d2e24c0935087781118d8046
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592081"
---
# <a name="real-time-activity-service"></a>Serviço de atividade em tempo real

O serviço de atividade em tempo real (RTA) permite que um aplicativo em qualquer dispositivo para assinar dados de estado, as estatísticas de usuário e presença. O sistema permite que as assinaturas para dados própria e outros dados em qualquer título com base em suas configurações de privacidade. Isso permite que um fluxo de informações sem precisar sondar constantemente para obter os dados mais recentes.


## <a name="developer-scenarios"></a>Cenários de desenvolvedor

Há muitos cenários que dá suporte a RTA. Algumas delas são listadas aqui, mas o poder real dos RTA é muitos cenários em que você virá com o que estamos ainda não tenha imaginado ainda. Você pode ajudar a definir a próxima geração do jogo em que os usuários geralmente têm em mãos seu Microsoft Surface ou Apple iPad enquanto eles está interagindo com o título do console. RTA usa a tecnologia de WebSocket, portanto, passo a passo subtópico vários inclui uma visão geral da implementação usando a API de WebSockets fornecida pelo Windows.

A seguir está alguns cenários simples que você pode criar para seu título usando RTA:

-   Aplicativo de progresso conquistas
-   Aplicativo de jogo de ajuda
-   Aplicativo do Visualizador de Esquadrão
-   Visualizador de estatísticas
-   Visualizador de presença


## <a name="achievements-progress-app"></a>Aplicativo de progresso conquistas

Os usuários quase sempre estiver curiosos sobre seu progresso em direção a determinados conquistas, especialmente as realizações que exigem a executar uma ação de um determinado número de vezes. Com acesso em tempo real às estatísticas do usuário (que são agregados no serviço de estatísticas do Xbox Live Player), você pode apresentar o progresso em tempo real para os jogadores e seus amigos em direção conquistas e etapas do projeto, no Xbox One ou em um dispositivo complementar, enquanto o usuário o título está em execução.


## <a name="game-help-app"></a>Aplicativo de jogo de ajuda

Como os usuários navegam no seu título, o acesso em tempo real aos dados permite que você presente jogo ajuda ao lado de experiência no Xbox One ou em qualquer dispositivo complementar. Usuários podem acusar um novo mapa, novo roteiro ou luta desafiador do chefe e seu jogo ajuda complementar poderia exibir vídeos gerados pelo usuário ou gerados pelo desenvolvedor e o texto para ajudar o usuário por meio da experiência do título.


## <a name="squad-viewer-app"></a>Aplicativo do Visualizador de Esquadrão

Em jogos para múltiplos jogadores de cooperação, um jogador e seus colegas de suas respectiva trabalham juntos para uma meta compartilhada. Com tantos jogadores, pode ser difícil de controlar o panorama geral. Com acesso a dados em tempo real, você pode criar um aplicativo complementar que mostra os mapas de calor e de mapa de alto nível de onde a ação pode ser.


## <a name="statistics-viewer"></a>Visualizador de estatísticas

Embora seja comum para pensar em aplicativos complementares ao pensar sobre RTA, você também pode usar RTA no título do núcleo. Por exemplo, você pode usar RTA para fornecer um player de um jogo com vários participantes com uma exibição das estatísticas atuais de todas as pessoas no jogo, talvez, o player simplesmente pressionando o botão de modo de exibição no controlador enquanto a correspondência com vários participantes.


## <a name="presence-viewer"></a>Visualizador de presença

Quando estiver em um corredor, é útil ter uma exibição atualizada dos amigos que estão online e se que estejam executando o mesmo título. Você pode se inscrever para a presença de amigos do usuário e mostrar amigos que se tornam online, se começar a reproduzir seu título, tudo em tempo real.


## <a name="subscription-privacy-and-authorization"></a>Autorização e a privacidade de assinatura

A versão mais recente do RTA inclui verificações para privacidade e o isolamento de autorização/conteúdo. Desde que as verificações de autorização e de privacidade são atendidas, seu aplicativo pode assinar qualquer estatística que é marcada como habilitado RTA. (Para obter mais informações sobre como tornar uma estatística RTA habilitado, consulte [se registrar para notificações stat](register-for-stat-notifications.md). Há nenhuma limitação quanto aos tipos de estatísticas que podem ser feitas habilitado RTA — cabe a você, o desenvolvedor. No entanto, há um limite para o *número* das estatísticas que pode se inscrever para um usuário por sessão do aplicativo. Se um usuário atingir esse limite, ele ou ela receberá um erro na próxima assinatura.


## <a name="in-this-section"></a>Nesta seção

[Registrar para notificações de alteração stat de player](register-for-stat-notifications.md)  
Descreve como habilitar a atividade em tempo real (RTA) para obter informações de estado ou de estatísticas.

[O serviço de atividade em tempo real (rta) usando as apis do winrt de programação](programming-the-real-time-activity-service.md)  
Descreve como programar o serviço de atividade em tempo real (RTA) usando APIs do WinRT.

[O serviço de atividade em tempo real (rta) usando a interface restful de programação](programming-the-real-time-activity-service.md)  
Descreve como programar o serviço de atividade em tempo real (RTA) usando a interface RESTful.

[Práticas recomendadas de atividade em tempo real (rta)](rta-best-practices.md)  
Práticas recomendadas para usar o serviço de atividade em tempo Real (RTA) de serviços do Xbox para inscrever-se aos dados de estatísticas e o estado da plataforma de dados do Xbox.
