---
title: Configurar o NodeJS no WSL 2
description: Um guia para ajudá-lo a obter o ambiente de desenvolvimento do node. js configurado no subsistema do Windows para Linux (WSL).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, nó no Windows, nó em WSL, nó no Linux no Windows, instalar nó no Windows, NodeJS com vs Code, desenvolver com nó no Windows, desenvolver com NodeJS no Windows, instalar nó em WSL, NodeJS no Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: cf4bf0ab4ea9019c1edc2bb96387ce6cedbe91dd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835376"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurar o ambiente de desenvolvimento do node. js com o WSL 2

Veja a seguir um guia passo a passo para ajudá-lo a obter o ambiente de desenvolvimento do node. js configurado usando o subsistema do Windows para Linux (WSL). Este guia atualmente requer que você instale e execute uma compilação do Windows Insider Preview para instalar e usar o [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/). O WSL 2 tem melhorias significativas de velocidade e desempenho em relação ao WSL 1, especialmente em relação ao node. js. Muitos módulos NPM e tutoriais para o desenvolvimento Web node. js são escritos para usuários do Linux e usam ferramentas de instalação e empacotamento baseadas em Linux. A maioria dos aplicativos Web também é implantada no Linux. portanto, usar o WSL 2 garantirá a consistência entre seus ambientes de desenvolvimento e produção.

