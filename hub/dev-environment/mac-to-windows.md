---
title: Ajudar a mudar do Mac (Unix) para o Windows
description: Um guia para ajudar a fazer a transição de um Mac (Unix) para um ambiente de desenvolvimento do Windows, incluindo o mapeamento de teclas de atalho e uma breve visão geral dos conceitos que diferem entre Mac e Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac para Windows, mapeamento de teclas de atalho, migrar do UNIX para o Windows, fazer a transição do Mac para o Windows, ajudar a migrar do MacBook para o Surface, como usar o Windows para um usuário Macintosh, alternar do Macintosh para o Windows, ajudar a alterar os ambientes de desenvolvimento, Mac OS X para o Windows, ajuda a migrar do Mac para o PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 457abcec97247afcc0d63c983c8a6cda2de51c66
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81643695"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guia para alterar o ambiente de desenvolvimento do Mac para o Windows

As dicas e os controles equivalentes a seguir devem ajudar em sua transição entre um ambiente de desenvolvimento Mac e Windows (ou WSL/Linux).

Para o desenvolvimento de aplicativos, o equivalente mais próximo ao Xcode seria o [Visual Studio](https://visualstudio.microsoft.com). Também há uma versão do [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), caso você sinta a necessidade de retornar. Para a edição de código-fonte de plataforma cruzada (e diversos plug-ins), o [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) é a escolha mais conhecida.

## <a name="keyboard-shortcuts"></a>Atalhos de teclado

| **Operação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Command+C | Ctrl+C |
| Recortar | Command+X | Ctrl+X |
| Colar | Command+V | Ctrl+V |
| Desfazer | Command+Z | Ctrl+Z |
| Salvar | Command+S | Ctrl+S |
| Abrir | Command+O | Ctrl+O |
| Bloquear o computador | Command+Control+Q | Tecla do Windows+L |
| Mostrar a área de trabalho | Command+F3 | Tecla do Windows+D |
| Abrir o navegador de arquivos | Command+N | Tecla do Windows+E |
| Minimizar janelas | Command+M | Tecla do Windows+M |
| Pesquisar | Command+Espaço | Tecla do Windows |
| Fechar janela ativa | Command+W | Control+W |
| Alternar para a tarefa atual | Command+Tab | Alt+Tab |
| Maximizar uma janela para tela inteira | Control+Command+F | Tecla do Windows+seta para cima |
| Salvar tela (captura de tela) | Command+Shift+3 | Tecla do Windows+Shift+S |
| Salvar janela | Command+Shift+4 | Tecla do Windows+Shift+S |
| Exibir informações ou propriedades do item | Command+I | Alt+Enter |
 | Selecionar todos os itens | Command+A | Ctrl+A |
| Selecionar mais de um item em uma lista (não contíguo) | Command, depois clicar em cada item | Controle, depois clicar em cada item |
| Digitar caracteres especiais | Option+tecla de caractere | Alt+tecla de caractere|

## <a name="trackpad-shortcuts"></a>Atalhos do trackpad

Observação: Alguns desses atalhos exigem um "trackpad de precisão", como o trackpad em dispositivos Surface e outros laptops de terceiros.

 **Operação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Rolagem | Passar dois dedos na vertical | Passar dois dedos na vertical |
| Zoom | Pinçar com dois dedos para reduzir e aumentar | Pinçar com dois dedos para reduzir e aumentar |
| Passar o dedo para trás e para frente entre exibições | Passar dois dedos nas laterais | Passar dois dedos nas laterais |
| Alternar espaços de trabalho virtuais | Passar quatro dedos nas laterais | Passar quatro dedos nas laterais |
| Exibir aplicativos abertos no momento | Passar quatro dedos para cima | Passar três dedos para cima |
| Alternar entre aplicativos | N/D | Passar devagar três dedos nas laterais |
| Acessar a área de trabalho | Espalhar quatro dedos para fora | Passar três dedos para baixo |
| Abrir a Cortana/Central de Ações | Deslizar dois dedos a partir da direita | Tocar com três dedos |
| Abrir informações adicionais | Tocar com três dedos | N/D |
|Mostrar a barra inicial/iniciar um aplicativo | Pinçar com quatro dedos | Tocar com quatro dedos |

Observação: as opções de trackpad são configuráveis nas duas plataformas.

## <a name="terminal-and-shell"></a>Terminal e Shell

O Windows fornece várias alternativas ao emulador de terminal do Mac.

1. Linha de comando do Windows

A linha de comando do Windows aceita os comandos DOS e é a ferramenta de linha de comando mais usada no Windows. Para abrir: Pressione **Tecla do Windows+R** para abrir a caixa **Executar**, digite **cmd** e clique em **OK**. Para abrir uma linha de comando de administrador, digite **cmd** e pressione **Ctrl+Shift+Enter**.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) é "um shell de linha de comando baseado em tarefas e uma linguagem de script compilada no .NET. O PowerShell ajuda os administradores de sistema e os usuários avançados a automatizar rapidamente tarefas que gerenciam sistemas operacionais". Em outras palavras, é uma linha de comando muito poderosa especialmente importante para administradores do sistema.

Além disso, o PowerShell [também está disponível para Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. WSL (Subsistema do Windows para Linux)

O WSL permite que você execute um shell do Linux no Windows. Isso significa que você pode executar o **bash** ou outro shell, dependendo da escolha e da distribuição específica do Linux instalada. O uso de WSL fornecerá o tipo de ambiente mais conhecido para os usuários do Mac. Por exemplo, você usa **ls** para listar os arquivos em um diretório atual, e não **dir** como faria na linha de comando do Windows. Para saber mais sobre como instalar e usar o WSL, confira o [Guia de Instalação do Subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Terminal do Windows (versão prévia)

O Terminal do Windows é um aplicativo que combina ferramentas de linha de comando e shells de várias fontes, incluindo a linha de comando tradicional do Windows, o PowerShell e o Subsistema do Windows para Linux. Embora atualmente ainda esteja em versão prévia, ele já contém vários recursos úteis, como suporte para várias guias, painéis divididos, temas e estilos personalizados e suporte completo a Unicode. O Terminal do Windows pode ser instalado por meio da [Microsoft Store no Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Aplicativos e utilitários

 **Aplicativo** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configurações e Preferências | Preferências do Sistema | Settings |
| Gerenciador de tarefas | Monitor de Atividade | Gerenciador de Tarefas |
| Formatação de disco | Utilitário de disco | Gerenciamento de disco |
| Edição de texto | TextEdit | Bloco de notas |
| Exibição de evento | Console | Visualizador de Eventos |
| Localizar arquivos/aplicativos | Command+Espaço | Tecla do Windows |
