---
author: QuinnRadich
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Habilitar seu dispositivo para desenvolvimento
description: Configure seu dispositivo Windows 10 para desenvolvimento e depuração.
keywords: Introdução, Licença de desenvolvedor, Visual Studio, licença de desenvolvedor, habilitar dispositivo
ms.author: quradic
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad817bbae2fb8b28b95095880aa1a65c391720f3
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839829"
---
# <a name="enable-your-device-for-development"></a>Habilitar seu dispositivo para desenvolvimento

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Ative o Modo de Desenvolvedor, faça sideload de aplicativos e acesse outros recursos de desenvolvedor

![Habilitar seus dispositivos para desenvolvimento](images/developer-poster.png)

Se você estiver usando o computador para atividades diárias comuns, como jogos, navegação na Web, email ou aplicativos do Office, você *não* precisa ativar o Modo de Desenvolvedor, na verdade, você não deve ativá-lo. As informações contidas nesta página não serão úteis para você, portanto, não é necessário prosseguir. Agradecemos pela visita!

No entanto, se estiver escrevendo software com o Visual Studio em um computador pela primeira vez, você *precisará* habilitar o Modo de Desenvolvedor no computador de desenvolvimento e nos dispositivos que serão usados para testar o código. Abrir um projeto UWP quando o Modo de Desenvolvedor não está habilitado abrirá a página de configurações **Para desenvolvedores** ou fará com que essa caixa de diálogo apareça no Visual Studio:

![Habilitar a caixa de diálogo do modo de desenvolvedor exibida no Visual Studio](images/latestenabledialog.png)

Quando você vir essa caixa de diálogo, clique em **configurações para desenvolvedores** para abrir a página de configurações **Para desenvolvedores**.

> [!NOTE]
> Você pode ir para a página **Para desenvolvedores** a qualquer momento para habilitar ou desabilitar o Modo de Desenvolvedor: basta inserir "para desenvolvedores" na caixa de pesquisa da Cortana na barra de tarefas.

## <a name="accessing-settings-for-developers"></a>Acessar configurações para desenvolvedores

Para habilitar o Modo de desenvolvedor ou acessar outras configurações:

1.  Na caixa de diálogo de configurações **Para desenvolvedores**, escolha o nível de acesso de que você precisa.
2.  Leia o aviso de isenção da configuração escolhida e, depois, clique em **Sim** para aceitar a alteração.

> [!NOTE]
> Você precisa de acesso de administrador para habilitar o modo de desenvolvedor. Se o dispositivo pertencer a uma organização, essa opção pode ter sido desabilitada.

Aqui está a página de configurações na família de dispositivos de desktop:

![Vá para Configurações, escolha Atualização e segurança e Para desenvolvedores para exibir suas opções](images/devmode-pc-options.png)

Aqui está a página de configurações na família de dispositivos móveis:

![Nas Configurações do telefone, escolha Atualização e segurança](images/devmode-mob.png)

## <a name="which-setting-should-i-choose-sideload-apps-or-developer-mode"></a>Qual configuração devo escolher: sideload de aplicativos ou Modo de Desenvolvedor?

 Você pode habilitar um dispositivo para desenvolvimento ou apenas para sideload.

-   *Aplicativos Microsoft Store* é a configuração padrão. Se você não estiver desenvolvendo aplicativos ou estiver usando aplicativos de desenvolvimento emitidos pela sua empresa, mantenha essa configuração ativa.
-   *Sideload* significa instalar e executar ou testar um app que não foi certificado pela Microsoft Store. Por exemplo, um aplicativo que é interno somente para sua empresa.
-   *Modo de desenvolvedor* permite o sideload de aplicativos e também executar aplicativos do Visual Studio no modo de depuração. 

Por padrão, você só pode instalar aplicativos da Plataforma Universal do Windows (UWP) da Microsoft Store. A alteração dessas configurações para usar recursos de desenvolvedor pode alterar o nível de segurança do seu dispositivo. Você não deve instalar aplicativos de fontes não verificadas.

### <a name="sideload-apps"></a>Fazer o sideload de aplicativos