> [!NOTE]
> Se você estiver comprometido usando o Node. js diretamente no Windows ou se planeja usar um ambiente de produção do Windows Server, consulte nosso guia para [Configurar o ambiente de desenvolvimento do node. js diretamente no Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Instalar o Build do Windows 10 Insider Preview

1. **[Instalar a versão mais recente do Windows 10](https://www.microsoft.com/software-download/windows10)** : selecione **Atualizar agora** para baixar o assistente de atualização. Depois de baixado, abra o assistente de atualização para ver se você está executando a versão mais recente do Windows e, se não estiver, selecione **Atualizar agora** na janela do assistente para atualizar seu computador. *(Essa etapa é opcional se você estiver executando uma versão relativamente recente do Windows 10.)*

    ![Assistente de Windows Update](../images/windows-update-assistant2019.png)

2. **[Vá para iniciar > configurações > programa do Windows Insider](ms-settings:windowsinsider)** : na janela do programa Windows Insider **, selecione introdução** e **vincule uma conta**.

    ![Configurações do programa Windows Insider](../images/windows-insider-program-settings.png)

3. **[Registre-se como um Windows Insider](https://insider.windows.com/getting-started/#register)** : se você não estiver registrado com o programa Insider, você precisará fazer isso com seu [conta Microsoft](https://account.microsoft.com/account).

    ![Registro do Windows Insider](../images/windows-insider-account.png)

4. Opte por receber atualizações de **anel rápido** ou **pule para o próximo conteúdo da versão do Windows** . Confirme e escolha **reiniciar mais tarde**. Precisaremos alterar algumas configurações adicionais antes de reiniciar.

    ![Anel rápido do Windows Insider](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Habilitar o subsistema do Windows para Linux e plataforma de máquina virtual

1. Enquanto ainda estiver nas **configurações do Windows**, procure ativar **ou desativar recursos do Windows**.
2. Depois que a lista de **recursos do Windows** for exibida, role para localizar **plataforma de máquina virtual** e **subsistema do Windows para Linux**, verifique se a caixa está marcada para habilitar ambas e selecione **OK**.
3. Reinicie seu computador quando solicitado.

    ![Habilitar recursos do Windows](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Instalar uma distribuição do Linux

Existem várias distribuições do Linux disponíveis para execução no WSL. Encontre suas favoritas na Microsoft Store e instale-as. Recomendamos começar com o [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), pois é atual, popular e tem um bom suporte.

1. Abra este link do [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), abra a Microsoft Store e selecione **Obter**. *(Esse é um download razoavelmente grande e pode levar algum tempo para ser instalado.)*

2. Após a conclusão do download, selecione **Iniciar** na Microsoft Store ou inicie-o digitando "Ubuntu 18.04 LTS" no menu **Iniciar**.

3. Você deverá criar um nome e uma senha de conta ao executar uma distribuição pela primeira vez. Depois disso, você será automaticamente conectado como esse usuário por padrão. Você pode escolher qualquer nome de usuário e senha. Eles não têm nenhuma influência no seu nome de usuário do Windows.

    ![Distribuições do Linux no Microsoft Store](../images/store-linux-distros.png)

Verifique a distribuição do Linux que você está usando no momento inserindo `lsb_release -dc`. Para atualizar a distribuição do Ubuntu, use `sudo apt update && sudo apt upgrade`. Recomendamos fazer uma atualização regular para garantir que você tenha os pacotes mais recentes. O Windows não cuida automaticamente dessa atualização. Para obter links para outras distribuições do Linux disponíveis na Microsoft Store, métodos de instalação alternativos ou solução de problemas, confira [Guia de Instalação do Subsistema do Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Instalar o WSL 2

WSL 2 é uma [nova versão da arquitetura](https://docs.microsoft.com/windows/wsl/wsl2-about) no WSL que muda a forma como o Linux distribuições interage com o Windows, melhorando o desempenho e adicionando compatibilidade total com a chamada do sistema.

1. No PowerShell, digite o comando: `wsl -l` para exibir a lista de distribuições WSL que você instalou em seu computador. Agora você deve ver Ubuntu-18, 4 nesta lista.
2. Agora, digite o comando: `wsl --set-version Ubuntu-18.04 2` para definir sua instalação do Ubuntu para usar o WSL 2.
3. Verifique se a versão do WSL de cada uma das distribuições instaladas está usando com: `wsl --list --verbose` (ou `wsl -l -v`).

    ![Versão de conjunto do subsistema do Windows para Linux](../images/wsl-versions.png)

> [!TIP]
> Você pode definir qualquer distribuição do Linux que tenha instalado no WSL 2 seguindo as mesmas instruções (usando o PowerShell), basta alterar "Ubuntu-18, 4" para o nome do distribuição instalado que você deseja direcionar. Para alterar de volta para WSL 1, execute o mesmo comando acima, mas substituindo ' 2 ' por um ' 1 '.  Você também pode definir WSL 2 como o padrão para as distribuições recentemente instaladas digitando: `wsl --set-default-version 2`.

## <a name="install-nvm-nodejs-and-npm"></a>Instalar NVM, Node. js e NPM

Há várias maneiras de instalar o Node. js. É recomendável usar um Gerenciador de versão como versões alteradas muito rapidamente. Provavelmente, você precisará alternar entre várias versões com base nas necessidades de projetos diferentes nos quais está trabalhando. O node versão Manager, mais comumente chamado de NVM, é a maneira mais popular de instalar várias versões do node. js. Veremos as etapas para instalar o NVM e, em seguida, usá-lo para instalar o Node. js e o Gerenciador de pacotes de nó (NPM). Há [gerenciadores de versão alternativos](#alternative-version-managers) a considerar também abordados na próxima seção.

> [!IMPORTANT]
> É sempre recomendável remover todas as instalações existentes do node. js ou do NPM do seu sistema operacional antes de instalar um Gerenciador de versão, pois os diferentes tipos de instalação podem levar a conflitos estranhos e confusos. Por exemplo, a versão do nó que pode ser instalada com o comando `apt-get` do Ubuntu está desatualizada no momento. Para obter ajuda com a remoção de instalações anteriores, consulte [como remover o NodeJS do Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).)

1. Abra a linha de comando do Ubuntu 18, 4.
2. Instalação de ondulação (uma ferramenta usada para baixar conteúdo da Internet na linha de comando) com: `sudo apt-get install curl`
3. Instale o NVM, com: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. Para verificar a instalação, digite: `command -v nvm`... Isso deve retornar ' NVM ', se você receber ' comando não encontrado ' ou nenhuma resposta, feche o terminal atual, reabra-o e tente novamente. [Saiba mais no repositório GitHub do NVM](https://github.com/nvm-sh/nvm).
5. Lista quais versões do nó estão instaladas no momento (não deve ser nenhuma neste ponto): `nvm ls`

    ![Lista de NVM mostrando nenhuma versão de nó](../images/nvm-no-node.png)

6. Instale a versão atual do node. js (para testar as melhorias de recursos mais recentes, mas tem mais probabilidade de ter problemas): `nvm install node`
7. Instale a versão mais recente do LTS estável do node. js (recomendado): `nvm install --lts`
8. Lista de quais versões do nó estão instaladas: `nvm ls`... Agora você deve ver as duas versões que acabou de instalar listadas.

    ![Lista de NVM mostrando as versões de LTS e nó atual](../images/nvm-node-installed.png)

9. Verifique se o Node. js está instalado e a versão padrão atual com: `node --version`. Em seguida, verifique se você tem o NPM também, com: `npm --version` (você também pode usar `which node` ou `which npm` para ver o caminho usado para as versões padrão).
10. Para alterar a versão do node. js que você deseja usar para um projeto, crie um novo diretório do projeto `mkdir NodeTest`e insira o diretório `cd NodeTest`, em seguida, digite `nvm use node` para alternar para a versão atual ou `nvm use --lts` para alternar para a versão LTS. Você também pode usar o número específico para qualquer versão adicional instalada, como `nvm use v8.2.1`. (Para listar todas as versões do node. js disponíveis, use o comando: `nvm ls-remote`).

> [!TIP]
> Se você estiver usando o NVM para instalar o Node. js e o NPM, não será necessário usar o comando SUDO para instalar novos pacotes.

> [!NOTE]
> No momento da publicação, o NVM v 0.35.2 foi a versão mais recente disponível. Você pode verificar a [página de projeto do GitHub para obter a versão mais recente do NVM](https://github.com/nvm-sh/nvm)e ajustar o comando acima para incluir a versão mais recente.
A instalação da versão mais recente do NVM usando a ondulação substituirá a mais antiga, deixando a versão do nó que você usou NVM para instalar intacto. Por exemplo: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Gerenciadores de versão alternativos

Embora NVM seja atualmente o Gerenciador de versão mais popular para o nó, há algumas alternativas a serem consideradas:

- [n](https://www.npmjs.com/package/n#installation) é uma alternativa de `nvm` de longa duração que realiza a mesma coisa com comandos ligeiramente diferentes e é instalada por meio de `npm` em vez de um script bash.
- [FNM](https://github.com/Schniz/fnm#using-a-script) é um Gerenciador de versão mais recente, alegando ser muito mais rápido do que `nvm`. (Ele também usa [Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops).)
- [Voltar](https://github.com/volta-cli/volta#installing-volta) é um novo Gerenciador de versões da equipe do LinkedIn que alega uma velocidade aprimorada e suporte de plataforma cruzada.
- [asdf-VM](https://asdf-vm.com/#/core-manage-asdf-vm) é uma única CLI para vários idiomas, como Ike GVM, NVM, rbenv & pyenv (e mais) em um.
- [NVS](https://github.com/jasongin/nvs) (alternador de versão do nó) é uma plataforma cruzada `nvm` alternativa com a capacidade de [integrar com vs Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Instalar seu editor de código favorito

É recomendável usar **Visual Studio Code** com a **extensão WSL remota** para projetos node. js. Isso divide VS Code em uma arquitetura de "cliente-servidor", com o cliente (a interface do usuário) em execução no computador com Windows e no servidor (seu código, git, plug-ins, etc) em execução remota.

- Há suporte para o IntelliSense e o despano baseado em Linux.
- Seu projeto será criado automaticamente no Linux.
- Você pode usar todas as suas extensões em execução no Linux ([es fiapos, NPM IntelliSense, trechos de ES6, etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Os editores de texto baseados em terminal (vim, Emacs, Nano) também são úteis para fazer alterações rápidas diretamente dentro do seu console. ([Este artigo](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) faz um bom trabalho explicando a diferença e um pouco sobre como usar cada um deles.)

> [!NOTE]
> Alguns editores de GUI (Atom, Sublime Text, Eclipse) podem ter problemas para acessar o local de rede compartilhada WSL (\\WSL $ \Ubuntu\home\) e tentará criar seus arquivos do Linux usando as ferramentas do Windows, que podem não ser as que você deseja. A extensão WSL remota no VS Code tratará dessa compatibilidade para você.

Para instalar VS Code e a extensão WSL remota:

1. [Baixe e instale o VS Code para Windows](https://code.visualstudio.com). O VS Code também está disponível para Linux, mas o Subsistema do Windows para Linux não dá suporte a aplicativos de GUI e, portanto, precisamos instalá-lo no Windows. Não se preocupe, você ainda poderá fazer a integração à linha de comando e às ferramentas do Linux usando a Extensão WSL – Remoto.

2. Instale a [Extensão WSL – Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) no VS Code. Isso permite que você use o WSL como o ambiente de desenvolvimento integrado e cuidará da compatibilidade e dos caminhos para você. [Saiba mais](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Se você já tiver o VS Code instalado, precisará verificar se tem a [versão de maio 1.35](https://code.visualstudio.com/updates/v1_35) ou posterior para instalar a [Extensão do WSL Remoto](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). Não recomendamos o uso de WSL em VS Code sem a extensão WSL remota, pois você perderá o suporte para preenchimento automático, depuração, refiapo, etc. Fatos divertidos: essa extensão WSL é instalada em $HOME/.vscode-Server/Extensions.

### <a name="helpful-vs-code-extensions"></a>Extensões de VS Code úteis

Embora VS Code venha com muitos recursos para o desenvolvimento do node. js prontos para uso, há algumas extensões úteis para considerar a instalação disponível no [pacote de extensão do node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack). Instale todos ou escolha e escolha o que parece mais útil para você.

Para instalar o pacote de extensão do node. js:

1. Abra a janela **extensões** (Ctrl + Shift + X) em vs Code.

    A janela extensões agora está dividida em três seções (porque você instalou a extensão WSL remota).
    - "Instalado localmente": as extensões instaladas para uso com o sistema operacional Windows.
    - "WSL: Ubuntu-18, 4-installed": as extensões instaladas para uso com o sistema operacional Ubuntu (WSL).
    - "Recomendado": extensões recomendadas pelo VS Code com base nos tipos de arquivo em seu projeto atual.

    ![Extensões de VS Code locais vs remotas](../images/vscode-extensions-local-remote.png)

2. Na caixa de pesquisa na parte superior da janela extensões, insira: **pacote de extensão de nó** (ou o nome de qualquer extensão que você esteja procurando). A extensão será instalada para as instâncias local ou WSL de VS Code dependendo de onde você tiver o projeto atual aberto. Você pode saber selecionando o link remoto no canto inferior esquerdo da janela de VS Code (em verde). Ele fornecerá a opção de abrir ou fechar uma conexão remota. Instale suas extensões do node. js no ambiente "WSL: Ubuntu-18, 4".

    ![VS Code link remoto](../images/wsl-remote-extension.png)

Algumas extensões adicionais que talvez você queira considerar incluem:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): depois de concluir o desenvolvimento no lado do servidor com o Node. js, você precisará desenvolver e testar o lado do cliente. Essa extensão integra seu editor de VS Code com o serviço de depuração do navegador Chrome, tornando as coisas um pouco mais eficientes.
- [Keymaps de outros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): essas extensões podem ajudar seu ambiente a se sentir em casa, se você estiver fazendo a transição de outro editor de texto (como Atom, sublime, vim, Emacs, notepad + +, etc.).
- [Sincronização de configurações](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): permite que você sincronize suas configurações de vs Code em diferentes instalações usando o github. Se você trabalha em diferentes computadores, isso ajuda a manter seu ambiente consistente entre eles.

## <a name="install-windows-terminal-optional"></a>Instalar o terminal do Windows (opcional)

O novo terminal do Windows permite várias guias (alternar rapidamente entre o prompt de comando, o PowerShell ou várias distribuições do Linux), associações de chave personalizadas (crie suas próprias teclas de atalho para abrir ou fechar guias, copiar + colar, etc.), emojis ☺ e temas personalizados (esquemas de cores, estilos e tamanhos de fonte, imagem de plano de fundo/desfoque [Saiba mais](https://devblogs.microsoft.com/commandline/).

1. Obter [o terminal do Windows (versão prévia) no Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): ao instalar por meio da loja, as atualizações são manipuladas automaticamente.

2. Depois de instalado, abra Terminal do Windows e selecione **configurações** para personalizar o terminal usando o arquivo de `profile.json`. [Saiba mais sobre como editar as configurações de terminal do Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Configurações de terminal do Windows](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Configurar o Git (opcional)

Se você planeja colaborar com outras pessoas ou hospedar seu projeto em um site de software livre (como o GitHub), VS Code dá suporte ao [controle de versão com o Git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). A guia Controle do Código-fonte no VS Code acompanha todas as alterações e tem comandos Git comuns (add, commit, push e pull) incorporados diretamente na interface do usuário.

1. O Git vem instalado com o subsistema do Windows para Linux distribuições, no entanto, você precisará configurar o arquivo de configuração do git. Para fazer isso, em seu terminal, digite: `git config --global user.name "Your Name"` e, em seguida, `git config --global user.email "youremail@domain.com"`. Se você ainda não tiver uma conta do git, poderá [se inscrever para uma no GitHub](https://github.com/join). Se você nunca trabalhou com o Git antes, os [Guias do GitHub](https://guides.github.com/) podem ajudar você a começar a usá-lo. Se você precisar editar a configuração do git, poderá fazer isso com um editor de texto interno como o nano: `nano ~/.gitconfig`.

2. É recomendável adicionar um [arquivo. gitignore](https://help.github.com/en/articles/ignoring-files) aos seus projetos de nó. Aqui está o [modelo de gitignore padrão do GitHub para node. js](https://github.com/github/gitignore/blob/master/Node.gitignore). Se você optar por [criar um novo repositório usando o site do GitHub](https://help.github.com/articles/create-a-repo), há caixas de seleção disponíveis para inicializar o repositório com um arquivo Leiame, arquivo. gitignore configurado para projetos do node. js e opções para adicionar uma licença se precisar de uma.

## <a name="next-steps"></a>Próximas etapas

Agora você tem um ambiente de desenvolvimento do node. js configurado. Para começar a usar o ambiente do node. js, considere a possibilidade de experimentar um destes tutoriais:

- [Introdução ao node. js para iniciantes](./beginners.md)
- [Introdução às estruturas da Web do node. js no Windows](./web-frameworks.md)
- [Introdução à conexão de aplicativos node. js a um banco de dados](./databases.md)
- [Introdução ao uso de contêineres do Docker com node. js](./containers.md)
