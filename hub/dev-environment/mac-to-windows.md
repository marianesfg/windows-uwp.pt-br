---
title: Ajude a mudar do Mac (Unix) para o Windows
description: Um guia para ajudá-lo a fazer a transição de um Mac (Unix) para um ambiente de desenvolvimento do Windows, incluindo o mapeamento de teclas de atalho e uma breve visão geral dos conceitos que diferem entre Mac e Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac para Windows, mapeamento de teclas de atalho, mover do UNIX para o Windows, fazer a transição do Mac para o Windows, ajudar a mudar do MacBook para a superfície, como usar o Windows para um usuário Macintosh, alternando do Macintosh para o Windows, ajudar a alterar os ambientes de desenvolvimento, Mac OS X para o Windows, ajuda movendo do Mac para o PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218436"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guia para alterar o ambiente de desenvolvimento do Mac para o Windows

As dicas e os equivalentes de controle a seguir devem ajudá-lo em sua transição entre um ambiente de desenvolvimento Mac e Windows (ou WSL/Linux).

Para o desenvolvimento de aplicativos, o equivalente mais próximo ao Xcode seria o [Visual Studio](https://visualstudio.microsoft.com). Também há uma versão do [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), se você já sentir a necessidade de voltar. Para a edição de código-fonte de plataforma cruzada (e um grande número de plug-ins) [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) é a opção mais popular.

## <a name="keyboard-shortcuts"></a>Atalhos de teclado

| **Operacional** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Cópia | Comando + C | Ctrl+C |
| Recortar | Comando + X | Ctrl+X |
| Colar | Comando + V | Ctrl+V |
| Desfazer | Comando + Z | Ctrl+Z |
| Salvar | Comando + S | Ctrl+S |
| Abrir | Comando + O | Ctrl+O |
| Bloquear computador | Comando + Control + Q | WindowsKey + L |
| Mostrar área de trabalho | Comando + F3 | WindowsKey + D |
| Abrir navegador de arquivos | Comando + N | WindowsKey + E |
| Minimizar o Windows | Comando + M | WindowsKey + M |
| Pesquisar | Comando + espaço | WindowsKey |
| Fechar janela ativa | Comando + W | Controle + W |
| Alternar tarefa atual | Comando + guia | Alt+Tab |
| Maximizar uma janela para tela inteira | Controle + comando + F | WindowsKey + up |
| Salvar tela (Screen dump) | Comando + Shift + 3 | WindowsKey + Shift + S |
| Salvar janela | Comando + Shift + 4 | WindowsKey + Shift + S |
| Exibir informações ou propriedades do item | Comando + I | Alt+Enter |
 | Selecionar todos os itens | Comando + A | Ctrl+A |
| Selecionar mais de um item em uma lista (não contíguo) | E, em seguida, clique em cada item | Controle e clique em cada item |
| Digitar caracteres especiais | Opção + chave de caractere | ALT + tecla de caractere|

## <a name="trackpad-shortcuts"></a>Atalhos do trackpad

Observação: alguns desses atalhos exigem uma "precisão trackpad", como o trackpad em dispositivos de superfície e outros laptops de terceiros.

 **Operacional** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Rolagem | Dedo vertical de dois dedos | Dedo vertical de dois dedos |
| {1&gt;Zoom&lt;1} | Apertar e reduzir os dois dedos | Apertar e reduzir os dois dedos |
| Passar o dedo para trás e para frente entre exibições | Passar o dedo lateral com dois dedos | Passar o dedo lateral com dois dedos |
| Alternar espaços de trabalho virtuais | Passe o dedo lateral com quatro dedos | Passe o dedo lateral com quatro dedos |
| Exibir aplicativos abertos no momento | Passar o dedo para cima com quatro dedos | Dedo para cima de três dedos |
| Alternar entre aplicativos | {1&gt;N/A&lt;1} | Passar o dedo lateral com três dedos |
| Ir para área de trabalho | Distribuir quatro dedos | Dedo com três dedos para baixo |
| Abrir o Cortana/central de ações | Slide de dois dedos à direita | Toque de três dedos |
| Abrir informações extras | Toque de três dedos | {1&gt;N/A&lt;1} |
|Mostrar Launchpad/iniciar um aplicativo | Pinçar com quatro dedos | Toque com quatro dedos |

Observação: as opções de trackpad são configuráveis em ambas as plataformas.

## <a name="terminal-and-shell"></a>Terminal e Shell

O Windows fornece várias alternativas ao emulador de terminal do Mac.

1. A linha de comando do Windows

A linha de comando do Windows aceitará os comandos DOS e é a ferramenta de linha de comando mais usada no Windows. Para abri-lo: pressione **WindowsKey + R** para abrir a caixa **executar** , digite **cmd** e clique em **OK**. Para abrir uma linha de comando de administrador, digite **cmd** e pressione **Ctrl + Shift + Enter**.

2. PowerShell

O [PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) é um "PowerShell é um shell de linha de comando baseado em tarefa e linguagem de script criado no .net. O PowerShell ajuda os administradores de sistema e os usuários a automatizar rapidamente tarefas que gerenciam sistemas operacionais. Em outras palavras, é uma linha de comando muito poderosa e é especialmente importante para administradores do sistema.

Incidentalmente, o PowerShell [também está disponível para Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. WSL (Subsistema do Windows para Linux)

O WSL permite que você execute um shell do Linux no Windows. Isso significa que você pode executar o *bash** ou outro shell, dependendo da escolha e do distribuição Linux específico instalado. O uso de WSL fornecerá o tipo de ambiente mais familiar para os usuários do Mac. Por exemplo, você será o **ls** para listar os arquivos em um diretório atual, não o **dir** como você faria com a linha de comando do Windows. Para saber mais sobre como instalar e usar o WSL, consulte o [Guia de instalação do subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Terminal do Windows (versão prévia)

O terminal do Windows é um aplicativo que combina ferramentas de linha de comando e shells de várias fontes, incluindo a linha de comando tradicional do Windows, o PowerShell e o subsistema Windows para Linux. Embora ainda esteja atualmente em versão prévia, ele alreaedy contém vários recursos úteis, como suporte para várias guias, painéis divididos, temas e estilos personalizados e suporte completo a Unicode. O terminal do Windows pode ser instalado por meio do [Microsoft Store no Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Aplicativos e utilitários

 **Aplicação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configurações e preferências | Preferências do sistema | Configurações |
| Gerenciador de tarefas | Monitor de atividade | Gerenciador de Tarefas |
| Formatação de disco | Utilitário de disco | Gerenciamento do disco |
| Edição de texto | TextEdit | Bloco de notas |
| Exibição de eventos | Console | Visualizador de Eventos |
| Localizar arquivos/aplicativos | Comando + espaço | tecla do Windows |
