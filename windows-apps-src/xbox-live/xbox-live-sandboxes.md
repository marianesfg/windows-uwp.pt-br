---
title: Áreas restritas do Xbox Live
description: Saiba mais sobre as áreas restritas do desenvolvimento do Xbox Live.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599471"
---
# <a name="xbox-live-sandboxes-intro"></a>Introdução de áreas restritas ao vivo do Xbox

Na [configuração do serviço Xbox Live](xbox-live-service-configuration.md), foi explicado que você deve configurar as informações sobre seu título on-line, geralmente em [Partner Center](https://partner.microsoft.com/dashboard).  Essas informações incluem itens como o placar de líderes que seu título quer exibir, conquistas players podem desbloquear, configuração de emparelhamento, etc.

Quando você faz alterações em sua configuração de serviço, eles precisam ser publicados no Partner Center antes das alterações são detectadas pelo restante do Xbox Live e podem ser vistas por seu título.

Você publica o que é chamado de uma área restrita de desenvolvimento.  Elas permitem que você trabalhe nas alterações ao seu título em um ambiente isolado.  Eles oferecem várias vantagens descritas no seção abaixo.

Por padrão, os Consoles um Xbox e PCs com Windows 10 são na área restrita de varejo.

## <a name="benefits"></a>Benefícios

Áreas de segurança de desenvolvimento oferecem alguns benefícios:


1. Você pode iterar sobre as alterações a uma atualização para o título sem afetar a versão atualmente disponível.
2. Algumas ferramentas só funcionam em uma área restrita de desenvolvimento por motivos de segurança.
3. Alguns desenvolvedores na sua equipe talvez queira "branch" e o teste de alterações de configuração de serviço sem afetar sua configuração de serviço do primário no desenvolvimento.
4. Outros editores não podem ver o que você está trabalhando sem obter acesso a sua área restrita.

Você também pode **opcionalmente** criar contas de teste.  Você pode usá-los se você não quiser usar sua conta do Xbox Live regular para testar seu título ou, você precisa de várias contas para testar cenários como a interação social (por exemplo: exibir estatísticas de um amigo) ou com vários participantes.

Contas de teste podem apenas entrar para áreas restritas de desenvolvimento e serão explicadas em uma seção abaixo.

## <a name="finding-out-about-your-sandbox"></a>Para obter informações sobre sua área restrita

A grande maioria dos desenvolvedores precisam apenas uma área de segurança.  Felizmente, uma área restrita é criada para você quando você cria um título.

1. Você saber sobre sua área restrita indo até o Partner Center aqui: ![](images/getting_started/first_xbltitle_dashboard.png)

1. Em seguida, clique em seu título: ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. Por fim clique em serviços -> Xbox Live no menu à esquerda ![](images/getting_started/first_xbltitle_leftnav.png)

1. Agora você pode ver sua área restrita listada da seguinte maneira ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>Como a sua área restrita afeta seu fluxo de trabalho

Normalmente você trabalhar com áreas de segurança das seguintes maneiras:

1. (Uma vez) Alterne o seu computador ou no Xbox One para sua área restrita de desenvolvimento.
2. (Muitos vezes) Conforme você faz alterações em sua configuração de serviço, você publicará as alterações à sua área restrita de desenvolvimento.  Alterações de configuração de serviço são coisas como definindo conquistas, adicionando os placares de líderes ou modificação de um modelo de sessão com vários participantes.
3. (Algumas vezes) Se você estiver trabalhando com outros membros da equipe, você pode ter acesso a sua área restrita
4. (Uma vez) Se você precisar testar algo no varejo ou deseja colocar uma quebra de seu jogo favorito Xbox, você precisará alternar sua área restrita para varejo.

Esses cenários serão descritos mais detalhadamente abaixo.  O processo tem algumas diferenças em PC e Console, portanto, há seções separadas para cada um.

## <a name="switch-your-pcs-development-sandbox"></a>Alternar a área restrita para desenvolvimento do seu PC

Se você desejar alternar de área restrita para desenvolvimento do seu PC, a maneira recomendada para fazer isso usando o Windows Device Portal WDP ().  Você também pode fazer isso por meio da linha de comando.  Descreveremos as duas maneiras.

### <a name="windows-device-portal"></a>Windows Device Portal

Se você ainda não tiver habilitado WDP em seu computador, siga estas instruções para fazer isso. [Portal do programa de instalação de dispositivo na área de trabalho do Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

Assim que tiver feito isso, abra o Portal de desenvolvimento do Windows ao se conectar a ele no seu navegador da web, conforme descrito no artigo acima.

Em seguida, clique no "Xbox Live" para ir a seção apropriada, conforme mostrado abaixo.

![](images/getting_started/wdp_switch_sandbox.png)

Em seguida, você pode inserir a área restrita do que você obteve por meio das etapas na *Localizando seu Sandbox* e clique em "Alterar".

Para voltar para varejo, você pode inserir varejo aqui.

### <a name="powershell-module"></a>Módulo do PowerShell

[Módulo de PowerShell do Xbox Live](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule contém vários utilitários para ajudar o desenvolvimento de Xbox Live incluindo alterar áreas restritas no PC ou console.

* Para consumi-la [da Galeria do PowerShell](https://www.powershellgallery.com/packages/XboxlivePSModule), abra uma janela do PowerShell:
    1. Baixe e instale o módulo: `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. Começar a usar, executando `Import-Module XboxlivePSModule`
    3. Executar os cmdlets, ou seja, XDKS.1 XblSandbox Set ou Get-XblSandbox

* Para consumi-lo em um arquivo zip no [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), abra uma janela do PowerShell
    1. Execute `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. Executar os cmdlets, ou seja, XDKS.1 XblSandbox Set ou Get-XblSandbox

### <a name="command-prompt-script"></a>Prompt de comando de Script

Baixar o pacote de ferramentas do Xbox Live em [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) e descompacte.  Você encontrará um arquivo de lote SwitchSandbox.cmd dentro.

Executá-lo no modo administrador para alternar sua área restrita.  O primeiro argumento é a área restrita.  Por exemplo se você está tentando alternar para a área restrita XDKS.1, você faria:

```
SwitchSandbox.cmd XDKS.1
```

Para voltar para varejo, você pode simplesmente fornecer que como o segundo argumento.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Alternar a área de restrita de desenvolvimento do console Xbox One

### <a name="using-windows-dev-portal"></a>Usando o Portal de desenvolvimento do Windows

Você pode usar o Portal de desenvolvimento do Windows para alterar a área restrita no seu console.  Para fazer isso, vá para "Home Dev" no seu console e habilitá-lo.

Depois que você pode digitar o endereço IP no navegador da web em seu computador para se conectar ao console do.  Você pode, em seguida, clique no "Xbox Live" e insira a área restrita, na caixa de texto.

### <a name="using-xbox-one-manager"></a>Usando o Xbox One Manager

Xbox One Manager permite que você administrar determinados aspectos do seu console do seu computador.  Isso inclui a reinicialização, gerenciamento de aplicativos instalados e alterando sua área restrita.

Clique com botão direito no console que você deseja alterar a área restrita para e vá para "Configurações..."

Em seguida, você pode inserir uma área restrita de lá.

### <a name="using-xbox-one-console-ui"></a>Usando a interface do usuário do console Xbox One

Se você quiser alterar sua área restrita para desenvolvimento diretamente do seu console, você pode ir para "Configurações".  Em seguida, vá para "Configurações do desenvolvedor" e você verá uma opção para alterar sua área restrita.

## <a name="sandbox-uses"></a>Usos de área restrita

### <a name="data-that-is-sandboxed"></a>Dados que é uma área restrita
Você pode usar os recursos de área restrita para gerenciar o acesso entre os desenvolvedores de sua equipe durante o processo de desenvolvimento.  Por exemplo, convém isolar dados entre sua equipe de desenvolvimento e testadores.

Os dados na área restrita incluem:
- Conquistas, placares de líderes e estatísticas para um usuário.  Realizações acumuladas para um usuário em uma área de segurança não traduzem a área restrita do outro.
- Com vários participantes e o relacionamento de pessoas.  Os usuários não podem executar vários jogadores jogo com alguém é uma área de segurança diferente.
- Configuração do serviço.  Se você adicionar uma nova medalha para um título em uma área de segurança, não é visível em uma área diferente.  Isso se aplica a todos os dados de configuração de serviço.

Dados de área restrita e não são informações predominantemente sociais.  Então, por exemplo, se um usuário segue a outro usuário, que a relação é independente de área restrita.

### <a name="examples"></a>Exemplos
Alguns exemplos serão fornecidos abaixo para ajudar a ilustrar alguns dos benefícios de por que você talvez queira usar várias áreas de segurança.

> **Observação**: Se você estiver no programa de criadores do Xbox, você pode ter apenas uma área de segurança.  Se precisar criar várias áreas de segurança, aplicar o ID@Xbox programa.

#### <a name="service-config-isolation"></a>Isolamento de configuração de serviço
Conforme mencionado acima, configuração de serviço é específico de área restrita.  Você pode ter um *Development* área restrita e uma *testes* área restrita.  Quando você der um build do seu título para que seus testadores, você poderá publicar seu [configuração de serviço](xbox-live-service-configuration.md) para o *testes* área restrita.

Em seguida, enquanto isso, você poderia adicionar realizações ou tipos de sessão para múltiplos jogadores diferente para seu *desenvolvimento* sandbox sem afetar a configuração de serviço que seus testadores estão observando.

#### <a name="multiplayer"></a>Multijogadores
Veja o exemplo acima, com um *Development* e *testes* área restrita.  Talvez sua configuração de serviço é o mesmo entre as áreas restritas, mas os desenvolvedores estão criando recursos com vários participantes e deseja testes para que haja correspondência entre si.  Os testadores também estão testando com vários participantes.

Em casos assim, seus desenvolvedores talvez não queira o serviço Xbox Live cruzamento associá-las aos testadores, porque eles são problemas de depuração separadamente.  Uma boa maneira de evitar que isso seria para desenvolvedores no *Development* área restrita e testadores para estar em um separado *testes* área restrita.  Isso impede que os dois grupos isolados.

## <a name="advanced"></a>Avançado

Para simplificar o processo de desenvolvimento, começar com a área restrita do padrão e adicionar novas áreas restritas criteriosamente.

Depois de encontrar seu isolamento de controle e de dados de acesso precisa crescendo, você pode ver [Advanced o Xbox Live áreas restritas](advanced-xbox-live-sandboxes.md) artigo.  
