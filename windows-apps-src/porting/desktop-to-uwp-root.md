---
author: normesta
Description: "Crie um pacote de aplicativo do Windows moderno para seu app ou jogo Win32, WPF ou Windows Forms. Adicione experiências modernas para usuários do Windows 10 e simplifique a implantação e a monetização."
Search.Product: eADQiWindows 10XVcnh
title: Ponte de Desktop
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.openlocfilehash: 1830c1661325afe68e8e7cd32528ec075e098b1d
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="desktop-bridge"></a>Ponte de Desktop

Pegue seu aplicativo da área de trabalho existente e adicione experiências modernas para usuários do Windows 10. Em seguida, obtenha maior alcance em todos os mercados internacionais ao distribuí-lo por meio da Windows Store. Você pode monetizar seu app de maneiras muito mais simples, aproveitando os recursos integrados da loja. Obviamente, você não precisa usar a loja. Fique à vontade para usar seus canais existentes.
<div style="float: left; padding: 10px">
    ![desktop para imagem de ponte UWP](images/desktop-to-uwp/desktop-bridge-4.png)
</div>
A ponte de Desktop para UWP é uma infraestrutura integrada à plataforma que permite que você distribua seu aplicativo da área de trabalho ou jogo Win32, WPF ou Windows Forms com eficiência usando um pacote de aplicativo do Windows moderno.

Este pacote fornece ao app uma identidade e, com essa identidade, seu aplicativo da área de trabalho tem acesso a APIs da UWP (Plataforma Universal do Windows). Você pode usá-las para facilitar experiências envolventes e modernas, como blocos dinâmicos e notificações.  Use a compilação condicional simples e as verificações de tempo de execução para executar código UWP somente quando seu aplicativo for executado no Windows 10.

Além do código que você usa para aprimorar as experiências do Windows 10, o app permanece inalterado e você pode continuar a distribuí-lo para sua base de usuários existente do Windows 7, Windows Vista ou Windows XP. No Windows 10, o app continua sendo executado no modo de usuário com confiança total assim como acontece atualmente.

> [!NOTE]
> Confira <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">esta série</a> de vídeos curtos publicados pela Microsoft Virtual Academy. Esses vídeos o acompanham durante todo o processo de trazer seu aplicativo de desktop para a Plataforma Universal do Windows (UWP).

## <a name="benefits"></a>Benefícios

Aqui estão alguns motivos para criar um pacote de aplicativo do Windows para seu aplicativo da área de trabalho:

**Implantação simplificada**. Aplicativos e jogos que usam a ponte têm uma ótima experiência de implantação. Esta experiência garante que os usuários possam instalar um aplicativo confiável e atualizá-lo. Se um usuário optar por desinstalar o aplicativo, ele é removido completamente sem deixar nenhum rastro. Isso reduz o tempo para criar experiências de instalação e manter os usuários atualizados.

**Atualizações automáticas e licenciamento**. Seu aplicativo pode participar dos recursos internos de atualização automática e de licenciamento da Windows Store. A atualização automática é um mecanismo altamente confiável e eficiente, pois somente as partes alteradas dos arquivos são baixadas.

**Maior alcance e monetização simplificada**. Se optar por distribuir por meio da Windows Store, você poderá alcançar milhões de usuários do Windows 10, que podem adquirir aplicativos, jogos e compras realizadas em aplicativo com opções de pagamento locais.

**Adicione recursos UWP**.  Em seu próprio ritmo, você pode adicionar recursos UWP ao pacote do seu aplicativo, como uma interface de usuário XAML, atualizações de bloco dinâmico, tarefas de UWP em segundo plano, serviços de aplicativo, e muito mais.

**Casos de uso ampliados em dispositivos**. Usando a ponte, você pode migrar gradualmente o código para a Plataforma Universal do Windows a fim de alcançar todos os dispositivos Windows 10, inclusive telefones, Xbox One e HoloLens.

