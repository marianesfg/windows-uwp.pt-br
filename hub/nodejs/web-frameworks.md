---
title: Introdução às estruturas da Web do node. js no Windows
description: Um guia para ajudá-lo a começar a usar as estruturas da Web do node. js no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, nó no Windows, nó em WSL, nó no Linux no Windows, instalar nó no Windows, NodeJS com vs Code, desenvolver com nó no Windows, desenvolver com NodeJS no Windows, instalar nó em WSL, NodeJS no Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a3c1cd980884dc50107c05207665d0c1ef88938e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314950"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Introdução às estruturas da Web do node. js no Windows

Um guia passo a passo para ajudá-lo a começar a usar as estruturas da Web do node. js no Windows, incluindo o próximo. js, o Nuxt. js e o Gatsby.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar seu ambiente de desenvolvimento do node. js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instale o Windows 10 Insider Preview versão 18932 ou posterior.
- Habilite o recurso WSL 2 no Windows.
- Instale uma distribuição do Linux (Ubuntu 18, 4 para nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`
- Verifique se a distribuição do Ubuntu 18, 4 está em execução no modo WSL 2. (WSL pode executar distribuições no modo v1 ou v2.) Você pode verificar isso abrindo o PowerShell e inserindo: `wsl -l -v`
- Usando o PowerShell, defina o Ubuntu 18, 4 como sua distribuição padrão, com: `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Introdução ao próximo. js

Next. js é uma estrutura para criar aplicativos JavaScript renderizados pelo servidor com base em reagir. js, Node. js, webpack e Babel. js. Ele é basicamente um clichê do projeto para reagir, criado com atenção às práticas recomendadas, que permite criar aplicativos Web "Universal" de forma simples e consistente, com uma configuração de longe. Esses aplicativos Web "Universal" renderizados pelo servidor também são chamados de "isomórficos", o que significa que o código é compartilhado entre o cliente e o servidor.

Para criar um projeto Next. js, que inclui a instalação de Next, reagir e reagir-dom:

1. Abra o terminal do WSL (isto é, Ubuntu 18, 4).

2. Crie uma nova pasta de projeto: `mkdir NextProjects` e insira esse diretório: `cd NextProjects`.

3. Instale o Next. js e crie um projeto (substituindo ' meu-Next-app ' por aquilo que você gostaria de chamar seu aplicativo): `npm create next-app my-next-app`.

4. Depois que o pacote tiver sido instalado, altere os diretórios para sua nova pasta de aplicativo, `cd my-next-app`, em seguida, use `code .` para abrir o próximo projeto. js no VS Code. Isso permitirá que você examine a próxima estrutura. js que foi criada para seu aplicativo. Observe que VS Code abriu seu aplicativo em um ambiente WSL (conforme indicado na guia verde na parte inferior esquerda da janela VS Code). Isso significa que, enquanto você estiver usando VS Code para edição no sistema operacional Windows, ele ainda estará executando seu aplicativo no sistema operacional Linux.

    ![WSL-extensão remota](../images/wsl-remote-extension.png)

5. Há três comandos que você precisa saber quando o próximo. js está instalado:

    - `npm run dev` para executar uma instância de desenvolvimento com Recarregamento automático, observação de arquivo e execução de tarefa novamente.
    - `npm run build` para compilar seu projeto.
    - `npm start` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**exibir > terminal**). Verifique se o caminho do terminal é apontado para o diretório do projeto (por ex. `~/NextProjects/my-next-app$`). Em seguida, tente executar uma instância de desenvolvimento do novo aplicativo Next. js usando: `npm run dev`

6. O servidor de desenvolvimento local será iniciado e, depois que as páginas do projeto forem concluídas, o terminal exibirá "compilado com êxito-pronto no [http://localhost:3000](http://localhost:3000)". Selecione este link do localhost para abrir seu novo aplicativo Next. js em um navegador da Web.

    ![Seu próximo aplicativo. js em execução no localhost: 3000](../images/next-app.png)

