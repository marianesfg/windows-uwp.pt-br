---
title: Usar a ferramenta winget para instalar e gerenciar aplicativos
description: A ferramenta de linha de comando winget permite que os desenvolvedores descubram, instalem, atualizem, removam e configurem aplicativos em computadores Windows 10.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 3b3f108de117fb937a7a670497a4a1a1d5810aca
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334534"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>Usar a ferramenta winget para instalar e gerenciar aplicativos

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

A ferramenta de linha de comando **winget** permite que os desenvolvedores descubram, instalem, atualizem, removam e configurem aplicativos em computadores Windows 10. Essa ferramenta é a interface do cliente para o serviço Gerenciador de Pacotes do Windows.

No momento, a ferramenta **winget** é uma versão prévia; portanto, nem toda a funcionalidade planejada está disponível no momento.

## <a name="install-winget"></a>Instalar o winget

Há várias maneiras de instalar a ferramenta **winget**:

* A ferramenta **winget** está incluída na versão de pré-lançamento ou na versão prévia do [Instalador de Aplicativo do Windows](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab). Você deve instalar a versão prévia do **Instalador de Aplicativo** para usar o **winget**. Para obter acesso antecipado, envie sua solicitação para o [Programa Insiders do Gerenciador de Pacotes do Windows](https://aka.ms/AppInstaller_InsiderProgram). Participar do modo de versão de pré-lançamento garantirá que você veja as atualizações mais recentes da versão prévia.

* Participe do [Modo de versão de pré-lançamento do Participante do Programa Windows Insider](https://insider.windows.com).

* Instale o pacote do Instalador de Aplicativo da Área de Trabalho do Windows localizado na pasta de versão do [repositório do winget](https://github.com/microsoft/winget-cli).

> [!NOTE]
> A ferramenta **winget** requer o Windows 10, versão 1709 (10.0.16299) ou uma versão mais recente dele.

## <a name="administrator-considerations"></a>Considerações sobre o administrador

O comportamento do instalador poderá ser diferente dependendo se você estiver executando o **winget** com privilégios de administrador.

* Ao executar o **winget** sem privilégios de administrador, alguns aplicativos podem [exigir elevação](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/) para serem instalados. Quando o instalador for executado, o Windows solicitará que você [eleve](https://docs.microsoft.com/windows/security/identity-protection/user-account-control). Se você optar por não elevar, o aplicativo não será instalado.  

* Ao executar o **winget** em um Prompt de Comando de Administrador, você não verá [prompts de elevação](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/how-user-account-control-works) se o aplicativo os exigir. Sempre tome cuidado ao executar o prompt de comando como administrador e instale apenas aplicativos confiáveis.

## <a name="use-winget"></a>Usar o winget

Depois que o **Instalador de Aplicativo** estiver instalado, será possível executar o **winget** digitando "winget" em um Prompt de Comando.

Um dos cenários de uso mais comuns é pesquisar e instalar uma ferramenta favorita.

1. Para [pesquisar](search.md) uma ferramenta, digite `winget search \<appname>`.
2. Depois de confirmar que a ferramenta que você deseja está disponível, será possível [instalá-la](install.md) digitando `winget install \<appname>`. A ferramenta **winget** iniciará o instalador e instalará o aplicativo em seu computador.
    ![winget commandline](images\install.png)

3. Além de instalar e pesquisar, **winget** fornece vários outros comandos que permitem que você [mostre detalhes](show.md) sobre aplicativos, [altere fontes](source.md) e [validade pacotes](validate.md). Para obter uma lista completa de comandos, digite: `winget --help`.
    ![winget help](images\help.png)

### <a name="commands"></a>Comandos

A versão prévia atual da ferramenta **winget** dá suporte aos comandos a seguir.

| Comando | Descrição |
|---------|-------------|
| [hash](hash.md) | Gera o hash SHA256 para o instalador. |
| [help](help.md) | Exibe a ajuda para os comandos da ferramenta **winget**. |
| [install](install.md) | Instala o aplicativo especificado. |
| [search](search.md) | Pesquisa um aplicativo. |
| [show](show.md) | Exibe os detalhes do aplicativo especificado. |
| [source](source.md) | Adiciona, remove e atualiza os repositórios do Gerenciador de Pacotes do Windows acessados pela ferramenta **winget**. |
| [validate](validate.md) | Valida um arquivo de manifesto para envio ao repositório do Gerenciador de Pacotes do Windows. |

### <a name="options"></a>Opções

A versão prévia atual da ferramenta **winget** dá suporte às opções a seguir.

| Opção | Descrição |
|--------------|-------------|
| **-v,--version** | essa opção retorna a versão atual do winget. |
| **--info** |  essa opção fornece todas as informações detalhadas sobre o winget, incluindo os links para a licença e a política de privacidade. |
| **-?, --help** |  obter winget de ajuda adicional |

## <a name="supported-installer-formats"></a>Formatos de instalador com suporte

A versão prévia atual da ferramenta **winget** dá suporte aos tipos de instaladores a seguir.

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>Winget de script

É possível criar scripts em lote e do powershell para instalar vários aplicativos.

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> Quando tiver script, o **winget** iniciará os aplicativos na ordem especificada. Quando um instalador retornar êxito ou falha, o **winget** iniciará o próximo instalador. Se um instalador iniciar outro processo, será possível que ele retorne ao **winget** prematuramente. Isso fará o **winget** instalar o próximo instalador antes que o instalador anterior tenha sido concluído.

## <a name="missing-tools"></a>Ferramentas ausentes

Se o [repositório da comunidade](../package/repository.md) não incluir sua ferramenta ou aplicativo. Envie um pacote para o nosso [repositório](https://github.com/microsoft/winget-pkgs). Ao adicionar sua ferramenta favorita, ela estará disponível para você e todos os outros.

## <a name="open-source-details"></a>Detalhes do software livre

A ferramenta **winget** é um software livre disponível no GitHub no repositório [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/). A fonte para a criação do cliente está localizada na pasta [src](https://github.com/microsoft/winget-cli/tree/master/src).

A fonte para **winget** está contida em uma solução em C++ do Visual Studio 2019. Para compilar a solução corretamente, instale a [carga de trabalho mais recente do Visual Studio com o C++](https://visualstudio.microsoft.com/downloads/).

Incentivamos você a contribuir com a fonte do **winget** no GitHub. Primeiro, você deve concordar e assinar o CLA da Microsoft.
