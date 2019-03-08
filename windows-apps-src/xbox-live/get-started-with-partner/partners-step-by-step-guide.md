---
title: Guia passo a passo para parceiros do Xbox Live
description: Fornece uma orientação de etapas para integrar o Xbox Live para parceiros gerenciados.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660841"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>Guia passo a passo para integrar o Xbox Live para parceiros gerenciados e ID@Xbox membros

Nesta seção ajudarão você a entrar em funcionamento com o Xbox Live:

## <a name="1-choose-a-platform"></a>1. Escolha uma plataforma
Decida entre fazer um Kit de desenvolvimento do Xbox (XDK), plataforma Universal do Windows (UWP) ou jogo entre-play.

- XDK com base em jogos executados somente no console do Xbox One
- Jogos UWP podem direcionar qualquer plataforma do Windows como o PC do Windows, Windows Phone ou Xbox One
  - Para o Xbox One, consulte [UWP no Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) e, especificamente [recursos do sistema para aplicativos UWP e jogos do Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Entre jogue normalmente é jogos Xbox One e PC do Windows usando o XDK e a UWP caminhos de destino.

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Verifique se que você tem um título criado no Partner Center ou XDP
Cada título Xbox Live deve ser definido no Partner Center ou o Portal de desenvolvedor do Xbox (XDP) antes de poder entrar e fazer chamadas de serviço do Xbox Live.  [Criando um novo título](create-a-new-title.md) mostrará a você como fazer isso.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Siga o guia apropriado para configurar seu IDE ou mecanismo de jogos
Você pode siga o guia de Introdução apropriado para sua plataforma e o mecanismo e conheça os fundamentos do Xbox Live que você avança.

* [Guia de Introdução usando o Visual Studio para jogos da UWP](get-started-with-visual-studio-and-uwp.md) mostrará como vincular o projeto do Visual Studio com a configuração do Xbox Live no Partner Center.

* [Introdução ao uso de Unity para jogos da UWP](partner-add-xbox-live-to-unity-uwp.md) mostrará a você como criar um novo serviço Xbox Live habilitado título do Unity, adicionar recursos como placares de líderes ao seu título e gerar um projeto nativo do Visual Studio.

* [Jogos baseados no guia de Introdução usando o Visual Studio para o XDK](xdk-developers.md) mostrará a você como obter sua configuração de projeto do Visual Studio se você estiver fazendo um Xbox One título usando o XDK.

* [Ao obter Introdução fazendo um jogo entre-play](get-started-with-cross-play-games.md) explica como fazer um produto que é um jogo para Xbox One e com base em um UWP para XDK com base em jogos para PC do Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Conceitos de Xbox Live
Após a criação de um título, você deve aprender sobre os conceitos do Xbox Live que afetarão sua experiência desenvolvendo títulos.

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

ID@Xbox títulos e títulos lançados pelo editor de jogo é um Microsoft Partner devem passar por um processo de certificação antes do lançamento.  Isso ocorre porque esses títulos têm acesso a recursos adicionais, como com vários participantes, conquistas e presença avançada.  Quando estiver pronto para lançar seu título, entre em contato com seu representante da Microsoft para obter mais detalhes sobre o processo de envio e de versão.
