---
author: martinekuan ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD title: Habilitar seu dispositivo para descrição de desenvolvimento: existe uma abordagem diferente para o desenvolvimento de dispositivos Windows 10.
keywords: Introdução às palavras-chave: palavras-chave de licença de desenvolvedor: Visual Studio, palavras-chave de licença de desenvolvedor: habilitar dispositivo
---
# Habilitar seu dispositivo para desenvolvimento

Há uma abordagem diferente para desenvolvimento para dispositivos Windows 10. Não é mais necessária uma licença de desenvolvedor para cada dispositivo que você deseja usar para desenvolver, instalar ou testar seu aplicativo. Você simplesmente habilita um dispositivo uma vez para essas tarefas nas configurações do dispositivo. Isso é tudo. Não será mais necessário renovar suas licenças de desenvolvedor a cada 30 ou 90 dias!

Se ainda estiver usando um dispositivo Windows 8.1 para desenvolver ou testar seus aplicativos com o Microsoft Visual Studio 2013 ou o Microsoft Visual Studio 2015, você precisará [obter uma licença de desenvolvedor](https://msdn.microsoft.com/library/windows/apps/Hh974578) ou [registrar seu Windows Phone](https://msdn.microsoft.com/library/windows/apps/Dn614128).

## Usar recursos de desenvolvedor

### Desenvolver seu aplicativo com o Microsoft Visual Studio

Se você usar o Microsoft Visual Studio em um dispositivo Windows 10 e abrir uma solução para um aplicativo do Windows 8.1 ou Windows 10, será solicitado que você habilite seu dispositivo com essa caixa de diálogo. Seu dispositivo deve estar habilitado para que você possa usar os designers e depurar o aplicativo.

![Habilitar a caixa de diálogo do modo de desenvolvedor exibida no Visual Studio](images/latestenabledialog.png)

Quando você vir essa caixa de diálogo, clique em **configurações para desenvolvedores** para ir diretamente à página **Atualização e segurança**, conforme mostrado abaixo. Ou clique em **OK** e siga as etapas abaixo para habilitar seu dispositivo Windows 10 para desenvolvimento.

### Habilitar seus dispositivos Windows 10

Para o Windows 10, você escolhe quais recursos de desenvolvedor deseja habilitar no dispositivo. Isso inclui quaisquer dispositivos: desktops, tablets e telefones Windows 10. Você pode habilitar um dispositivo para desenvolvimento ou apenas para sideload.

-   *Sideload* significa instalar e executar ou testar um aplicativo que não foi certificado pela Windows Store. Por exemplo, um aplicativo que é interno somente para sua empresa.
-   *Modo de desenvolvedor* permite o sideload de aplicativos e também executar aplicativos no Visual Studio no modo de depuração.

**Observação**  Se você fizer o sideload de aplicativos, ainda deverá instalar aplicativos somente de fontes confiáveis. Ao instalar um aplicativo de sideload que não foi certificado pela Windows Store, você concorda que obteve todos os direitos necessários para fazer sideload do aplicativo e que é o único responsável por qualquer dano resultante da instalação e da execução do aplicativo. Veja a seção Windows &gt; Windows Store desta [política de privacidade](http://go.microsoft.com/fwlink/?LinkId=521839).

**Para usar recursos de desenvolvedor**

1.  No dispositivo que você deseja habilitar, vá para **Configurações**. Escolha **Atualização e segurança** e, em seguida, **Para desenvolvedores**.
2.  Escolha o nível de acesso necessário. Para obter informações mais detalhadas sobre as opções, consulte [Quais configurações devo escolher: sideload de aplicativos ou modo de desenvolvedor?](#WhichSettings)
3.  Leia o aviso de isenção da configuração escolhida e clique em **Sim** para aceitar a alteração.

Aqui está a página de configurações na família de dispositivos de desktop.

![Vá para Configurações, escolha Atualização e segurança e Para desenvolvedores para exibir suas opções](images/devmode-pc-options.png)

Aqui está a página de configurações na família de dispositivos móveis.

![Nas Configurações do telefone, escolha Atualização e segurança](images/devmode-mob.png)

### Quais configurações devo escolher: sideload de aplicativos ou modo de desenvolvedor?

Por padrão, você só pode instalar aplicativos da Plataforma Universal do Windows (UWP) da Windows Store. A alteração dessas configurações para usar recursos de desenvolvedor pode alterar o nível de segurança do seu dispositivo. Você não deve instalar aplicativos de fontes não verificadas.

**Fazer o sideload de aplicativos**

A configuração Sideload de aplicativos normalmente é usada por empresas ou escolas que precisam instalar aplicativos personalizados em dispositivos gerenciados sem passar pela Windows Store. Nesse caso, é comum para a organização impor uma política que desabilita a configuração *Aplicativos da Windows Store*, como mostrado anteriormente na imagem da página de configurações do telefone. A organização também fornece o certificado necessário e o local de instalação para o sideload de aplicativos. Para saber mais, veja os artigos do TechNet [Sideload de aplicativos no Windows 10](https://technet.microsoft.com/library/mt269549.aspx) e [Introdução à implantação de aplicativo no Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Informações específicas à família de dispositivos

-   Na família de dispositivo de desktop: você pode instalar um pacote de aplicativo (.appx) e qualquer certificado que seja necessário para executar o aplicativo executando o script do Windows PowerShell que é criado com o pacote ("Add-AppDevPackage.ps1").

-   Na família de dispositivos móveis: se o certificado necessário já estiver instalado, você poderá tocar no arquivo para instalar qualquer .appx enviado a você por email ou em um cartão SD.

O **sideload de aplicativos** é uma opção mais segura que o modo de desenvolvedor, pois você não pode instalar aplicativos no dispositivo sem um certificado confiável.

**Modo de desenvolvedor**

Além de sideload, a configuração Modo de desenvolvedor permite a depuração e outras opções de implantação. Ela substitui a exigência do Windows 8.1 de uma licença de desenvolvedor.

Informações específicas à família de dispositivos

-   Na família de dispositivos de desktop:

    Habilite o Modo de desenvolvedor para desenvolver e depurar aplicativos no Visual Studio. Conforme afirmado anteriormente, você receberá um aviso no Visual Studio se o Modo de desenvolvedor não estiver habilitado.

-   Na família de dispositivos móveis:

    Habilite o modo de desenvolvedor para implantar aplicativos no Visual Studio e depurá-los no dispositivo.

    Você pode tocar no arquivo para instalar qualquer .appx enviado a você por email ou em um cartão SD. Não instale aplicativos de fontes não verificadas.

**Dica**  
Há várias ferramentas que você pode usar para implantar um aplicativo de um computador Windows 10 para um dispositivo móvel Windows 10. Ambos os dispositivos devem estar conectados à mesma sub-rede da rede por uma conexão com ou sem fio, ou eles devem estar conectados por USB. Qualquer uma das maneiras listadas instala apenas o pacote do aplicativo (.appx); elas não instalam certificados.

-   Use a ferramenta de implantação de aplicativos do Windows 10 (WinAppDeployCmd). Saiba mais sobre [a ferramenta WinAppDeployCmd](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   A partir do Windows 10, versão 1511, você pode usar o [Device Portal](#device_portal) para implantar do seu navegador para um dispositivo móvel executando o Windows 10, versão 1511 ou posterior. Use a página **Aplicativos** no Device Portal (&lt;IP&gt;/appmanager.md) para carregar um pacote de aplicativo (.appx) e instalá-lo no dispositivo.

 

### Definir políticas de grupo ou chaves do Registro

Você também pode usar políticas de grupo ou chaves do Registro como uma maneira alternativa de habilitar seu dispositivo de desktop Windows 10 para desenvolvimento.

**Na família de dispositivos de desktop**

Use gpedit.msc para definir as políticas de grupo para habilitar seu dispositivo, a menos que você tenha o Windows 10 Home. Se você tiver o Windows 10 Home, será necessário usar regedit ou comandos do PowerShell para definir as chaves do Registro diretamente para habilitar seu dispositivo.

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

## Recursos do modo de desenvolvedor

Para cada família de dispositivos, recursos de desenvolvedor adicionais podem estar disponíveis. Esses recursos estão disponível apenas quando o **Modo de desenvolvedor** está habilitado no dispositivo e podem variar dependendo da sua versão do sistema operacional.

Esta imagem mostra os recursos de desenvolvedor para a família de dispositivos móveis no Windows 10, versão 1511.

![Opções do Modo de desenvolvedor para dispositivos móveis](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"></span>Descoberta de dispositivo e o Device Portal

Para saber mais sobre a descoberta de dispositivo e o Device Portal, consulte [Visão geral do Windows Device Portal](../debug-test-perf/device-portal.md).

Para obter instruções de instalação específicas do dispositivo, consulte:
- [Device Portal para HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal para IoT](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [Device Portal para celulares](../debug-test-perf/device-portal-mobile.md)
- [Device Portal para Xbox](../debug-test-perf/device-portal-xbox.md)

### Relatório de erros

Defina esse valor para especificar quantos despejos de memória são salvos no seu telefone.

A coleta de despejos de memória no telefone oferece acesso instantâneo a informações importantes da falha logo depois que a falha ocorre. Os despejos são coletados somente para aplicativos assinados pelo desenvolvedor. Você pode encontrar os despejos no armazenamento do telefone, na pasta Documents\\Debug. Para saber mais sobre arquivos de despejo, consulte [usando arquivos de despejo](https://msdn.microsoft.com/library/d5zhxt22.aspx).

## Atualizando seu dispositivo do Windows 8.1 para Windows 10

Ao criar ou fazer sideload de aplicativos em seu dispositivo Windows 8.1, você precisa instalar uma licença de desenvolvedor. Se você atualizar seu dispositivo do Windows 8.1 para o Windows 10, essas informações serão mantidas. Execute o comando a seguir para remover essas informações do seu dispositivo Windows 10 atualizado. Esta etapa não será necessária se você atualizar diretamente do Windows 8.1 para o Windows 10, versão 1511 ou posterior.

**Para cancelar o registro de uma licença de desenvolvedor**

1.  Execute o PowerShell com privilégios de administrador.
2.  Execute este comando: **unregister-windowsdeveloperlicense**.

Depois disso, você precisa habilitar seu dispositivo para o desenvolvimento, conforme descrito neste tópico, para que possa continuar a desenvolver nele. Se não fizer isso, poderá ocorrer um erro quando você depurar seu aplicativo ou tentar criar um pacote para ele. Aqui está um exemplo desse erro:

Erro: DEP0700: Falha no registro do aplicativo.




<!--HONumber=May16_HO2-->


