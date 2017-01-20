---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Habilitar seu dispositivo para desenvolvimento
description: "Configure seu dispositivo Windows 10 para desenvolvimento e depuração."
keywords: "Introdução, Licença de desenvolvedor, Visual Studio, licença de desenvolvedor, habilitar dispositivo"
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: 7a6f0be15105bc70e580eaaf581152338c56bed7
ms.openlocfilehash: 3641fdd1f7eb74a11233115d9dfb809ea959926e

---
# <a name="enable-your-device-for-development"></a>Habilitar seu dispositivo para desenvolvimento

Antes de gravar aplicativos, você precisará habilitar o Modo de Desenvolvedor no seu computador de desenvolvimento e nos dispositivos em que testará seu código.

## <a name="use-developer-features"></a>Usar recursos de desenvolvedor

### <a name="develop-your-app-with-microsoft-visual-studio"></a>Desenvolver seu aplicativo com o Microsoft Visual Studio

Você deve habilitar o Modo de Desenvolvedor em seu computador para poder abrir um projeto de aplicativo UWP no Visual Studio. Se você abrir um projeto UWP e o Modo de Desenvolvedor não estiver habilitado, a página de configurações **Para desenvolvedores** será aberta automaticamente. Siga as instruções na próxima seção para habilitar o Modo de Desenvolvedor.

Quando você abrir um projeto de aplicativo UWP no Visual Studio no Windows 10, versão 1511 ou anterior, verá esta caixa de diálogo no Visual Studio. 

![Habilitar a caixa de diálogo do modo de desenvolvedor exibida no Visual Studio](images/latestenabledialog.png)

Quando você vir essa caixa de diálogo, clique em **configurações para desenvolvedores** para abrir a página de configurações **Para desenvolvedores** e habilite o Modo de Desenvolvedor.

> Você pode ir para a página **Para desenvolvedores** a qualquer momento para habilitar ou desabilitar o Modo de Desenvolvedor: basta inserir "configurações de desenvolvedor" na caixa de pesquisa da Cortana na barra de tarefas.

### <a name="enable-your-windows-10-devices"></a>Habilitar seus dispositivos Windows 10

Você pode habilitar um dispositivo para desenvolvimento ou apenas para sideload.

