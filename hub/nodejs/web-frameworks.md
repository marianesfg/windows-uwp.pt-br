---
title: Introdução às estruturas da Web do Node.js no Windows
description: Um guia para ajudar na introdução às estruturas da Web do Node.js no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, aprendizado do nodejs, nó no windows, nó no wsl, nó no linux no windows, instalar nó no windows, nodejs com vs code, desenvolver com nó no windows, desenvolver com nodejs no windows, instalar nó no WSL, NodeJS no Subsistema do Windows para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517794"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Introdução às estruturas da Web do Node.js no Windows

Um guia passo a passo para começar a usar as estruturas da Web do Node.js no Windows, incluindo Next.js, Nuxt.js e Gatsby.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar o ambiente de desenvolvimento do Node.js com o WSL 2](./setup-on-wsl2.md), incluindo:

- Instalar o build 18932 ou posterior do Windows 10 Insider Preview.
- Habilitar o recurso WSL 2 no Windows.
- Instalar uma distribuição do Linux (Ubuntu 18.04 em nossos exemplos). Você pode verificar isso com: `wsl lsb_release -a`
- Garantir que a distribuição do Ubuntu 18.04 esteja em execução no modo WSL 2. (O WSL pode executar distribuições no modo v1 ou v2.) Para verificar isso, abra o PowerShell e digite: `wsl -l -v`
- Usando o PowerShell, definir o Ubuntu 18.04 como a distribuição padrão com: `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Introdução ao Next.js

Next.js é uma estrutura para criar aplicativos JavaScript renderizados pelo servidor com base em React.js, Node.js, Webpack e Babel.js. Ele é basicamente um projeto genérico para o React, criado de acordo com as práticas recomendadas, que permite criar aplicativos Web "universais" de forma simples e consistente, praticamente com qualquer configuração. Esses aplicativos Web "universais" renderizados pelo servidor também são chamados de "isomórficos", o que significa que o código é compartilhado entre o cliente e o servidor.

Para criar um projeto Next.js que inclua a instalação de next, react e react-dom:

1. Abra o terminal do WSL (ou seja, Ubuntu 18.04).

2. Crie uma pasta de projeto, `mkdir NextProjects`, e insira este diretório: `cd NextProjects`.

3. Instale o Next.js e crie um projeto (substituindo "My-next-app" pelo nome que você gostaria para seu aplicativo): `npm create next-app my-next-app`.

4. Depois de instalar o pacote, altere os diretórios para sua nova pasta de aplicativo, `cd my-next-app`, e use `code .` para abrir o projeto Next.js no VS Code. Assim, você pode examinar a estrutura do Next.js criada para seu aplicativo. Observe que o VS Code abriu seu aplicativo em um ambiente WSL remoto (como indicado na guia verde na parte inferior esquerda da janela do VS Code). Isso significa que, enquanto você estiver usando o VS Code para editar no sistema operacional Windows, ele ainda estará executando seu aplicativo no sistema operacional Linux.

    ![Extensão remota do WSL](../images/wsl-remote-extension.png)

5. Há três comandos que você precisará saber quando o Next.js estiver instalado:

    - `npm run dev` para executar uma instância de desenvolvimento com recarga dinâmica, observação de arquivo e nova execução de tarefa.
    - `npm run build` para compilar seu projeto.
    - `npm start` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**Exibir > Terminal**). Verifique se o caminho do terminal está indicando o diretório do projeto (por exemplo, `~/NextProjects/my-next-app$`). Em seguida, tente executar uma instância de desenvolvimento do novo aplicativo Next.js usando: `npm run dev`

6. O servidor de desenvolvimento local será iniciado e, depois que a criação das páginas do projeto for concluída, o terminal exibirá "compilado com êxito – pronto em [http://localhost:3000](http://localhost:3000)". Selecione esse link do localhost para abrir o novo aplicativo Next.js em um navegador da Web.

    ![O aplicativo Next.js em execução no localhost:3000](../images/next-app.png)

7. Abra o arquivo `pages/index.js` no editor do VS Code. Localize o título da página `<h1 className='title'>Welcome to Next.js!</h1>` e altere-o para `<h1 className='title'>This is my new Next.js app!</h1>`. Com o navegador da Web ainda aberto em localhost:3000, salve sua alteração e observe que o recurso de recarga dinâmica compila e atualiza automaticamente a alteração no navegador.

8. Vejamos como o Next.js lida com erros. Remova a marca de fechamento `</h1>` para que o código de título agora tenha esta aparência: `<h1 className='title'>This is my new Next.js app!`. Salve essa alteração e observe que será exibido o erro "Falha ao compilar" no navegador e no seu terminal, informando que se espera uma marca de fechamento para `<h1>`. Substitua a marca de fechamento `</h1>`, salve, e a página será recarregada.

Você pode usar o depurador do VS Code com o aplicativo Next.js clicando na tecla F5 ou acessando **Exibir > Depurar** (Ctrl+Shift+D) e **Exibir > Console de Depuração** (Ctrl+Shift+Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela de Depuração, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Confira mais informações em [Depuração do VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Janela de depuração do VS Code e ícone de configuração de launch.json](../images/vscode-debug-launch-configuration.png)

Para saber mais sobre o Next.js, confira os [Documentos do Next.js](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Introdução ao Nuxt.js

Nuxt.js é uma estrutura para criar aplicativos JavaScript renderizados pelo servidor com base em Vue.js, Node.js, Webpack e Babel.js. Ele foi inspirado no Next.js. Basicamente, é um projeto genérico para Vue. Assim como o Next.js, ele foi criado de acordo com as práticas recomendadas e permite desenvolver aplicativos Web "universais" de forma simples e consistente, praticamente com qualquer configuração. Esses aplicativos Web "universais" renderizados pelo servidor também são chamados de "isomórficos", o que significa que o código é compartilhado entre o cliente e o servidor.

Para criar um projeto Nuxt.js, o que inclui responder a diversas perguntas sobre qual tipo de estrutura de servidor integrado, estrutura da IU, estrutura de teste, modo, módulos e linter que você gostaria de instalar:

1. Abra o terminal do WSL (ou seja, Ubuntu 18.04).

2. Crie uma pasta de projeto, `mkdir NuxtProjects`, e insira este diretório: `cd NuxtProjects`.

3. Instale o Nuxt.js e crie um projeto (substituindo "my-nuxt-app" pelo nome que você gostaria para seu aplicativo): `npm create nuxt-app my-nuxt-app`

4. Agora, o instalador do Nuxt.js fará as seguintes perguntas:
    - Nome do projeto: my-nuxtjs-app
    - Descrição do projeto: descrição do meu aplicativo Nuxt.js.
    - Nome do autor: uso meu alias do GitHub.
    - Escolher o gerenciador de pacotes: Yarn ou **Npm** – usamos Npm em nossos exemplos.
    - Escolher estrutura da IU: nenhuma, Ant Design Vue, Bootstrap Vue etc. Vamos escolher **Vuetify** para este exemplo, mas a Comunidade Vue criou um interessante [resumo de comparação dessas estruturas de IU](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) para ajudar na escolha do melhor esquema para seu projeto.
    - Escolher estruturas de servidor personalizadas: nenhuma, AdonisJs, Express, Fastify etc. Vamos escolher **Nenhuma** para este exemplo, mas você pode encontrar uma [comparação de estruturas de servidor 2019-2020](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) no site Dev.to.
    - Escolher os módulos Nuxt.js (use a barra de espaços para selecionar módulos ou apenas Enter, caso não queira nenhum): Axios (para simplificar as solicitações HTTP) ou [suporte do PWA](https://pwa.nuxtjs.org/) (para adicionar um service-worker, arquivo manifest.json etc.). Não vamos adicionar um módulo a este exemplo.
    - Escolher ferramentas de lint: **ESLint**, Prettier, arquivos Lint em etapas. Vamos escolher **ESLint** (uma ferramenta para analisar seu código e avisar sobre possíveis erros).
    - Escolher uma estrutura de teste: **Nenhuma**, Jest, AVA. Vamos escolher **Nenhuma**, já que não abordaremos os testes neste guia de início rápido.
    - Escolher modo renderização: **Universal (SSR)** ou SPA (Aplicativo de Página Única). Vamos escolher **Universal (SSR)** para nosso exemplo, mas os [documentos do Nuxt.js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) indicam algumas diferenças, como SSR exigindo um servidor Node.js em execução para renderizar seu aplicativo no servidor e um SPA para hospedagem estática.
    - Escolher ferramentas de desenvolvimento: **jsconfig.json** (recomendado para VS Code para que a conclusão do código IntelliSense funcione)

5. Depois que o projeto for criado, use `cd my-nuxtjs-app` para inserir o diretório do projeto Nuxt.js, depois insira `code .` para abrir o projeto no ambiente WSL remoto do VS Code.

    ![Extensão remota do WSL](../images/wsl-remote-extension.png)

6. Há três comandos que você precisará saber quando o Nuxt.js estiver instalado:

    - `npm run dev` para executar uma instância de desenvolvimento com recarga dinâmica, observação de arquivo e nova execução de tarefa.
    - `npm run build` para compilar seu projeto.
    - `npm start` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**Exibir > Terminal**). Verifique se o caminho do terminal está indicando o diretório do projeto (por exemplo, `~/NuxtProjects/my-nuxt-app$`). Em seguida, tente executar uma instância de desenvolvimento do novo aplicativo Nuxt.js usando: `npm run dev`

6. O servidor de desenvolvimento local será iniciado (exibindo algumas barras de progresso legais para as compilações do cliente e do servidor). Após a criação do projeto, seu terminal exibirá "Compilado com êxito" com o tempo que a compilação levou. Aponte o navegador da Web para [http://localhost:3000](http://localhost:3000) a fim de abrir o novo aplicativo Nuxt.js.

    ![O aplicativo Nuxt.js em execução no localhost:3000](../images/nuxt-app.png)

7. Abra o arquivo `pages/index.vue` no editor do VS Code. Localize o título da página `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` e altere-o para `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Com o navegador da Web ainda aberto em localhost:3000, salve sua alteração e observe que o recurso de recarga dinâmica compila e atualiza automaticamente a alteração no navegador.

