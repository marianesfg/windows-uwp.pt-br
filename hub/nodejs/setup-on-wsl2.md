---
title: Configurar o NodeJS no WSL 2
description: Um guia para ajudar você a obter o ambiente de desenvolvimento do Node.js configurado no WSL (Subsistema do Windows para Linux).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, learning nodejs, node on windows, node on wsl, node on linux on windows, install node on windows, nodejs with vs code, develop with node on windows, develop with nodejs on windows, install node on WSL, NodeJS on Windows Subsystem for Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 1ea8973e1db665d1fe66ef6b5f5699319131d605
ms.sourcegitcommit: 2af814b7f94ee882f42fae8f61130b9cc9833256
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83717125"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurar o ambiente de desenvolvimento do Node.js com o WSL 2

É possível ver, abaixo, um guia passo a passo para ajudar você a obter o ambiente de desenvolvimento do Node.js configurado no WSL (Subsistema do Windows para Linux).

É recomendável instalar e executar o WSL 2 atualizado, pois você se beneficiará de melhorias significativas na velocidade do desempenho e de compatibilidade com a chamada do sistema, incluindo a capacidade de executar o [Desktop do Docker](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download). Muitos dos tutoriais e módulos de npm para desenvolvimento para a Web do Node.js são escritos para usuários do Linux e usam ferramentas de instalação e empacotamento baseadas em Linux. A maioria dos aplicativos Web também é implantada no Linux, portanto, usar o WSL 2 garantirá a consistência entre os ambientes de desenvolvimento e produção.

> [!NOTE]
> Se você estiver comprometido em usar o Node.js diretamente no Windows ou se planejar usar um ambiente de produção do Windows Server, confira nosso guia para [Configurar o ambiente de desenvolvimento do Node.js diretamente no Windows](./setup-on-windows.md).

## <a name="install-wsl-2"></a>Instalar o WSL 2

