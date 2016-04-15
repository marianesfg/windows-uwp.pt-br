---
title: Perguntas frequentes
description: Perguntas frequentes sobre a UWP no Xbox.
area: Xbox
---

# Perguntas frequentes

As coisas não estão funcionando da forma esperada? 
Procure nesta página de perguntas frequentes. 
Confira também o tópico [Problemas conhecidos](known-issues.md) e o fórum [Desenvolvendo aplicativos Universal do Windows](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### Por que meus jogos e aplicativos não estão funcionando?

Se seus jogos e aplicativos não estiverem funcionando ou se você não tiver acesso à loja ou ao Live Services, você provavelmente está executando em modo de desenvolvedor. 
Você pode saber se está executando no modo de desenvolvedor, se você selecionar Início e vir um grande bloco de Dev Home no lado direito da tela, em vez do conteúdo Gold/Live. 
Se você quiser jogar jogos, poderá abrir a Dev Home e alternar para o modo de varejo usando o botão **Sair do modo de desenvolvedor**.

### Por que não consigo me conectar ao Xbox One usando o Visual Studio?

Comece verificando se você está executando no modo de desenvolvedor e não no modo de varejo. 
Você não pode se conectar ao Xbox One quando está no modo de varejo. 
Você pode verificar isso simplesmente pressionando o botão **Início** e procurando o bloco Dev Home no lado direito da tela. 
Se o bloco não estiver lá, mas se em vez disso você vir conteúdo Gold/Live, você estará no modo de varejo. 
Você precisa executar o aplicativo de ativação Desenvolver Mais para alternar para o modo de desenvolvedor.

Para saber mais, veja [Corrigindo falhas na implantação](frequently-asked-questions.md#fixing-deployment-failures) posteriormente nesta página.

### Como alternar entre o modo de varejo e modo de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados.

### Como saber se estou no modo de varejo ou de desenvolvedor?

Siga as instruções em [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md) para entender mais sobre esses estados. 

Você pode verificar isso simplesmente pressionando o botão **Início** e examinando o lado direito da tela. 
Se você estiver no modo de desenvolvedor, você verá o bloco Dev Home no lado direito. 
Se você estiver no modo de varejo, você verá o conteúdo Gold/Live habitual.

### Meus aplicativos e jogos ainda funcionarão se ativar o modo de desenvolvedor?

Sim, você pode alternar do modo de desenvolvedor para o modo de varejo, onde você pode jogar seus jogos. 
Para saber mais, veja a página [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md). 

> **CUIDADO**&nbsp;&nbsp;A Atualização do Xbox Developer Preview System inclui software experimental e de pré-lançamento inicial. 
Isso significa que alguns aplicativos e jogos populares não funcionarão conforme o esperado, e você poderá experimentar falhas ocasionais e perda de dados.

### Irei perder meus aplicativos e jogos ou alterações salvas?

Se você decidir sair do programa Developer Preview, talvez você precise fazer uma redefinição de fábrica que apagará todo o conteúdo do seu console. 
Se isso acontecer, você precisará reinstalar todos os jogos e aplicativos. 
Se você estava online quando jogou seus jogos, todos os jogos salvos estarão salvos em seu perfil na nuvem da conta do Live, portanto não os perderá.

### Como posso sair do Developer Preview?

Veja o tópico [Desativação do modo de desenvolvedor do Xbox One](devkit-deactivation.md) para obter detalhes sobre como sair do Developer Preview.

### Eu vendi meu Xbox One e o deixei no modo de desenvolvedor. Como posso desativar o modo de desenvolvedor?

Se você não tiver mais acesso ao Xbox One, você poderá desativá-lo no Centro de Desenvolvimento do Windows. 
Para obter detalhes, veja a seção **Desativar seu console usando o Centro de Desenvolvimento do Windows** no tópico [Desativação do modo de desenvolvedor do Xbox One](devkit-deactivation.md#deactivate-your-console-through-windows-dev-center).

### Saí do Developer Preview usando o Centro de Desenvolvimento do Windows, mas ainda estou no modo de desenvolvedor. O que devo fazer?

Inicie o Dev Home e selecione o botão **Sair do modo de desenvolvedor**. 
Isso reiniciará o seu console no modo de varejo. 

### Posso publicar meu aplicativo?

A publicação de aplicativos estará disponível por meio do Centro de Desenvolvimento mais tarde neste ano. 
Os aplicativos UWP criados e testados em um Xbox One de varejo irão passar pelo mesmo processo de ingestão, revisão e publicação que o Windows realiza atualmente, com análises adicionais para atender aos padrões atuais do Xbox One.

### Posso publicar meu jogo?

Você pode usar a UWP e o Xbox One no modo de desenvolvedor para compilar e testar seus jogos no Xbox One. 
Para publicar jogos UWP você deve se registrar no [ID@XBOX](http://www.xbox.com/en-us/Developers/id). 
O [ID@XBOX](http://www.xbox.com/en-us/Developers/id) fornece aos desenvolvedores acesso total às APIs do Xbox Live para seus jogos, incluindo pontuação do jogador e conquistas, 
além da habilidade de tirar proveito de múltiplos jogadores entre dispositivos, salvos na nuvem e todos os recursos do Xbox Live no Xbox One. 
O [ID@Xbox](http://www.xbox.com/en-us/Developers/id) também pode fornecer acesso aos kits de desenvolvimento do Xbox One para jogos que exigem acesso ao potencial máximo do hardware do Xbox One.

### Os mecanismos de jogo padrão funcionarão?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão de teste.

### Quais funcionalidades e recursos do sistema estão disponíveis para jogos UWP no Xbox One? 

Veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md) para saber mais.

### Se eu criar um jogo UWP no DirectX 12, ele executará no meu Xbox One em modo de desenvolvedor?

Veja [Recursos do sistema para aplicativos e jogos UWP no Xbox One](system-resource-allocation.md) para saber mais.

### Toda a superfície de API da UWP estará disponível no Xbox?

Confira a página [Problemas conhecidos](known-issues.md) para esta versão de teste.

### Corrigindo falhas de implantação

Se você não puder implantar seu aplicativo no Visual Studio, estas etapas poderão ajudá-lo a resolver o problema. 
Se você ficar paralisado, peça ajuda no fórum.

Se o Visual Studio não puder se conectar ao Xbox One:

1. Verifique se você está no modo de desenvolvedor (discutido anteriormente nesta página).
2. Verifique se você configurou seu computador de desenvolvimento corretamente. Você seguiu *todas* as direções em [Introdução ao desenvolvimento de aplicativos UWP no Xbox One](getting-started.md)? 

3. Se ainda não tiver lido, leia o tópico [Configuração do ambiente de desenvolvimento](development-environment-setup.md) e o tópico [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

4. Verifique se você está usando uma conexão de rede com fio para o Xbox One. Vimos problemas de desempenho e de conectividade com alguns pontos de extremidade sem fio.

5. Verifique se você pode executar "ping" no endereço IP do console em seu computador de desenvolvimento.

6. Verifique se você está usando o Universal (protocolo não criptografado) na lista suspensa Autenticação na guia **Depurar**. Veja [Configuração do ambiente de desenvolvimento](development-environment-setup.md) para obter mais detalhes.

7. Verifique se você não está atingindo problemas de emparelhamento de PIN. Veja "Falhas de emparelhamento de PIN no Visual Studio/Xbox" no tópico [Perguntas frequentes](frequently-asked-questions.md).

Se o Visual Studio puder se conectar, mas houver falha na implantação (por exemplo se você receber esta mensagem de erro: "DEP0700: falha no registro do aplicativo.(0x80073cf9)"):

1. Verifique se seu aplicativo não está instalado desinstalando-o no aplicativo Coleções no shell do Xbox One. 

> **Observação**&nbsp;&nbsp;Desinstalar seu aplicativo no Windows Device Portal (WDP) não resolverá o problema.

2. Se os problemas persistirem, desinstale seu aplicativo ou jogo no aplicativo Coleções, saia do modo de desenvolvedor, reinicie no modo de varejo e alterne novamente para o modo de desenvolvedor. 
Isso limpará o armazenamento do dispositivo.

3. Se os problemas persistirem, siga as etapas acima e use **Redefinir e manter meus aplicativos e jogos** para excluir todos os estados armazenados em seu Xbox One. 
Vá para Configurações > Sistema > Informações e atualizações do console > Redefinir console e selecione o botão **Redefinir e manter meus aplicativos e jogos**.

> **Cuidado**&nbsp;&nbsp;Fazer isso excluirá todas as configurações salvas em seu Xbox One incluindo configurações sem fio, contas de usuário e qualquer progresso de jogo que não tenha sido salvo no armazenamento em nuvem.

> **Cuidado**&nbsp;&nbsp;NÃO selecione o botão **Redefinir e remover tudo**.
Isso irá excluir todos os jogos, aplicativos, configurações e conteúdo, desativar o modo de desenvolvedor e remover seu console do grupo Developer Preview.

### Se eu estiver criando um aplicativo usando HTML/JavaScript, como habilito a navegação Gamepad?

TVHelpers é um conjunto de amostras e bibliotecas de JavaScript e XAML/C# para ajudar você a criar excelentes experiências de TV e do Xbox One em JavaScript e C#. 
TVJS é uma biblioteca que ajuda você a criar aplicativos UWP especiais para o Xbox One. TVJS inclui suporte para navegação de controladores automáticos, reprodução de mídia avançada, pesquisa e muito mais. 
Você pode usar TVJS com seu aplicativo Web hospedado de maneira tão fácil quanto com um aplicativo Web UWP empacotado, com acesso total às APIs do Windows Runtime.

Para saber mais, veja o projeto [TVHelpers](https://github.com/Microsoft/TVHelpers) e o [wiki](https://github.com/Microsoft/TVHelpers/wiki) do projeto.

## Veja também
- [Problemas conhecidos com a UWP no Xbox One Developer Preview](known-issues.md)
- [UWP no Xbox One](index.md)


<!--HONumber=Mar16_HO5-->