7. Abra o arquivo `pages/index.js` em seu editor de VS Code. Localize o título da página `<h1 className='title'>Welcome to Next.js!</h1>` e altere-o para `<h1 className='title'>This is my new Next.js app!</h1>`. Com o navegador da Web ainda aberto para localhost: 3000, salve sua alteração e observe que o recurso de Recarregamento automático compila e atualiza automaticamente sua alteração no navegador.

8. Vejamos como o próximo. js lida com erros. Remova a marca de fechamento `</h1>` para que o código de título agora tenha esta aparência: `<h1 className='title'>This is my new Next.js app!`. Salve essa alteração e observe que um erro "falha ao compilar" será exibido no navegador e no seu terminal, informando ao seu conhecimento que uma marca de fechamento para `<h1>` é esperada. Substitua a marca de fechamento `</h1>`, salve e a página será recarregada.

Você pode usar o depurador do VS Code com seu próximo aplicativo. js selecionando a tecla F5 ou acessando **exibir > depurar** (Ctrl + Shift + D) e **Exibir > console de depuração** (Ctrl + Shift + Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela depurar, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Para saber mais, confira [vs Code depuração](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code a janela de depuração e o ícone de configuração de inicialização. JSON](../images/vscode-debug-launch-configuration.png)

Para saber mais sobre o próximo. js, consulte os [documentos do próximo. js](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Introdução ao Nuxt. js

Nuxt. js é uma estrutura para a criação de aplicativos JavaScript renderizados pelo servidor com base em Vue. js, Node. js, webpack e Babel. js. Ele foi inspirado pelo Next. js. Ele é basicamente um clichê de projeto para Vue. Assim como o Next. js, ele foi criado com atenção às práticas recomendadas e permite que você crie aplicativos Web "universais" de forma simples e consistente, com pouco nenhuma configuração. Esses aplicativos Web "Universal" renderizados pelo servidor também são chamados de "isomórficos", o que significa que o código é compartilhado entre o cliente e o servidor.

Para criar um projeto Nuxt. js, que inclui responder a uma série de perguntas sobre qual tipo de estrutura de servidor integrado, estrutura de interface do usuário, estrutura de teste, modo, módulos e fiapos você gostaria de instalar:

1. Abra o terminal do WSL (isto é, Ubuntu 18, 4).

2. Crie uma nova pasta de projeto: `mkdir NuxtProjects` e insira esse diretório: `cd NuxtProjects`.

3. Instale o Nuxt. js e crie um projeto (substituindo ' My-Nuxt-app ' por tudo o que você gostaria de chamar seu aplicativo): `npm create nuxt-app my-nuxt-app`

4. O instalador Nuxt. js agora lhe fará as seguintes perguntas:
    - Nome do projeto: My-nuxtjs-app
    - Descrição do projeto: Descrição do meu aplicativo Nuxt. js.
    - Nome do autor: Eu uso meu alias do GitHub.
    - Escolha o Gerenciador de pacotes: Yarn ou **NPM** – usamos NPM para nossos exemplos.
    - Escolha estrutura de interface do usuário: Nenhum, Vue de design do Ant, Vue de inicialização, etc. Vamos escolher **Vuetify** para este exemplo, mas a Comunidade Vue criou um ótimo Resumo comparando [essas estruturas de interface do usuário](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) para ajudá-lo a escolher o melhor ajuste para o seu projeto.
    - Escolha estruturas de servidor personalizadas: Nenhum, AdonisJs, Express, Fastify, etc. Vamos escolher **None** para este exemplo, mas você pode encontrar uma [comparação de estrutura de servidor 2019-2020](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) no site dev.to.
    - Escolha os módulos Nuxt. js (use a barra de espaços para selecionar módulos ou apenas Enter se não desejar): Axios (para simplificar as solicitações HTTP) ou [suporte do PWA](https://pwa.nuxtjs.org/) (para adicionar um arquivo de trabalho de serviço, Manifest. JSON, etc.). Não vamos adicionar um módulo para este exemplo.
    - Escolha ferramentas de repanoização: **ESLint**, melhores, arquivos em etapas de fiapos. Vamos escolher **ESLint** (uma ferramenta para analisar seu código e avisá-lo de possíveis erros).
    - Escolha uma estrutura de teste: **None**, JEST, Ava. Vamos escolher **nenhum** , pois não abordaremos os testes neste guia de início rápido.
    - Escolha o modo de renderização: **Universal (SSR)** ou aplicativo de página única (Spa). Vamos escolher **Universal (SSR)** para nosso exemplo, mas os [documentos do Nuxt. js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) destacam algumas das diferenças--SSR exigindo um servidor node. js em execução no servidor-renderizar seu aplicativo e o Spa para hospedagem estática.
    - Escolha as ferramentas de desenvolvimento: **jsconfig. JSON** (recomendado para vs Code para que a conclusão do código do IntelliSense funcione)

5. Depois que o projeto for criado, `cd my-nuxtjs-app` para inserir o diretório do projeto Nuxt. js e, em seguida, insira `code .` para abrir o projeto no ambiente VS Code WSL-Remote.

    ![WSL-extensão remota](../images/wsl-remote-extension.png)

6. Há três comandos que você precisa saber quando o Nuxt. js está instalado:

    - `npm run dev` para executar uma instância de desenvolvimento com Recarregamento automático, observação de arquivo e execução de tarefa novamente.
    - `npm run build` para compilar seu projeto.
    - `npm start` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**exibir > terminal**). Verifique se o caminho do terminal é apontado para o diretório do projeto (por ex. `~/NuxtProjects/my-nuxt-app$`). Em seguida, tente executar uma instância de desenvolvimento do seu novo aplicativo Nuxt. js usando: `npm run dev`

6. O servidor de desenvolvimento local será iniciado (exibindo algumas barras de progresso frio para o cliente e o servidor serão compilados). Após a criação do projeto, seu terminal exibirá "compilado com êxito", juntamente com o tempo necessário para a compilação. Aponte seu navegador da Web para [http://localhost:3000](http://localhost:3000) para abrir seu novo aplicativo Nuxt. js.

    ![Seu aplicativo Nuxt. js em execução no localhost: 3000](../images/nuxt-app.png)

7. Abra o arquivo `pages/index.vue` em seu editor de VS Code. Localize o título da página `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` e altere-o para `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Com o navegador da Web ainda aberto para localhost: 3000, salve sua alteração e observe que o recurso de Recarregamento automático compila e atualiza automaticamente sua alteração no navegador.

8. Vejamos como o Nuxt. js lida com erros. Remova a marca de fechamento `</v-card-title>` para que o código de título agora tenha esta aparência: `<v-card-title class="headline">This is my new Nuxt.js app!`. Salve essa alteração e observe que um erro de compilação será exibido em seu navegador e, no seu terminal, informando que uma marca de fechamento para `<v-card-title>` está ausente, junto com os números de linha em que o erro pode ser encontrado em seu código. Substitua a marca de fechamento `</v-card-title>`, salve e a página será recarregada.

Você pode usar o depurador do VS Code com seu aplicativo Nuxt. js selecionando a tecla F5 ou acessando **exibir > depurar** (Ctrl + Shift + D) e **Exibir > console de depuração** (Ctrl + Shift + Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela depurar, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Para saber mais, confira [vs Code depuração](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code a janela de depuração e o ícone de configuração de inicialização. JSON](../images/vscode-debug-launch-configuration.png)

Para saber mais sobre o Nuxt. js, consulte o [Guia do Nuxt. js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Introdução ao Gatsby. js

O Gatsby. js é uma estrutura de geração de site estática com base em reajam. js, em vez de ser processado pelo servidor, como Next. js e Nuxt. js. Um gerador de site estático gera HTML estático no tempo de compilação. Ele não requer um servidor. Next. js e Nuxt. js geram HTML em tempo de execução (sempre que uma nova solicitação chega). Eles exigem que um servidor seja executado. O Gatsby também determina como lidar com dados em seu aplicativo (com GraphQL), enquanto Next. js e Nuxt. js deixam essa decisão para você.

Para criar um projeto Gatsby. js:

1. Abra o terminal do WSL (isto é, Ubuntu 18, 4).
2. Crie uma nova pasta de projeto: `mkdir GatsbyProjects` e insira esse diretório: `cd GatsbyProjects`
3. Use NPM para instalar a CLI do Gatsby: `npm install -g gatsby-cli`. Depois de instalado, verifique a versão com `gatsby --version`.
4. Crie seu projeto Gatsby. js: `gatsby new my-gatsby-app`
5. Depois que o pacote tiver sido instalado, altere os diretórios para sua nova pasta de aplicativo, `cd my-gatsby-app`, em seguida, use `code .` para abrir o projeto Gatsby no VS Code. Isso permitirá que você examine a estrutura Gatsby. js que foi criada para seu aplicativo usando o explorador de arquivos do VS Code. Observe que VS Code abriu seu aplicativo em um ambiente WSL (conforme indicado na guia verde na parte inferior esquerda da janela VS Code). Isso significa que, enquanto você estiver usando VS Code para edição no sistema operacional Windows, ele ainda estará executando seu aplicativo no sistema operacional Linux.

    ![WSL-extensão remota](../images/wsl-remote-extension.png)

6. Há três comandos que você precisa saber quando o Gatsby está instalado:

    - `gatsby develop` para executar uma instância de desenvolvimento com recarga automática.
    - `gatsby build` para criar uma compilação de produção.
    - `gatsby serve` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**exibir > terminal**). Verifique se o caminho do terminal é apontado para o diretório do projeto (por ex. `~/GatsbyProjects/my-gatsby-app$`). Em seguida, tente executar uma instância de desenvolvimento do seu novo aplicativo usando: `gatsby develop`

7. Depois que o novo projeto Gatsby concluir a compilação, o terminal exibirá que "agora você pode exibir Gatsby-Starter-padrão no navegador. [http://localhost:8000/](http://localhost:8000/). " Selecione este link do localhost para exibir o novo projeto criado em um navegador da Web.

> [!NOTE]
> Você observará que a saída do terminal também saberá que você pode "Exibir GraphiQL, um IDE no navegador, para explorar os dados e o esquema do seu site: [http://localhost:8000/___graphql](http://localhost:8000/___graphql)". O GraphQL consolida suas APIs em um IDE de autodocumentação (GraphiQL) embutido no Gatsby. Além de explorar os dados e o esquema do seu site, você pode executar operações GraphQL, como consultas, mutações e assinaturas. Para obter mais informações, consulte [Introducing GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Abra o arquivo `src/pages/index.js` em seu editor de VS Code. Localize o título da página `<h1 >Hi people</h1>` e altere-o para `<h1 >Hi (Your Name)!</h1>`. Com seu navegador da Web ainda aberto para localhost: 8000, salve sua alteração e observe que o recurso de Recarregamento automático compila e atualiza automaticamente sua alteração no navegador.

    ![Seu aplicativo Gatsby. js em execução no localhost: 3000](../images/gatsby-app.png)

9. Vejamos como o próximo. js lida com erros. Remova a marca de fechamento `</h1>` para que o código de título agora tenha esta aparência: `<h1>Hi (Your Name)!`. Salve essa alteração e observe que um erro "falha ao compilar" será exibido no navegador e no seu terminal, informando ao seu conhecimento que uma marca de fechamento para `<h1>` é esperada. Substitua a marca de fechamento `</h1>`, salve e a página será recarregada.

Você pode usar o depurador do VS Code com seu próximo aplicativo. js selecionando a tecla F5 ou acessando **exibir > depurar** (Ctrl + Shift + D) e **Exibir > console de depuração** (Ctrl + Shift + Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela depurar, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Para saber mais, confira [vs Code depuração](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code a janela de depuração e o ícone de configuração de inicialização. JSON](../images/vscode-debug-launch-configuration.png)

Para saber mais sobre o Gatsby, consulte os [documentos do Gatsby. js](https://www.gatsbyjs.org/docs/).
