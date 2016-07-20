---
author: Mtoepke
title: Perguntas frequentes
description: Perguntas frequentes sobre a UWP no Xbox.
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 01173aeabb74d6d703597a9bd7a48c179c688c07

---

# Perguntas frequentes

As coisas não estão funcionando da forma esperada? Procure nesta página de perguntas frequentes. Confira também o tópico [Problemas conhecidos](known-issues.md) e o fórum [Desenvolvendo aplicativos Universal do Windows](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### Por que meus jogos e aplicativos não estão funcionando?

Se seus jogos e aplicativos não estiverem funcionando ou se você não tiver acesso à loja ou ao Live Services, você provavelmente está executando em modo de desenvolvedor. Você pode saber se está executando no modo de desenvolvedor, se você selecionar Início e vir um grande bloco de Dev Home no lado direito da tela, em vez do conteúdo Gold/Live. Se você quiser jogar jogos, poderá abrir a Dev Home e alternar para o modo de varejo usando o botão **Sair do modo de desenvolvedor**.

### Por que não consigo me conectar ao Xbox One usando o Visual Studio?

Comece verificando se você está executando no modo de desenvolvedor e não no modo de varejo. Você não pode se conectar ao Xbox One quando está no modo de varejo. Você pode verificar isso simplesmente pressionando o botão **Início** e procurando o bloco Dev Home no lado direito da tela. Se o bloco não estiver lá, mas se em vez disso você vir conteúdo Gold/Live, você estará no modo de varejo. Você precisa executar o aplicativo de ativação Desenvolver Mais para alternar para o modo de desenvolvedor.

> 
              **Observação**&nbsp;&nbsp;Você deve ter um usuário conectado para implantar um aplicativo.

Para saber mais, veja [Corrigindo falhas na implantação](frequently-asked-questions.md#fixing-deployment-failures) posteriormente nesta página.

### Como alternar entre o modo de varejo e modo de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados.

### Como saber se estou no modo de varejo ou de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados. 

Você pode verificar isso simplesmente pressionando o botão **Início** e examinando o lado direito da tela. Se você estiver no modo de desenvolvedor, você verá o bloco Dev Home no lado direito. Se você estiver no modo de varejo, você verá o conteúdo Gold/Live habitual.

### Meus aplicativos e jogos ainda funcionarão se ativar o modo de desenvolvedor?

Sim, você pode alternar do modo de desenvolvedor para o modo de varejo, onde você pode jogar seus jogos. Para saber mais, veja a página [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md). 

<!-- > **CAUTION**&nbsp;&nbsp;The Xbox Developer Preview System Update includes experimental and early pre-release software. 
This means that some popular games and apps will not work as expected and you may experience occasional crashes and data loss. -->

### Irei perder meus aplicativos e jogos ou alterações salvas?

Se você decidir sair do programa Developer Preview, talvez você precise fazer uma redefinição de fábrica que apagará todo o conteúdo do seu console. Se isso acontecer, você precisará reinstalar todos os jogos e aplicativos. Se você estava online quando jogou seus jogos, todos os jogos salvos estarão salvos em seu perfil na nuvem da conta do Live, portanto não os perderá.

### Como posso sair do Developer Preview?

Veja o tópico [Desativação do modo de desenvolvedor do Xbox One](devkit-deactivation.md) para obter detalhes sobre como sair do Developer Preview.

### Eu vendi meu Xbox One e o deixei no modo de desenvolvedor. Como posso desativar o modo de desenvolvedor?

Se você não tiver mais acesso ao Xbox One, você poderá desativá-lo no Centro de Desenvolvimento do Windows. Para obter detalhes, veja a seção **Desativar seu console usando o Centro de Desenvolvimento do Windows** no tópico [Desativação do modo de desenvolvedor do Xbox One](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### Saí do Developer Preview usando o Centro de Desenvolvimento do Windows, mas ainda estou no modo de desenvolvedor. O que devo fazer?

Inicie o Dev Home e selecione o botão **Sair do modo de desenvolvedor**. Isso reiniciará o seu console no modo de varejo. 

### Posso publicar meu aplicativo?

A publicação de aplicativos estará disponível por meio do Centro de Desenvolvimento mais tarde neste ano. Os aplicativos UWP criados e testados em um Xbox One de varejo irão passar pelo mesmo processo de ingestão, revisão e publicação que o Windows realiza atualmente, com análises adicionais para atender aos padrões atuais do Xbox One.

### Posso publicar meu jogo?

Você pode usar a UWP e o Xbox One no modo de desenvolvedor para compilar e testar seus jogos no Xbox One. Para publicar jogos UWP você deve se registrar no [ID@XBOX](http://www.xbox.com/Developers/id). 

              O [ID@XBOX](http://www.xbox.com/Developers/id) fornece aos desenvolvedores total acesso às APIs do Xbox Live para seus jogos, incluindo pontuação e conquistas, bem como a funcionalidade de vários jogadores entre dispositivos, salvamentos na nuvem e todos os recursos do Xbox Live no Xbox One. 

              O [ID@Xbox](http://www.xbox.com/Developers/id) também pode fornecer acesso aos kits de desenvolvimento do Xbox One para jogos que exigem acesso ao potencial máximo do hardware do Xbox One.

### Os mecanismos de jogo padrão funcionarão?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão de teste.

### Quais funcionalidades e recursos do sistema estão disponíveis para jogos UWP no Xbox One? 

Veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md) para saber mais.

### Se eu criar um jogo UWP no DirectX 12, ele executará no meu Xbox One em modo de desenvolvedor?

Veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md) para saber mais.

### Toda a superfície de API da UWP estará disponível no Xbox?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão de teste.

### Corrigindo falhas de implantação

Se você não puder implantar seu aplicativo no Visual Studio, estas etapas poderão ajudá-lo a resolver o problema. Se você ficar paralisado, peça ajuda no fórum.

> 
              **Observação**&nbsp;&nbsp;Você deve ter um usuário conectado para implantar um aplicativo. Se você receber uma mensagem de erro 0x87e10008, verifique se tem um usuário conectado e tente novamente.

Se o Visual Studio não puder se conectar ao Xbox One:

1. Verifique se você está no modo de desenvolvedor (discutido anteriormente nesta página).
2. Verifique se você configurou seu computador de desenvolvimento corretamente. Você seguiu *todas* as direções em [Introdução ao desenvolvimento de aplicativos UWP no Xbox One](getting-started.md)? 

3. Se ainda não tiver lido, leia o tópico [Configuração do ambiente de desenvolvimento](development-environment-setup.md) e o tópico [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

4. Verifique se você pode executar "ping" no endereço IP do console em seu computador de desenvolvimento.
> 
              **Observação**&nbsp;&nbsp;Para obter o melhor desempenho de implantação, recomendamos que você use uma conexão com fio com seu console.

5. Verifique se você está usando o Universal (protocolo não criptografado) na lista suspensa Autenticação na guia **Depurar**. Veja [Configuração do ambiente de desenvolvimento](development-environment-setup.md) para obter mais detalhes.

<!--6. Make sure you are not hitting a PIN pairing issue; see "Visual Studio/Xbox PIN pairing failures" in the [Known Issues](known-issues.md) topic.-->

<!--
If Visual Studio can connect, but deployment is failing (for example you get this error message: "DEP0700 : Registration of the app failed.(0x80073cf9)"):

1. Make sure that your app is not installed by uninstalling it from the Collections app in the Xbox One shell. 

> **Note**&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

2. If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode, and then switch back to Developer Mode. 
This will clear Dev Storage.

3. If your issues persist, follow the steps above and then use **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.
-->

### Se eu estiver criando um aplicativo usando HTML/JavaScript, como habilito a navegação Gamepad?

TVHelpers é um conjunto de amostras e bibliotecas de JavaScript e XAML/C# para ajudar você a criar excelentes experiências de TV e do Xbox One em JavaScript e C#. TVJS é uma biblioteca que ajuda você a criar aplicativos UWP especiais para o Xbox One. TVJS inclui suporte para navegação de controladores automáticos, reprodução de mídia avançada, pesquisa e muito mais. Você pode usar TVJS com seu aplicativo Web hospedado de maneira tão fácil quanto com um aplicativo Web UWP empacotado, com acesso total às APIs do Windows Runtime.

Para saber mais, veja o projeto [TVHelpers](https://github.com/Microsoft/TVHelpers) e o [wiki](https://github.com/Microsoft/TVHelpers/wiki) do projeto.

## Consulte também
- [Problemas conhecidos com a UWP no Xbox One Developer Preview](known-issues.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Jul16_HO2-->