Para ver uma lista mais completa dos benefícios, consulte [Ponte de Desktop ](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Preparar

Você planeja publicar seu aplicativo para a [loja de aplicativos Windows ](https://www.microsoft.com/store/apps). Em caso afirmativo, comece preenchendo [esse formulário ](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). A Microsoft entrará em contato com você para iniciar o processo de instalação. Como parte deste processo, você reservará um nome na loja e obterá as informações necessárias para criar o pacote do aplicativo do Windows.

Em seguida, examine o artigo [Preparar para empacotar seu aplicativo da área de trabalho](desktop-to-uwp-prepare.md) e resolva todos os problemas que se apliquem ao app antes de criar um pacote de aplicativo Windows para ele. Talvez você não precise fazer muitas alterações no seu app antes de criar o pacote. No entanto, existem algumas situações que podem exigir que você ajuste seu app antes de criar um pacote para ele.

<span id="convert" />
## <a name="package"></a>Pacote

Aqui estão algumas ferramentas que você pode usar para criar um pacote de aplicativo do Windows para seu app.

### <a name="desktop-app-converter"></a>Desktop App Converter

Embora o termo "Converter" apareça no nome dessa ferramenta, na verdade ela não converte seu app. Seu app permanece inalterado. No entanto, essa ferramenta gera um pacote de aplicativo do Windows para você. Ela pode ser muito conveniente nos casos onde seu app faz muitas modificações no sistema, ou se você tem alguma dúvida sobre o que o instalador faz.

O Desktop App Converter também faz algumas coisas extras para você. Aqui está uma lista desses valores.

* Registre automaticamente seus manipuladores de visualização, manipuladores de miniaturas, manipuladores de propriedades, regras de firewall, sinalizadores de URL.

* Registre automaticamente os mapeamentos do tipo de arquivo que permitem aos usuários agrupar arquivos usando a coluna **Tipo** no Explorador de Arquivos.

* Registre os servidores COM públicos.

* Gere um certificado para que você possa usar para executar seu aplicativo.

* Valide seu app em relação aos requisitos da Ponte de Desktop e da Windows Store.

Consulte [Empacotar um app usando o Desktop App Converter (Ponte de Desktop para UWP)](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="manual-packaging"></a>Empacotamento manual

Se você gosta de um controle granular sobre sua conversão, você pode criar um arquivo de manifesto e, em seguida, executar o **MakeAppx.exe** para criar seu pacote de aplicativo do Windows.

Esta abordagem pode ter sentido se você estiver familiarizado com as mudanças que o instalador faz para o sistema, ou se você não possui um instalador e a maneira como você instala seu aplicativo é copiando fisicamente arquivos para um local de pasta ou usando comandos como **xcopy**. Mas não deixe a ausência de um instalador impedi-lo de empacotar manualmente seu app. Você pode usar o Desktop App Converter para empacotar seu app, mesmo que não tenha um instalador.

Consulte [Converter um app manualmente (Ponte de Desktop para UWP)](desktop-to-uwp-manual-conversion.md).

### <a name="visual-studio"></a>Visual Studio

Esta opção é semelhante à opção manual descrita acima, exceto que o Visual Studio faz algumas coisas para você, como gerar um pacote do app e os ativos visuais para seu app. Pense no Visual Studio como uma ferramenta que você pode usar para empacotar manualmente seu app junto com algumas conveniências extras.

Consulte [Empacotar um aplicativo .NET usando o Visual Studio (Ponte UWP para Desktop)](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Instalador de terceiros

 Vários produtos e instaladores de terceiros populares agora dão suporte à Ponte de Desktop para UWP. Você pode usá-los para gerar instaladores MSI ou pacotes do app com apenas alguns cliques. Embora não produza documentação sobre como usar essas ferramentas, visite seus sites para saber mais.

 Eis algumas opções:

#### <a name="advanced-installer"></a>Instalador avançado

A Caphyon fornece uma ferramenta de empacotamento de aplicativo da área de trabalho baseada em GUI gratuita que ajuda você a gerar um pacote de aplicativo do Windows para seu app com apenas alguns cliques. Ele pode usar qualquer instalador; mesmo aqueles que são executados em modo silencioso e executam uma verificação de validação para determinar se o aplicativo é adequado para empacotamento.
<div style="float: left; padding: 10px; width: 20%">
     ![Logotipo do Instalador Avançado](images/desktop-to-uwp/Advanced_Installer_Vertical.png)
</div>
O Desktop App Converter também se integra ao Hyper-V e ao [VMware](http://www.vmware.com/). Isso significa que você pode usar suas próprias máquinas virtuais, sem precisar baixar uma imagem [Docker](https://docs.docker.com/) correspondente que pode ter mais de 3GB de tamanho.

Você pode usar o [Instalador Avançado](http://www.advancedinstaller.com/) para gerar MSI e [pacotes de aplicativos Windows](http://www.advancedinstaller.com/uwp-app-package.html) a partir dos projetos existentes. Você também pode usar o instalador avançado para importar pacotes de aplicativos do Windows que você gera usando o Microsoft Desktop App Converter. Uma vez importado, você pode mantê-los usando ferramentas visuais especificamente projetadas para aplicativos UWP.

O Advanced Installer também fornece uma extensão para o Visual Studio 2017 e 2015 que pode ser usada para [compilar e depurar aplicativos de ponte de Desktop](http://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Veja este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para uma rápida visão geral.

#### <a name="cloudhouse-compatibility-containers"></a>Contêineres de Compatibilidade Cloudhouse

Para os clientes corporativos com apps de linha de negócios incompatíveis com o Windows 10 e o Windows 10 S, os Contêineres de Compatibilidade da Cloudhouse permitem que os aplicativos do Windows XP e do Windows 7 sejam convertidos em UWP e entregues por meio da Windows Store for Business ou do Microsoft Intune, sem alteração no código-fonte.
<div style="float: left; padding: 10px; width: 20%">
     ![Cloudhouse-Compatibility-Containers](images/desktop-to-uwp/container.png)
</div>
A Cloudhouse fornece um Empacotador Automático que pega qualquer aplicativo e cria um Contêiner de Compatibilidade ao empacotar o app onde ele é executado hoje, por exemplo, no Windows XP. O Contêiner pode então ser convertido no novo formato UWP via integração com a ferramenta Conversor de Ponte de Desktop para criar o pacote de aplicativo do Windows.

O Empacotador Automático usa análise de instalação/captura e de tempo de execução para criar um Contêiner para o aplicativo, que inclui os arquivos e o Registro do aplicativo, e o mecanismo de compatibilidade e redirecionamento que permite que o aplicativo seja executado no Windows 10. Além disso, você pode incluir e isolar qualquer tempo de execução ou pré-requisito necessário para executar o aplicativo para que ele não afete ou entre em conflito com outros aplicativos ou tempos de execução que podem já estar instalados.


Confira o [blog](http://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp) anunciando o suporte para Aplicativos Universais do Windows e a Windows Store for Business.

#### <a name="firegiant"></a>FireGiant

A extensão [Appx FireGiant](https://www.firegiant.com/products/wix-expansion-pack/appx) permite criar pacotes de aplicativo do Windows e pacotes MSI simultaneamente do mesmo código-fonte WiX. Sempre que você compilar, poderá ter como destino a Ponte de Desktop no Windows 10 com um pacote de aplicativo do Windows e versões anteriores do Windows com MSI.
<div style="float: left; padding: 10px; width: 20%">
     ![Logotipo do Instalador Avançado](images/desktop-to-uwp/FG3rdPartyLogo.png)
</div>
A extensão Appx FireGiant usa análise estática e emulação inteligente de projetos WiX para criar pacotes de aplicativo do Windows sem a sobrecarga de espaço em disco e de tempo de execução de contêineres ou máquinas virtuais.

Como a extensão Appx FireGiant não converte o instalador ao executá-lo, você pode manter seu instalador WiX sem precisar repetidamente convertê-lo em pacotes de aplicativo do Windows. Todos os usuários em diferentes versões do Windows obtêm seus últimos aprimoramentos e você não precisa se preocupar com pacotes de aplicativos MSI e Windows fora de sincronia.

Confira este [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) e veja como em algumas linhas de código o CEO da FireGiant, Rob Mensching, cria uma versão do Appx (pacote de aplicativo do Windows) da conhecida ferramenta de compactação 7-Zip e então como ele aprimora os pacotes de aplicativos Windows e MSI com alterações no mesmo código-fonte WiX.

#### <a name="installaware"></a>InstallAware

A Install**Aware** fornece extensões gratuitas do Install**Aware** para as versões 2012 a 2017 do . Você pode usá-las para criar pacotes de aplicativo Windows com um único clique diretamente da [barra de ferramentas do Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).
<div style="float: left; padding: 10px; width: 20%">
    ![Logotipo da FireGiant](images/desktop-to-uwp/installaware.png)
</div>
Você também pode importar qualquer instalação, mesmo se não tiver o código-fonte dessa instalação, usando o Package**Aware** (capturas de instalação sem instantâneos) ou o Assistente de Importação de Banco de Dados (para todos os instaladores MSI e módulos de mesclagem MSM). Você pode usar as [ferramentas de GUI](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para manter e aprimorar suas importações, visualmente ou por meio de script.

As [opções avançadas de criação de APPX](https://www.installaware.com/mhtml5/desktop/appx.htm) ajudam a direcionar envios da Windows Store ou a produzir binários de pacote de aplicativo do Windows para distribuição de sideload para os usuários finais. Você pode até mesmo compilar pacotes do Instalador **WSA** (Aplicativos do Windows Server) destinados a implantações para **Nano Servidor** desde uma única origem e com suporte total para [automação de linha de comando](https://www.installaware.com/scripting-automation-interface.htm), além de uma GUI.

A Install**Aware** também criou como [software livre](https://www.installaware.com/gnu.asp) uma **biblioteca de compilador APPX**, além de um applet de linha de comando de exemplo, sob a licença da GNU Affero GPL. Tudo isso foi projetado para uso com plataformas de software livre, como a WiX.

#### <a name="installshield"></a>InstallShield

A InstallShield fornece uma única solução para desenvolver instaladores MSI e EXE, criar pacotes UWP (Plataforma Universal do Windows) e WSA (Aplicativo de Windows Server) e para virtualizar aplicativos com um mínimo de scripts, codificação e reformulação.
<div style="float: left; padding: 10px; width: 20%">
    ![Logotipo da InstallShield](images/desktop-to-uwp/InstallShield-logo.jpg)
</div>
Examine seu projeto InstallShield em segundos para economizar horas de trabalho de investigação ao identificar automaticamente potenciais problemas de compatibilidade entre seu aplicativo e pacotes UWP e WSA.

Prepare-se para a Windows Store e simplifique a experiência de instalação do software no Windows 10 com a criação de pacotes de aplicativo UWP de seus projetos existentes do InstallShield. Crie pacotes do Windows Installer e de aplicativo UWP para dar suporte a todos os cenários de implantação desejados por seus clientes. Dê suporte a implantações do Nano Servidor e do Windows Server 2016 ao criar pacotes WSA de seus projetos existentes do InstallShield.

Desenvolva sua instalação em módulos para facilitar a implantação e a manutenção e então mescle os componentes e as dependências em tempo de compilação em um único pacote de aplicativo UWP para a Windows Store. Para a distribuição direta fora da loja, empacote seus Pacotes de Aplicativo UWP e outras dependências com um instalador de IU de pacote/avançado.

Saiba mais neste [livro eletrônico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).


#### <a name="rad-studio"></a>RAD Studio

Consulte [RAD Studio da Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="integrate"></a>Integrar

Se seu aplicativo precisa ser integrado com o sistema (por exemplo: estabelecer regras de firewall), descreva isso no manifesto do pacote do seu app e o sistema fará o resto. Para a maioria dessas tarefas, você não precisará escrever qualquer código. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar seu aplicativo no Explorador de Arquivos e adicionar em seu aplicativo uma lista de destinos de impressão que aparecem em outros aplicativos.

Consulte [Integrar seu aplicativo com o Windows 10 (Ponte de Desktop do Windows)](desktop-to-uwp-extensions.md).

## <a name="enhance"></a>Aprimorar

Depois de ter empacotado seu app, você poderá aprimorá-lo com recursos como blocos dinâmicos e notificações por push. Alguns desses recursos podem melhorar significativamente o nível de envolvimento de seu aplicativo e custam muito pouco tempo para adicionar. Alguns aprimoramentos exigem um pouco mais de código.

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](desktop-to-uwp-enhance.md).

## <a name="extend"></a>Estender

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Em geral, você deve primeiro determinar se pode adicionar sua experiência por [Aprimoramento](desktop-to-uwp-enhance.md) do seu aplicativo da área de trabalho existente com APIs UWP. Se você tiver de usar um componente UWP, para obter a experiência, então poderá adicionar um projeto UWP à sua solução e usar os serviços de aplicativo para fazer a comunicação entre seu aplicativo da área de trabalho e o componente UWP.

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](desktop-to-uwp-extend.md).

## <a name="migrate"></a>Migrar

Você pode migrar gradualmente o código anterior para a UWP ao mesmo tempo em que mantém a capacidade de executar e publicar o app na área de trabalho do Windows. Depois que tiver migrado totalmente para a UWP (e o aplicativo não contiver mais nenhum componente Win32/WPF), você poderá atingir todos os dispositivos Windows, inclusive telefones, Xbox One e HoloLens.

## <a name="test"></a>Testar

Para testar seu aplicativo em uma configuração realista enquanto você se prepara para distribuição, é melhor assinar seu aplicativo e instalá-lo. Consulte [Testar seu app](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app).

>[!IMPORTANT]
> Se você planeja publicar seu app na Windows Store, certifique-se de que seu aplicativo funcione corretamente em dispositivos que executem o Windows 10 S. Esse é um requisito da loja. Consulte [Testar seu aplicativo do Windows para o Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para dar ao seu aplicativo a melhor chance de ser publicado na Windows Store ou tornar-se [certificado pelo Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666), valide e teste ele localmente antes de enviá-lo para certificação.

Se você estiver usando o DAC para empacotar seu app, poderá usar o novo sinalizador ``-Verify`` para validar o seu pacote em relação aos requisitos da Ponte de Desktop e da Loja. Consulte [Empacotar um app, assinar o app e prepará-lo para envio à loja](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Se você estiver usando o Visual Studio, você pode validar seu aplicativo a partir do assistente de **Criação de Pacotes de Aplicativo**. Consulte [Crie um pacote do aplicativo](../packaging/packaging-uwp-apps.md#create-an-app-package).

Para executar a ferramenta manualmente, consulte [Kit de certificação de aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md).

Para revisar a lista de testes que a certificação de aplicativo do Windows usa para validar seu aplicativo, consulte [testes de aplicativo da ponte de desktop do Windows](../debug-test-perf/windows-desktop-bridge-app-tests.md).

## <a name="distribute"></a>Distribuir

Você pode distribuir seu app publicando-o na Windows Store ou fazendo sideload dele para outros sistemas.

Consulte [Distribuir um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-distribute.md).

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.

## <a name="in-this-section"></a>Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Preparar para empacotar um app](desktop-to-uwp-prepare.md) | Fornece uma lista de itens para examinar antes de empacotar seu aplicativo da área de trabalho. |
| [Empacotar um app usando o Desktop App Converter (Ponte de Desktop)](desktop-to-uwp-run-desktop-app-converter.md) | Mostra como executar o Desktop App Converter. |
| [Empacotar um app manualmente (Ponte de Desktop)](desktop-to-uwp-manual-conversion.md) | Saiba como criar um pacote de aplicativo e manifestá-lo manualmente. |
| [Empacotar um aplicativo .NET usando o Visual Studio (Ponte de Desktop)](desktop-to-uwp-packaging-dot-net.md)| Mostra como empacotar seu aplicativo da área de trabalho usando o Visual Studio. |
| [Integrar seu app com o Windows 10 (Ponte de Desktop)](desktop-to-uwp-extensions.md) | Integre seu aplicativo com o Windows 10 ao descrever tarefas no arquivo de manifesto do pacote do seu projeto de empacotamento. |
| [Aprimorar seu aplicativo da área de trabalho para Windows 10](desktop-to-uwp-enhance.md)| Use as APIs UWP para adicionar experiências modernas que atraiam os usuários do Windows 10. |
| [APIs UWP disponíveis para um aplicativo de área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-supported-api.md) | Veja quais APIs UWP estão disponíveis para seu aplicativo da área de trabalho empacotado a ser usado. |
| [Estender seu aplicativo da área de trabalho com componentes UWP modernos](desktop-to-uwp-extend.md)| Adicione experiências avançadas que devem ser executadas dentro de um contêiner de aplicativo UWP. Conecte seu aplicativo da área de trabalho ao processo UWP usando serviços de aplicativo.|
| [Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-debug.md) | Explica as opções para depurar seu app empacotado. |
| [Distribuir um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-distribute.md) | Veja como você pode distribuir seu app convertido para usuários.  |
| [Nos bastidores da Ponte de Desktop (Ponte de Desktop)](desktop-to-uwp-behind-the-scenes.md) | Veja mais de perto como a Ponte de Desktop para UWP funciona nos bastidores. |
| [Problemas conhecidos (Ponte de Desktop)](desktop-to-uwp-known-issues.md) | Listas problemas conhecidos com a Ponte de Desktop para UWP. |
| [Exemplos de código da Ponte de Desktop](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Exemplo de código no GitHub demonstrando recursos de aplicativos convertidos. |
