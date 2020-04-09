---
title: Introdução ao NodeJS no Windows para iniciantes
description: Um guia para ajudar iniciantes a começarem a usar o desenvolvimento do Node.js no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, aprendizagem do nodejs, nó no Windows, nó no Windows para iniciantes, desenvolver com nó no Windows, desenvolvedor com nodejs no Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657078"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Introdução ao uso de Node.js no Windows para iniciantes

Se você estiver começando a usar o Node.js, este guia ajudará você a se familiarizar com algumas noções básicas.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [configurar o ambiente de desenvolvimento do Node.js no Windows nativo](./setup-on-windows.md), incluindo:

- Instalar um gerenciador de versão do Node.js.
- Instalar o Visual Studio Code.

Instalar o Node.js diretamente no Windows é a maneira mais simples de começar a executar operações do Node.js básicas com uma quantidade mínima de configuração.

Quando você estiver pronto para usar o Node.js para desenvolver aplicativos de produção, o que normalmente envolve a implantação em um servidor Linux, recomendamos que você [configure o ambiente de desenvolvimento do Node.js com o WSL2](./setup-on-wsl2.md). Embora seja possível implantar aplicativos Web em servidores Windows, é muito mais comum [usar servidores Linux para hospedar aplicativos Node.js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Tipos de aplicativos Node.js

O Node.js é um runtime do JavaScript usado principalmente para criar aplicativos Web. Em outras palavras, é uma implementação no lado do servidor do JavaScript usada para escrever o back-end de um aplicativo. (Embora muitas estruturas Node.js também possam lidar com o front-end.) Aqui estão alguns exemplos do que você pode criar com o Node.js.

- **SPAs (aplicativos de página única)** : são aplicativos Web que funcionam dentro de um navegador e não precisam recarregar uma página sempre que você usá-la para obter novos dados. Alguns exemplos de SPAs incluem aplicativos de rede social, aplicativos de mapa ou email, texto online ou ferramentas de desenho etc.
- **RTAs (aplicativos em tempo real)** : são aplicativos Web que permitem aos usuários receber as informações assim que publicadas por um autor, em vez de exigir que o usuário (ou software) verifique uma fonte periodicamente em busca de atualizações. Alguns exemplos de RTAs incluem aplicativos de mensagens instantâneas ou salas de chat, jogos com vários jogadores online que podem ser reproduzidos no navegador, documentos de colaboração online, armazenamento da Comunidade, aplicativos de videoconferência etc.
- **Aplicativos de streaming de dados**: são aplicativos (ou serviços) que enviam dados/conteúdo à medida que chegam (ou são criados) e ao mesmo tempo mantêm a conexão aberta para continuar baixando dados, conteúdo ou componentes adicionais, conforme necessário. Alguns exemplos incluem aplicativos de streaming de vídeo e áudio.
- **APIs REST**: são as interfaces que fornecem dados para interação com o aplicativo Web de outra pessoa. Por exemplo, um serviço de API de Calendário pode fornecer datas e horas para um local de show que poderia ser usado pelo site de eventos locais de outra pessoa.
- **Aplicativos SSR (renderizados do lado do servidor)** : esses aplicativos Web podem ser executados no cliente (no navegador/front-end) e no servidor (o back-end), permitindo que as páginas dinâmicas exibam (gerem HTML para) qualquer conteúdo conhecido e extraiam rapidamente conteúdo não conhecido quando disponível. Eles são frequentemente chamados de aplicativos "isomórficos" ou "universais". Os SSRs utilizam métodos SPA, de modo que não precisam ser recarregados toda vez que forem usados. No entanto, os SSRs oferecem alguns benefícios que podem ou não ser importantes para você, como fazer com que o conteúdo do seu site apareça nos resultados da pesquisa do Google e fornecer uma imagem de visualização quando os links para seu aplicativo são compartilhados em mídias sociais, como o Twitter ou o Facebook. A possível desvantagem é que ele exige um servidor Node.js em constante execução. A título de exemplo, um aplicativo de rede social com suporte a eventos que os usuários desejam que apareçam nas resultados da pesquisa e em mídias sociais pode se beneficiar de SSR, mas aplicativos de email podem se beneficiar mais de SPA. Você também pode executar aplicativos não SPA renderizados pelo servidor, como um blog do WordPress. Como podemos ver, conforme as coisas vão ficando complicadas, você precisa decidir o que é importante.
- **Ferramentas da linha de comando**: permitem automatizar tarefas repetitivas e, em seguida, distribuir sua ferramenta pelo vasto ecossistema Node.js. Um exemplo de ferramenta de linha de comando é cURL, que representa a URL do cliente e é usada para baixar conteúdo de uma URL da internet. A cURL geralmente é usada para instalar itens como Node.js ou, em nosso caso, um gerenciador de versão do Node.js.
- **Programação de hardware**: embora não seja tão conhecido quanto os aplicativos Web, o Node.js vem crescendo em popularidade para usos de IoT, como coletar dados de sensores, sinalizadores, transmissores, motores ou qualquer item que gere grandes quantidades de dados. O Node.js pode habilitar a coleta e análise de dados, a comunicação entre um dispositivo e um servidor e a execução de ações com base na análise. O NPM contém mais de 80 pacotes para controladores Arduino, Raspberry Pi, Intel IoT Edison, vários sensores e dispositivos Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Tentar usar o Node.js no VS Code

1. Abra a linha de comando (prompt de comando, PowerShell ou o que preferir) e crie um diretório: `mkdir HelloNode`; em seguida, insira o diretório: `cd HelloNode`

2. Crie um arquivo JavaScript chamado "app.js" e adicione uma variável chamada "msg" dentro: `echo var msg > app.js`

3. Abra o diretório e seu arquivo app.js no VS Code: `code .`

4. Adicione uma variável de cadeia de caracteres simples ("Olá, Mundo") e envie o conteúdo da cadeia de caracteres para o console digitando isto no arquivo "app.js":

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Para executar o arquivo "app.js" com o Node.js, abra seu terminal no VS Code selecionando **Exibir** > **Terminal** (ou use as teclas CTRL+`, clicando no caractere de acento grave). Se você precisar alterar o terminal padrão, selecione o menu suspenso e escolha **Selecionar Shell Padrão**.

6. No terminal, digite: `node app.js`. Você deverá ver a saída: "Olá, Mundo".

> [!NOTE]
> Quando você digita `console` no arquivo "app.js", o VS Code exibe opções com suporte relacionadas ao objeto [`console`](https://developer.mozilla.org/docs/Web/API/Console) para você escolher usando o IntelliSense. Tente experimentar o IntelliSense usando outros [objetos JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Experimente o novo [terminal do Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) se você planeja usar várias linhas de comando (Ubuntu, PowerShell, prompt de comando do Windows etc.) ou se quiser [personalizar seu terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluindo texto, cores de tela de fundo, painéis de múltiplas janelas, associações de teclas etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configurar uma estrutura de aplicativo Web básica usando o Express

O Express é uma estrutura Node.js mínima, flexível e simplificada que facilita o desenvolvimento de um aplicativo Web capaz de lidar com vários tipos de solicitações, como GET, PUT, POST e DELETE. O Express vem com um gerador de aplicativos que cria automaticamente uma arquitetura de arquivos para seu aplicativo.

Para criar um projeto com o Express.js:

1. Abra a linha de comando (prompt de comando, PowerShell ou o que preferir).
2. Crie uma pasta de projeto, `mkdir ExpressProjects`, e insira este diretório: `cd ExpressProjects`
3. Use o Express para criar um modelo de projeto OláMundo: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Estamos usando o comando `npx` aqui para executar o pacote de nós do Express.js sem realmente instalá-lo (ou instalando-o temporariamente, dependendo de como você quiser interpretar). Se você tentar usar o comando `express` ou verificar a versão do Express instalado usando `express --version`, receberá uma resposta informando que não foi possível encontrar o Express. Para instalar o Express globalmente e usá-lo várias vezes, utilize: `npm install -g express-generator`. Você pode exibir uma lista dos pacotes que foram instalados pelo npm usando `npm list`. Eles serão listados por profundidade (o número de diretórios aninhados em profundidade). Os pacotes que você instalou estarão em profundidade 0. As dependências do pacote estarão em profundidade 1, as dependências seguintes na profundidade 2, e assim por diante. Para saber mais, confira [Diferença entre npx e npm?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) no StackOverflow.

4. Examine os arquivos e pastas incluídos pelo Express abrindo o projeto no VS Code, com: `code .`

   Os arquivos gerados pelo Express criarão um aplicativo Web que usa uma arquitetura que, a princípio, pode parecer um pouco complexa. Você verá na janela do **Explorador** do VS Code (CTRL+Shift+E para exibir) que os seguintes arquivos e pastas foram gerados:

   - `bin`. Contém o arquivo executável que inicializa seu aplicativo. Ele aciona um servidor (na porta 3000, se nenhuma alternativa for fornecida) e configura o tratamento básico de erros. 
   - `public`. Contém todos os arquivos acessados publicamente, incluindo arquivos JavaScript, folhas de estilo CSS, arquivos de fonte, imagens e quaisquer outros ativos que as pessoas precisem ao se conectarem ao seu site.
   - `routes`. Contém todos os manipuladores de rota do aplicativo. Dois arquivos, `index.js` e `users.js`, são gerados automaticamente nessa pasta para servir como exemplo de como separar a configuração de rota do aplicativo.
   - `views`. Contém os arquivos usados por seu mecanismo de modelo. O Express está configurado para procurar uma exibição correspondente quando o método de renderização é chamado. O Jade é o mecanismo de modelo padrão, mas ele foi preterido em favor do Pug, portanto, usamos o sinalizador `--view` para alterar o mecanismo de exibição (modelo). Você pode ver as opções de sinalizador `--view`, entre outras, usando `express --help`.
   - `app.js`. O ponto de partida do aplicativo. Ele carrega tudo e começa a atender às solicitações do usuário. É basicamente a cola que mantém todas as partes juntas.
   - `package.json`. Contém a descrição do projeto, o gerenciador de scripts e o manifesto do aplicativo. Sua principal finalidade é controlar as dependências do aplicativo e suas respectivas versões.

5. Agora, você precisa instalar as dependências que o Express usa para criar e executar seu aplicativo OláMundo (os pacotes usados para tarefas como executar o servidor, conforme definido no arquivo `package.json`). Dentro do VS Code, abra seu terminal selecionando **Exibir** > **Terminal** (ou as teclas CTRL+`, usando o caractere de acento grave), certifique-se de que você ainda esteja no diretório do projeto "OláMundo". Instale as dependências do pacote do Express com:

```bash
npm install
```

6. Neste ponto, você tem a estrutura configurada para um aplicativo Web de várias páginas com acesso a uma grande variedade de APIs e métodos utilitários de HTTP e middleware, facilitando a criação de uma API robusta. Inicie o aplicativo Express em um servidor virtual digitando:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> A parte do comando `DEBUG=myapp:*` acima indica que você está dizendo ao Node.ja que deseja ativar o registro em log para fins de depuração. Lembre-se de substituir "myapp" pelo nome do aplicativo. Você pode encontrar o nome do aplicativo no arquivo `package.json`, na propriedade "name". O uso de `npx cross-env` define a variável de ambiente `DEBUG` em qualquer terminal, mas você também pode defini-la com o método específico de seu terminal. O comando `npm start` está dizendo para o npm executar os scripts em seu arquivo de `package.json`.

7. Agora você pode exibir o aplicativo em execução abrindo um navegador da Web e acessando: **localhost:3000**

   ![Captura de tela do aplicativo Express em execução em um navegador](../images/express-app.png)

8. Agora que seu aplicativo Express OláMundo está sendo executado localmente em seu navegador, tente fazer uma alteração abrindo a pasta "views" no diretório do projeto e selecionando o arquivo "index.pug". Uma vez aberto, altere `h1= title` para `h1= "Hello World!"` e selecione **Salvar** (CTRL+S). Exiba sua alteração atualizando a URL **localhost:3000** no navegador da Web.

9. Para interromper a execução do aplicativo Express, digite no terminal: **CTRL+C**

## <a name="try-using-a-nodejs-module"></a>Experimente usar um módulo Node.js

O Node.js possui ferramentas para ajudá-lo a desenvolver aplicativos Web do lado do servidor, aplicativos integrados e muitos outros disponíveis via npm. Esses módulos podem ajudar com muitas tarefas:

|Ferramenta               |Usada para                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulação de imagem, incluindo edição, redimensionamento, compactação, etc. diretamente no seu código JavaScript |
|PDFKit             |Geração de PDF                                                                                            |
|validator.js       |Validação de cadeia de caracteres                                                                                         |
|imagemin, UglifyJS2|Minificação                                                                                              |
|spritesmith        |Geração de folha de Sprite                                                                                   |
|winston            |Log                                                                                                  |
|commander.js       |Criar aplicativos de linha de comando                                                                       |

Vamos usar o módulo de SO interno para obter algumas informações sobre o sistema operacional do seu computador:

1) Na linha de comando, abra a CLI do Node.js. Você verá o aviso `>` informando que você está usando o Node.js depois de inserir: `node`