A configuração Sideload de aplicativos normalmente é usada por empresas ou escolas que precisam instalar aplicativos personalizados em dispositivos gerenciados sem passar pela Microsoft Store. Nesse caso, é comum para a organização impor uma política que desabilita a configuração *Aplicativos UWP*, como mostrado anteriormente na imagem da página de configurações. A organização também fornece o certificado necessário e o local de instalação para o sideload de aplicativos. Para saber mais, veja os artigos do TechNet [Sideload de aplicativos no Windows 10](https://technet.microsoft.com/library/mt269549.aspx) e [Introdução à implantação de aplicativo no Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Informações específicas à família de dispositivos

-   Na família de dispositivos de desktop: você pode instalar um pacote de aplicativo (.appx) e qualquer certificado que seja necessário para executar o aplicativo executando o script do Windows PowerShell que é criado com o pacote ("Add-AppDevPackage.ps1"). Para obter mais informações, consulte [Empacotando aplicativos UWP](../packaging/packaging-uwp-apps.md).

-   Na família de dispositivos móveis: se o certificado necessário já estiver instalado, você poderá tocar no arquivo para instalar qualquer .appx enviado a você por email ou em um cartão SD.


**O sideload de aplicativos** é uma opção mais segura que o Modo de Desenvolvedor, pois você não pode instalar aplicativos no dispositivo sem um certificado confiável.

> [!NOTE]
> Se você fizer o sideload de aplicativos, ainda deverá instalar aplicativos somente de fontes confiáveis. Ao instalar um app de sideload que não foi certificado pela Microsoft Store, você concorda que obteve todos os direitos necessários para fazer sideload do app e que é o único responsável por qualquer dano resultante da instalação e da execução do app. Veja a seção Windows &gt; Microsoft Store desta [política de privacidade](http://go.microsoft.com/fwlink/?LinkId=521839).


### <a name="developer-mode"></a>Modo de Desenvolvedor

O Modo de Desenvolvedor substitui os requisitos do Windows 8.1 de uma licença de desenvolvedor.  Além de sideload, a configuração Modo de Desenvolvedor permite a depuração e outras opções de implantação. Isso inclui iniciar um serviço SSH para permitir a implantação desse dispositivo. Para interromper esse serviço, você precisa desabilitar o Modo de Desenvolvedor.

Quando você habilita o Modo de Desenvolvedor no desktop, é instalado um pacote de recursos que inclui:
- Portal de Dispositivos do Windows. O Portal de Dispositivos é habilitado e as regras de firewall só serão configuradas para ele somente quando a opção **Habilitar o Portal de Dispositivos** estiver ativada.
- Instala e configura as regras de firewall para serviços SSH que permitem a instalação remota de apps. Habilitar a **Descoberta de Dispositivo** ativará o servidor SSH.


## <a name="additional-developer-mode-features"></a>Recursos adicionais do Modo de Desenvolvedor

Recursos de desenvolvedor adicionais podem estar disponíveis para cada família de dispositivos. Esses recursos estão disponíveis apenas quando o Modo de Desenvolvedor está habilitado no dispositivo e podem variar dependendo da sua versão do sistema operacional.

Esta imagem mostra os recursos de desenvolvedor para o Windows 10:

![Opções do modo de desenvolvedor](images/devmode-mob-options.png) 

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Portal de Dispositivos

Para saber mais sobre o Device Portal, consulte [Visão geral do Windows Device Portal](../debug-test-perf/device-portal.md).

Para obter instruções de instalação específicas ao dispositivo, consulte:
- [Device Portal para desktop](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Device Portal para HoloLens](https://developer.microsoft.com/windows/holographic/using_the_windows_device_portal)
- [Device Portal para IoT](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Device Portal para celulares](../debug-test-perf/device-portal-mobile.md)
- [Device Portal para Xbox](../debug-test-perf/device-portal-xbox.md)

Se você tiver problemas para habilitar o Modo de Desenvolvedor ou o Device Portal, consulte o fórum dos [Problemas conhecidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) para encontrar soluções alternativas para esses problemas ou visite [Falha ao instalar o pacote do Modo de Desenvolvedor](#failure-to-install-developer-mode-package) para obter detalhes adicionais e saber quais KBs WSUS você deve permitir para desbloquear o pacote do Modo de Desenvolvedor. 

### <a name="ssh"></a>SSH

Os serviços SSH são habilitados quando você habilita a Descoberta de Dispositivo em seu dispositivo.  Isso é usado quando o dispositivo é um destino de implantação remoto para aplicativos UWP.   Os nomes dos serviços são "SSH Server Broker" e "SSH Server Proxy".

> [!NOTE]
> Isso não é a implementação de OpenSSH da Microsoft, que pode ser encontrada em [GitHub](https://github.com/PowerShell/Win32-OpenSSH).  

Para aproveitar os serviços SSH, você pode habilitar a descoberta de dispositivos e permitir que o emparelhamento de PIN. Se você pretende executar outro serviço SSH, configure-o em uma porta diferente ou desative os serviços SSH do Modo de Desenvolvedor. Para desativar os serviços SSH, desative a Descoberta de Dispositivo.  

O logon SSH é feito por meio da conta "DevToolsUser", que aceita uma senha para autenticação.  Essa senha é o PIN exibido no dispositivo depois de pressionar o botão "Emparelhar" de descoberta de dispositivo e só é válida enquanto o PIN é exibido.  Um subsistema SFTP também é habilitado, para o gerenciamento manual da pasta DevelopmentFiles onde são instaladas as implantações flexíveis de arquivo do Visual Studio. 

#### <a name="caveats-for-ssh-usage"></a>Restrições de uso de SSH
O servidor SSH existente usado no Windows ainda não é compatível com protocolo, portando o uso de um cliente SFTP ou SSH poderá exigir configuração especial.  Em particular, o subsistema SFTP é executado na versão 3 ou inferior, de forma que qualquer cliente conectado deve ser configurado para esperar um servidor antigo.  O servidor SSH em dispositivos mais antigos usa `ssh-dss` para autenticação de chave pública, que o OpenSSH substituiu.  Para se conectar a tais dispositivos, o cliente SSH cliente deve ser configurado manualmente para aceitar `ssh-dss`.  

### <a name="device-discovery"></a>Descoberta de dispositivos

Ao habilitar a descoberta de dispositivos, você permite que seu dispositivo fique visível para outros dispositivos na rede por meio de mDNS.  Esse recurso também permite que você obtenha o PIN SSH para emparelhamento com esse dispositivo pressionando o botão "Emparelhar" exposto assim que a descoberta de dispositivo for habilitada.  Essa solicitação de PIN deve ser exibida na tela para a conclusão da sua primeira implantação do Visual Studio direcionada ao dispositivo.  

![Emparelhamento de PIN](images/devmode-pc-pinpair.PNG)

Você deve habilitar a descoberta de dispositivos somente se pretender transformar o dispositivo em um destino de implantação. Por exemplo, se você usar o Device Portal para implantar um aplicativo em um telefone para teste, precisará ativar a descoberta de dispositivos no telefone, mas não no computador de desenvolvimento.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Otimizações para Windows Explorer, Área de Trabalho Remota e PowerShell (somente desktop)

 Na família de dispositivos de desktop, a página de configurações **Para desenvolvedores** tem atalhos para as configurações que você pode usar para otimizar seu computador para tarefas de desenvolvimento. Para cada configuração, você pode marcar a caixa de seleção e clicar em **Aplicar**, ou clicar no link **Mostrar configurações** para abrir a página de configurações dessa opção. 


## <a name="notes"></a>Observações
Em versões anteriores do Windows 10 Mobile, uma opção Despejos de Memória estava presente no menu Configurações de Desenvolvedor.  Isso foi movido para o [Portal de Dispositivos](../debug-test-perf/device-portal.md) para que ele possa ser usado remotamente em vez de apenas por USB.  

Há várias ferramentas que você pode usar para implantar um app de um computador Windows 10 para um dispositivo Windows 10. Ambos os dispositivos devem estar conectados à mesma sub-rede da rede por uma conexão com ou sem fio, ou eles devem estar conectados por USB. Ambas as maneiras listadas instalam somente o pacote do aplicativo (.appx/.appxbundle); elas não instalam certificados.

-   Use a ferramenta de implantação de aplicativos do Windows 10 (WinAppDeployCmd). Saiba mais sobre [a ferramenta WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Você pode usar o [Portal de Dispositivos](../debug-test-perf/device-portal.md) para implantar do seu navegador para um dispositivo móvel executando o Windows 10, versão 1511 ou posterior. Use a página **[Aplicativos](../debug-test-perf/device-portal.md#apps-manager)** no Device Portal para carregar um pacote de aplicativo (.appx) e instalá-lo no dispositivo.

## <a name="failure-to-install-developer-mode-package"></a>Falha ao instalar o pacote do Modo de Desenvolvedor
Às vezes, devido a problemas de rede ou de ordem administrativa, o Modo de Desenvolvedor não é instalado corretamente. O pacote do Modo de Desenvolvedor é necessário para implantação **remota** neste computador, usando o Device Portal em um navegador ou a descoberta de dispositivos para habilitar SSH, mas não para o desenvolvimento local.  Mesmo que você encontre esses problemas, você ainda pode implantar seu aplicativo localmente por meio do Visual Studio ou deste dispositivo para outro. 

Consulte o fórum [Problemas conhecidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) para encontrar soluções alternativas para esses problemas e muito mais. 

> [!NOTE]
> Se o modo de desenvolvedor não for instalado corretamente, recomendamos que você para uma solicitação de comentários de arquivos. No app **Hub de comentários** , selecione **Adicionar comentários de novo**e escolha a categoria de **Plataforma do desenvolvedor** e a subcategoria de **Modo do desenvolvedor** . Enviar comentários ajudarão Microsoft resolver o problema que você encontrou.

### <a name="failed-to-locate-the-package"></a>Falha ao localizar o pacote

"O pacote do Modo de Desenvolvedor não foi localizado no Windows Update. Código de erro 0x80004005 Saiba mais"   

Esse erro pode ocorrer devido a um problema de conectividade de rede, configurações corporativas ou o pacote pode estar faltando. 

Para resolver este problema:

1. Certifique-se de que o computador esteja conectado à Internet. 
2. Se você estiver em um computador associado a um domínio, fale com o administrador da rede. O pacote do Modo de Desenvolvedor, como todos os Recursos sob Demanda, é bloqueado por padrão no WSUS. 2.1. Para desbloquear o pacote do Modo de Desenvolvedor nas versões atuais e anteriores, os seguintes KBs devem ser permitidos no WSUS: 4016509, 3180030, 3197985  
3. Verifique se existem atualizações do Windows em Configurações > Atualizações e Segurança > Atualizações do Windows.
4. Verifique se o pacote do Modo de Desenvolvedor do Windows está presente em Configurações > Sistema > Aplicativos e Recursos > Gerenciar recursos opcionais > Adicionar um recurso. Se ele estiver ausente, o Windows não poderá encontrar o pacote correto para o seu computador. 

Depois de realizar qualquer uma das etapas acima, desabilite e habilite novamente o Modo de Desenvolvedor para verificar a correção. 


### <a name="failed-to-install-the-package"></a>Falha ao instalar o pacote

"Falha ao instalar o pacote do Modo de Desenvolvedor. Código de erro 0x80004005  Saiba mais"

Esse erro pode ocorrer devido à incompatibilidade entre a compilação do Windows e o pacote do Modo de Desenvolvedor. 

Para resolver este problema:

1. Verifique se existem atualizações do Windows em Configurações > Atualizações e Segurança > Atualizações do Windows.
2. Reinicialize o computador para garantir que todas as atualizações sejam aplicadas.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Usar políticas de grupo ou chaves do Registro para habilitar um dispositivo

Para a maioria dos desenvolvedores, convém usar o aplicativo Configurações para habilitar seu dispositivo para depuração. Em determinados cenários, como testes automatizados, você pode usar outras maneiras de habilitar seu dispositivo de desktop Windows 10 para desenvolvimento.  Observe que essas etapas não habilitarão o servidor SSH ou permitirão que o dispositivo seja direcionado para depuração e implantação remota. 

Você pode usar gpedit.msc para definir as políticas de grupo para habilitar seu dispositivo, a menos que você tenha o Windows 10 Home. Se você tiver o Windows 10 Home, será necessário usar regedit ou comandos do PowerShell para definir as chaves do Registro diretamente para habilitar seu dispositivo.

**Usar gpedit para habilitar seu dispositivo**

1.  Execute **Gpedit.msc**.
2.  Vá para Política do Computador Local &gt; Configuração do Computador &gt; Modelos Administrativos &gt; Componentes do Windows &gt; App Package Deployment
3.  Para habilitar o sideload, edite as políticas para habilitar:

    -   **Permitir instalação de todos os aplicativos confiáveis**

    - OU -

    Para habilitar o modo de desenvolvedor, edite as políticas para habilitar estas duas opções:

    -   **Permitir instalação de todos os aplicativos confiáveis**
    -   **Permitir o desenvolvimento de aplicativos UWP e a instalação de um ambiente de desenvolvimento integrado (IDE)**

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

Erro: DEP0700: Falha no registro do app.

## <a name="see-also"></a>Consulte também

* [Seu primeiro app](your-first-app.md)
* [Como publicar seu aplicativo UWP](https://developer.microsoft.com/store/publish-apps).
* [Artigos de instruções sobre como desenvolver aplicativos UWP](https://developer.microsoft.com/windows/apps/develop)
* [Amostras de código para desenvolvedores UWP](https://developer.microsoft.com/windows/samples)
* [O que é um aplicativo UWP?](universal-application-platform-guide.md)
* [Inscreva-se para a conta do Windows](sign-up.md)
