---
author: awkoren
Description: Execute o conversor de aplicativos da área de trabalho para converter um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms) em um aplicativo UWP (Plataforma Universal do Windows).
Search.Product: eADQiWindows 10XVcnh
title: Visualização Conversor de Aplicativos da Área de Trabalho (Projeto Centennial)
---

# Visualização Conversor de Aplicativos da Área de Trabalho (Projeto Centennial)

\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não dá nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

[Baixe o conversor de aplicativos da área de trabalho.](http://go.microsoft.com/fwlink/?LinkId=785437)

O conversor de aplicativos da área de trabalho é uma ferramenta de pré-lançamento que permite converter seus aplicativos de área de trabalho existentes escritos para .NET 4.6.1 ou Win32 em UWP (Plataforma Universal do Windows). Você pode executar seus instaladores da área de trabalho por meio do conversor em um modo autônomo (silencioso) e obter um pacote de AppX que pode instalar usando o cmdlet do PowerShell Add-AppxPackage no computador de desenvolvimento.

O conversor executa o instalador da área de trabalho em um ambiente isolado do Windows usando uma imagem de base limpa fornecida como parte do download do conversor. Ele captura qualquer E/S do registro e do sistema de arquivos feita pelo instalador da área de trabalho e a empacota como parte da saída. O conversor gera um AppX com indentificador de pacote e a capacidade para chamar uma variedade maior de APIs do WinRT.

## Requisitos do sistema

### Sistema operacional com suporte
+ Visualização da edição Enterprise da Atualização de Aniversário do Windows 10 (compilação 10.0.14316.0 e posterior)

### Configuração de hardware necessária

Seu computador deve ter as seguintes funcionalidades mínimas:
+ Processador de 64 bits (x64)
+ Virtualização assistida por hardware
+ Conversão de Endereços de Segundo Nível (SLAT)

### Recursos recomendados
+ [Software Development Kit do Windows (SDK do Windows) para Windows 10](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)

## Configurar o conversor de aplicativos da área de trabalho   
O conversor de aplicativos da área de trabalho depende de recursos do Windows 10 que são liberados como parte das compilações do Windows Insider Preview. Certifique-se de que você tem a compilação mais recente para utilizar o conversor.

1. Certifique-se de que você tenha o sistema operacional do Windows 10 Insider Preview mais recente - Enterprise edition (compilação 10.0.14316.0 e superior).
2. Baixe o DesktopAppConverter.zip e o BaseImage-14316.wim.
3. Extraia o DesktopAppConverter.zip para uma pasta local.
4. Em uma janela do administrador do PowerShell:  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. Execute o comando a seguir em uma janela do administrador do PowerShell para configurar o conversor:
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-14316.wim
```
6. Se executar o comando anterior exigir uma reinicialização, reinicie o computador e execute o comando novamente.

## Execute o conversor de aplicativos da área de trabalho
O conversor de aplicativos da área de trabalho tem dois pontos de entrada: PowerShell e Shell de Comando. Você pode usar qualquer um dos pontos de entrada a seguir para iniciar o processo de conversão.

### Utilização
```CMD
DesktopAppConverter.ps1
-ExpandedBaseImage <String>
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-NatSubnetPrefix <String>]
[-LogFile <String>]
[<CommonParameters>]  
```

### Exemplo
O exemplo a seguir mostra como converter um aplicativo da área de trabalho denominado *MyApp* de *&lt;publisher_name&gt;* em um pacote UWP (AppX).

+ Em uma janela do administrador do PowerShell, execute o seguinte comando:
```CMD
PS C:\>.\DesktopAppConverter.ps1 -ExpandedBaseImage C:\ProgramData\Microsoft\Windows\Images\BaseImage-14316
-Installer C:\Installer\MyApp.exe -InstallerArguments "/S" -Destination C:\Output\MyApp
-PackageName "MyApp" -Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## Implantar seu AppX convertido
Use o cmdlet [Add-AppxPackage](https://technet.microsoft.com/en-us/library/hh856048.aspx) no PowerShell para implantar um pacote de aplicativo assinado (.appx) em uma conta de usuário. Para assinar o pacote .appx, consulte a seção a seguir, "Assinando seu pacote .Appx". Além disso, você também pode incluir o parâmetro *Register* do cmdlet para instalar de uma pasta de arquivos não empacotados durante o processo de desenvolvimento. Para obter mais informações, consulte [Implantar e depurar seu aplicativo UWP convertido](desktop-to-uwp-deploy-and-debug.md).

## Assinar seu pacote .AppX

O cmdlet Add-AppxPackage requer que o pacote do aplicativo (.appx) que está sendo implantado seja assinado. Use o SignTool.exe, que é fornecido no SDK do Microsoft Windows 10, para assinar o pacote .appx.

### Exemplo
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**Observação:** quando você executar o MakeCert.exe e for solicitado a inserir uma senha, selecione **Nenhuma**.

Para obter mais informações sobre assinatura e certificados, consulte:

+ [Como: criar certificados temporários para uso durante o desenvolvimento](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe (ferramenta de assinatura)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### Advertências
1. A compilação do Windows 10 no computador host deve corresponder à imagem base que você obteve como parte do download do conversor de aplicativos da área de trabalho.  
2. Verifique se o instalador da área de trabalho está em um diretório independente, pois o conversor copia todo o conteúdo do diretório para o ambiente isolado do Windows.  
3. No momento, o conversor de aplicativos da área de trabalho aceita a execução do processo de conversão apenas em um sistema operacional de 64 bits. Você pode implantar os pacotes.appx convertidos apenas em um sistema operacional de 64 bits (x64).  
4. O conversor de aplicativos da área de trabalho requer que o instalador da área de trabalho seja executado no modo autônomo. Certifique-se de passar o sinalizador silencioso do instalador para o conversor usando o parâmetro *- InstallerArguments*.
5. Publicar assemblies SxS Fusion públicos não funcionará. Durante a instalação, um aplicativo pode publicar assemblies Fusion públicos lado a lado, acessíveis para qualquer outro processo. Durante a criação do contexto de ativação do processo, esses assemblies são recuperados por um processo do sistema denominado CSRSS.exe. Quando isso é feito em um processo Centennial, a criação do contexto de ativação e o carregamento do módulo desses assemblies falhará. Assemblies de caixa de entrada, como ComCtl, são fornecidos com o sistema operacional, portanto, assumir uma dependência deles de processos Centennial é seguro. Os assemblies SxS Fusion são registrados nos seguintes locais:
  + Registro: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + Sistema de arquivos: %windir%\\SideBySide

## Telemetria do conversor de aplicativos da área de trabalho  
O conversor de aplicativos da área de trabalho pode coletar informações sobre você e o seu uso do software e enviar essas informações para a Microsoft. Você pode saber mais sobre a coleta e o uso de dados da Microsoft na documentação do produto e na [Política de Privacidade da Microsoft](http://go.microsoft.com/fwlink/?LinkId=521839). Você concorda em cumprir todas as provisões aplicáveis da Política de Privacidade da Microsoft.

Por padrão, a telemetria será habilitada para o conversor de aplicativos da área de trabalho. Adicione a seguinte chave do registro para definir a telemetria com uma configuração desejada:  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ Adicione ou edite o valor *DisableTelemetry* usando um DWORD definido como 1.
+ Para habilitar telemetria, remova a chave ou defina o valor como 0.

## Uso do conversor de aplicativos da área de trabalho
Aqui está uma lista de parâmetros do conversor de aplicativos da área de trabalho. Você também pode exibir essa lista na janela do Windows Powershell, executando o seguinte comando:  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### Parâmetros de configuração  
|Parâmetro|Descrição|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | Use este sinalizador para executar o DesktopAppConverter no modo de instalação. O modo de instalação aceita a expansão de uma imagem de base fornecida.|
|```-BaseImage <String>``` | Caminho completo para uma imagem de base não expandida. Este parâmetro será necessário se -Setup for especificado.|
|```-LogFile <String>``` [opcional] | Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado.|

### Parâmetros de conversão  
|Parâmetro|Descrição|
|---------|-----------|
|```-ExpandedBaseImage <String>``` | Caminho completo para uma imagem de base já expandida.|
|```-Installer <String>``` | O caminho para o instalador do seu aplicativo - deve ser capaz de ser executado de forma autônoma/silenciosa|
|```-InstallerArguments <String>``` [opcional] | Uma lista separada por vírgulas ou cadeia de caracteres de argumentos para forçar o instalador a ser executado de forma autônoma/silenciosa. Este parâmetro é opcional se o instalador for um msi. Para obter um log do seu instalador, forneça o argumento de registro em log do instalador aqui e use o caminho ```<log_folder>```, que é um token que o conversor substitui pelo caminho correto. <br><br>**OBSERVAÇÃO: os argumentos de log e os sinalizadores autônomo/silencioso variam entre as tecnologias de instalador.** <br><br>Um exemplo de uso deste parâmetro: ```-InstallerArguments "/silent /log <log_folder>\install.log"``` Outro exemplo que não produz um arquivo de log pode ter esta aparência: ```-InstallerArguments "/quiet", "/norestart"``` Novamente, você deve direcionar literalmente todos os logs para o caminho do token ```<log_folder>``` se quiser que o conversor os detecte e os coloque na pasta de log final.|
|```-InstallerValidExitCodes <Int32>``` [opcional] | Uma lista separada por vírgulas de códigos de saída que indicam que o instalador foi executado com êxito (por exemplo: 0, 1234, 5678).  Por padrão, o valor é 0 para não msi e 0, 1641, 3010 para msi.|
|```-Destination <String>``` | O destino desejado para a saída de appx do conversor - o DesktopAppConverter pode criar esse local caso ele ainda não exista.|

### Parâmetros de identidade do AppX  
|Parâmetro|Descrição|
|---------|-----------|
|```-PackageName <String>``` | O nome do seu pacote de aplicativo Universal do Windows
|```-Publisher <String>``` | O editor do seu pacote de aplicativo Universal do Windows
|```-Version <Version>``` | O número de versão do seu pacote de aplicativo Universal do Windows

### Parâmetros opcionais do manifesto do Appx  
|Parâmetro|Descrição|
|---------|-----------|
|```-AppExecutable <String>``` [opcional] | O caminho completo para o executável principal do seu aplicativo se ele fosse ser instalado (ele não tem que ser), por exemplo, "C:\Program Files (x86)\MyApp\MyApp.exe".|
|```-AppFileTypes <String>``` [opcional] | Uma lista separada por vírgulas de tipos de arquivo aos quais o aplicativo será associado (por exemplo, ".txt, .doc", sem aspas).|
|```-AppId <String>``` [opcional] | Especifica um valor para definir a Id de aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|```-AppDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|```-AppDescription <String>``` [opcional] | Especifica um valor para definir a descrição do aplicativo no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*.|
|```-PackageDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do pacote no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *PackageName*. |
|```-PackagePublisherDisplayName <String>``` [opcional] | Especifica um valor para definir o nome para exibição do editor no manifesto do appx. Se não for especificado, ele será definido como o valor passado para *Publisher*. |

### Outros parâmetros de conversão  
|Parâmetro|Descrição|
|---------|-----------|
|```-MakeAppx [<SwitchParameter>]``` [opcional] | Um botão que, quando presente, informa a este script para chamar MakeAppx na saída. |
|```-NatSubnetPrefix <String>``` [opcional] | Valor de prefixo a ser usado para a instância do Nat. Normalmente, você desejaria alterar isso somente se seu computador host fosse anexado à mesma faixa de sub-rede do NetNat do conversor. Você pode consultar a configuração atual do NetNat do conversor usando o cmdlet **Get-NetNat**. |
|```-LogFile <String>``` [opcional] | Especifica um arquivo de log. Se omitido, um local temporário de arquivo de log será criado. |
|```<Common parameters>``` | Esse cmdlet oferece suporte aos parâmetros comuns: *Verbose*, *Debug*, *ErrorAction*, *ErrorVariable*, *WarningAction*, *WarningVariable*, *OutBuffer*, *PipelineVariable* e *OutVariable*. Para obter mais informações, consulte [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216). |

## Veja também
+ [Baixar o conversor de aplicativos da área de trabalho](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [Traga seu aplicativo da área de trabalho para a Plataforma Universal do Windows](https://developer.microsoft.com/en-us/windows/bridges/desktop)
+ [Transformando aplicativos da área de trabalho em UWP usando o conversor de aplicativos da área de trabalho](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial: Transformando aplicativos da área de trabalho em UWP usando o conversor de aplicativos da área de trabalho](https://channel9.msdn.com/events/Build/2016/B829)  
+ [Ponte UserVoice para área de trabalho (Project Centennial)](http://aka.ms/UserVoiceDesktopToUwp)


<!--HONumber=May16_HO2-->


