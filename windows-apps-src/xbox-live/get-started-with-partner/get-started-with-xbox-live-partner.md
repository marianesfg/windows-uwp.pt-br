---
title: Introdução ao Xbox Live como um parceiro
description: Fornece links para ajudar um parceiro gerenciado ou ID@Xbox membro Introdução ao desenvolvimento do Xbox Live.
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, parceiro, ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659821"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>Introdução ao Xbox Live como um parceiro gerenciado ou um ID@Xbox developer

Esta seção aborda o guia de Introdução com o Xbox Live como um parceiro gerenciado ou como um ID@Xbox developer. Parceiros e desenvolvedores de ID podem acessar os recursos do intervalo completo do Xbox Live, incluindo conquistas, com vários participantes e muito mais.

Gerenciado parceiros e ID@Xbox os desenvolvedores podem desenvolver Xbox Live títulos para o Windows UWP (plataforma Universal) ou a plataforma do Kit de desenvolvedor do Xbox (XDK).

Além do conteúdo disponível aqui, também há documentação adicional está disponível para parceiros com uma conta no Partner Center autorizado. Você pode acessar esse conteúdo aqui: [Conteúdo de parceiro do Xbox Live](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content).

## <a name="why-should-you-use-xbox-live"></a>Por que você deve usar o Xbox Live?

Xbox Live oferece uma matriz de recursos projetados para ajudar a promover seu jogo e manter os jogadores envolvidos:

- Plataforma social Xbox Live permite que os jogadores se conectar com amigos e falar sobre o jogo.
- Conquistas do Xbox Live ajuda seu jogo popular, fornecendo seu jogo promoção gratuita para o grafo social Xbox Live, quando os jogadores desbloquear conquistas.
- Xbox Live placares de líderes ajuda a estimular a participação do seu jogo, permitindo que os jogadores concorrer a superar seus amigos e mover para cima as classificações.
- Xbox Live multiplayer permite jogadores brincar com seus amigos ou get correspondida com outros jogadores competir ou colaborar em seu jogo.
- Xbox Live conectado ofertas de armazenamento livre Salvar jogo roaming entre dispositivos para que os jogadores facilmente podem continuar progresso jogo entre o PC do Windows e o Xbox One.

## <a name="1-choose-a-platform"></a>1. Escolha uma plataforma
Decida entre tornar um Kit de desenvolvimento do Xbox (XDK), plataforma Universal do Windows (UWP) ou entre-play jogo:

- XDK com base em jogos executados somente no console do Xbox One
- Jogos UWP podem direcionar qualquer plataforma do Windows como o PC do Windows, Windows Phone ou Xbox One
  - Para o Xbox One, consulte [UWP no Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) e, especificamente [recursos do sistema para aplicativos UWP e jogos do Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Entre jogue normalmente é jogos Xbox One e PC do Windows usando o XDK e a UWP caminhos de destino.

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Certifique-se de que você tenha um título criado no Partner Center ou XDP
Cada título Xbox Live deve ser definido no Partner Center ou o Portal de desenvolvedor do Xbox (XDP) antes de poder entrar e fazer chamadas de serviço do Xbox Live.  [Criando um novo título](create-a-new-title.md) mostrará a você como fazer isso.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Siga o guia apropriado para seu IDE ou mecanismo de jogo de instalação
Você pode siga o guia de Introdução apropriado para sua plataforma e o mecanismo e conheça os fundamentos do Xbox Live que você avança.

* [Guia de Introdução usando o Visual Studio para jogos da UWP](get-started-with-visual-studio-and-uwp.md) mostrará como vincular o projeto do Visual Studio com a configuração do Xbox Live no Partner Center.
* [Introdução ao uso de Unity para jogos da UWP](partner-add-xbox-live-to-unity-uwp.md) mostrará a você como criar um novo serviço Xbox Live habilitado título do Unity, adicionar recursos como placares de líderes ao seu título e gerar um projeto nativo do Visual Studio.
* [Jogos baseados no guia de Introdução usando o Visual Studio para o XDK](xdk-developers.md) mostrará a você como obter sua configuração de projeto do Visual Studio se você estiver fazendo um Xbox One título usando o XDK.
* [Ao obter Introdução fazendo um jogo entre-play](get-started-with-cross-play-games.md) explica como fazer um produto que é um jogo para Xbox One e com base em um UWP para XDK com base em jogos para PC do Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Conceitos de Xbox Live
Quando você tiver um título de criado, você deve aprender sobre os conceitos do Xbox Live que afetam sua experiência de desenvolvimento de títulos:

- [Áreas restritas do Xbox Live](../xbox-live-sandboxes.md)
- [Contas de teste do Xbox Live](../xbox-live-test-accounts.md)
- [Introdução às APIs do Xbox Live](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Adicionar recursos ao vivo do Xbox

Adicione recursos do Xbox Live para seu jogo:

- [Plataforma Social - perfil, amigos, a presença do Xbox Live](../social-platform/social-platform.md)
- [Placar de líderes de plataforma – estatísticas, de dados, conquistas do Xbox Live](../data-platform/data-platform.md)
- [Xbox Live para múltiplos jogadores plataforma - torneios convidar, para que haja correspondência,](../multiplayer/multiplayer-intro.md)
- [Armazenamento de título da plataforma - armazenamento conectado, de armazenamento ao vivo Xbox](../storage-platform/storage-platform.md)
- [Pesquisa contextual](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. O título da versão

ID@Xbox e parceiros gerenciados devem passar por um processo de certificação antes de liberar seus títulos.  Isso ocorre porque os títulos têm acesso a recursos adicionais como online com vários participantes, conquistas e recursos avançados de presença.  Quando estiver pronto para lançar seu título, entre em contato com seu representante da Microsoft para obter mais detalhes sobre o processo de envio e de versão.
