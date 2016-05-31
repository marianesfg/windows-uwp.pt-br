---
author: msatranjr
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Instalar aplicativos usando a ferramenta WinAppDeployCmd.exe
description: O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo da Plataforma Universal do Windows (UWP) de uma máquina com o Windows 10 em qualquer dispositivo com o Windows 10 Mobile.
---
# Instalar aplicativos usando a ferramenta WinAppDeployCmd.exe

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo da Plataforma Universal do Windows (UWP) de uma máquina com o Windows 10 em qualquer dispositivo com o Windows 10 Mobile. É possível usar essa ferramenta para implantar um pacote .appx quando o dispositivo com o Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta.

Basta o SDK do Windows 10 instalado para executar a ferramenta WinAppDeployCmd em um prompt de comando ou em um arquivo de script. Quando você instala um aplicativo com WinAppDeployCmd.exe, ele usa o arquivo .appx para fazer o sideload do aplicativo para um dispositivo com o Windows 10 Mobile. Esse comando não instala o certificado necessário para o aplicativo. Para executar o aplicativo, o dispositivo com o Windows 10 Mobile deve estar no modo de desenvolvedor ou já ter o certificado instalado.

Para implantar o aplicativo em dispositivos com o Windows 10 Mobile, você deve criar um pacote primeiro. Para obter mais informações, consulte \[link pai aqui\].

A ferramenta **WinAppDeployCmd.exe** se localiza aqui no computador com o Windows 10: **C:\\Arquivos de Programas (x86)\\Kits do Windows\\10\\bin\\x86\\WinAppDeployCmd.exe** (com base no caminho de instalação do SDK). Primeiro, conecte o dispositivo com o Windows 10 Mobile à mesma sub-rede ou conecte-o diretamente ao computador com o Windows 10 usando uma conexão USB. Em seguida, use a sintaxe e os exemplos a seguir desse comando neste artigo para implantar o pacote .appx:

## Sintaxe e opções de WinAppDeployCmd

Aqui está a sintaxe possível que você pode usar para **WinAppDeployCmd.exe**

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

É possível instalar ou desinstalar um aplicativo no dispositivo de destino, ou atualizar um aplicativo já instalado. Para manter dados ou configurações salvos por um aplicativo já instalado, use as opções **update** em vez de **install**.

A tabela a seguir descreve os comandos de **WinAppDeployCmd.exe**.

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **Comando** | **Descrição**                                                     |
| dispositivos     | Mostre a lista de dispositivos de rede disponíveis.                         |
| install     | Instale um pacote do aplicativo UWP para o dispositivo de destino.                     |
| update      | Atualize um aplicativo UWP que já esteja instalado no dispositivo de destino.    |
| list        | Mostre a lista de aplicativos UWP instalados no dispositivo de destino especificado. |
| uninstall   | Desinstale o pacote do aplicativo especificado do dispositivo de destino.         |

 

A tabela a seguir descreve as opções de **WinAppDeployCmd.exe**

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Comando**      | **Descrição**                                                                                                                                                                                               |
| -h (-help)       | Mostre os comandos, as opções e os argumentos.                                                                                                                                                                     |
| -ip              | Endereço IP do dispositivo de destino.                                                                                                                                                                              |
| -g (-guid)       | Identificador exclusivo do dispositivo de destino.                                                                                                                                                                       |
| -d (-dependency) | (Opcional) Especifica o caminho de dependência para cada uma das dependências do pacote. Caso nenhum caminho seja especificado, a ferramenta procura dependências no diretório raiz do pacote do aplicativo e os diretórios SDK. |
| -f (-file)       | Caminho do arquivo para o pacote do aplicativo instalar, atualizar ou desinstalar.                                                                                                                                                |
| -p (-package)    | O nome do pacote completo para o pacote do aplicativo a ser desinstalado. (É possível usar o comando list para encontrar os nomes completos de pacotes já instalados no dispositivo.)                                                   |
| -pin             | Um PIN, caso ele seja necessário para estabelecer uma conexão com o dispositivo de destino. (Você deverá tentar novamente usando a opção -pin caso a autenticação seja necessária.)                                                 |

 

A tabela a seguir descreve as opções de **WinAppDeployCmd.exe**.

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Argumento**           | **Descrição**                                                              |
| &lt;x&gt;              | Tempo limite em segundos. (O padrão é 10)                                          |
| &lt;address&gt;        | Endereço IP ou identificador exclusivo do dispositivo de destino.                        |
| &lt;a&gt;&lt;b&gt; ... | Caminho de dependência para cada uma das dependências do pacote do aplicativo.                    |
| &lt;p&gt;              | PIN alfanumérico mostrado nas configurações do dispositivo para estabelecer uma conexão. |
| &lt;path&gt;           | Caminho do sistema de arquivos.                                                            |
| &lt;name&gt;           | Nome do pacote completo para o pacote do aplicativo a ser desinstalado.                          |

 
## Exemplos de WinAppDeployCmd.exe

Aqui estão alguns exemplos de como fazer a implantação usando a linha de comando com a sintaxe para **WinAppDeployCmd.exe**.

Mostra os dispositivos disponíveis para implantação. O comando atinge o tempo limite em três segundos.

``` syntax
WinAppDeployCmd devices 3
```

Instala o aplicativo do pacote MyApp.appx que está no diretório Downloads do computador para um dispositivo com o Windows 10 Mobile usando um endereço IP 192.168.0.1 com um PIN A1B2C3 para estabelecer uma conexão com o dispositivo

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Desinstala o pacote especificado (com base no nome completo) de um dispositivo com o Windows 10 Mobile usando um endereço IP 192.168.0.1. Você pode usar o comando list para consultar os nomes completos de todos os pacotes instalados em um dispositivo.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Atualiza o aplicativo que já está instalado no dispositivo com o Windows 10 Mobile usando um endereço IP 192.168.0.1 com o pacote .appx especificado.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```



<!--HONumber=May16_HO2-->