8. Vejamos como o Nuxt.js lida com erros. Remova a marca de fechamento `</v-card-title>` para que o código de título agora tenha esta aparência: `<v-card-title class="headline">This is my new Nuxt.js app!`. Salve essa alteração e observe que um erro de compilação será exibido no navegador e no terminal, informando que a marca de fechamento `<v-card-title>` está ausente, e os números de linha em que o erro pode ser encontrado em seu código. Substitua a marca de fechamento `</v-card-title>`, salve, e a página será recarregada.

Você pode usar o depurador do VS Code com o aplicativo Nuxt.js clicando na tecla F5 ou acessando **Exibir > Depurar** (Ctrl+Shift+D) e **Exibir > Console de Depuração** (Ctrl+Shift+Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela de Depuração, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Confira mais informações em [Depuração do VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Janela de depuração do VS Code e ícone de configuração de launch.json](../images/vscode-debug-launch-configuration.png)

Para saber mais sobre o Nuxt.js, confira o [Guia do Nuxt.js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Introdução ao Gatsby.js

O Gatsby.js é uma estrutura de geração de site estático com base em React.js e que não é processada pelo servidor como o Next.js e o Nuxt.js. Um gerador de site estático gera HTML estático no tempo de compilação. Ele não requer um servidor. O Next.js e o Nuxt.js geram HTML em runtime (sempre que uma nova solicitação chega). Eles necessitam de um servidor para a execução. O Gatsby também determina como lidar com dados em seu aplicativo (com GraphQL), enquanto o Next.js e o Nuxt.js deixam essa decisão para você.

Para criar um projeto Gatsby.js:

1. Abra o terminal do WSL (ou seja, Ubuntu 18.04).
2. Crie uma pasta de projeto, `mkdir GatsbyProjects`, e insira este diretório: `cd GatsbyProjects`
3. Use npm para instalar a CLI do Gatsby: `npm install -g gatsby-cli`. Depois de instalado, verifique a versão com `gatsby --version`.
4. Crie seu projeto Gatsby.js: `gatsby new my-gatsby-app`
5. Depois de instalar o pacote, altere os diretórios para a nova pasta de aplicativo, `cd my-gatsby-app`, e use `code .` para abrir o projeto Gatsby.js no VS Code. Assim, você pode examinar a estrutura do Gatsby.js criada para seu aplicativo usando o Explorador de Arquivos do VS Code. Observe que o VS Code abriu seu aplicativo em um ambiente WSL remoto (como indicado na guia verde na parte inferior esquerda da janela do VS Code). Isso significa que, enquanto você estiver usando o VS Code para editar no sistema operacional Windows, ele ainda estará executando seu aplicativo no sistema operacional Linux.

    ![Extensão remota do WSL](../images/wsl-remote-extension.png)

6. Há três comandos que você precisará saber quando o Gatsby estiver instalado:

    - `gatsby develop` para executar uma instância de desenvolvimento com recarga dinâmica.
    - `gatsby build` para criar um build de produção.
    - `gatsby serve` para iniciar seu aplicativo no modo de produção.

    Abra o terminal WSL integrado no VS Code (**Exibir > Terminal**). Verifique se o caminho do terminal está indicando o diretório do projeto (por exemplo, `~/GatsbyProjects/my-gatsby-app$`). Em seguida, tente executar uma instância de desenvolvimento do novo aplicativo usando: `gatsby develop`

7. Depois que o novo projeto Gatsby concluir a compilação, o terminal exibirá que "Agora você pode exibir gatsby-starter-default no navegador. [http://localhost:8000/](http://localhost:8000/)." Selecione esse link do localhost para abrir o novo projeto criado em um navegador da Web.

> [!NOTE]
> Você observará que a saída do terminal também informará que você pode "Exibir GraphiQL, um IDE no navegador, para explorar os dados e o esquema do seu site: [http://localhost:8000/___graphql](http://localhost:8000/___graphql)". O GraphQL consolida as APIs em um IDE de autodocumentação (GraphiQL) integrado ao Gatsby. Além de explorar os dados e o esquema de seu site, você pode executar operações GraphQL, como consultas, mutações e assinaturas. Confira mais informações em [Introdução ao GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Abra o arquivo `src/pages/index.js` no editor do VS Code. Localize o título da página `<h1 >Hi people</h1>` e altere-o para `<h1 >Hi (Your Name)!</h1>`. Com o navegador da Web ainda aberto em localhost:8000, salve sua alteração e observe que o recurso de recarga dinâmica compila e atualiza automaticamente sua alteração no navegador.

    ![O aplicativo Gatsby.js em execução no localhost:3000](../images/gatsby-app.png)

9. Vejamos como o Next.js lida com erros. Remova a marca de fechamento `</h1>` para que o código de título agora tenha esta aparência: `<h1>Hi (Your Name)!`. Salve essa alteração e observe que será exibido o erro "Falha ao compilar" no navegador e no seu terminal, informando que se espera uma marca de fechamento para `<h1>`. Substitua a marca de fechamento `</h1>`, salve, e a página será recarregada.

Você pode usar o depurador do VS Code com o aplicativo Next.js clicando na tecla F5 ou acessando **Exibir > Depurar** (Ctrl+Shift+D) e **Exibir > Console de Depuração** (Ctrl+Shift+Y) na barra de menus. Se você selecionar o ícone de engrenagem na janela de Depuração, um arquivo de configuração de inicialização (`launch.json`) será criado para que você salve os detalhes da configuração de depuração. Confira mais informações em [Depuração do VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Janela de depuração do VS Code e ícone de configuração de launch.json](../images/vscode-debug-launch-configuration.png)

Confira mais informações sobre o Gatsby em [Documentos do Gatsby.js](https://www.gatsbyjs.org/docs/).
