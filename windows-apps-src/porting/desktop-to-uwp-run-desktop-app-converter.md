---
author: normesta
Description: Run the Desktop Converter App to package a Windows desktop application (like Win32, WPF, and Windows Forms).
Search.Product: eADQiWindows 10XVcnh
title: Empacotar um app usando o Desktop App Converter (Ponte de Desktop)
ms.author: normesta
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.localizationpriority: medium
ms.openlocfilehash: 8748b68bf4efbcc79d0bba475db32f3a2d7cc933
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2018
ms.locfileid: "3990161"
---
# <a name="package-an-app-using-the-desktop-app-converter-desktop-bridge"></a>Empacotar um app usando o Desktop App Converter (Ponte de Desktop)

[Baixe o Desktop App Converter](https://aka.ms/converter)

Você pode usar o Desktop App Converter (DAC) para trazer seus aplicativos de desktop para a Plataforma Universal do Windows (UWP). Isso inclui aplicativos Win32 e aplicativos que você criou usando o .NET 4.6.1.

![Ícone DAC](images/desktop-to-uwp/dac.png)

Embora o termo "Converter" apareça no nome dessa ferramenta, na verdade ela não converte seu app. Seu app permanece inalterado. Entretanto, essa ferramenta gera um pacote de aplicativo do Windows com um identificador de pacote e a capacidade para chamar uma grande variedade de APIs do WinRT.

Você pode instalar esse pacote usando o cmdlet Add-AppxPackage PowerShell em sua máquina de desenvolvimento.

O conversor executa o instalador da área de trabalho em um ambiente Windows isolado usando uma imagem de base limpa fornecida como parte do download do conversor. Ele captura qualquer registro e E/S do sistema de arquivos feito pelo instalador da área de trabalho e os pacotes como parte da saída.

>[!IMPORTANT]
>A Ponte de Desktop foi introduzida na versão 1607 do Windows 10 e pode ser usada somente em projetos para a Atualização de Aniversário do Windows 10 (10.0; compilação 14393) ou uma versão posterior no Visual Studio.

> [!NOTE]
> Confira <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">essa série</a> de vídeos de curta duração publicados na Microsoft Virtual Academy. Esses vídeos o orientam através de algumas maneiras comuns de usar o Desktop App Converter.

## <a name="the-dac-does-more-than-just-generate-a-package-for-you"></a>O DAC faz mais do que apenas gerar um pacote para você

Aqui estão algumas coisas extras que ele pode fazer por você.

**Atualização do Windows 10 para Criadores**

:heavy_check_mark: Registra automaticamente seus manipuladores de visualização, manipuladores de miniaturas, manipuladores de propriedades, regras de firewall, sinalizadores de URL.

:heavy_check_mark: Registre automaticamente os mapeamentos do tipo de arquivo que permitem aos usuários agrupar arquivos usando a coluna **Tipo** no Explorador de Arquivos.

:heavy_check_mark: Registre os servidores COM públicos.

**Atualização de Aniversário do Windows 10 ou posterior**

:heavy_check_mark: Assine automaticamente o seu pacote para que você possa testar seu aplicativo.

:heavy_check_mark: Valide seu app em relação aos requisitos da Ponte de Desktop e da Microsoft Store.

Para encontrar uma lista completa de opções, veja a seção [Parâmetros](#command-reference) deste guia.

Se você estiver pronto para criar seu pacote, vamos começar.

## <a name="first-prepare-your-application"></a>Primeiro, prepare seu aplicativo

Consulte este guia antes de começar a criar um pacote para seu aplicativo: [Preparar para empacotar um aplicativo (Ponte de Desktop)](desktop-to-uwp-prepare.md).

## <a name="make-sure-that-your-system-can-run-the-converter"></a>Certifique-se de que seu sistema possa executar o conversor

Certifique-se de que seu sistema atenda aos seguintes requisitos:

* Atualização de Aniversário do Windows 10 (10.0.14393.0 e posterior) Edição Pro ou Enterprise.
* Processador 64 bit (x64)
* Virtualização assistida por hardware
* Tradução de endereços de segundo nível (SLAT)
* [Software Development Kit do Windows (SDK do Windows) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375).

## <a name="start-the-desktop-app-converter"></a>Inicie o Desktop App Converter

1. Baixe e instale o [Desktop App Converter](https://aka.ms/converter).

2. Execute o Desktop App Converter como administrador.

    ![Execute o DAC como administrador](images/desktop-to-uwp/run-converter.png)

   Aparece uma janela de console. Você usará essa janela de console para executar comandos.

## <a name="set-a-few-things-up-apps-with-installers-only"></a>Ajuste algumas coisas (aplicativos apenas com instaladores)

Você pode avançar para a próxima seção se seu aplicativo não tiver um instalador.

1. Identifique o número da versão do seu sistema operacional.

   Para fazer isso, digite **winver** na caixa de diálogo **Executar** e escolha o botão **OK**.

   ![winver](images/desktop-to-uwp/winver.png)

   Você encontrará a versão da compilação do Windows usando a caixa de diálogo **Sobre o Windows**.

   ![Versão do Windows 10](images/desktop-to-uwp/win-10-version.png)

2. Baixe a imagem de base apropriada do [Desktop App Converter](https://aka.ms/converterimages).

   Verifique se o número da versão que aparece no nome do arquivo corresponde ao número da versão da compilação do Windows.

   >[!IMPORTANT]
   > Se estiver usando o número da compilação **15063**, e a versão secundária da compilação for igual ou superior a **.483** (por exemplo: **15063.540**), certifique-se de baixar o arquivo **BaseImage-15063-UPDATE.wim**. Se a versão secundária de compilação for inferior a **.483**, baixe o arquivo **BaseImage 15063.wim**. Se você já tiver configurado uma versão incompatível desse arquivo base, é possível corrigi-lo. Essa [postagem de blog](https://blogs.msdn.microsoft.com/appconsult/2017/08/04/desktop-app-converter-fails-on-windows-10-15063-483-and-later-how-to-solve-it/) explica como fazer isso.

3. Coloque o arquivo baixado em qualquer lugar em seu computador, onde você poderá encontrá-lo mais tarde.

4. Na janela do console que apareceu quando você iniciou o Desktop App Converter, execute este comando: ```Set-ExecutionPolicy bypass```.
5. Configure o conversor executando este comando: ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose```.
6. Reinicie seu computador se você for solicitado a fazê-lo.

   As mensagens de status aparecem na janela do console, pois o conversor expande a imagem base. Se você não ver nenhuma mensagem de status, pressione qualquer tecla. Isso pode fazer com que o conteúdo da janela do console seja atualizado.

   ![Mensagens de status na janela do console](images/desktop-to-uwp/bas-image-setup.png)

   Quando a imagem base estiver totalmente expandida, vá para a próxima seção.

## <a name="package-an-app"></a>Empacotar um app

Para empacotar seu aplicativo, execute o comando ``DesktopAppConverter.exe`` na janela do console que abriu quando você iniciou o Desktop App Converter.  

Você especificará o nome do pacote, o editor e o número da versão do aplicativo usando os parâmetros.

> [!NOTE]
> Se você reservou o nome do seu aplicativo na loja do Windows, você pode obter o pacote e os nomes dos editores usando o Centro de Desenvolvimento do Windows. Se você planeja fazer o upload de seu aplicativo em outros sistemas, você pode fornecer seus próprios nomes para isso, desde que o nome do editor que você escolher corresponda ao nome no certificado que você usa para assinar seu aplicativo.

### <a name="a-quick-look-at-command-parameters"></a>Um olhar rápido sobre os parâmetros do comando

Aqui estão os parâmetros necessários.

```CMD
DesktopAppConverter.exe
-Installer <String>
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
```
Você pode ler sobre cada um [aqui](#command-reference).

### <a name="examples"></a>Exemplos

Aqui estão algumas maneiras comuns de empacotar seu aplicativo.

* [Empacotar um app que tenha um arquivo instalador (.msi)](#installer-conversion)
* [Empacotar um app que tenha um arquivo executável de instalação](#setup-conversion)
* [Empacotar um app que não tenha um instalador](#no-installer-conversion)
* [Empacotar um app, assinar o app e prepará-lo para o envio à loja](#optional-parameters)

<a id="installer-conversion" />

#### <a name="package-an-app-that-has-an-installer-msi-file"></a>Empacotar um app que tenha um arquivo instalador (.msi)

Aponte para o arquivo instalador usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.msi -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

> [!IMPORTANT]
> Há dois coisas importantes para ter em mente aqui. Primeiro, certifique-se de que seu instalador esteja localizado em uma pasta independente e que apenas os arquivos relacionados a esse instalador estão na mesma pasta. O conversor copia todo o conteúdo dessa pasta para o ambiente Windows isolado. <br> Em segundo lugar, se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro.  

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-an-MSI-Installer-Kh1UU2WhD_7106218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

Se o instalador incluir instaladores para bibliotecas ou estruturas dependentes, talvez você precise organizar itens de uma forma um pouco diferente. Consulte [Encadeamento de vários instaladores com a Ponte de Desktop](https://blogs.msdn.microsoft.com/appconsult/2017/09/11/chaining-multiple-installers-with-the-desktop-app-converter/).

<a id="setup-conversion" />

#### <a name="package-an-app-that-has-a-setup-executable-file"></a>Empacotar um app que tenha um arquivo executável de instalação

Aponte para o executável de instalação usando o parâmetro ``Installer``.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```
>[!IMPORTANT]
>Se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro.

O parâmetro ``InstallerArguments`` é um parâmetro opcional. No entanto, como o Desktop App Converter precisa do seu instalador para ser executado no modo autônomo, talvez seja necessário usá-lo se o aplicativo precisar de sinalizadores silenciosos para executar silenciosamente. O sinalizador ``/S`` é um sinalizador silencioso muito comum, mas o sinalizador que você usa pode ser diferente dependendo da tecnologia do instalador que você usou para criar o arquivo de configuração.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-an-Application-That-Has-a-Setup-exe-Installer-amWit2WhD_5306218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="no-installer-conversion" />

#### <a name="package-an-app-that-doesnt-have-an-installer"></a>Empacotar um app que não tenha um instalador

Neste exemplo, use o parâmetro ``Installer`` para apontar para a pasta raiz dos arquivos do seu aplicativo.

Use o parâmetro `AppExecutable` para apontar para o arquivo executável de seus aplicativos.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1
```

>[!IMPORTANT]
>Se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro.

**Vídeo**

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Convert-a-No-Installer-Application-agAXF2WhD_3506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

<a id="optional-parameters" />

#### <a name="package-an-app-sign-the-app-and-run-validation-checks-on-the-package"></a>Empacotar um app, assinar o app e executar verificações de validação no pacote

Este exemplo é semelhante ao primeiro, exceto que mostra como você pode assinar seu aplicativo para testes locais e, em seguida, validar seu aplicativo para os requisitos da Ponte de Desktop e da Microsoft Store.

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose -Verify
```
>[!IMPORTANT]
>Se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro.

O parâmetro ``Sign`` gera um certificado e, em seguida, assina seu aplicativo com ele. Para executar seu aplicativo, você terá que instalar esse certificado gerado. Para saber como, consulte a seção [Executar o aplicativo empacotado](#run-app) deste guia.

Você pode validar seu aplicativo usando o parâmetro ``Verify``.

### <a name="a-quick-look-at-optional-parameters"></a>Uma análise rápida dos parâmetros opcionais

Os parâmetros ``Sign`` e ``Verify`` são opcionais. Existem muitos outros parâmetros opcionais.  Aqui estão alguns dos parâmetros opcionais mais comumente usados.

```CMD
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]
```
Você pode ler sobre todos eles na próxima seção.

<a id="command-reference" />

### <a name="parameter-reference"></a>Parâmetro Referência

Aqui está a lista completa de parâmetros (organizados por categoria) que você pode usar quando você executa o Desktop App Converter.

* [Configuração](#setup-params)
* [Conversão](#conversion-params)
* [Identidade do pacote](#identity-params)
* [Manifesto do pacote](#manifest-params)
* [Limpeza](#cleanup-params)
* [Arquitetura](#architecture-params)
* [Diversos](#other-params)

Você também pode exibir toda a lista executando o comando ``Get-Help`` na janela do console do aplicativo.   

||||
|-------------|-----------|-------------|
|<a id="setup-params" /> <strong>Parâmetros de configuração</strong>  ||
|-Setup [&lt;SwitchParameter&gt;] |Necessário |Execute o DesktopAppConverter no modo de configuração. O modo de configuração oferece suporte à expansão uma imagem base fornecida.|
|-BaseImage &lt;Cadeia de Caracteres&gt; | Necessário |Caminho completo para uma imagem base não expandida. Este parâmetro é necessário se -Setup for especificado.|
| -LogFile &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um arquivo de registro. Se for omitido, será criada uma localização temporária do arquivo de registro.|
|-NatSubnetPrefix &lt;Cadeia de Caracteres&gt; |Opcional |Valor de prefixo a ser usado para a instância Nat. Normalmente, você desejaria alterar isso somente se seu computador host fosse anexado à mesma faixa de sub-rede do NetNat do conversor. Você pode consultar o conversor atual NetNat config usando o cmdlet **Get-NetNat**. |
|-NoRestart [&lt;SwitchParameter&gt;] |Necessário |Não solicite a reinicialização ao executar a instalação (é necessário reiniciar para ativar o recurso do contêiner). |
|<a id="conversion-params" /> <strong>Parâmetros de conversão</strong>|||
|-AppInstallPath &lt;Cadeia de Caracteres&gt;  |Opcional |O caminho completo para a pasta raiz do seu aplicativo para os arquivos instalados se ele foi instalado (por exemplo, "C:\Arquivos de Programas (x86)\ MyApp").|
|-Destination &lt;Cadeia de Caracteres&gt; |Necessário |O destino desejado para a saída appx do conversor - o DesktopAppConverter pode criar esta localização se ela ainda não existir.|
|-Installer &lt;Cadeia de Caracteres&gt; |Necessário |O caminho para o instalador para seu aplicativo - deve ser capaz de executar sem supervisão / silenciosamente. Conversão sem instalador, este é o caminho para o diretório raiz dos arquivos do seu aplicativo. |
|-InstallerArguments &lt;Cadeia de Caracteres&gt; |Opcional |Uma lista separada por vírgulas ou uma série de argumentos para forçar o instalador a executar sem supervisão/silenciosamente. Este parâmetro é opcional se o instalador for um msi. Para obter um registro do seu instalador, forneça o argumento de registro para o instalador aqui e use o caminho &lt;log_folder&gt;, que é um token que o conversor substitui com o caminho apropriado. <br><br>**OBSERVAÇÃO**: Os sinalizadores sem supervisão/silenciosos e os argumentos de registro variarão entre as tecnologias instaladoras. <br><br>Um exemplo de uso para este parâmetro: -InstallerArguments "/silent /log &lt;log_folder&gt;\install.log" Outro exemplo que não gera um arquivo de registro pode ser ```-InstallerArguments "/quiet", "/norestart"``` Mais uma vez, você deve literalmente dirigir todos os registros para o caminho do token &lt;log_folder&gt; se quiser que o conversor capture e coloque na pasta de registro final.|
|-InstallerValidExitCodes &lt;Int32&gt; |Opcional |Uma lista separada por vírgulas de códigos de saída que indicam o seu instalador executado com sucesso (por exemplo: 0, 1234, 5678).  Por padrão, isso é 0 para non-msi e 0, 1641, 3010 para msi.|
|-MakeAppx [&lt;SwitchParameter&gt;]  |Opcional |Um interruptor que, quando presente, informa esse script para chamar MakeAppx na saída. |
|-MakeMSIX [&lt;SwitchParameter&gt;]  |Opcional |Um botão que, quando presente, informa este script para a saída como um pacote MSIX do pacote. |
|<a id="identity-params" /><strong>Parâmetros de identidade do pacote</strong>||
|-PackageName &lt;Cadeia de Caracteres&gt; |Necessário |O nome do seu pacote de aplicativos Universais do Windows. Se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro. |
|-Publisher &lt;Cadeia de Caracteres&gt; |Necessário |O editor do pacote da sua aplicativo Universal Windows |
|-Version &lt;Versão&gt; |Necessário |O número da versão do pacote da sua aplicativo Universal Windows |
|<a id="manifest-params" /><strong>Parâmetros do manifesto do pacote</strong>||
|-AppExecutable &lt;Cadeia de Caracteres&gt; |Opcional |O nome do executável principal do seu aplicativo (por exemplo, "MyApp.exe"). Esse parâmetro é necessário para uma conversão sem instalador. |
|-AppFileTypes &lt;Cadeia de Caracteres&gt;|Opcional |Uma lista separada por vírgulas de tipos de arquivos com a qual o aplicativo será associado. Exemplo de uso: - AppFileTypes "'.md', '.markdown'".|
|-AppId &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir a Id do Aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. Em muitos casos, usar o *PackageName* funciona bem. No entanto, se o Centro de Desenvolvimento atribuir uma identidade ao pacote que começa com um número, transmita também o parâmetro <i>-AppId</i> e use apenas o sufixo da cadeia de caracteres (após o separador decimal) como o valor desse parâmetro. |
|-AppDisplayName &lt;Cadeia de Caracteres&gt;  |Opcional |Especifica um valor para definir o Nome da exibição do aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-AppDescription &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir a Descrição do Aplicativo no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|-PackageDisplayName &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir o nome da exibição do pacote no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|-PackagePublisherDisplayName &lt;Cadeia de Caracteres&gt; |Opcional |Especifica um valor para definir o nome de exibição do publicador de pacotes no manifesto do pacote do aplicativo do Windows. Se não for especificado, ele será definido como o valor passado para *Publisher*. |
|<a id="cleanup-params" /><strong>Parâmetros de limpeza</strong>|||
|-Cleanup [&lt;Opção&gt;] |Necessário |Executa a limpeza dos artefatos do DesktopAppConverter. Existem 3 opções válidas para o modo de limpeza. |
|-Cleanup All | |Exclui todas as imagens de base expandidas, remove todos os arquivos temporários do conversor, remove a rede do contêiner e desativa o recurso opcional do Windows, Recipientes. |
|-Cleanup WorkDirectory |Necessário |Remove todos os arquivos do conversor temporário. |
|-Cleanup ExpandedImage |Necessário |Exclui todas as imagens de base expandidas instaladas em sua máquina host. |
|<a id="architecture-params" /><strong>Parâmetros da arquitetura do pacote</strong>|||
|-PackageArch &lt;Cadeia de Caracteres&gt; |Necessário |Gera um pacote com a arquitetura especificada. As opções válidas são 'x86' ou 'x64'; por exemplo, -PackageArch x86. Este parâmetro é opcional. Se não for especificado, o DesktopAppConverter tentará detectar automaticamente a arquitetura do pacote. Se a auto-detecção falhar, será padrão para o pacote x64. |
|<a id="other-params" /><strong>Parâmetros diversos</strong>|||
|-ExpandedBaseImage &lt;Cadeia de Caracteres&gt;  |Opcional |Caminho completo para uma imagem base já expandida.|
|-LogFile &lt;String&gt;  |Opcional |Especifica um arquivo de registro. Se for omitido, será criada uma localização temporária do arquivo de registro. |
| -Sign [&lt;SwitchParameter&gt;] |Opcional |Avisa este script para assinar o pacote de aplicativos do Windows de saída usando um certificado gerado para fins de teste. Este interruptor deve estar presente ao lado do interruptor ```-MakeAppx```. |
|&lt;Parâmetros comuns&gt; |Necessário |Esse cmdlet oferece suporte aos parâmetros comuns: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* e *OutVariable*. Para mais informações, consulte [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |
| -Verify [&lt;SwitchParameter&gt;] |Opcional |Um interruptor que, quando presente, informa ao DAC para validar o pacote de apps em relação aos requisitos da Ponte de Desktop e da Microsoft Store. O resultado é um relatório de validação "VerifyReport.xml", que é melhor visualizado em um navegador. Este interruptor deve estar presente ao lado do interruptor `-MakeAppx`. |
|-PublishComRegistrations| Opcional| Digitaliza todos os registos COM públicos feitos pelo seu instalador e publica os válidos no seu manifesto. Use este sinalizador somente se desejar disponibilizar esses registros para outros aplicativos. Você não precisa usar este sinalizador se esses registros forem usados apenas pelo seu aplicativo. <br><br>Examine [este artigo](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#lDg5gSFxJ2TDlpC6.97) para certificar-se de que seus registros COM se comportam como esperado após o empacotamento do seu app.

<a id="run-app" />

## <a name="run-the-packaged-app"></a>Execute o app empacotado

Há dois meios de executar seu aplicativo.

Uma maneira é abrir um prompt de comando do PowerShell e, em seguida, digitar este comando ```Add-AppxPackage –Register AppxManifest.xml```. Provavelmente é a maneira mais fácil de executar seu aplicativo porque você não precisa assinar.

Outra maneira é assinar seu aplicativo com um certificado. Se você usar o parâmetro ```sign``` o Desktop App Converter irá gerar um para você e, em seguida, assinar seu aplicativo com ele. O arquivo recebe o nome de **auto-generated.cer**, e você pode encontrá-lo na pasta raiz do seu app empacotado.

Siga estas etapas para instalar o certificado gerado e, em seguida, execute seu aplicativo.

1. Clique duas vezes no arquivo **auto-generated.cer** para instalar o certificado.

   ![arquivo de certificado gerado](images/desktop-to-uwp/generated-cert-file.png)

   > [!NOTE]
   > Se for solicitado uma senha a você, use a senha padrão "123456".

2. Na caixa de diálogo **Certificado**, clique no botão **Instalar certificado**.
3. No **Assistente de Importação de Certificados**, instale o certificado na **Máquina Local**, e coloque o certificado no repositório de certificados **Pessoas de confiança**.

   ![Repositório de pessoas de confiança](images/desktop-to-uwp/trusted-people-store.png)

5. Na pasta raiz de seu app empacotado, clique duas vezes no arquivo do pacote do aplicativo do Windows.

   ![Arquivo do pacote de aplicativo do Windows](images/desktop-to-uwp/windows-app-package.png)

6. Instale o aplicativo, pressionando o botão **Instalar**.

   ![Botão de instalação](images/desktop-to-uwp/install.png)


## <a name="modify-the-packaged-app"></a>Modificar o app empacotado

Você provavelmente irá fazer alterações em seu app empacotado para endereçar bugs, adicionar recursos visuais ou aprimorar seu app com experiências modernas, como blocos dinâmicos.

Depois de fazer suas alterações, você não precisa executar o conversor novamente. Na maioria dos casos, você pode reempacotar seu aplicativo usando a ferramenta MakeAppx e o arquivo appxmanifest.xml que o DAC gera para o aplicativo. Consulte [Gerar um pacote de aplicativo do Windows](desktop-to-uwp-manual-conversion.md#make-appx).

* Se você modificar qualquer um dos ativos visuais do aplicativo, gere um novo arquivo de Índice de recurso do pacote e execute a ferramenta MakeAppx para gerar um novo pacote. Consulte [Gerar um arquivo PRI (Índice de Recurso do Pacote)](desktop-to-uwp-manual-conversion.md#make-pri).

* Se você deseja adicionar ícone ou blocos que aparecem na barra de tarefas do Windows, na visão de tarefas, em LT+TAB, no Assistente de Ajuste e no canto inferior direito dos blocos em Iniciar, consulte [(Opcional) Adicionar ativos sem fundo com base em destino](desktop-to-uwp-manual-conversion.md#target-based-assets).

> [!NOTE]
> Se você fizer alterações nas configurações de registro que o instalador faz, você terá que executar o Conversor de aplicativos do desktop novamente para escolher essas alterações.

**Vídeos**

|Modificar e reempacotar a saída |Demonstração: Modificar e reempacotar a saída|
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Modifying-and-Repackaging-Output-from-Desktop-App-Converter-OwpAJ3WhD_6706218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Modify-Output-from-Desktop-App-Converter-gEnsa3WhD_8606218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

As duas seções a seguir descrevem algumas correções opcionais para o app empacotado que você pode considerar.

### <a name="delete-unnecessary-files-and-registry-keys"></a>Exclua arquivos desnecessários e chaves de registro

O Desktop App Converter usa uma abordagem muito conservadora para filtrar os arquivos e o ruído do sistema no recipiente.

Se você quiser, você pode verificar a pasta VFS e excluir quaisquer arquivos que o seu instalador não necessita.  Você também pode rever o conteúdo do Reg.dat e excluir as chaves que não são instaladas/necessárias pelo aplicativo.

### <a name="fix-corrupted-pe-headers"></a>Corrigir cabeçalhos PE corrompidos

Durante o processo de conversão, o DesktopAppConverter executa automaticamente o PEHeaderCertFixTool para corrigir todos os cabeçalhos PE corrompidos. No entanto, você também pode executar o PEHeaderCertFixTool em um pacote de aplicativos UWP do Windows, arquivos soltos ou um binário específico. Veja um exemplo.

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="telemetry-from-desktop-app-converter"></a>Telemetria do Desktop App Converter

O conversor de aplicativos da área de trabalho pode coletar informações sobre você e o seu uso do software e enviar essas informações para a Microsoft. Você pode saber mais sobre a coleta e o uso de dados da Microsoft na documentação do produto e na [Política de Privacidade da Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Você concorda em cumprir todas as provisões aplicáveis da Política de Privacidade da Microsoft.

Por padrão, a telemetria será habilitada para o conversor de aplicativos da área de trabalho. Adicione a seguinte chave do registro para definir a telemetria com uma configuração desejada:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Adicione ou edite o valor *DisableTelemetry* usando um DWORD definido como 1.
+ Para habilitar a telemetria, remova a chave ou ajuste o valor para 0.

### <a name="language-support"></a>Suporte ao idioma

O Desktop App Converter não dá suporte a Unicode; portanto, nenhum caractere chinês ou caractere não-ASCII pode ser usado com a ferramenta.

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Você também pode consultar [esta](desktop-to-uwp-known-issues.md#app-converter) lista de problemas conhecidos.

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Execute seu aplicativo/encontre e corrija os problemas**

Consulte [Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-debug.md)

**Distribua seu aplicativo**

Consulte [Distribuir um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-distribute.md)
