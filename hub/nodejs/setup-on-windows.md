---
title: Configurar o NodeJS no Windows nativo
description: Um guia para ajudar você a obter o ambiente de desenvolvimento do Node.js configurado diretamente no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, native windows, directly on windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 456aac17f61ab0add3d35a48c74e151fa15e9e83
ms.sourcegitcommit: 8efeb6672f759b1ea7e3e9e2f90e764480791142
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728467"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configurar o ambiente de desenvolvimento do Node.js diretamente no Windows

Veja a seguir um guia passo a passo para ajudar você a começar a usar o Node.js em um ambiente de desenvolvimento nativo do Windows.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Instalar NVM-Windows, Node.js e NPM

Há várias maneiras de instalar o Node.js. É recomendável usar um gerenciador de versão, pois as versões mudam muito rapidamente. Provavelmente, você precisará alternar entre várias versões com base nas necessidades de projetos diferentes nos quais está trabalhando. O Node Version Manager, mais comumente chamado de NVM, é a maneira mais popular de instalar várias versões do Node.js, mas só está disponível para Mac/Linux e não tem suporte no Windows. Em vez disso, veremos as etapas para instalar o NVM-Windows e, em seguida, usá-lo para instalar o Node.js e o NPM (Node Package Manager). Há [gerenciadores de versão alternativos](#alternative-version-managers) que serão considerados e abordados na próxima seção.

> [!IMPORTANT]
> É sempre recomendável remover todas as instalações existentes do Node.js ou do NPM do seu sistema operacional antes de instalar um gerenciador de versão, pois os diferentes tipos de instalação podem levar a conflitos estranhos e confusos. Isso inclui a exclusão de diretórios de instalação do Node.js existentes (por exemplo, "C:\Arquivos de Programas\nodejs") que possam permanecer. O link simbólico gerado pelo NVM não substituirá um diretório de instalação existente (mesmo vazio). Para obter ajuda com a remoção de instalações anteriores, confira [Como remover completamente o Node.js do Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).

1. Abra o [repositório do Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) no navegador da Internet e selecione o link **Baixar Agora**.
2. Baixe o arquivo **nvm-setup.zip** para obter a versão mais recente.
3. Depois de baixado, abra o arquivo zip e abra o arquivo **nvm-setup.exe**.
4. O assistente de instalação Setup-NVM-for-Windows guiará você pelas etapas de instalação, incluindo a escolha do diretório onde NVM-Windows e Node.js serão instalados.

    ![Assistente de instalação do NVM para Windows](../images/install-nvm-for-windows-wizard.png)

5. Quando a instalação for concluída. Abra o PowerShell e tente usar o Windows-NVM para listar quais versões do Node estão instaladas no momento (não deve ter nenhuma neste ponto): `nvm ls`

    ![Lista do NVM mostrando nenhuma versão do Node](../images/windows-nvm-powershell-no-node.png)

6. Instale a versão atual do Node.js (para testar os aprimoramentos dos recursos mais recentes, mas com mais probabilidade de ter problemas do que a versão LTS): `nvm install latest`
7. Instale a versão mais recente do LTS estável do Node.js (recomendada) examinando primeiro qual é o número de versão atual do LTS: `nvm list available`, em seguida, instale o número de versão do LTS com: `nvm install <version>` (substitua `<version>` pelo número, ou seja: `nvm install 12.14.0`).

    ![Lista do NVM de versões disponíveis](../images/windows-nvm-list.png)

8. Liste quais versões do Node estão instaladas: `nvm ls` ... agora você deve ver listadas as duas versões que acabou de instalar.

    ![Lista do NVM mostrando as versões do Node instaladas](../images/windows-nvm-node-installs.png)

9. Para verificar qual versão do Node.js é o padrão no momento, digite: `node --version`
10. Para alterar a versão do Node.js que você deseja usar para um projeto, crie um diretório de projeto `mkdir NodeTest` e insira o diretório `cd NodeTest`. Em seguida, digite `nvm use <version>` substituindo `<version>` pelo número de versão que deseja usar (como v10.16.3).
11. Verifique qual versão do NPM está instalada com: `npm --version`, este número de versão será alterado automaticamente para qualquer versão do NPM associada à sua versão atual do Node.js.

## <a name="alternative-version-managers"></a>Gerenciadores de versão alternativos

Embora o Windows-NVM seja atualmente o gerenciador de versão mais popular para o nó, há alternativas a serem consideradas:

- O [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) é uma alternativa `nvm` multiplataforma com a capacidade de [integração ao VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

- O [Volta](https://github.com/volta-cli/volta#installing-volta) é um novo gerenciador de versão da equipe do LinkedIn que alega uma velocidade aprimorada e suporte multiplataforma.

Para instalar o Volta como seu gerenciador de versão (em vez do Windows-NVM), vá para a seção **Instalação do Windows** de seu [guia de Introdução](https://docs.volta.sh/guide/getting-started), em seguida, baixe e execute o instalador do Windows Installer, seguindo as instruções de instalação.

> [!IMPORTANT]
> Você precisa garantir que o [Modo de Desenvolvedor](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) esteja habilitado no computador Windows antes de instalar o Volta.

Para saber mais sobre como usar o Volta para instalar várias versões do Node.js no Windows, confira os [Documentos do Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Instalar seu editor de código favorito

Recomendamos que você [instale o VS Code](https://code.visualstudio.com), bem como o [Pacote de Extensão do Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), para desenvolver com o Node.js no Windows. Instale todas ou escolha a que parecer mais útil para você.

Para instalar o pacote de extensão do Node.js:

1. Abra a janela **Extensões** (Ctrl + Shift + X) no VS Code.
2. Na caixa de pesquisa na parte superior da janela Extensões, digite: "Pacote de extensão do Node" (ou o nome de qualquer extensão que você estiver procurando).
3. Clique em **Instalar**. Uma vez instalada, sua extensão aparecerá na pasta "Habilitado" da janela **Extensões**. É possível desabilitar, desinstalar ou definir as configurações selecionando o ícone de engrenagem ao lado da descrição de sua nova extensão.

Algumas extensões adicionais que talvez você queira considerar incluem:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): depois de concluir o desenvolvimento no lado do servidor com o Node.js, você precisará desenvolver e testar o lado do cliente. Essa extensão integra seu editor do VS Code ao serviço de depuração do navegador Chrome, tornando as coisas um pouco mais eficientes.
- [Mapas de teclas de outros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): essas extensões poderão ajudar na familiaridade com seu ambiente se você estiver fazendo a transição de outro editor de texto (como Atom, Sublime, Vim, eMacs, Notepad++ etc.).
- [Sincronização de configurações](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): permite que você sincronize suas configurações do VS Code em diferentes instalações usando o GitHub. Se você trabalha em diferentes computadores, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="install-git-optional"></a>Instalar o Git (opcional)

Se você pretende colaborar com outras pessoas ou hospedar seu projeto em um site de software livre (como o GitHub), o VS Code é compatível com o [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia Controle do Código-fonte no VS Code acompanha todas as alterações e tem comandos Git comuns (add, commit, push e pull) incorporados diretamente na interface do usuário. Primeiro, você precisa instalar o Git para ativar o painel Controle do Código-fonte.

1. Baixe e instale o Git para Windows no [site do git-scm](https://git-scm.com/download/win).

2. Um Assistente de Instalação está incluído e fará uma série de perguntas sobre as configurações da instalação do Git. Recomendamos o uso de todas as configurações padrão, a menos que você tenha um motivo específico para alterar algo.

3. Se você nunca trabalhou com o Git antes, os [Guias do GitHub](https://guides.github.com/) podem ajudar você a começar a usá-lo.

4. É recomendável adicionar um [arquivo .gitignore](https://help.github.com/en/articles/ignoring-files) aos seus projetos do Node. Este é o [modelo de gitignore padrão do GitHub para Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore).

## <a name="use-windows-subsystem-for-linux-for-production"></a>Usar o Subsistema do Windows para Linux para produção

O uso do Node.js diretamente no Windows é ótimo para aprender e experimentar o que você pode fazer. Quando você estiver pronto para criar aplicativos Web prontos para produção, que normalmente são implantados em um servidor baseado em Linux, recomendamos o uso do Subsistema do Windows para Linux versão 2 (WSL 2) para o desenvolvimento de aplicativos Web Node.js. Muitos pacotes e estruturas do Node.js são criados com um ambiente nix* em mente e a maioria dos aplicativos Node.js é implantada no Linux. Portanto, o desenvolvimento em WSL garante a consistência entre seus ambientes de desenvolvimento e de produção. Para configurar um ambiente de desenvolvimento WSL, confira [Configurar o ambiente de desenvolvimento do Node.js com o WSL 2](./setup-on-wsl2.md).

> [!NOTE]
> Se você está na situação (um pouco rara) de precisar hospedar um aplicativo Node.js em um servidor Windows, o cenário mais comum parece ser [usar um proxy reverso](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Veja aqui duas maneiras de fazer isso: 1) [usando iisnode](https://harveywilliams.net/blog/installing-iisnode) ou [diretamente](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). Não mantemos esses recursos e recomendamos [usar servidores Linux para hospedar seus aplicativos Node.js](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs).
