---
title: Plataforma para múltiplos jogadores do Xbox Live
description: Saiba mais sobre as plataformas com vários participantes que têm o suporte ao Xbox Live.
ms.assetid: 958b94b3-dccd-479a-bf52-54f7ff1656fa
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, com vários participantes
ms.localizationpriority: medium
ms.openlocfilehash: c71fc28a9bb8668d0253834c1a2c9c6ca7736fd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651151"
---
# <a name="xbox-live-multiplayer-platform"></a>Plataforma para múltiplos jogadores do Xbox Live

A plataforma de vários jogadores do Xbox Live permite que seu jogo para reunir todos os jogadores do Xbox Live na Internet e drasticamente pode estender a vida útil e o uso de um título além do play solo típico.

Criando uma ótima experiência com vários participantes para seu público-alvo, você pode aproveitar a rede social grande de jogadores Xbox Live para aumentar a base para o seu jogo de usuários e promover uma comunidade prolongada de fãs dedicados reproduzindo juntos.


## <a name="what-is-the-xbox-live-multiplayer-platform"></a>O que é a plataforma de vários jogadores do Xbox Live?

A plataforma de vários jogadores do Xbox Live é um conjunto de APIs que você pode usar para implementar o jogo para múltiplos jogadores em tempo real do cliente. Os subsistemas principais do conjunto de API são:

-   O **Xbox Live Multiplayer sessão diretório (MPSD)** service. O serviço MPSD funciona com experiências de interface do usuário integradas para facilitar a usuários localizar e convidando uns aos outros para reprodução. Integração com serviços do Xbox Live também permite que os clientes usem o Xbox Live terceiros bate-papo para montar.
-   **Instalações para que haja correspondência simples e avançada.** Xbox Live fornece recursos de quickmatch tradicionais, mas também a sessão procurar e suporte para cenários altamente personalizado para que haja correspondência. Xbox Live procurando para grupo (LFG) também permite que os jogadores localizar uns aos outros, rally no bate-papo de terceiros e, em seguida, seu jogo.
-   **Ponto a ponto a ponto e de cliente-servidor as APIs de rede** fornecem comunicação segura em tempo real, aproveitando os modernos padrões da Internet e ativamente monitorado pelo Xbox Live. A padronização e integração com a rede do Xbox Live experiências de solução de problemas permitem aos usuários corrigir rapidamente problemas de conectividade.  
-   **Soluções de bate-papo integrado de voz e texto** que facilitam a comunicação de jogo segura aproveitando o grafo social Xbox Live, os serviços de mídia e hardware especializado de codificação em dispositivos Xbox One.

Para obter uma visão geral de alguns dos cenários mais comuns com vários participantes e qual funcionalidade do Xbox Live pode ajudar a implementar esses cenários, consulte [níveis para múltiplos jogadores](multiplayer-scenarios.md).

## <a name="how-can-i-implement-xbox-live-multiplayer-in-my-game"></a>Como posso implementar o Xbox Live Multiplayer no meu jogo?
Dependendo do cenário, a plataforma de vários jogadores do Xbox Live fornece várias abordagens para implementar com vários participantes Xbox Live em seu jogo.

### <a name="xbox-integrated-multiplayer-xim"></a>XIM (Xbox Integrated Multiplayer)
XIM é uma solução completa para adição de rede para múltiplos jogadores em tempo real e comunicação para seu jogo com a potência de serviços do Xbox Live. O objetivo de XIM é tornar mais fácil do que nunca criar jogos com vários participantes da alta qualidade ponto a ponto (P2P) no Xbox One e Windows 10.

XIM fornece a seguinte funcionalidade:
- Suporte para convites de jogos e emparelhamento simple.
- Uma rede de em tempo real de simple e segura que transparente é ampliada pelo serviço de retransmissão do Xbox Live com vários participantes.
- Simples de voz e texto bate-papo, com recursos para transcrever e narração de comunicação para uma experiência do usuário final mais acessível.
- Auxílios para detectar e gerenciar o congestionamento da rede, bem como para migrar estado do jogo.

XIM é a solução mais simples para múltiplos jogadores disponível por meio da plataforma para múltiplos jogadores Xbox Live, mas também a menos personalizável e só é adequado para jogos de P2P. Para obter mais informações sobre XIM, consulte [Multiplayer integrado do Xbox](xbox-integrated-multiplayer.md).

### <a name="xbox-multiplayer-manager"></a>Gerenciador do Xbox com vários participantes
Xbox com vários participantes Manager (MPM) é um API que fornece acesso flexível ao diretório de sessão para múltiplos jogadores do Xbox Live, convite e serviços de compatibilidade do cliente.

Ele implementa vários cenários comuns de vários jogadores de maneira eficiente que segue as práticas recomendadas e também lida com muitos dos requisitos Xbox (XRs) que seu jogo deve implementar para passar na certificação.

O Gerenciador do Xbox com vários participantes não implementa um sistema de rede ou uma camada de bate-papo. MPM foi projetado como uma flexível, mas simplificada e consolidada para múltiplos jogadores API de gerenciamento para o seu jogo emparelhado com uma camada de rede segura implementada por meio de Windows.Networking.XboxLive. No jogo comunicação pode ser adicionada com o [jogo 2 bate-papo](chat/game-chat-2-overview.md) API ou por meio de reservas de bate-papo XIM. As APIs de bate-papo 2 jogos e de redes estão documentadas no XDK de um Xbox e Xbox Live SDK da plataforma extensões.

Se você estiver hospedando servidores dedicados para o seu jogo com vários participantes, MPM é a melhor opção. MPM também é bem adequado para cenários avançados, como a integração com o Xbox Live torneios. Para obter mais informações sobre MPM, consulte [Introdução ao Gerenciador de vários jogadores](multiplayer-manager/multiplayer-manager-api-overview.md).

Para usar o Gerenciador de vários jogadores, você deve configurar o serviço Xbox Live para seus cenários com vários participantes. Para obter mais informações sobre essa configuração, consulte [configurar o serviço com vários participantes](service-configuration/configure-the-multiplayer-service.md).

>Atualmente, o Multiplayer Manager não dá suporte para a navegação de sessão. Para obter informações, consulte [navegue de sessão com vários participantes](session-browse.md).

### <a name="api-capabilites"></a>Recursos de API

Funcionalidade | Xbox integrado com vários participantes| Gerenciador de vários jogadores
--  | -- | --
Visibilidade |  XIM é fornecido como uma biblioteca compilada sem fonte.  | MPM é fornecido com o código-fonte, portanto, você pode personalizar o comportamento interagindo diretamente com os serviços Xbox Live ou com XSAPI.
Sessão e compatibilidade | XIM fornece regras simples de cruzamento de pré-compilação configurado e não requer configuração para vários participantes. | MPM [requer a configuração do serviço de vários jogadores](service-configuration/configure-the-multiplayer-service.md), que permite a especificação sofisticada do comportamento de emparelhamento e a sessão.
Rede | XIM fornece uma rede de player para player simple e segura, com suporte pelo serviço Xbox Live retransmissão quando necessário. | MPM foi projetada para que você pode conectar sua própria solução de rede segura usando Windows.Networking.XboxLive.
Jogo bate-papo | XIM fornece integrado bate-papo de voz e texto. | Comunicação do jogo pode ser implementada com a API de 2 de bate-papo jogos ou usando XIM reservas de out-of-band para habilitar o bate-papo para um MPM gerenciados lista.
