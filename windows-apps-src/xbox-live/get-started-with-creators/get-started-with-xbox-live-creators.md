---
title: "Introdução ao Programa de Criadores do Xbox Live"
author: KevinAsgari
description: "Fornece links para ajudá-lo a começar a usar o Programa de Criadores do Xbox Live."
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.author: kevinasg
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: high
ms.openlocfilehash: ba7f974ec6157e8f1d94232a64099fcae766eba8
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Introdução ao Programa de Criadores do Xbox Live
 
O Programa de Criadores do Xbox Live permite que você publique seus jogos de forma rápida e direta no Xbox One e no Windows 10, com um processo de certificação simplificado e sem a necessidade de aprovação de conceito. Se seu jogo se integra ao Xbox Live e segue nossas [políticas padrão da Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx), você está pronto para publicar. Este artigo descreverá as etapas necessárias para lançar seu jogo e executá-lo com a integração com o Xbox Live. 

Os jogos do Programa de Criadores do Xbox Live devem ser um aplicativo UWP (Plataforma Universal do Windows). Para Xbox One, consulte [UWP no Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) e especificamente [Recursos do sistema para aplicativos UWP e jogos no Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation). Jogos publicados por meio do Programa de Criadores do Xbox Live não têm acesso às conquistas ou aos serviços multijogador online. Para obter uma lista completa de serviços compatíveis, consulte [Visão geral do programa de desenvolvedores: tabela de recursos](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table).

## <a name="1-ensure-you-have-a-title-created-on-dev-center"></a>1. Verificar se você tem um título criado no Centro de Desenvolvimento
Todo título do Xbox Live deve ser definido no Centro de Desenvolvimento antes que você possa entrar e realizar chamadas de serviço do Xbox Live.  [Criar um título Criadores](create-and-test-a-new-creators-title.md) mostrará como fazê-lo.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. Seguir o guia adequado para definir seu IDE ou mecanismo de jogo
Você pode seguir o "guia de introdução" adequado para sua plataforma e mecanismo e aprender os conceitos básicos do Xbox Live enquanto você continua:

* [Desenvolver um título Criadores com o Visual Studio](develop-creators-title-with-visual-studio.md) mostrará como vincular seu projeto do Visual Studio com a sua configuração do Xbox Live no Centro de Desenvolvimento.
* [Desenvolver um título Criadores com o Unity](develop-creators-title-with-unity.md) mostrará como criar um novo jogo do Unity habilitado para Xbox Live, usar credenciais de um único usuário e de vários usuários, adicionar recursos, como placares de líderes e estatísticas, e gerar um projeto nativo do Visual Studio.

Embora o Unity seja o único mecanismo de jogo de terceiro para o qual fornecemos documentação, os mecanismos de jogo [Construct (2 & 3)](https://www.scirra.com/construct2) e [Game Maker Studio](https://www.yoyogames.com/gamemaker) também apresentam documentação para ajudá-lo a integrar Xbox Live no seu jogo do Construct ou Game Maker Studio respectivamente.

* [Game Maker Studio 2 UWP agora é compatível com o Programa de Criadores do Xbox Live](https://www.yoyogames.com/gamemaker/xblc) mostrará como exportar projetos do Game Maker Studio para que eles sejam executados no Xbox One e em um computador com Windows 10.
* [Usando o Xbox Live em aplicativos UWP - Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps) mostrará como usar o Xbox Live em seus jogos do Construct 2 e 3.

Para outros mecanismos de desenvolvimento de jogos sem integração documentada com o Xbox Live, você ainda poderá usar as APIs do Xbox Live para adicionar o Xbox Live a seu título. Para usar a API do Xbox Live de seu projeto, você pode adicionar referências aos binários com pacotes NuGet ou adicionar a fonte do API. A adição de pacotes NuGet torna a compilação mais rápida enquanto a adição da fonte torna a depuração mais rápida.

Para suporte por meio dos Serviços Xbox Live com mecanismos de jogo de terceiros que não sejam do Unity, entra em contato com uma equipe de mecanismo de jogo adequada que possa responder a suas perguntas.

## <a name="3-xbox-live-concepts--testing"></a>3. Conceitos e teste do Xbox Live
Após a criação de um título, você deve aprender sobre os conceitos do Xbox Live que afetarão sua experiência desenvolvendo títulos. É importante testar seu jogo em todas as plataformas compatíveis com ele para garantir que se comportará como o esperado.

- [Configuração do serviço do Xbox Live para o Programa de Criadores](xbox-live-service-configuration-creators.md)
- [Ambiente de teste do Xbox Live](../xbox-live-sandboxes.md)
- [Autorizar contas do Xbox Live](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. Habilitar credenciais do Xbox Live
Todos os jogos do Programa de Criadores do Xbox Live devem integrar credenciais do Xbox Live e exibir a identidade do usuário (gamertag, imagem do jogador etc.). Você pode optar por conectar automaticamente o usuário ou fazer com que ele aperte um botão para iniciar a conexão. A gamertag deve ser exibida após a conexão para que o jogador possa confirmar que está usando o perfil correto.

- [Plataforma social do Xbox Live - perfil, amigos, presença](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. Adicionar recursos opcionais do Xbox Live

O Programa de Criadores do Xbox Live oferece uma gama de recursos criados para ajudar a promover seu jogo e manter os jogadores envolvidos:

- [Plataforma de dados do Xbox Live - estatísticas, placares de líderes](../data-platform/data-platform.md) ajuda a aumentar o interesse dos jogares por seu jogo permitindo que eles compitam com amigos e alcancem classificações cada vez maiores.
- [Plataforma de armazenamento do Xbox Live - Armazenamento Conectado, Armazenamento de Títulos](../storage-platform/storage-platform.md) oferece um roaming de jogo seguro entre dispositivos. Com isso, os jogadores podem continuar um progresso de jogo facilmente entre o Xbox One e o computador Windows.
- [Plataforma social do Xbox Live - perfil, amigos, presença](../social-platform/social-platform.md) permite que jogadores se conectem com amigos e conversem sobre o jogo.

É importante observar que o Programa de Criadores do Xbox Live não é compatível com multijogador online, conquistas ou pontuação.

## <a name="6-release-your-game"></a>6. Lançar seu jogo

Se você está usando o Programa de Criadores do Xbox Live, o processo não é diferente de lançar qualquer outro aplicativo UWP:

- [Políticas da Microsoft Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx) incluindo [Jogos e políticas do Xbox](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [Publicar aplicativos do Windows](https://developer.microsoft.com/en-us/store/publish-apps)