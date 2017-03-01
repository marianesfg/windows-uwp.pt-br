---
author: awkoren
Description: "Execute o Desktop Converter App para converter um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms) em um aplicativo UWP (Plataforma Universal do Windows)."
Search.Product: eADQiWindows 10XVcnh
title: Desktop App Converter
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 046a3b474aa281b1b09002a922dc2acdb7833599
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-app-converter"></a>Desktop App Converter

[Baixe o Desktop App Converter](https://aka.ms/converter)

O Desktop App Converter (DAC) é uma ferramenta que permite converter seus aplicativos de área de trabalho existentes escritos para .NET 4.6.1 ou Win32 em UWP (Plataforma Universal do Windows). Você pode executar seus instaladores da área de trabalho por meio do conversor em um modo autônomo (silencioso) e obter um pacote de AppX que pode instalar usando o cmdlet do PowerShell Add-AppxPackage no computador de desenvolvimento.

O Desktop App Converter está disponível agora na [Windows Store](https://aka.ms/converter).

O conversor executa o instalador da área de trabalho em um ambiente isolado do Windows usando uma imagem de base limpa fornecida como parte do download do conversor. Ele captura qualquer E/S do registro e do sistema de arquivos feita pelo instalador da área de trabalho e a empacota como parte da saída. O conversor gera um AppX com identificador de pacote e a capacidade para chamar uma grande variedade de APIs do WinRT.

## <a name="whats-new"></a>Novidades

A versão mais recente do DAC é v1.0.6.0. Novo nesta atualização:

* Extração de ícone: O DAC usa ícones em seu aplicativo da área de trabalho para gerar ativos visuais usados por seu pacote de aplicativo convertido.
* Limpeza aprimorada de imagens de base expandidas, arquivos temporários e rede de contêiner e funcionalidade.
* Várias correções de bugs. 

## <a name="system-requirements"></a>Requisitos do sistema

### <a name="operating-system"></a>Sistema operacional

+ Atualização de Aniversário do Windows 10 (10.0.14393.0 e posterior) Pro ou Enterprise Edition.

### <a name="hardware-configuration"></a>Configuração do hardware

+ Processador de 64 bits (x64)
+ Virtualização assistida por hardware
+ Conversão de Endereços de Segundo Nível (SLAT)

### <a name="required-resources"></a>Recursos necessários

+ [Software Development Kit do Windows (SDK do Windows) para Windows 10](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>Configurar o Desktop App Converter

O Desktop App Converter depende dos recursos mais recentes do Windows 10. Certifique-se de que você esteja usando a Atualização de Aniversário do Windows 10 (14393.0) ou builds posteriores.

1.    Baixe o [DesktopAppConverter na Windows Store](https://aka.ms/converter) e o [arquivo .wim de imagem base correspondente à compilação](https://aka.ms/converterimages).  
2.    Execute o DesktopAppConverter como administrador. Você pode fazer isso no menu Iniciar clicando com o botão direito do mouse no bloco e selecionando *Executar como administrador* em *Mais* ou, na barra de tarefas, clicando com o botão direito do mouse no bloco, clicando uma segunda vez no nome do aplicativo exibido e selecionando *Executar como administrador*.
3.    Na janela de console do aplicativo, execute ```Set-ExecutionPolicy bypass```.
4.    Configure o conversor executando ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` na janela de console do aplicativo.
5.    Se a execução do comando anterior exigir uma reinicialização, reinicie o computador.

## <a name="run-the-desktop-app-converter"></a>Executar o Desktop App Converter

+ **Download da Loja**: use ```DesktopAppConverter.exe``` para executar o conversor.

### <a name="usage"></a>Uso

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
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

### <a name="example"></a>Exemplo

O exemplo a seguir mostra como converter um aplicativo da área de trabalho denominado *MyApp* de *&lt;publisher_name&gt;* em um pacote UWP (AppX).

```CMD
DesktopAppConverter.exe -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## <a name="deploy-your-converted-appx"></a>Implantar seu AppX convertido

Use o cmdlet [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) no PowerShell para implantar um pacote de aplicativo assinado (.appx) em uma conta de usuário. 

Você pode usar o sinalizador ```-Sign``` no Conversor de Aplicativos de Área de Trabalho (v0.1.24) para assinar de forma automática seu aplicativo convertido. Como alternativa, consulte [Assinar seu aplicativo da área de trabalho convertido](desktop-to-uwp-signing.md) para aprender a autoassinar pacotes AppX.

Você também pode utilizar o parâmetro ```-Register``` do cmdlet Add-AppXPackage PowerShell para instalar de uma pasta de arquivos não empacotados durante o processo de desenvolvimento. 

Para obter mais informações sobre como implementar e depurar seu aplicativo convertido, consulte [Implementar e depurar seu aplicativo UWP convertido](desktop-to-uwp-deploy-and-debug.md). 

## <a name="sign-your-appx-package"></a>Assinar seu pacote .AppX

O cmdlet Add-AppxPackage requer que o pacote do aplicativo (.appx) que está sendo implantado seja assinado. Use o sinalizador ```-Sign``` como parte da linha de comando do conversor ou SignTool.exe, que é fornecido no SDK do Microsoft Windows 10, para assinar o pacote .appx.

Para obter detalhes adicionais sobre como assinar o pacote .appx, consulte [Assinar aplicativo da área de trabalho convertido](desktop-to-uwp-signing.md). 

Observação: se você tentar assinar um pacote usando o certificado gerado automaticamente, deverá usar a senha padrão "123456".

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>Modificar a pasta VFS e o Hive do Registro (opcional)

O Desktop App Converter adota uma abordagem muito conservadora para filtrar arquivos e ruído do sistema no contêiner.  Isso não é necessário, mas após a conversão, você pode:

1. Examinar a pasta VFS e excluir todos os arquivos que não são necessários para o instalador.
2. Examinar o conteúdo do Reg.dat e excluir as chaves que não são instaladas/necessárias no app.

Se você fizer alterações em seu app convertido (incluindo as mencionadas acima), não precisa executar o Converter novamente, basta reempacotar manualmente seu app usando a ferramenta MakeAppx e o arquivo appxmanifest.xml que o DAC gera para seu app. Para obter ajuda, consulte [Converter manualmente o app em UWP usando a ponte da área de trabalho](desktop-to-uwp-manual-conversion.md).

## <a name="caveats"></a>Advertências

1. A compilação do Windows 10 no computador host deve corresponder à imagem base que você obteve como parte do download do Desktop App Converter.  
2. Verifique se o instalador da área de trabalho está em um diretório independente, pois o conversor copia todo o conteúdo do diretório para o ambiente isolado do Windows.  
3. No momento, o conversor de aplicativos da área de trabalho aceita a execução do processo de conversão apenas em um sistema operacional de 64 bits. Você pode implantar os pacotes.appx convertidos apenas em um sistema operacional de 64 bits (x64).  
4. O conversor de aplicativos da área de trabalho requer que o instalador da área de trabalho seja executado no modo autônomo. Certifique-se de passar o sinalizador silencioso do instalador para o conversor usando o parâmetro *- InstallerArguments*.
5. Publicar assemblies SxS Fusion públicos não funcionará. Durante a instalação, um aplicativo pode publicar assemblies Fusion públicos lado a lado, acessíveis para qualquer outro processo. Durante a criação do contexto de ativação do processo, esses assemblies são recuperados por um processo do sistema denominado CSRSS.exe. Quando isso é feito em um processo convertido, a criação do contexto de ativação e o carregamento do módulo desses assemblies falhará. Assemblies de caixa de entrada, como ComCtl, são fornecidos com o sistema operacional, portanto, assumir uma dependência deles é seguro. Os assemblies SxS Fusion são registrados nos seguintes locais:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de arquivos: %windir%\\SideBySide

## <a name="known-issues"></a>Problemas conhecidos

* Estamos investigando os seguintes erros que ocorrem em algumas versões do sistema operacional:
    
    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```
    
    Se você encontrar qualquer um desses erros, verifique se está usando uma imagem base válida do [centro de download](https://aka.ms/converterimages). Se você estiver usando um arquivo .wim válido, envie-nos seus logs pelo email converter@microsoft.com para nos ajudar a investigar. 

* Se você receber uma versão de pré-lançamento do Windows Insider em um computador de desenvolvedor que anteriormente tinha o Desktop App Converter instalado, poderá receber o erro `New-ContainerNetwork: The object already exists` ao configurar a nova imagem de base. Como alternativa, execute o comando `Netsh int ipv4 reset` em um prompt de comando com privilégios elevados e reinicie sua máquina. 

* Ocorrerá uma falha na instalação de um aplicativo .NET compilado com a opção de compilação "AnyCPU" se o executável principal ou qualquer uma das dependências forem colocados em "Arquivos de Programas" ou "Windows\System32". Como alternativa, use o instalador de área de trabalho específico da sua arquitetura (32 bits ou 64 bits) para gerar com êxito um pacote AppX.

## <a name="telemetry-from-desktop-app-converter"></a>Telemetria do conversor de aplicativos da área de trabalho

O conversor de aplicativos da área de trabalho pode coletar informações sobre você e o seu uso do software e enviar essas informações para a Microsoft. Você pode saber mais sobre a coleta e o uso de dados da Microsoft na documentação do produto e na [Política de Privacidade da Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Você concorda em cumprir todas as provisões aplicáveis da Política de Privacidade da Microsoft.

Por padrão, a telemetria será habilitada para o conversor de aplicativos da área de trabalho. Adicione a seguinte chave do registro para definir a telemetria com uma configuração desejada:  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Adicione ou edite o valor *DisableTelemetry* usando um DWORD definido como 1.
+ Para habilitar telemetria, remova a chave ou defina o valor como 0.

## <a name="desktop-app-converter-usage"></a>Uso do conversor de aplicativos da área de trabalho

Aqui está uma lista de parâmetros do Desktop App Converter. Você também pode exibir essa lista executando:   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>Parâmetros de configuração  

|Parâmetro|Descrição|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Executa DesktopAppConverter no modo de instalação. O modo de instalação aceita a expansão de uma imagem de base fornecida.|
|```-BaseImage <String>``` | Caminho completo para uma imagem de base não expandida. Este parâmetro será necessário se -Setup for especificado.|
|```-LogFile <String>``` [opcional] | Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado.|
|```-NatSubnetPrefix <String>``` [opcional] | Valor de prefixo a ser usado para a instância do Nat. Normalmente, você desejaria alterar isso somente se seu computador host fosse anexado à mesma faixa de sub-rede do NetNat do conversor. Você pode consultar a configuração atual do NetNat do conversor usando o cmdlet **Get-NetNat**. |
|```-NoRestart [<SwitchParameter>]``` | Não solicite a reinicialização ao executar a instalação (é necessário reiniciar para habilitar o recurso de contêiner). |

### <a name="conversion-parameters"></a>Parâmetros de conversão  

|Parâmetro|Descrição|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | O caminho completo da pasta raiz do aplicativo dos arquivos instalados se ele estivesse instalado (por exemplo, "C:\Program Files (x86) \MyApp").| 
|```-Destination <String>``` | O destino desejado para a saída de appx do conversor - o DesktopAppConverter pode criar esse local caso ele ainda não exista.|
|```-Installer <String>``` | O caminho para o instalador do seu aplicativo - deve ser capaz de ser executado de forma autônoma/silenciosa|
|```-InstallerArguments <String>``` [opcional] | Uma lista separada por vírgulas ou cadeia de caracteres de argumentos para forçar o instalador a ser executado de forma autônoma/silenciosa. Este parâmetro é opcional se o instalador for um msi. Para obter um log do seu instalador, forneça o argumento de registro em log do instalador aqui e use o caminho ```<log_folder>```, que é um token que o conversor substitui pelo caminho correto. <br><br>**OBSERVAÇÃO: os argumentos de log e os sinalizadores autônomo/silencioso variam entre as tecnologias de instalador.** <br><br>Um exemplo de uso deste parâmetro: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Outro exemplo que não produz um arquivo de log pode ter esta aparência: ```-InstallerArguments "/quiet", "/norestart"``` Novamente, você deve direcionar literalmente todos os logs para o caminho do token ```<log_folder>``` se quiser que o conversor os detecte e os coloque na pasta de log final.|
|```-InstallerValidExitCodes <Int32>``` [opcional] | Uma lista separada por vírgulas de códigos de saída que indicam que o instalador foi executado com êxito (por exemplo: 0, 1234, 5678).  Por padrão, o valor é 0 para não msi e 0, 1641, 3010 para msi.|

### <a name="appx-identity-parameters"></a>Parâmetros de identidade do AppX  

|Parâmetro|Descrição|
|---------|-----------|
|```-PackageName <String>``` | O nome do seu pacote de aplicativo universal do Windows
|```-Publisher <String>``` | O editor do seu pacote de aplicativo Universal do Windows
|```-Version <Version>``` | O número de versão do seu pacote de aplicativo Universal do Windows

### <a name="optional-appx-manifest-parameters"></a>Parâmetros opcionais do manifesto do Appx  

|Parâmetro|Descrição|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [opcional] | O nome do executável principal do aplicativo (ex: "MyApp.exe"). |
|```-AppFileTypes <String>``` [opcional] | Uma lista separada por vírgulas de tipos de arquivo aos quais o aplicativo será associado (por exemplo, ".txt, .doc", sem aspas).|
|```-AppId <String>``` [opcional] | Especifica um valor para definir a Id de aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|```-AppDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|```-AppDescription <String>``` [opcional] | Especifica um valor para definir a descrição do aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|```-PackageDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do pacote no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do editor no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *Publisher*. |

### <a name="other-conversion-parameters"></a>Outros parâmetros de conversão  

|Parâmetro|Descrição|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [opcional] | Caminho completo para uma imagem de base já expandida.|
|```-MakeAppx [<SwitchParameter>]``` [opcional] | Um botão que, quando presente, informa a este script para chamar MakeAppx na saída. |
|```-LogFile <String>``` [opcional] | Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado. |
| ```Sign [<SwitchParameter>] [optional]``` | Informa este script para assinar o appx de saída. Essa opção deve estar presente junto com a opção ```-MakeAppx```. 
|```<Common parameters>``` | Esse cmdlet oferece suporte aos parâmetros comuns: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* e *OutVariable*. Para obter mais informações, consulte [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

### <a name="cleanup-parameters"></a>Parâmetros de limpeza

|Parâmetro|Descrição|
|---------|-----------|
|```Cleanup [<Option>]``` | Executa a limpeza de artefatos DesktopAppConverter. Há 3 opções válidas para o modo de limpeza. |
|```Cleanup All``` | Exclui todas as imagens de base expandidas, remove todos os arquivos temporários do conversor, remove a rede de contêiner e desabilita o recurso Windows opcional, Contêineres. |
|```Cleanup WorkDirectory``` | Remove todos os arquivos temporários do conversor. |
|```Cleanup ExpandedImage``` | Exclui todas as imagens de base expandidas instaladas no computador host. |

### <a name="package-architecture"></a>Arquitetura de pacotes

O Desktop App Converter agora dá suporte à criação de pacotes de aplicativos x86 e x64 que você pode instalar e executar em computadores x86 e amd64. Observe que o Desktop App Converter ainda precisa ser executado em um computador AMD64 para realizar uma conversão bem-sucedida.

|Parâmetro|Descrição|
|---------|-----------|
|```-PackageArch <String>``` | Gera um pacote com a arquitetura especificada. As opções válidas são 'x86' ou 'x64'; por exemplo, -PackageArch x86. Este parâmetro é opcional. Se não for especificado, o DesktopAppConverter tentará detectar automaticamente a arquitetura do pacote. Se a detecção automática falhar, o padrão será pacote x64. 

### <a name="running-the-peheadercertfixtool"></a>Executando o PEHeaderCertFixTool

Durante o processo de conversão, o DesktopAppConverter executa automaticamente o PEHeaderCertFixTool a fim de corrigir quaisquer cabeçalhos PE corrompidos. No entanto, você também pode executar o PEHeaderCertFixTool em um aplicativo UWP, arquivos soltos ou um binário específico. Exemplo de uso: 

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>Suporte ao idioma

O Desktop App Converter não possui suporte para Unicode, assim, não podem ser usados caracteres chineses ou caracteres não ASCII com a ferramenta.

## <a name="see-also"></a>Consulte também

+ [Transformando aplicativos da área de trabalho em UWP usando o Desktop App Converter](https://channel9.msdn.com/events/Build/2016/P504)
+ [Projeto Centennial: trazendo aplicativos da área de trabalho existentes para a Plataforma Universal do Windows](https://channel9.msdn.com/events/Build/2016/B829)  
