---
title: Perguntas frequentes
description: Perguntas frequentes sobre a UWP no Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 036c3c1832bbb3e27a93671f399a9a97c7afaba3
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840676"
---
# <a name="frequently-asked-questions"></a>Perguntas frequentes

As coisas não estão funcionando da forma esperada? Procure nesta página de perguntas frequentes. Confira também o tópico [Problemas conhecidos](known-issues.md) e o fórum [Desenvolvendo aplicativos Universal do Windows](https://go.microsoft.com/fwlink/?linkid=839446). 

### <a name="why-arent-my-games-and-apps-working"></a>Por que meus jogos e aplicativos não estão funcionando?

Se seus jogos e aplicativos não estiverem funcionando ou se você não tiver acesso à loja ou ao Live Services, você provavelmente está executando em modo de desenvolvedor. Para descobrir qual modo você está no momento, pressione o botão **Home** no controlador. Se isso levar você ao Dev Home em vez da experiência Retail Home, você está no Modo Desenvolvedor. Se você quiser jogar jogos, poderá abrir a Dev Home e alternar para o modo de varejo usando o botão **Sair do modo de desenvolvedor**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Por que não consigo me conectar ao Xbox One usando o Visual Studio?

Comece verificando se você está executando no modo de desenvolvedor e não no modo de varejo. Você não pode se conectar ao seu Xbox One quando estiver no modo de varejo. Para descobrir qual modo você está no momento, pressione o botão **Home** no controlador. Se você ver o conteúdo Gold/Live em vez de Dev Home, você está no modo de varejo e você precisa executar o aplicativo de ativação do modo Dev para mudar para o modo de desenvolvedor.

> [!NOTE]
> Você deve ter um usuário conectado para implantar um aplicativo.

Para saber mais, veja [Corrigindo falhas na implantação](#fixing-deployment-failures) posteriormente nesta página.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Como alternar entre o modo de varejo e modo de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Como saber se estou no modo de varejo ou de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados. 

Para descobrir qual modo você está no momento, pressione o botão **Home** no controlador. 
- Se isso levar você à Dev Home, você está no modo de desenvolvedor.
- Se você vir conteúdo Gold/Live, você está no modo de varejo.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Meus aplicativos e jogos ainda funcionarão se ativar o modo de desenvolvedor?

Sim, você pode alternar do modo de desenvolvedor para o modo de varejo, onde você pode jogar seus jogos. Para saber mais, veja a página [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Posso desenvolver e publicar aplicativos x86 para Xbox?
Xbox não dá mais suporte ao desenvolvimento de aplicativos x86 ou envios de aplicativos x86 para a loja. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Irei perder meus aplicativos e jogos ou alterações salvas?

Se você decidir sair do programa de desenvolvedor, não perderá seus aplicativos e jogos instalados. Além disso, se você estava online quando jogou seus jogos, todos os jogos salvos estarão salvos em seu perfil na nuvem da conta do Live, portanto não os perderá.

### <a name="how-do-i-leave-the-developer-program"></a>Como posso sair do programa de desenvolvedor?

Veja o tópico [Desativação do modo de desenvolvedor do Xbox One](devkit-deactivation.md) para obter detalhes sobre como sair do programa de desenvolvedor.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Eu vendi meu Xbox One e o deixei no modo de desenvolvedor. Como posso desativar o modo de desenvolvedor?

Se você não tiver acesso ao Xbox One, você pode desativá-lo no Windows Partner Center. Para obter detalhes, consulte a seção de **desativar seu console usando o Partner Center** no tópico [Xbox One desativação do modo de desenvolvedor](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Saí do programa de desenvolvedor usando o Partner Center, mas estou no modo de desenvolvedor ainda. O que devo fazer?

Inicie o Dev Home e selecione o botão **Sair do modo de desenvolvedor**. Isso reiniciará o seu console no modo de varejo. 

### <a name="can-i-publish-my-app"></a>Posso publicar meu aplicativo?

Você pode [publicar aplicativos](../publish/index.md) por meio do Partner Center se você tiver uma [conta de desenvolvedor](https://developer.microsoft.com/store/register). Os aplicativos UWP criados e testados em um console do Xbox One de varejo irão passar pelo mesmo processo de ingestão, revisão e publicação que o Windows realiza atualmente, com análises adicionais para atender aos padrões atuais do Xbox One.

### <a name="can-i-publish-my-game"></a>Posso publicar meu jogo?

Você pode usar a UWP e o Xbox One no modo de desenvolvedor para compilar e testar seus jogos no Xbox One. Para publicar os jogos UWP, você deve se registrar no [ID@XBOX](http://www.xbox.com/Developers/id) ou fazer parte do [Xbox Live Creators Program](https://developer.microsoft.com/games/xbox/xboxlive/creator). Para saber mais, veja a [Visão geral do Programa para Desenvolvedores](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Os mecanismos de jogo padrão funcionarão?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Quais funcionalidades e recursos do sistema estão disponíveis para jogos UWP no Xbox One? 

Para saber mais, veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Se eu criar um jogo UWP no DirectX 12, ele executará no meu Xbox One em modo de desenvolvedor?

Para saber mais, veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>Toda a superfície de API da UWP estará disponível no Xbox?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão.

### <a name="fixing-deployment-failures"></a>Corrigindo falhas de implantação

Se você não puder implantar seu aplicativo no Visual Studio, estas etapas poderão ajudá-lo a resolver o problema. Se você ficar paralisado, peça ajuda no fórum.

> [!NOTE]
> Você deve ter um usuário conectado para implantar um aplicativo. Se você receber uma mensagem de erro 0x87e10008, verifique se tem um usuário conectado e tente novamente.

Se o Visual Studio não puder se conectar ao Xbox One:

1. Verifique se você está no modo de desenvolvedor (discutido anteriormente nesta página).
2. Verifique se você configurou seu computador de desenvolvimento corretamente. Você seguiu *todas* as direções em [Introdução ao desenvolvimento de aplicativos UWP no Xbox One](getting-started.md)? 

3. Se ainda não tiver lido, leia o tópico [Configuração do ambiente de desenvolvimento](development-environment-setup.md) e o tópico [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

4. Verifique se você pode executar "ping" no endereço IP do console em seu computador de desenvolvimento.
  > [!NOTE]
  > Para obter o melhor desempenho de implantação, recomendamos que você use uma conexão com fio com seu console.

5. Verifique se você está usando o Universal (protocolo não criptografado) na lista suspensa Autenticação na guia **Depurar**. Para obter mais detalhes, consulte [Configuração do ambiente de desenvolvimento](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Se eu estiver criando um aplicativo usando HTML/JavaScript, como habilito a navegação Gamepad?

TVHelpers é um conjunto de amostras e bibliotecas de JavaScript e XAML/C# para ajudar você a criar excelentes experiências de TV e do Xbox One em JavaScript e C#. TVJS é uma biblioteca que ajuda você a criar aplicativos UWP especiais para o Xbox One. TVJS inclui suporte para navegação de controladores automáticos, reprodução de mídia avançada, pesquisa e muito mais. Você pode usar TVJS com o seu aplicativo Web hospedado de maneira tão fácil quanto com um aplicativo UWP Web empacotado, com total acesso às APIs do Windows Runtime.

Para saber mais, consulte o projeto [TVHelpers](https://github.com/Microsoft/TVHelpers) e o [wiki](https://github.com/Microsoft/TVHelpers/wiki) do projeto.

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos com UWP no Xbox One](known-issues.md)
- [UWP no Xbox One](index.md)
- [UWP no Xbox One](index.md)
