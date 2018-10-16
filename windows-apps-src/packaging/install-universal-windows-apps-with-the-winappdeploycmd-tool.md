---
author: laurenhughes
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: Instalar apps usando a ferramenta WinAppDeployCmd.exe
description: O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo da Plataforma Universal do Windows (UWP) de um computador com o Windows 10 em qualquer dispositivo com o Windows 10.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 229f0e9993abc9c5600c55a1a0eddc2e262f1c4c
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689565"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>Instalar apps usando a ferramenta WinAppDeployCmd.exe


O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo da Plataforma Universal do Windows (UWP) de um computador com o Windows 10 em qualquer dispositivo com o Windows 10. Você pode usar essa ferramenta para implantar um pacote do aplicativo quando o dispositivo Windows 10 está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Você também pode implementar o aplicativo sem empacotar primeiro a um computador ou Xbox One remoto. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta.

Basta o SDK do Windows 10 instalado para executar a ferramenta WinAppDeployCmd em um prompt de comando ou em um arquivo de script. Quando você instala um aplicativo com WinAppDeployCmd.exe, usa o arquivo de.appx/.msix ou AppxManifest (para arquivos soltos) para carregar no lado do seu aplicativo em um dispositivo Windows 10. Esse comando não instala o certificado necessário para o aplicativo. Para executar o aplicativo, o dispositivo com o Windows 10 deve estar no modo de desenvolvedor ou já ter o certificado instalado.