Para habilitar e instalar o WSL 2, siga as etapas na [documentação de instalação do WSL](https://docs.microsoft.com/windows/wsl/install-win10). Essas etapas incluirão a escolha da distribuição do Linux (por exemplo, o Ubuntu).

Após instalar o WSL 2 e uma distribuição do Linux, abra a distribuição do Linux (encontrada no menu Iniciar do Windows) e verifique a versão e o codinome usando o comando: `lsb_release -dc`.

É recomendável atualizar a distribuição do Linux regularmente, inclusive imediatamente após a instalação, para garantir que os pacotes são os mais recentes. O Windows não cuida automaticamente dessa atualização. Para atualizar a distribuição, use o comando: `sudo apt update && sudo apt upgrade`.  

## <a name="install-windows-terminal-optional"></a>Instalar o Terminal do Windows (opcional)

O novo Terminal do Windows permite habilitar várias guias (para alternar rapidamente entre as linhas de comando do Linux, o prompt de comando do Windows, o PowerShell, a CLI do Azure etc.), criar associações de chave personalizadas (teclas de atalho para abrir ou fechar guias, copiar + colar etc.), usar o recurso de pesquisa e configurar temas personalizados (esquemas de cores, estilos e tamanhos de fonte, imagem de tela de fundo/desfoque/transparência). [Saiba mais](https://docs.microsoft.com/windows/terminal).

1. Obtenha o [Terminal do Windows (versão prévia) na Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): Ao instalar por meio da loja, as atualizações serão manipuladas automaticamente.

2. Depois de instalado, abra o Terminal do Windows e selecione **Configurações** para personalizar o terminal usando o arquivo `settings.json`.

    ![Configurações do Terminal do Windows](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>Instalar NVM, Node.js e NPM

Há várias maneiras de instalar o Node.js. É recomendável usar um gerenciador de versão, pois as versões mudam muito rapidamente. Provavelmente, você precisará alternar entre várias versões com base nas necessidades de projetos diferentes nos quais está trabalhando. O Node Version Manager, mais comumente chamado de NVM, é a maneira mais popular de instalar várias versões do Node.js. Veremos as etapas para instalar o NVM e, em seguida, usá-lo para instalar o Node.js e o NPM (Node Package Manager). Há [gerenciadores de versão alternativos](#alternative-version-managers) que serão considerados e abordados na próxima seção.

> [!IMPORTANT]
> É sempre recomendável remover todas as instalações existentes do Node.js ou do NPM do seu sistema operacional antes de instalar um gerenciador de versão, pois os diferentes tipos de instalação podem levar a conflitos estranhos e confusos. Por exemplo, a versão do Node que pode ser instalada com o comando `apt-get` do Ubuntu está desatualizada no momento. Para obter ajuda com a remoção de instalações anteriores, confira [Como remover o Node.js do Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).

1. Abra a linha de comando do Ubuntu 18.04.
2. Instale o cURL (uma ferramenta usada para baixar conteúdo da Internet na linha de comando) com: `sudo apt-get install curl`
3. Instale o NVM, com: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. Para verificar a instalação, digite: `command -v nvm` ...isso deverá retornar 'nvm'. Se você receber 'comando não encontrado' ou nenhuma resposta, feche o terminal atual, reabra-o e tente novamente. [Saiba mais no repositório GitHub do NVM](https://github.com/nvm-sh/nvm).
5. Lista quais versões do Node estão instaladas no momento (não deve ter nenhuma neste ponto): `nvm ls`

    ![Lista do NVM mostrando nenhuma versão do Node](../images/nvm-no-node.png)

6. Instale a versão atual do Node.js (para testar os aprimoramentos dos recursos mais recentes, mas com mais probabilidade de ter problemas): `nvm install node`
7. Instale a versão mais recente e estável do LTS do Node.js (recomendado): `nvm install --lts`
8. Liste quais versões do Node estão instaladas: `nvm ls` ... agora você deve ver listadas as duas versões que acabou de instalar.

    ![Lista do NVM mostrando as versões LTS e atual do Node](../images/nvm-node-installed.png)

9. Verifique se o Node.js está instalado e a versão padrão atual com: `node --version`. Em seguida, verifique se você também tem o NPM, com: `npm --version` (Você também pode usar `which node` ou `which npm` para ver o caminho usado para as versões padrão).
10. Para alterar a versão do Node.js que você deseja usar para um projeto, crie um diretório de projeto `mkdir NodeTest` e insira o diretório `cd NodeTest`. Em seguida, digite `nvm use node` para alternar para a versão atual ou `nvm use --lts` para alternar para a versão LTS. Você também pode usar o número específico para qualquer versão adicional instalada, como `nvm use v8.2.1`. (Para listar todas as versões disponíveis do Node.js, use o comando: `nvm ls-remote`).

Se você estiver usando o NVM para instalar o Node.js e o NPM, não será necessário usar o comando SUDO para instalar novos pacotes.

> [!NOTE]
> No momento da publicação, o NVM v0.35.2 era a versão mais recente disponível. Você pode verificar a [página do projeto GitHub para obter a versão mais recente do NVM](https://github.com/nvm-sh/nvm) e ajustar o comando acima para incluir a versão mais recente.
A instalação da versão mais recente do NVM usando cURL substituirá a mais antiga, deixando intacta a versão do Node que você usou o NVM para instalar. Por exemplo: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Gerenciadores de versão alternativos

Embora o NVM seja atualmente o gerenciador de versão mais popular para o nó, há algumas alternativas a serem consideradas:

- O [n](https://www.npmjs.com/package/n#installation) é uma alternativa `nvm` de longa duração que realiza a mesma coisa com comandos ligeiramente diferentes e é instalada por meio de `npm`, em vez de um script Bash.
- O [fnm](https://github.com/Schniz/fnm#using-a-script) é um gerenciador de versão mais recente, que alega ser muito mais rápido do que `nvm`. (Ele também usa o [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops).)
- O [Volta](https://github.com/volta-cli/volta#installing-volta) é um novo gerenciador de versão da equipe do LinkedIn que alega uma velocidade aprimorada e suporte multiplataforma.
- O [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) é uma CLI para vários idiomas, como ike gvm, nvm, rbenv e pyenvv (e muito mais), tudo em um.
- O [nvs](https://github.com/jasongin/nvs) (Node Version Switcher) é uma alternativa `nvm` multiplataforma com a capacidade de [integração ao VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Instalar seu editor de código favorito

É recomendável usar o **Visual Studio Code** com a **Extensão Remote-WSL** para projetos Node.js. Isso divide o VS Code em uma arquitetura de "cliente-servidor", com o cliente (a interface do usuário) em execução no computador do Windows e o servidor (seu código, git, plug-ins etc.) em execução remota.

- Há suporte para lint e IntelliSense baseado em Linux.
- Seu projeto será criado automaticamente no Linux.
- Você pode usar todas as suas extensões em execução no Linux ([ES Lint, NPM IntelliSense, snippets ES6 etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Os editores de texto baseados em terminal (vim, emacs, nano) também são úteis para fazer alterações rápidas diretamente dentro do seu console. ([Este artigo](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) faz um bom trabalho explicando a diferença e um pouco sobre como usar cada um deles.)

> [!NOTE]
> Alguns editores de GUI (Atom, Sublime Text, Eclipse) podem ter problemas para acessar a localização da rede compartilhada do WSL (\\wsl$\Ubuntu\home\) e tentarão criar seus arquivos do Linux usando as ferramentas do Windows, o que pode não ser o que você deseja. A extensão Remote-WSL no VS Code tratará dessa compatibilidade para você.

Para instalar o VS Code e a extensão Remote-WSL:

1. [Baixe e instale o VS Code para Windows](https://code.visualstudio.com). O VS Code também está disponível para Linux, mas o Subsistema do Windows para Linux não dá suporte a aplicativos de GUI e, portanto, precisamos instalá-lo no Windows. Não se preocupe, você ainda poderá fazer a integração à linha de comando e às ferramentas do Linux usando a Extensão WSL – Remoto.

2. Instale a [Extensão WSL – Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) no VS Code. Isso permite que você use o WSL como o ambiente de desenvolvimento integrado e cuidará da compatibilidade e dos caminhos para você. [Saiba mais](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Se você já tiver o VS Code instalado, precisará verificar se tem a [versão de maio 1.35](https://code.visualstudio.com/updates/v1_35) ou posterior para instalar a [Extensão do WSL Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Não recomendamos o uso do WSL no VS Code sem a extensão do WSL Remoto, pois você perderá o suporte de preenchimento automático, depuração, linting etc. Curiosidade: Essa extensão do WSL é instalada em $HOME/.vscode-server/extensions.

### <a name="helpful-vs-code-extensions"></a>Extensões úteis do VS Code

Embora o VS Code venha com muitos recursos prontos para uso para desenvolvimento do Node.js, há algumas extensões úteis que você pode considerar instalar e que estão disponíveis no [Pacote de extensão do Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack). Instale todas ou escolha a que parecer mais útil para você.

Para instalar o pacote de extensão do Node.js:

1. Abra a janela **Extensões** (Ctrl + Shift + X) no VS Code.

    A janela Extensões agora está dividida em três seções (porque você instalou a extensão Remote-WSL).
    - "Local – Instalado": as extensões instaladas para uso com o sistema operacional Windows.
    - "WSL:Ubuntu-18.04 – Instalado": as extensões instaladas para uso com o sistema operacional Ubuntu (WSL).
    - "Recomendado": as extensões recomendadas pelo VS Code com base nos tipos de arquivo em seu projeto atual.

    ![Extensões do VS Code locais vs. remotas](../images/vscode-extensions-local-remote.png)

2. Na caixa de pesquisa na parte superior da janela Extensões, digite: **Pacote de extensão do Node** (ou o nome de qualquer extensão que você estiver procurando). A extensão será instalada para as instâncias local ou WSL do VS Code, dependendo de onde você tiver aberto o projeto atual. Você pode saber selecionando o link remoto no canto inferior esquerdo da janela do VS Code (em verde). Ele fornecerá a opção de abrir ou fechar uma conexão remota. Instale suas extensões do Node.js no ambiente "WSL:Ubuntu-18.04".

    ![Link remoto do VS Code](../images/wsl-remote-extension.png)

Algumas extensões adicionais que talvez você queira considerar incluem:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): depois de concluir o desenvolvimento no lado do servidor com o Node.js, você precisará desenvolver e testar o lado do cliente. Essa extensão integra seu editor do VS Code ao serviço de depuração do navegador Chrome, tornando as coisas um pouco mais eficientes.
- [Mapas de teclas de outros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): essas extensões poderão ajudar na familiaridade com seu ambiente se você estiver fazendo a transição de outro editor de texto (como Atom, Sublime, Vim, eMacs, Notepad++ etc.).
- [Sincronização de configurações](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): permite que você sincronize suas configurações do VS Code em diferentes instalações usando o GitHub. Se você trabalha em diferentes computadores, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="set-up-git-optional"></a>Configurar o Git (opcional)

Se você pretende colaborar com outras pessoas ou hospedar seu projeto em um site de software livre (como o GitHub), o VS Code é compatível com o [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia Controle do Código-fonte no VS Code acompanha todas as alterações e tem comandos Git comuns (add, commit, push e pull) incorporados diretamente na interface do usuário.

1. O Git vem instalado com as distribuições do Subsistema do Windows para Linux. No entanto, você precisará configurar o arquivo git config. Para fazer isso, em seu terminal, digite: `git config --global user.name "Your Name"` e, em seguida, `git config --global user.email "youremail@domain.com"`. Se você ainda não tiver uma conta do Git, poderá [se inscrever em uma no GitHub](https://github.com/join). Se você nunca trabalhou com o Git antes, os [Guias do GitHub](https://guides.github.com/) podem ajudar você a começar a usá-lo. Se você precisar editar o git config, poderá fazer isso com um editor de texto interno como o nano: `nano ~/.gitconfig`.

2. É recomendável adicionar um [arquivo .gitignore](https://help.github.com/en/articles/ignoring-files) aos seus projetos do Node. Este é o [modelo de gitignore padrão do GitHub para Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore). Se você optar por [criar um repositório usando o site do GitHub](https://help.github.com/articles/create-a-repo), verá caixas de seleção disponíveis para inicializar o repositório com um arquivo LEIAME, arquivo .gitignore configurado para projetos do Node.js e opções para adicionar uma licença se você precisar de uma.

## <a name="next-steps"></a>Próximas etapas

Agora você tem um ambiente de desenvolvimento do Node.js configurado. Para começar a usar o ambiente do Node.js, considere tentar um destes tutoriais:

- [Introdução ao Node.js para iniciantes](./beginners.md)
- [Introdução às estruturas da Web do Node.js no Windows](./web-frameworks.md)
- [Introdução à conexão de aplicativos do Node.js a um banco de dados](./databases.md)
- [Introdução ao uso de contêineres do Docker com o Node.js](./containers.md)