2) Para identificar o sistema operacional usado no momento (o que deve retornar uma resposta indicando que você está no Windows), digite: `os.platform()`

3) Para verificar a arquitetura da CPU, digite: `os.arch()`

4) Para exibir as CPUs disponíveis no sistema, digite: `os.cpus()`

5) Saia da CLI do Node.js inserindo `.exit` ou selecionando as teclas CTRL+C duas vezes.

   > [!TIP]
   > Você pode usar o módulo do sistema operacional Node.js para realizar ações como verificar a plataforma e retornar uma variável específica da plataforma: Win32/.bat para desenvolvimento do Windows, Darwin/.sh para Mac/unix, Linux, SunOS e assim por diante (por exemplo, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Próximas etapas

Neste guia, você aprendeu algumas noções básicas sobre o que é possível fazer com o Node.js. Você experimentou a linha de comando do Node.js em VS Code, criou um aplicativo Web simples com o Express.js, executou-o localmente no navegador da Web e, em seguida, tentou usar alguns dos módulos internos do Node.js. Para saber mais sobre como instalar e usar algumas estruturas da Web conhecidas do Node.js, acesse o próximo guia que aborda o Next.js (uma estrutura da Web renderizada pelo servidor com base em React), Nuxt.js (uma estrutura da Web renderizada pelo servidor com base em Vue) e Gatsby (a estrutura da Web renderizada estaticamente com base em React). Você também pode ir direto para o artigo sobre como trabalhar com bancos de dados MongoDB ou PostgreSQL ou contêineres Docker.

- [Introdução às estruturas da Web do Node.js no Windows](./web-frameworks.md)
- [Introdução à conexão de aplicativos do Node.js a um banco de dados](./databases.md)
- [Introdução ao uso de contêineres do Docker com o Node.js](./containers.md)