Para implementar em dispositivos móveis, você deve primeiro criar um pacote. Para obter mais informações, veja [aqui](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

A ferramenta **WinAppDeployCmd.exe** se localiza aqui no computador com o Windows 10: **C:\\Program Files (x86)\\Windows Kits\\10\\bin\\<SDK Version>\\x86\\WinAppDeployCmd.exe** (com base no caminho de instalação do SDK). 
> [!NOTE]
> Na versão 15063 e posterior do SDK, ele está instalado lado a lado em pastas específicas à versão.  SDKs anteriores (14393 e anteriores) são gravados diretamente na pasta pai.

Primeiro, conecte o dispositivo com o Windows 10 à mesma sub-rede ou conecte-o diretamente ao computador com o Windows 10 usando uma conexão USB. Em seguida, use a sintaxe e os exemplos a seguir desse comando neste artigo para implementar o aplicativo UWP:

## <a name="winappdeploycmd-syntax-and-options"></a>Sintaxe e opções de WinAppDeployCmd

Esta é a sintaxe geral usada para **WinAppDeployCmd.exe**:
```syntax
WinAppDeployCmd command -option <argument>
```

Aqui estão alguns exemplos de sintaxe adicionais para o uso de vários comandos:
```syntax
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

É possível instalar ou desinstalar um app no dispositivo de destino, ou você atualizar um app já instalado. Para manter dados ou configurações salvos por um aplicativo já instalado, use as opções **update** em vez de **install**.

A tabela a seguir descreve os comandos de **WinAppDeployCmd.exe**.


| **Comando**  | **Descrição**                                                     |
|--------------|---------------------------------------------------------------------|
| dispositivos      | Mostre a lista de dispositivos de rede disponíveis.                         |
| install      | Instale um pacote do aplicativo UWP para o dispositivo de destino.                     |
| update       | Atualize um aplicativo UWP que já esteja instalado no dispositivo de destino.    |
| list         | Mostre a lista de aplicativos UWP instalados no dispositivo de destino especificado. |
| uninstall    | Desinstale o pacote do aplicativo especificado do dispositivo de destino.         |
| deployfiles  | Copia arquivos soltos de aplicativo no caminho de destino para o caminho relativo remoto no dispositivo.|
| registerfiles| Registra o aplicativo de arquivos soltos no diretório de implementação remota.         |
| addcreds     | Adiciona as credenciais a um Xbox para permitir que ele tenha acesso a um local de rede para registro do aplicativo.|
| getcreds     | Obtêm credenciais de rede para o uso no destino ao executar um aplicativo a partir de um compartilhamento de rede.|
| deletecreds  | Exclui credenciais de rede de uso no destino ao executar um aplicativo a partir de um compartilhamento de rede.|


A tabela a seguir descreve as opções de **WinAppDeployCmd.exe**.


| **Comando**  | **Descrição**  |
|--------------|------------------|
| -h (-help)       | Mostre os comandos, as opções e os argumentos. |
| -ip              | Endereço IP do dispositivo de destino. |
| -g (-guid)       | Identificador exclusivo do dispositivo de destino.|
| -d (-dependency) | (Opcional) Especifica o caminho de dependência para cada uma das dependências do pacote. Caso nenhum caminho seja especificado, a ferramenta procura dependências no diretório raiz do pacote do aplicativo e os diretórios SDK.|
| -f (-file)       | Caminho do arquivo para o pacote do aplicativo instalar, atualizar ou desinstalar.|
| -p (-package)    | O nome do pacote completo para o pacote do aplicativo a ser desinstalado. (É possível usar o comando list para encontrar os nomes completos de pacotes já instalados no dispositivo) |
| -pin             | Um PIN, caso ele seja necessário para estabelecer uma conexão com o dispositivo de destino. (Você deverá tentar novamente usando a opção -pin caso a autenticação seja necessária) |
| -credserver      | O nome do servidor das credenciais de rede para ser usado pelo destino. |
| -credusername    | O nome do usuário das credenciais de rede para ser usado pelo destino. |
| -credpassword    | A senha das credenciais de rede usadas pelo destino. |
| -connecttimeout  | O tempo limite em segundos usado ao se conectar ao dispositivo. |
| -remotedeploydir | Caminho/nome do diretório relativo para copiar arquivos para o dispositivo remoto. Esta será uma pasta de implantação remota conhecida, determinada automaticamente. |
| -deleteextrafile | Alternar para indicar se os arquivos existentes no diretório remoto devem ser limpos para corresponder o diretório de origem. |


A tabela a seguir descreve as opções de **WinAppDeployCmd.exe**.

| **Argumento**           | **Descrição**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | Tempo limite em segundos. (O padrão é 10)                                          |
| &lt;address&gt;        | Endereço IP ou identificador exclusivo do dispositivo de destino.                        |
| &lt;a&gt;&lt;b&gt; ... | Caminho de dependência para cada uma das dependências do pacote do aplicativo.                    |
| &lt;p&gt;              | PIN alfanumérico mostrado nas configurações do dispositivo para estabelecer uma conexão. |
| &lt;path&gt;           | Caminho do sistema de arquivos.                                                            |
| &lt;name&gt;           | Nome do pacote completo para o pacote do aplicativo a ser desinstalado.                          |
| &lt;server&gt;         | Servidor na rede do arquivo.                                                  |
| &lt;username&gt;       | Usuário para as credenciais com acesso ao servidor da rede do arquivo.      |
| &lt;password&gt;       | Senha para as credenciais com acesso ao servidor da rede dos arquivos. |
| &lt;remotedeploydir&gt;| Diretório no dispositivo em relação à localização de implementação                      |

 
## <a name="winappdeploycmdexe-examples"></a>Exemplos de WinAppDeployCmd.exe

Aqui estão alguns exemplos de como fazer a implantação usando a linha de comando com a sintaxe para **WinAppDeployCmd.exe**.

Mostra os dispositivos disponíveis para implantação. O comando atinge o tempo limite em três segundos.

``` syntax
WinAppDeployCmd devices 3
```

Instala o aplicativo do pacote MyApp.appx que está no diretório Downloads do computador para um dispositivo com o Windows 10 usando um endereço IP 192.168.0.1 com um PIN A1B2C3 para estabelecer uma conexão com o dispositivo

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

Desinstala o pacote especificado (com base no nome completo) de um dispositivo com o Windows 10 usando um endereço IP 192.168.0.1. Você pode usar o comando list para consultar os nomes completos de todos os pacotes instalados em um dispositivo.

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

Atualizações de aplicativo que já esteja instalado no dispositivo Windows 10 com um endereço IP 192.168.0.1 usando o pacote do aplicativo especificado.

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

Implementa os arquivos de um aplicativo em um computador ou Xbox com um endereço IP 192.168.0.1 na mesma pasta do AppxManifest para o diretório app1_F5 sob o caminho de implementação do dispositivo.

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

Registra o aplicativo no diretório app1_F5 sob o caminho de implementação do computador ou Xbox em 192.168.0.1.

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>Usando WinAppDeployCmd para configurar a implantação de Executar no Computador no Xbox One

Executar no Computador permite que você implante um aplicativo UWP em um Xbox One sem copiar os binários. m vez disso, os binários são hospedados em um compartilhamento de rede na mesma rede que o Xbox.  Para fazer isso, você precisa de um Xbox One desbloqueado por desenvolvedor e um aplicativo UWP de arquivo flexível em uma unidade de rede que o Xbox pode acessar.

Execute isso para registrar o aplicativo:
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
