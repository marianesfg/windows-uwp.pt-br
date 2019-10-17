---
title: Configurar o NodeJS no Windows nativo
description: Um guia para ajudá-lo a obter o ambiente de desenvolvimento do node. js configurado diretamente no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node. js, Windows 10, nativo do Windows, diretamente no Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 18a8d07f790c391a6e10577ff512347106e1cf21
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517834"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configurar o ambiente de desenvolvimento do node. js diretamente no Windows

Veja a seguir um guia passo a passo para ajudá-lo a começar a usar o Node. js em um ambiente de desenvolvimento nativo do Windows.

> [!NOTE]
> Embora usar o Node. js no Windows seja certamente uma opção viável, geralmente recomendamos o uso do subsistema do Windows para Linux (WSL) para o desenvolvimento de aplicativos Web node. js. Muitos pacotes e estruturas de Node. js são criados com um ambiente * Nix em mente e a maioria dos aplicativos node. js são implantados no Linux, portanto, o desenvolvimento em WSL garante a consistência entre seus ambientes de desenvolvimento e produção. Para configurar um ambiente de desenvolvimento WSL, consulte [Configurar o ambiente de desenvolvimento do node. js com o WSL 2](./setup-on-wsl2.md).

## <a name="install-nvm-windows-nodejs-and-npm"></a>Instalar NVM-Windows, Node. js e NPM

Há várias maneiras de instalar o Node. js. É recomendável usar um Gerenciador de versão como versões alteradas muito rapidamente. Provavelmente, você precisará alternar entre várias versões com base nas necessidades de projetos diferentes nos quais está trabalhando. O node versão Manager, mais comumente chamado de NVM, é a maneira mais popular de instalar várias versões do node. js, mas só está disponível para Mac/Linux e sem suporte no Windows. Em vez disso, veremos as etapas para instalar o NVM-Windows e, em seguida, usá-lo para instalar o Node. js e o Gerenciador de pacotes de nó (NPM). Há [gerenciadores de versão alternativos](#alternative-version-managers) a considerar também abordados na próxima seção.

> [!IMPORTANT]
> É sempre recomendável remover todas as instalações existentes do node. js ou do NPM do seu sistema operacional antes de instalar um Gerenciador de versão, pois os diferentes tipos de instalação podem levar a conflitos estranhos e confusos. Isso inclui a exclusão de quaisquer diretórios de instalação do NodeJS existentes (por exemplo, "C:\Program Files\nodejs") que possam permanecer. O symlink gerado pelo NVM não substituirá um diretório de instalação existente (mesmo vazio). Para obter ajuda com a remoção de instalações anteriores, consulte [como remover completamente o Node. js do Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).)

1. Abra o [repositório do Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) no navegador da Internet e selecione o link **baixar agora** .
2. Baixe o arquivo **NVM-setup. zip** para a versão mais recente.
3. Depois de baixado, abra o arquivo zip e, em seguida, abra o arquivo **NVM-setup. exe** .
4. O assistente de instalação do setup-NVM-for-Windows guiará você pelas etapas de instalação, incluindo a escolha do diretório onde NVM-Windows e node. js serão instalados.

    ![Assistente de instalação do NVM para Windows](../images/install-nvm-for-windows-wizard.png)

5. Quando a instalação for concluída. Abra o PowerShell e tente usar o Windows-NVM para listar quais versões do nó estão instaladas no momento (não deve ser nenhuma neste ponto): `nvm ls`

    ![Lista de NVM mostrando nenhuma versão de nó](../images/windows-nvm-powershell-no-node.png)

6. Instale a versão atual do node. js (para testar as melhorias de recursos mais recentes, mas com mais probabilidade de ter problemas do que a versão LTS): `nvm install latest`
7. Instale a versão mais recente do LTS estável do node. js (recomendado) examinando primeiro qual é o número de versão atual do LTS: `nvm list available`, instalando o número de versão do LTS com: `nvm install <version>` (substituindo `<version>` pelo número, IE: `nvm install 10.16.3`).

    ![NVM lista de versões disponíveis](../images/windows-nvm-list.png)

