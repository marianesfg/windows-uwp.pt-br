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
ms.openlocfilehash: f72e688417726d3c3193d831dc886f6a33ec98f8
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315320"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guia para alterar o ambiente de desenvolvimento do Mac para o Windows

As dicas e os equivalentes de controle a seguir devem ajudá-lo em sua transição entre um ambiente de desenvolvimento Mac e Windows (ou WSL/Linux).

Para o desenvolvimento de aplicativos, o equivalente mais próximo ao Xcode seria o [Visual Studio](https://visualstudio.microsoft.com). Também há uma versão do [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), se você já sentir a necessidade de voltar. Para a edição de código-fonte de plataforma cruzada (e um grande número de plug-ins) [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) é a opção mais popular.

## <a name="keyboard-shortcuts"></a>Atalhos de teclado

| **Operação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Comando + C | CTR + C |
| Recortar | Comando + X | CTR + X |
| Colar | Comando + V | CTR + V |
| Desfazer | Comando + Z | Ctrl+Z |
| Salvar | Comando + S | Ctrl+S |
| Abrir | Comando + O | Ctrl+O |
| Bloquear computador | Comando + Control + Q | CTR + L |
| Mostrar área de trabalho | Comando + F3 | Ctrl+D |
| Minimizar o Windows | Tecla Windows + M | COMANDO + M |
| Pesquisa | Comando + espaço | tecla do Windows |
| Fechar janela ativa | Comando + W | Controle + W |
| Alternar tarefa atual | Comando + guia | Alt+Tab |
| Salvar tela (Screen dump) | Comando + Shift + 3 | Windows + Shift + S |
| Salvar janela | Comando + Shift + 4 | Windows + Shift + S |
| Exibir informações ou propriedades do item | Comando + I | Alt+Enter |
 | Selecionar todos os itens | Comando + A | Ctrl+A |
| Selecionar mais de um item em uma lista (não contíguo) | E, em seguida, clique em cada item | Controle e clique em cada item |
| Digitar caracteres especiais | Opção + chave de caractere | ALT + tecla de caractere|

## <a name="trackpad-shortcuts"></a>Atalhos do trackpad

Observação: Alguns desses atalhos exigem uma "precisão trackpad", como o trackpad em dispositivos de superfície e outros laptops de terceiros.

 **Operação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Rolagem | Dedo vertical de dois dedos | Dedo vertical de dois dedos |
| Zoom | Apertar e reduzir os dois dedos | Apertar e reduzir os dois dedos |
| Passar o dedo para trás e para frente entre exibições | Passar o dedo lateral com dois dedos | Passar o dedo lateral com dois dedos |
| Alternar espaços de trabalho virtuais | Passe o dedo lateral com quatro dedos | Passe o dedo lateral com quatro dedos |
| Exibir aplicativos abertos no momento | Passar o dedo para cima com quatro dedos | Dedo para cima de três dedos |
| Alternar entre aplicativos | N/D | Passar o dedo lateral com três dedos |
| Ir para área de trabalho | Distribuir quatro dedos | Dedo com três dedos para baixo |
| Abrir o Cortana/central de ações | Slide de dois dedos à direita | Toque de três dedos |
| Abrir informações extras | Toque de três dedos | N/D |
|Mostrar Launchpad/iniciar um aplicativo | Pinçar com quatro dedos | Toque com quatro dedos |

Observação: As opções de trackpad são configuráveis em ambas as plataformas.

## <a name="terminal-and-shell"></a>Terminal e Shell

O Windows fornece várias alternativas ao emulador de terminal do Mac.

1. A linha de comando do Windows

A linha de comando do Windows aceitará os comandos DOS e é a ferramenta de linha de comando mais usada no Windows. Para abri-lo: Pressione **Windows + R** para abrir a caixa **executar** e digite **cmd** e clique em **OK**. Para abrir uma linha de comando de administrador, digite **cmd** e pressione **Ctrl + Shift + Enter**. 

2. PowerShell

O [PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) é um "PowerShell é um shell de linha de comando baseado em tarefa e linguagem de script criado no .net. O PowerShell ajuda os administradores de sistema e os usuários a automatizar rapidamente tarefas que gerenciam sistemas operacionais. Em outras palavras, é uma linha de comando muito poderosa e é especialmente importante para administradores do sistema.

Incidentalmente, o PowerShell [também está disponível para Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Subsistema do Windows para Linux (WSL)

O WSL permite que você execute um shell do Linux no Windows. Isso significa que você pode executar o *bash** ou outro shell, dependendo da escolha e do distribuição Linux específico instalado. O uso de WSL fornecerá o tipo de ambiente mais familiar para os usuários do Mac. Por exemplo, você será o **ls** para listar os arquivos em um diretório atual, não o **dir** como você faria com a linha de comando do Windows. Para saber mais sobre como instalar e usar o WSL, consulte o [Guia de instalação do subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

## <a name="apps-and-utilities"></a>Aplicativos e utilitários

 **Aplicação** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configurações e preferências | Preferências do sistema | Configurações |
| Gerenciador de tarefas | Monitor de atividade | Gerenciador de Tarefas |
| Formatação de disco | Utilitário de disco | Gerenciamento de disco |
| Edição de texto | TextEdit | Bloco de notas |
| Exibição de eventos | Console | Visualizador de Eventos |
| Localizar arquivos/aplicativos | Comando + espaço | tecla do Windows |
