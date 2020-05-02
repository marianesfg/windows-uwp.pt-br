---
title: Introdução ao uso de contêineres do Docker com o Node.js
description: Um guia passo a passo para ajudá-lo a começar a usar contêineres do Docker com aplicativos Node.js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75683669"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Introdução ao uso de contêineres do Docker com o Node.js

Um guia passo a passo para ajudá-lo a começar a usar contêineres do Docker com aplicativos Node.js.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar o ambiente de desenvolvimento do Node.js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instalar o build 18932 ou posterior do Windows 10 Insider Preview.
- Habilitar o recurso WSL 2 no Windows.
- Instalar uma distribuição do Linux (Ubuntu 18.04 em nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`.
- Garantir que a distribuição do Ubuntu 18.04 esteja em execução no modo WSL 2. (O WSL pode executar distribuições no modo v1 ou v2.) Para verificar isso, abra o PowerShell e digite: `wsl -l -v`.
- Usando o PowerShell, definir o Ubuntu 18.04 como a distribuição padrão com: `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Visão geral dos contêineres do Docker

O **Docker** é uma ferramenta usada para criar, implantar e executar aplicativos usando contêineres. Os contêineres permitem que os desenvolvedores empacotem um aplicativo com todas as partes necessárias (bibliotecas, estruturas, dependências etc.) e enviem tudo isso como um pacote. O uso de um contêiner garante que o aplicativo seja executado da mesma forma, independentemente de configurações personalizadas ou das bibliotecas instaladas anteriormente no computador que o executa, que poderiam ser diferentes do computador usado para escrever e testar o código do aplicativo. Isso permite que os desenvolvedores se concentrem em escrever código sem se preocupar com o sistema no qual o código será executado.

Os contêineres do Docker são semelhantes às máquinas virtuais, mas não criam um sistema operacional virtual inteiro. Em vez disso, o Docker permite que o aplicativo use o mesmo kernel do Linux que o sistema em que ele está sendo executado. Isso permite que o pacote do aplicativo exija apenas as partes que ainda não estão no computador host, reduzindo o tamanho do pacote e melhorando o desempenho.

A disponibilidade contínua, usando contêineres do Docker com ferramentas como [Kubernetes](https://docs.microsoft.com/azure/aks/), é outro motivo para a popularidade dos contêineres. Isso permite que várias versões de seu contêiner de aplicativo sejam criadas em momentos diferentes. Em vez de precisar desativar um sistema inteiro para atualizações ou manutenção, cada contêiner (e seus microsserviços específicos) pode ser substituído de forma dinâmica. Você pode preparar um novo contêiner com todas as atualizações, configurar o contêiner para produção e apenas apontar para o novo contêiner quando ele estiver pronto. Também é possível arquivar versões diferentes do aplicativo usando contêineres e mantê-los em execução como um fallback de segurança, se necessário.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Instalar a Visualização Técnica do Docker Desktop WSL 2

Anteriormente, o WSL 1 não podia executar o daemon do Docker de forma direta, mas isso mudou no WSL 2 e levou a melhorias significativas na velocidade e no desempenho do Docker Desktop para WSL 2.

Para instalar e executar a Visualização Técnica do Docker Desktop WSL 2:

1. Baixe o [Instalador da Visualização Técnica do Docker Desktop WSL 2](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Você pode consultar os [documentos do instalador](https://docs.docker.com/docker-for-windows/wsl-tech-preview/), se necessário).

2. Abra o instalador do Docker que você acabou de baixar. O assistente de instalação perguntará se você deseja "Usar contêineres do Windows em vez de contêineres do Linux" – deixe isso desmarcado, pois usaremos o subsistema Linux. O Docker será instalado em um diretório gerenciado na sua distribuição padrão do WSL 2 e incluirá o daemon do Docker, a CLI e a CLI do Compose.

    ![Inicializar o Docker Desktop](../images/install-docker-1.png)

3. Se você ainda não tiver uma ID do Docker, será necessário configurar uma, acessando: [https://hub.docker.com/signup](https://hub.docker.com/signup). Sua ID deve ter todos os caracteres alfanuméricos minúsculos.

4. Depois de instalado, inicie o Docker Desktop selecionando o ícone de atalho na área de trabalho ou localizando-o no menu Iniciar do Windows. O ícone do Docker será exibido no menu de ícones ocultos da barra de tarefas. Clique com o botão direito do mouse no ícone para exibir o menu de comandos do Docker e selecione "Visualização Técnica do WSL 2".

5. Após a abertura da janela da visualização técnica, selecione **Iniciar** para começar a executar o daemon do Docker (processo em segundo plano) no WSL 2. Quando o daemon do Docker do WSL 2 é iniciado, um contexto da CLI do Docker é criado automaticamente para ele.

    ![Inicializar o Docker Desktop](../images/start-docker.gif)

6. Para confirmar se o Docker foi instalado e exibir o número de versão, abra uma linha de comando (WSL ou PowerShell) e digite: `docker --version`

7. Teste se a instalação funciona corretamente executando uma imagem interna simples do Docker: `docker run hello-world`

Aqui estão alguns comandos do Docker que você deve conhecer:

- Liste os comandos disponíveis na CLI do Docker digitando: `docker`
- Liste as informações para um comando específico com: `docker <COMMAND> --help`
- Liste as imagens do Docker em seu computador (que é apenas a imagem hello-world neste momento), com: `docker image ls --all`
- Liste os contêineres em seu computador, com: `docker container ls --all`
- Liste os recursos e as estatísticas de sistema do Docker (CPU e memória) disponíveis no contexto WSL 2, com: `docker info`
- Exiba onde o Docker está em execução no momento, com: `docker context ls`

Você pode ver que há dois contextos em execução do Docker – `default` (o daemon clássico do Docker) e `wsl` (nossa recomendação com o uso da visualização técnica). (Além disso, o comando `ls` é abreviação de `list` e pode ser usado de forma intercambiável.)

![Contexto de exibição do Docker no PowerShell](../images/docker-context.png)

> [!TIP]
> Tente criar um exemplo de imagem do Docker com este [tutorial sobre Docker Hub](https://hub.docker.com/?overlay=onboarding). O Docker Hub também contém milhares de imagens de software livre que podem corresponder ao tipo de aplicativo que você deseja colocar em contêiner. Você pode baixar imagens, como este [contêiner de estrutura Gatsby.js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) ou este [contêiner de estrutura Nuxt.js](https://hub.docker.com/r/hobord/nuxtexpress) e personalizar com seu próprio código de aplicativo. É possível pesquisar o registro usando o [Docker na sua linha de comando](https://docs.docker.com/engine/reference/commandline/search/) ou o [site do Docker Hub](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Instalar a extensão do Docker no VS Code

A extensão do Docker facilita a criação, o gerenciamento e a implantação de aplicativos em contêineres no Visual Studio Code.

1. Abra a janela **Extensões** (Ctrl+Shift+X) no VS Code e pesquise por **Docker**.

2. Selecione a [extensão do Microsoft Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) e **instalar**. Será necessário recarregar o VS Code após a instalação para habilitar a extensão.

    ![Extensão do Docker no VS Code em Remote-WSL](../images/docker-vscode-extension.png)

Após instalar a extensão do Docker no VS Code, você poderá exibir uma lista de comandos `Dockerfile` usados na próxima seção com o atalho: `Ctrl+Space`

Saiba mais sobre como [trabalhar com o Docker no VS Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Criar uma imagem de contêiner com o DockerFile

Uma **imagem de contêiner** armazena o código, as bibliotecas, os arquivos de configuração, as variáveis de ambiente e o runtime do aplicativo. O uso de uma imagem garante que o ambiente do contêiner seja padronizado e contenha apenas o que é necessário para criar e executar seu aplicativo.

Um arquivo **Dockerfile** contém as instruções necessárias para criar uma imagem de contêiner. Em outras palavras, esse arquivo cria a imagem de contêiner que define o ambiente do aplicativo para que ele possa ser reproduzido em qualquer lugar.

Vamos criar uma imagem de contêiner usando o aplicativo Next.js configurado na guia [estruturas da Web](./web-frameworks.md).

1. Abra o aplicativo Next.js no VS Code (garantindo que a extensão Remote-WSL esteja em execução conforme indicado na guia verde inferior esquerda). Abra o terminal WSL integrado no VS Code (**Exibir > Terminal**) e verifique se o caminho do terminal está apontado para o diretório do projeto Next.js (por exemplo, `~/NextProjects/my-next-app$`).

2. Crie um arquivo chamado `Dockerfile` na raiz do projeto Next.js e adicione o seguinte:

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. Para criar a imagem do Docker, execute o seguinte comando na raiz do projeto (substituindo `<your_docker_username>` pelo nome de usuário que você criou no Docker Hub): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> O Docker deve estar em execução com a Visualização Técnica do WSL para que esse comando funcione. Para ver um lembrete de como iniciar o Docker, confira a [etapa 4](#install-docker-desktop-wsl-2-tech-preview) da seção de instalação. O sinalizador `-t` especifica o nome da imagem a ser criada, em nosso caso, "my-nextjs-app:v1". Recomendamos que você sempre [use um número de versão em seus nomes de marca](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) ao criar uma imagem. Certifique-se de incluir o período no final do comando, que especifica o diretório de trabalho atual que deve ser usado para localizar e copiar os arquivos de build para seu aplicativo Next.js.

4. Para executar essa nova imagem do Docker do seu aplicativo Next.js em um contêiner, digite o comando: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. O sinalizador `-p` associa a porta "3000" (a porta em que o aplicativo está sendo executado dentro do contêiner) à porta local "3333" em seu computador, de modo que agora você pode apontar o navegador da Web para [http://localhost:3333](http://localhost:3333) e ver o aplicativo Next.js processado no lado do servidor como uma imagem de contêiner do Docker.

> [!TIP]
> Criamos nossa imagem de contêiner usando `FROM node:12`, que faz referência à imagem padrão do Node.js versão 12 armazenada no Docker Hub. Essa imagem padrão do Node.js se baseia em um sistema Linux Debian/Ubuntu, no entanto, há muitas imagens diferentes do Node.js para escolher, e convém considerar o uso de algo mais leve ou personalizado às suas necessidades. Saiba mais no [Registro de imagem Node.js no Docker Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Carregar sua imagem de contêiner em um repositório

Um **repositório de contêiner** armazena sua imagem de contêiner na nuvem. Geralmente, um repositório de contêiner contém uma coleção de imagens relacionadas, como versões diferentes, disponíveis para fácil configuração e implantação rápida. Normalmente, você pode acessar imagens em repositórios de contêiner por meio de pontos de extremidade HTTPs seguros, permitindo que você extraia, envie ou gerencie imagens por meio de qualquer sistema, hardware ou instância de VM.

Um **registro de contêiner**, por outro lado, armazena uma coleção de repositórios, bem como índices, regras de controle de acesso e caminhos de API. Eles podem ser hospedados de forma pública ou privada. O [Docker Hub](https://hub.docker.com/) é um registro de Docker de software livre e o padrão usado durante a execução de comandos `docker push` e `docker pull`. Ele é gratuito para repositórios públicos e requer uma taxa para repositórios privados.

Para carregar sua nova imagem de contêiner em um repositório hospedado no Docker Hub:

1. Faça logon no Docker Hub. Você será solicitado a inserir o nome de usuário e a senha usados para criar a conta no Docker Hub durante a etapa de instalação. Para fazer logon no Docker em seu terminal, digite: `docker login`

2. Para obter uma lista das imagens de contêiner do Docker que você criou em seu computador, digite: `docker image ls --all`

3. Envie por push sua imagem de contêiner para o Docker Hub, criando um repositório para ela com este comando: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Agora você pode exibir seu repositório no Docker Hub, inserir uma descrição e vincular sua conta do GitHub (se desejar), visitando: https://cloud.docker.com/repository/list

5. Também pode exibir uma lista dos contêineres do Docker ativos com: `docker container ls` (ou `docker ps`)

6. Você deve ver que o contêiner "my-nextjs-app:v1" está ativo na porta 3333 -> 3000/tcp. Também pode ver a "ID DO CONTÊINER" listada aqui. Para interromper a execução de seu contêiner, digite o comando: `docker stop <container ID>`

7. Normalmente, quando um contêiner é interrompido, ele também deve ser removido. A remoção de um contêiner limpa todos os recursos que ele deixa para trás. Depois que você remove um contêiner, todas as alterações feitas no sistema de arquivos de imagem são perdidas permanentemente. Será necessário criar uma imagem para representar as alterações. Para remover o contêiner, use o comando: `docker rm <container ID>`

Saiba mais sobre [criar um aplicativo Web em contêiner com o Docker](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Implantar no Registro de Contêiner do Azure

O [**ACR (Registro de Contêiner do Azure)** ](https://azure.microsoft.com/services/container-registry/) permite que você armazene, gerencie e mantenha suas imagens de contêiner seguras em repositórios privados e autenticados. Compatível com os comandos padrão do Docker, o ACR pode lidar com as tarefas críticas para você, como monitoramento e manutenção da integridade do contêiner, emparelhando com [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) para criar sistemas de orquestração escalonáveis. Crie sob demanda ou automatize totalmente os builds com gatilhos, como confirmações do código-fonte e atualizações da imagem base. O ACR também aproveita a ampla rede de nuvem do Azure para gerenciar a latência de rede, implantações globais e criar uma experiência nativa direta para qualquer pessoa que use o [Serviço de Aplicativo do Azure](https://docs.microsoft.com/azure/app-service/) (para hospedagem na Web, back-ends móveis, APIs REST) ou [outros serviços de nuvem do Azure](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Você precisa de sua própria assinatura do Azure para implantar um contêiner no Azure, e isso poderá ser cobrado. Se você ainda não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

Para obter ajuda com a criação de um Registro de Contêiner do Azure e a implantação da imagem de contêiner do aplicativo, confira o exercício: [Implante uma imagem do Docker em uma instância de Contêiner do Azure](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Recursos adicionais

- [Node.js no Azure](https://azure.microsoft.com/develop/nodejs/)
- Início Rápido: [Criar um aplicativo Web Node.js no Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Curso online: [Administrar contêineres no Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Usar o VS Code: [Trabalhar com o Docker](https://code.visualstudio.com/docs/azure/docker)
- Documentos do Docker: [Visualização Técnica do Docker Desktop WSL 2](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