-   *Sideload* significa instalar e executar ou testar um aplicativo que não foi certificado pela Windows Store. Por exemplo, um aplicativo que é interno somente para sua empresa.
-   *Modo de desenvolvedor* permite o sideload de aplicativos e também executar aplicativos no Visual Studio no modo de depuração. 

    Quando você ativa o Modo de Desenvolvedor, é instalado um pacote de opções que inclui:
    - Instala o Windows Device Portal. O Device Portal está habilitado e regras de firewall são definidas para ele somente quando a opção **Habilitar o Device Portal** está ativada.
    - Instala, habilita e configura as regras de firewall para serviços SSH que permitem a instalação remota de aplicativos.
    - (Somente desktop) Permite a habilitação do subsistema do Windows para Linux. Para obter mais informações, consulte [Sobre Bash do Ubuntu no Windows](https://msdn.microsoft.com/commandline/wsl/about).

Para obter informações mais detalhadas sobre as opções, consulte [Quais configurações devo escolher: sideload de aplicativos ou modo de desenvolvedor?](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)

**Para usar recursos de desenvolvedor**

1.  No dispositivo que você deseja habilitar, vá para **Configurações**. Escolha **Atualização e segurança** e, em seguida, **Para desenvolvedores**.
2.  Escolha o nível de acesso de que você precisa. Para desenvolver aplicativos UWP, escolha **Modo de Desenvolvedor**. 
3.  Leia o aviso de isenção da configuração escolhida e clique em **Sim** para aceitar a alteração.

> [!NOTE]
> Se seu dispositivo pertencer a uma organização, algumas opções poderão ter sido desabilitadas por sua organização, conforme mostrado aqui.

Aqui está a página de configurações na família de dispositivos de desktop.

![Vá para Configurações, escolha Atualização e segurança e Para desenvolvedores para exibir suas opções](images/devmode-pc-options.png)

Aqui está a página de configurações na família de dispositivos móveis.

![Nas Configurações do telefone, escolha Atualização e segurança](images/devmode-mob.png)

## <a name="developer-mode-features"></a>Recursos do Modo de Desenvolvedor

Para cada família de dispositivos, recursos de desenvolvedor adicionais podem estar disponíveis. Esses recursos estão disponíveis apenas quando o Modo de Desenvolvedor está habilitado no dispositivo e podem variar dependendo da sua versão do sistema operacional.

Esta imagem mostra os recursos de desenvolvedor para a família de dispositivos móveis com Windows 10, versão 1511.

![Opções do Modo de desenvolvedor para dispositivos móveis](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Device Portal

Para saber mais sobre a descoberta de dispositivo e o Device Portal, consulte [Visão geral do Windows Device Portal](../debug-test-perf/device-portal.md).

Para obter instruções de instalação específicas ao dispositivo, consulte:
- [Device Portal para desktop](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Device Portal para HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal para IoT](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Device Portal para celulares](../debug-test-perf/device-portal-mobile.md)
- [Device Portal para Xbox](../debug-test-perf/device-portal-xbox.md)

Se você encontrar problemas ao habilitar o Modo de Desenvolvedor ou Device Portal, consulte o fórum [Problemas Conhecidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) para encontrar soluções alternativas para esses problemas. 

###<a name="ssh"></a>SSH

Os serviços SSH são habilitados quando você ativa o Modo de Desenvolvedor em seu dispositivo.  Isso é usado quando o dispositivo é um destino de implantação para aplicativos UWP.   Os nomes dos serviços são "SSH Server Broker" e "SSH Server Proxy".

> [!NOTE]
> Isso não é a implementação de OpenSSH da Microsoft, que pode ser encontrada em [GitHub](https://github.com/PowerShell/Win32-OpenSSH).

Para aproveitar os serviços SSH, você pode habilitar a descoberta de dispositivos e permitir que o emparelhamento de PIN. Se você pretende executar outro serviço SSH, configure-o em uma porta diferente ou desative os serviços SSH do Modo de Desenvolvedor. Para desativar os serviços SSH, basta desabilitar o Modo de Desenvolvedor.  

### <a name="device-discovery"></a>Descoberta de dispositivos

Quando você habilita a descoberta de dispositivos, permite que seu dispositivo fique visível para outros dispositivos na rede por meio de mDNS.  Esse recurso também permite que você obtenha o PIN SSH para emparelhamento com esse dispositivo.  

![Emparelhamento de PIN](images/devmode-pc-pinpair.PNG)

Você deve habilitar a descoberta de dispositivos somente se pretender transformar o dispositivo em um destino de implantação. Por exemplo, se você usar o Device Portal para implantar um aplicativo em um telefone para teste, precisará ativar a descoberta de dispositivos no telefone, mas não no computador de desenvolvimento.

### <a name="error-reporting-mobile-only"></a>Relatório de erros (somente celular)

Defina esse valor para especificar quantos despejos de memória são salvos no seu telefone.

A coleta de despejos de memória no telefone oferece acesso instantâneo a informações importantes da falha logo depois que a falha ocorre. Os despejos são coletados somente para aplicativos assinados pelo desenvolvedor. Você pode encontrar os despejos no armazenamento do telefone, na pasta Documents\\Debug. Para saber mais sobre arquivos de despejo, consulte [Usando arquivos de despejo](https://msdn.microsoft.com/library/d5zhxt22.aspx).

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Otimizações para Windows Explorer, Área de Trabalho Remota e PowerShell (somente desktop)

 Na família de dispositivos de desktop, a página de configurações **Para desenvolvedores** tem atalhos para as configurações que você pode usar para otimizar seu computador para tarefas de desenvolvimento. Para cada configuração, você pode marcar a caixa de seleção e clicar em **Aplicar**, ou clicar no link **Mostrar configurações** para abrir a página de configurações dessa opção. 

## <a name="which-settings-should-i-choose-sideload-apps-or-developer-mode"></a>Quais configurações devo escolher: sideload de aplicativos ou modo de desenvolvedor?

Por padrão, você só pode instalar aplicativos da Plataforma Universal do Windows (UWP) da Windows Store. A alteração dessas configurações para usar recursos de desenvolvedor pode alterar o nível de segurança do seu dispositivo. Você não deve instalar aplicativos de fontes não verificadas.

### <a name="sideload-apps"></a>Fazer o sideload de aplicativos

A configuração Sideload de aplicativos normalmente é usada por empresas ou escolas que precisam instalar aplicativos personalizados em dispositivos gerenciados sem passar pela Windows Store. Nesse caso, é comum para a organização impor uma política que desabilita a configuração *Aplicativos da Windows Store*, como mostrado anteriormente na imagem da página de configurações. A organização também fornece o certificado necessário e o local de instalação para o sideload de aplicativos. Para saber mais, veja os artigos do TechNet [Sideload de aplicativos no Windows 10](https://technet.microsoft.com/library/mt269549.aspx) e [Introdução à implantação de aplicativo no Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Informações específicas à família de dispositivos

-   Na família de dispositivos de desktop: você pode instalar um pacote de aplicativo (.appx) e qualquer certificado que seja necessário para executar o aplicativo executando o script do Windows PowerShell que é criado com o pacote ("Add-AppDevPackage.ps1"). Para obter mais informações, consulte [Empacotando aplicativos UWP](../packaging/packaging-uwp-apps.md).

-   Na família de dispositivos móveis: se o certificado necessário já estiver instalado, você poderá tocar no arquivo para instalar qualquer .appx enviado a você por email ou em um cartão SD.

**O sideload de aplicativos** é uma opção mais segura que o Modo de Desenvolvedor, pois você não pode instalar aplicativos no dispositivo sem um certificado confiável.

> [!NOTE]
> Se você fizer o sideload de aplicativos, ainda deverá instalar aplicativos somente de fontes confiáveis. Ao instalar um aplicativo de sideload que não foi certificado pela Windows Store, você concorda que obteve todos os direitos necessários para fazer sideload do aplicativo e que é o único responsável por qualquer dano resultante da instalação e da execução do aplicativo. Veja a seção Windows &gt; Windows Store desta [política de privacidade](http://go.microsoft.com/fwlink/?LinkId=521839).

### <a name="developer-mode"></a>Modo de Desenvolvedor

O Modo de Desenvolvedor substitui os requisitos do Windows 8.1 de uma licença de desenvolvedor.  Além de sideload, a configuração Modo de Desenvolvedor permite a depuração e outras opções de implantação. Isso inclui iniciar um serviço SSH para permitir a implantação desse dispositivo. Para interromper esse serviço, você precisa desabilitar o Modo de Desenvolvedor.

Informações específicas à família de dispositivos

-   Na família de dispositivos de desktop:

    Habilite o Modo de Desenvolvedor para desenvolver e depurar aplicativos no Visual Studio. Conforme afirmado anteriormente, você receberá um aviso no Visual Studio se o Modo de Desenvolvedor não estiver habilitado.

    Permite a habilitação do subsistema do Windows para Linux. Para obter mais informações, consulte [Sobre Bash do Ubuntu no Windows](https://msdn.microsoft.com/commandline/wsl/about).

-   Na família de dispositivos móveis:

    Habilite o modo de desenvolvedor para implantar aplicativos no Visual Studio e depurá-los no dispositivo.

    Você pode tocar no arquivo para instalar qualquer .appx enviado a você por email ou em um cartão SD. Não instale aplicativos de fontes não verificadas.

**Dica**  
Há várias ferramentas que você pode usar para implantar um aplicativo de um computador Windows 10 para um dispositivo móvel Windows 10. Ambos os dispositivos devem estar conectados à mesma sub-rede da rede por uma conexão com ou sem fio, ou eles devem estar conectados por USB. Qualquer uma das maneiras listadas instala apenas o pacote do aplicativo (.appx); elas não instalam certificados.

-   Use a ferramenta de implantação de aplicativos do Windows 10 (WinAppDeployCmd). Saiba mais sobre [a ferramenta WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   A partir do Windows 10, versão 1511, você pode usar o [Device Portal](#device_portal) para implantar do seu navegador para um dispositivo móvel executando o Windows 10, versão 1511 ou posterior. Use a página **[Aplicativos](../debug-test-perf/device-portal.md#apps)** no Device Portal para carregar um pacote de aplicativo (.appx) e instalá-lo no dispositivo.

## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Usar políticas de grupo ou chaves de registro para habilitar um dispositivo

Para a maioria dos desenvolvedores, convém usar o aplicativo Configurações para habilitar seu dispositivo para depuração. Em determinados cenários, como testes automatizados, você pode usar outras maneiras de habilitar seu dispositivo de desktop Windows 10 para desenvolvimento.

Você pode usar gpedit.msc para definir as políticas de grupo para habilitar seu dispositivo, a menos que você tenha o Windows 10 Home. Se você tiver o Windows 10 Home, será necessário usar regedit ou comandos do PowerShell para definir as chaves do Registro diretamente para habilitar seu dispositivo.

**Usar gpedit para habilitar seu dispositivo**

1.  Execute **Gpedit.msc**.
2.  Vá para Política do Computador Local &gt; Configuração do Computador &gt; Modelos Administrativos &gt; Componentes do Windows &gt; App Package Deployment
3.  Para habilitar o sideload, edite as políticas para habilitar:

    -   **Permitir instalação de todos os aplicativos confiáveis**

    - OU -

    Para habilitar o modo de desenvolvedor, edite as políticas para habilitar estas duas opções:

    -   **Permitir instalação de todos os aplicativos confiáveis**
    -   **Permitir o desenvolvimento de aplicativos da Windows Store e a instalação de um ambiente de desenvolvimento integrado (IDE)**

4.  Reinicialize o computador.

**Usar regedit para habilitar seu dispositivo**

1.  Execute **regedit**.
2.  Para habilitar o sideload, defina o valor deste DWORD como 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - OU -

    Para habilitar o modo de desenvolvedor, defina os valores deste DWORD como 1:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Usar o PowerShell para habilitar seu dispositivo**

1.  Execute o PowerShell com privilégios de administrador.
2.  Para habilitar o sideload, execute este comando:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - OU -

    Para habilitar o modo de desenvolvedor, execute este comando:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Atualizar seu dispositivo do Windows 8.1 para Windows 10

Ao criar ou fazer sideload de aplicativos em seu dispositivo Windows 8.1, você precisa instalar uma licença de desenvolvedor. Se você atualizar seu dispositivo do Windows 8.1 para o Windows 10, essas informações serão mantidas. Execute o comando a seguir para remover essas informações do seu dispositivo Windows 10 atualizado. Esta etapa não será necessária se você atualizar diretamente do Windows 8.1 para o Windows 10, versão 1511 ou posterior.

**Para cancelar o registro de uma licença de desenvolvedor**

1.  Execute o PowerShell com privilégios de administrador.
2.  Execute este comando: **unregister-windowsdeveloperlicense**.

Depois disso, você precisa habilitar seu dispositivo para o desenvolvimento, conforme descrito neste tópico, para que possa continuar a desenvolver nele. Se não fizer isso, poderá ocorrer um erro quando você depurar seu aplicativo ou tentar criar um pacote para ele. Aqui está um exemplo desse erro:

Erro: DEP0700: Falha no registro do aplicativo.



<!--HONumber=Dec16_HO2-->