8. Lista de quais versões do nó estão instaladas: `nvm ls`... Agora você deve ver as duas versões que acabou de instalar listadas.

    ![Lista de NVM mostrando as versões de nó instaladas](../images/windows-nvm-node-installs.png)

9. Para verificar qual versão do node. js é o padrão no momento, digite: `node --version`
10. Para alterar a versão do node. js que você gostaria de usar para um projeto, crie um novo diretório de projeto `mkdir NodeTest` e insira o diretório `cd NodeTest` e, em seguida, digite `nvm use <version>` substituindo `<version>` pelo número de versão que você deseja usar (IE v 10.16.3 ').
11. Verifique qual versão do NPM está instalada com: `npm --version`, este número de versão será alterado automaticamente para qualquer versão do NPM associada à sua versão atual do node. js.

## <a name="alternative-version-managers"></a>Gerenciadores de versão alternativos

Embora o Windows-NVM seja atualmente o Gerenciador de versão mais popular para o nó, há alternativas a serem consideradas:

- [NVS](https://github.com/jasongin/nvs) (alternador de versão do nó) é uma alternativa `nvm` de plataforma cruzada com a capacidade de [integrar com vs Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).
- 
- [Voltar](https://github.com/volta-cli/volta#installing-volta) é um novo Gerenciador de versões da equipe do LinkedIn que alega uma velocidade aprimorada e suporte de plataforma cruzada.

Para fazer a instalação voltar como seu Gerenciador de versão (em vez do Windows-NVM), vá para a seção **instalação do Windows** de seu [Guia de introdução](https://docs.volta.sh/guide/getting-started), baixe e execute o Windows Installer, seguindo as instruções de instalação.

> [!IMPORTANT]
> Você deve garantir que o [modo de desenvolvedor](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) esteja habilitado no computador Windows antes da instalação.

Para saber mais sobre como usar voltar para instalar várias versões do node. js no Windows, consulte o [documentos de voltar](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Instalar seu editor de código favorito

Recomendamos que você [instale vs Code](https://code.visualstudio.com), bem como o [pacote de extensão do node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), para o desenvolvimento com node. js no Windows. Instale todos ou escolha e escolha o que parece mais útil para você.

Para instalar o pacote de extensão do node. js:

1. Abra a janela **extensões** (Ctrl + Shift + X) em vs Code.
2. Na caixa de pesquisa na parte superior da janela extensões, digite: "pacote de extensão de nó" (ou o nome de qualquer extensão que você esteja procurando).
3. Selecione **instalar**. Uma vez instalado, sua extensão será exibida na pasta "habilitado" da janela **extensões** . Você pode desabilitar, desinstalar ou definir as configurações selecionando o ícone de engrenagem ao lado da descrição de sua nova extensão.

Algumas extensões adicionais que talvez você queira considerar incluem:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): depois de concluir o desenvolvimento no lado do servidor com o Node. js, você precisará desenvolver e testar o lado do cliente. Essa extensão integra seu editor de VS Code com o serviço de depuração do navegador Chrome, tornando as coisas um pouco mais eficientes.
- [Keymaps de outros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): essas extensões podem ajudar seu ambiente a se sentir em casa, se você estiver fazendo a transição de outro editor de texto (como Atom, sublime, vim, Emacs, notepad + +, etc.).
- [Sincronização de configurações](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): permite que você sincronize suas configurações de vs Code em diferentes instalações usando o github. Se você trabalha em computadores diferentes, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você planeja colaborar com outras pessoas ou hospedar seu projeto em um site de software livre (como o GitHub), VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia controle do código-fonte no VS Code rastreia todas as suas alterações e tem comandos git comuns (adicionar, confirmar, enviar por push, pull) criados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ligar o painel de controle do código-fonte.

1. Baixe e instale o Git para Windows no [site do git-SCM](https://git-scm.com/download/win).

2. Um assistente de instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do git. É recomendável usar todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [guias do GitHub](https://guides.github.com/) podem ajudá-lo a começar.

4. É recomendável adicionar um [arquivo. gitignore](https://help.github.com/en/articles/ignoring-files) aos seus projetos de nó. Aqui está o [modelo de gitignore padrão do GitHub para node. js](https://github.com/github/gitignore/blob/master/Node.gitignore).
