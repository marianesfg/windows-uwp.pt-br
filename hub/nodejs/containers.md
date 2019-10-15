---
title: Introdução ao uso de contêineres do Docker com node. js
description: Um guia passo a passo para ajudá-lo a começar a usar contêineres do Docker com seus aplicativos node. js.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 16b1421606d3c8271141256b80ae2600ec9ca49d
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315120"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Introdução ao uso de contêineres do Docker com node. js

Um guia passo a passo para ajudá-lo a começar a usar contêineres do Docker com seus aplicativos node. js.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar seu ambiente de desenvolvimento do node. js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instale o Windows 10 Insider Preview versão 18932 ou posterior.
- Habilite o recurso WSL 2 no Windows.
- Instale uma distribuição do Linux (Ubuntu 18, 4 para nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`.
- Verifique se a distribuição do Ubuntu 18, 4 está em execução no modo WSL 2. (WSL pode executar distribuições no modo v1 ou v2.) Você pode verificar isso abrindo o PowerShell e inserindo: `wsl -l -v`.
- Usando o PowerShell, defina o Ubuntu 18, 4 como sua distribuição padrão, com: `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Visão geral dos contêineres do Docker

O **Docker** é uma ferramenta usada para criar, implantar e executar aplicativos usando contêineres. Os contêineres permitem que os desenvolvedores Empacotem um aplicativo com todas as partes de que precisa (bibliotecas, estruturas, dependências etc.) e enviam tudo isso como um pacote. O uso de um contêiner garante que o aplicativo será executado da mesma forma, independentemente de quaisquer configurações personalizadas ou de bibliotecas instaladas anteriormente no computador que o executa, o que poderia ser diferente do computador usado para gravar e testar o código do aplicativo. Isso permite que os desenvolvedores se concentrem em escrever código sem se preocupar com o sistema no qual o código será executado.

Os contêineres do Docker são semelhantes às máquinas virtuais, mas não criam um sistema operacional virtual inteiro. Em vez disso, o Docker permite que o aplicativo use o mesmo kernel do Linux que o sistema em que ele está sendo executado. Isso permite que o pacote do aplicativo exija apenas as partes que ainda não estão no computador host, reduzindo o tamanho do pacote e melhorando o desempenho.

A disponibilidade contínua, usando contêineres do Docker com ferramentas como [kubernetes](https://docs.microsoft.com/azure/aks/), é outro motivo para a popularidade dos contêineres. Isso permite que várias versões do seu contêiner de aplicativo sejam criadas em momentos diferentes. Em vez de precisar desativar um sistema inteiro para atualizações ou manutenção, cada contêiner (e seus microserviços específicos) pode ser substituído de forma dinâmica. Você pode preparar um novo contêiner com todas as suas atualizações, configurar o contêiner para produção e apenas apontar para o novo contêiner quando ele estiver pronto. Você também pode arquivar versões diferentes do seu aplicativo usando contêineres e mantê-los em execução como um fallback de segurança, se necessário.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Instalar a visualização técnica do Docker desktop WSL 2

Anteriormente, o WSL 1 não podia executar o daemon do Docker diretamente, mas isso mudou de WSL 2 e levou a melhorias significativas na velocidade e no desempenho com o Docker desktop para WSL 2.

Para instalar e executar o Docker desktop WSL 2 Tech Preview:

1. Baixe o [instalador do Docker desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe). (Você pode fazer referência aos [documentos do instalador](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) , se necessário).

2. Abra o instalador do Docker que você acabou de baixar. O assistente de instalação perguntará se você deseja "usar contêineres do Windows em vez de contêineres do Linux". deixe isso desmarcado, pois usaremos o subsistema Linux. O Docker será instalado em um diretório gerenciado na sua distribuição padrão do WSL 2 e incluirá o daemon do Docker, a CLI e a CLI do Compose.

    ![Inicialização da área de trabalho do Docker](../images/install-docker-1.png)

3. Se você ainda não tiver uma ID do Docker, será necessário configurar uma, visitando: [https://hub.docker.com/signup](https://hub.docker.com/signup). Sua ID deve ter todos os caracteres alfanuméricos minúsculos.

4. Depois de instalado, inicie o Docker desktop selecionando o ícone de atalho na área de trabalho ou localizando-o no menu Iniciar do Windows. O ícone do Docker será exibido no menu de ícones ocultos da barra de tarefas. Clique com o botão direito do mouse no ícone para exibir o menu de comandos do Docker e selecione "WSL 2 Tech Preview".

5. Após a abertura do Windows Tech Preview, selecione **Iniciar** para começar a executar o daemon do Docker (processo em segundo plano) no WSL 2. Quando o daemon do Docker do WSL 2 é iniciado, um contexto da CLI do Docker é criado automaticamente para ele.

    ![Inicialização da área de trabalho do Docker](../images/start-docker.gif)

6. Para confirmar se o Docker foi instalado e exibir o número de versão, abra uma linha de comando (WSL ou PowerShell) e digite: `docker --version`

7. Teste se a instalação funciona corretamente executando uma imagem interna do Docker simples: `docker run hello-world`

Aqui estão alguns comandos do Docker que você deve saber:

- Liste os comandos disponíveis na CLI do Docker digitando: `docker`
- Listar informações para um comando específico com: `docker <COMMAND> --help`
- Liste as imagens do Docker em seu computador (que é apenas a imagem Hello-World neste ponto), com: `docker image ls --all`
- Liste os contêineres em seu computador, com: `docker container ls --all`
- Liste os recursos e as estatísticas do sistema do Docker (CPU & memória) disponíveis no contexto WSL 2, com: `docker info`
- Exibir onde o Docker está em execução no momento, com: `docker context ls`

Você pode ver que há dois contextos que o Docker está executando no--`default` (o daemon clássico do Docker) e `wsl` (nossa recomendação usando a visualização técnica). (Além disso, o comando `ls` é curto para `list` e pode ser usado de forma intercambiável).

![Contexto de exibição do Docker no PowerShell](../images/docker-context.png)

> [!TIP]
> Tente criar um exemplo de imagem do Docker com este [tutorial no Hub do Docker](https://hub.docker.com/?overlay=onboarding). O Hub do Docker também contém muitos milhares de imagens de código aberto que podem corresponder ao tipo de aplicativo que você deseja colocar em contêiner. Você pode baixar imagens, como este [contêiner de estrutura Gatsby. js](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) ou esse [contêiner de estrutura Nuxt. js](https://hub.docker.com/r/hobord/nuxtexpress), e estendê-lo com seu próprio código de aplicativo. Você pode pesquisar o registro usando o [Docker na linha de comando](https://docs.docker.com/engine/reference/commandline/search/) ou no [site do Hub do Docker](https://hub.docker.com/search/?type=image).

## <a name="install-the-docker-extension-on-vs-code"></a>Instalar a extensão do Docker no VS Code

A extensão do Docker facilita a criação, o gerenciamento e a implantação de aplicativos em contêineres a partir de Visual Studio Code.

1. Abra a janela **extensões** (Ctrl + Shift + X) em vs Code e procure **Docker**.

2. Selecione a [extensão do Microsoft Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) e **Instale**o. Será necessário recarregar VS Code após a instalação para habilitar a extensão.

    ![Extensão do Docker em VS Code em WSL remota](../images/docker-vscode-extension.png)

Ao instalar a extensão do Docker no VS Code, agora você poderá abrir uma lista de comandos `Dockerfile` usados na próxima seção com o atalho: `Ctrl+Space`

Saiba mais sobre como [trabalhar com o Docker no vs Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Criar uma imagem de contêiner com DockerFile

Uma **imagem de contêiner** armazena o código do aplicativo, as bibliotecas, os arquivos de configuração, as variáveis de ambiente e o tempo de execução. O uso de uma imagem garante que o ambiente em seu contêiner seja padronizado e contenha apenas o que é necessário para compilar e executar seu aplicativo.

Um **DockerFile** contém as instruções necessárias para criar a nova imagem de contêiner. Em outras palavras, esse arquivo cria a imagem de contêiner que define o ambiente do aplicativo para que ele possa ser reproduzido em qualquer lugar.

Vamos criar uma imagem de contêiner usando o próximo aplicativo. js configurado no guia de [estruturas da Web](./web-frameworks.md) .

1. Abra seu próximo aplicativo. js no VS Code (garantindo que a extensão WSL remota esteja em execução conforme indicado na guia verde inferior esquerda). Abra o terminal WSL integrado no VS Code (**exibir > terminal**) e verifique se o caminho do terminal é apontado para seu próximo diretório do projeto. js (ou seja, `~/NextProjects/my-next-app$`).

2. Crie um novo arquivo chamado `Dockerfile` na raiz do seu próximo projeto. js e adicione o seguinte:

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

3. Para criar a imagem do Docker, execute o seguinte comando na raiz do seu projeto (mas substituindo `<your_docker_username>` pelo nome de usuário que você criou no Hub do Docker): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> O Docker deve estar em execução com o WSL Tech Preview para que esse comando funcione. Para obter um lembrete de como iniciar o Docker, consulte [step #4](#install-docker-desktop-wsl-2-tech-preview) da seção install. O sinalizador `-t` especifica o nome da imagem a ser criada, "My-nextjs-App: v1" em nosso caso. Recomendamos que você sempre [use uma versão # nos nomes de marca](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) ao criar uma imagem. Certifique-se de incluir o período no final do comando, que especifica o diretório de trabalho atual que deve ser usado para localizar e copiar os arquivos de compilação para seu próximo aplicativo. js.

4. Para executar essa nova imagem do Docker do seu próximo aplicativo. js em um contêiner, insira o comando: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. O sinalizador `-p` associa a porta ' 3000 ' (a porta em que o aplicativo está sendo executado dentro do contêiner) à porta local ' 3333 ' em seu computador, portanto, agora você pode apontar seu navegador da Web para [http://localhost:3333](http://localhost:3333) e ver o próximo aplicativo. js renderizado no lado do servidor como um Docker imagem de contêiner.

> [!TIP]
> Criamos nossa imagem de contêiner usando `FROM node:12`, que faz referência à imagem padrão do node. js versão 12 armazenada no Hub do Docker. Essa imagem padrão do node. js se baseia em um sistema Debian/Ubuntu Linux, há muitas imagens diferentes do node. js para escolher, no entanto, e convém considerar o uso de algo mais leve ou personalizado para suas necessidades. Saiba mais no [registro de imagem do node. js no Hub do Docker](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Carregar a imagem de contêiner em um repositório

Um **repositório de contêiner** armazena a imagem de contêiner na nuvem. Geralmente, um repositório de contêiner conterá, na verdade, uma coleção de imagens relacionadas, como versões diferentes, que estão disponíveis para fácil instalação e implantação rápida. Normalmente, você pode acessar imagens em repositórios de contêiner por meio de pontos de extremidade HTTPs seguros, permitindo que você extraia, envie ou gerencie imagens por meio de qualquer sistema, hardware ou instância de VM.

Um **registro de contêiner**, por outro lado, armazena uma coleção de repositórios, bem como índices, regras de controle de acesso e caminhos de API. Eles podem ser hospedados de forma pública ou privada. O [Hub do Docker](https://hub.docker.com/) é um registro de Docker de software livre e o padrão usado durante a execução de comandos `docker push` e `docker pull`. Ele é gratuito para repositórios pública e requer uma taxa para repositórios privada.

Para carregar sua nova imagem de contêiner em um repositório hospedado no Hub do Docker:

1. Faça logon no Hub do Docker. Você será solicitado a inserir o nome de usuário e a senha usados para criar sua conta do Hub do Docker durante a etapa de instalação. Para fazer logon no Docker em seu terminal, digite: `docker login`

2. Para obter uma lista das imagens de contêiner do Docker que você criou em seu computador, digite: `docker image ls --all`

3. Envie por push sua imagem de contêiner para o Hub do Docker, criando um novo repositório para ele, usando este comando: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Agora você pode exibir seu repositório no Hub do Docker, inserir uma descrição e vincular sua conta do GitHub (se desejar), visitando: https://cloud.docker.com/repository/list

5. Você também pode exibir uma lista de seus contêineres do Docker ativos com: `docker container ls` (ou `docker ps`)

6. Você deve ver que o contêiner "My-nextjs-App: v1" está ativo na porta 3333-> 3000/TCP. Você também pode ver sua "ID do contêiner" listada aqui. Para interromper a execução do seu contêiner, insira o comando: `docker stop <container ID>`

7. Normalmente, quando um contêiner é interrompido, ele também deve ser removido. A remoção de um contêiner limpa todos os recursos que ele deixa para trás. Depois de remover um contêiner, as alterações feitas em seu sistema de arquivos de imagem serão permanentemente perdidas. Será necessário criar uma nova imagem para representar as alterações. PARA remover o contêiner, use o comando: `docker rm <container ID>`

Saiba mais sobre como [criar um aplicativo Web em contêineres com o Docker](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Implantar no registro de contêiner do Azure

O ACR ( [**registro de contêiner do Azure**](https://azure.microsoft.com/services/container-registry/) ) permite que você armazene, gerencie e mantenha suas imagens de contêiner seguras em repositórios, autenticados e privados. Compatível com os comandos padrão do Docker, o ACR pode manipular tarefas críticas para você, como monitoramento e manutenção da integridade do contêiner, emparelhando com o [kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) para criar sistemas de orquestração escalonáveis. Crie sob demanda ou Automatize totalmente compilações com gatilhos, como confirmações de código-fonte e atualizações de imagem de base. O ACR também aproveita a vasta rede de nuvem do Azure para gerenciar a latência de rede, implantações globais e criar uma experiência nativa tranqüila para qualquer pessoa que use o [serviço de Azure app](https://docs.microsoft.com/azure/app-service/) (para hospedagem na Web, back-ends móveis, APIs REST) ou [outros serviços de nuvem do Azure ](https://azure.microsoft.com/product-categories/containers/).

> [!IMPORTANT]
> Você precisa de sua própria assinatura do Azure para implantar um contêiner no Azure e você pode receber uma cobrança. Se você ainda não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

Para obter ajuda para criar um registro de contêiner do Azure e implantar a imagem de contêiner do aplicativo, consulte o exercício: [Implante uma imagem do Docker em uma instância de contêiner do Azure](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Recursos adicionais

- [Node. js no Azure](https://azure.microsoft.com/en-us/develop/nodejs/)
- Início Rápido: [Criar um aplicativo Web node. js no Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Curso online: [Administrar contêineres no Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Usando VS Code: [Trabalhando com o Docker](https://code.visualstudio.com/docs/azure/docker)
- Documentos do Docker: [Visualização técnica do Docker desktop WSL 2](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
