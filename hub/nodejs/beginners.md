---
title: Introdução ao uso do NodeJS no Windows para iniciantes
description: Um guia para ajudar iniciantes a começarem a usar o desenvolvimento do node. js no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node. js, Windows 10, Microsoft, Learning NodeJS, nó no Windows, nó no Windows para iniciantes, desenvolva com node no Windows, desenvolvedor com NodeJS no Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657078"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Introdução ao uso do node. js no Windows para iniciantes

Se você estiver começando a usar o Node. js, este guia o ajudará a começar com algumas noções básicas.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que você já concluiu as etapas para [Configurar o ambiente de desenvolvimento do node. js no Windows nativo](./setup-on-windows.md), incluindo:

- Instale um Gerenciador de versão do node. js.
- Instale o Visual Studio Code.

Instalar o Node. js diretamente no Windows é a maneira mais simples de começar a executar operações do node. js básicas com uma quantidade mínima de configuração.

Quando você estiver pronto para usar o Node. js para desenvolver aplicativos para produção, o que normalmente envolve a implantação em um servidor Linux, recomendamos que você [Configure o ambiente de desenvolvimento do node. js com o WSL2](./setup-on-wsl2.md). Embora seja possível implantar aplicativos Web em servidores Windows, é muito mais comum [usar servidores Linux para hospedar seus aplicativos node. js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Tipos de aplicativos Node.js

Node. js é um tempo de execução do JavaScript usado principalmente para criar aplicativos Web. Em outras palavras, é uma implementação no lado do servidor do JavaScript usada para gravar o back-end de um aplicativo. (Embora muitas estruturas node. js também possam lidar com o front-end.) Aqui estão alguns exemplos do que você pode criar com o Node. js.

- **Aplicativos de página única (spas)** : são aplicativos Web que funcionam dentro de um navegador e não precisam recarregar uma página sempre que você usá-la para obter novos dados. Alguns exemplos de SPAs incluem aplicativos de rede social, email ou aplicativos de mapa, texto online ou ferramentas de desenho, etc.
- **Aplicativos em tempo real (RTAS)** : são aplicativos Web que permitem que os usuários recebam informações assim que são publicados por um autor, em vez de exigir que o usuário (ou software) verifique uma origem periodicamente para atualizações. Alguns exemplos de RTAs incluem aplicativos de mensagens instantâneas ou salas de bate-papo, jogos com vários jogadores online que podem ser reproduzidos no navegador, documentos de colaboração online, armazenamento da Comunidade, aplicativos de videoconferência, etc.
- **Aplicativos de streaming de dados**: são aplicativos (ou serviços) que enviam dados/conteúdo à medida que chegam (ou são criados) enquanto mantém a conexão aberta para continuar baixando dados, conteúdo ou componentes adicionais, conforme necessário. Alguns exemplos incluem aplicativos de streaming de vídeo e áudio.
- **APIs REST**: são interfaces que fornecem dados para o qual o aplicativo Web de outra pessoa interage. Por exemplo, um serviço de API de calendário pode fornecer datas e horas para um local de concerto que poderia ser usado pelo site de eventos locais de outra pessoa.
- **Aplicativos renderizados no lado do servidor (SSRS)** : esses aplicativos Web podem ser executados no cliente (no navegador/front-end) e no servidor (o back-end), permitindo que as páginas dinâmicas sejam exibidas (gere HTML para) qualquer conteúdo que seja conhecido e pegue rapidamente o conteúdo que não é conhecido como está disponível. Eles são frequentemente chamados de aplicativos "isomórficos" ou "Universal". O SSRs utiliza métodos SPA, pois eles não precisam ser recarregados toda vez que você usá-lo. No entanto, o SSRs oferece alguns benefícios que podem ou não ser importantes para você, como fazer com que o conteúdo do seu site apareça nos resultados do Google Search e fornecendo uma imagem de visualização quando os links para seu aplicativo são compartilhados em mídias sociais, como o Twitter ou o Facebook. A possível desvantagem é que eles exigem um servidor node. js constantemente em execução. Em termos de exemplos, um aplicativo de rede social que dá suporte a eventos que os usuários desejarão exibir nos resultados da pesquisa e na mídia social pode se beneficiar do SSR, enquanto um aplicativo de email pode ser bem como um SPA. Você também pode executar aplicativos não SPA processados pelo servidor, que são algo como um blog do WordPress. Como você pode ver, as coisas podem ficar complicadas, você só precisa decidir o que é importante.
- **Ferramentas de linha de comando**: elas permitem automatizar tarefas repetitivas e, em seguida, distribuir sua ferramenta pelo vasto ecossistema node. js. Um exemplo de uma ferramenta de linha de comando é uma ondulação, que representa a URL do cliente e é usado para baixar conteúdo de uma URL da Internet. a ondulação é geralmente usada para instalar coisas como node. js ou, em nosso caso, um Gerenciador de versão do node. js.
- **Programação de hardware**: embora não seja tão popular quanto os aplicativos Web, o Node. js está crescendo na popularidade dos usos de IOT, como a coleta de dados de sensores, beacons, transmissores, motores ou qualquer coisa que gere grandes quantidades de dados. O Node. js pode habilitar a coleta de dados, a análise desses dados, a comunicação entre um dispositivo e um servidor e a execução de ações com base na análise. O NPM contém mais de 80 pacotes para controladores Arduino, Raspberry Pi, Intel IoT Edison, vários sensores e dispositivos Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Tente usar o Node. js no VS Code

1. Abra a linha de comando (prompt de comando, o PowerShell ou o que preferir) e crie um novo diretório: `mkdir HelloNode`e, em seguida, insira o diretório: `cd HelloNode`

2. Crie um arquivo JavaScript chamado "app. js" com uma variável chamada "msg" dentro de: `echo var msg > app.js`

3. Abra o diretório e seu arquivo app. js no VS Code:: `code .`

4. Adicione uma variável de cadeia de caracteres simples ("Olá, Mundo") e, em seguida, envie o conteúdo da cadeia de caracteres para o console digitando-o no arquivo "app. js":

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Para executar o arquivo "app. js" com o Node. js. Abra o seu terminal no VS Code selecionando **exibir** > **terminal** (ou selecione CTRL + ', usando o caractere de acento grave). Se você precisar alterar o terminal padrão, selecione o menu suspenso e escolha **selecionar shell padrão**.

6. No terminal, digite: `node app.js`. Você deverá ver a saída: "Olá, Mundo".

> [!NOTE]
> Observe que, quando você digita `console` no arquivo ' app. js ', VS Code exibe as opções com suporte relacionadas ao objeto [`console`](https://developer.mozilla.org/docs/Web/API/Console) para que você escolha usar o IntelliSense. Experimente experimentar o IntelliSense usando outros [objetos JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Experimente o novo [terminal do Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) se você planeja usar várias linhas de comando (Ubuntu, PowerShell, prompt de comando do Windows, etc.) ou se deseja [Personalizar o terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluindo texto, cores de plano de fundo, associações de teclas, vários painéis de janela, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configurar uma estrutura básica de aplicativo Web usando o Express

O Express é uma estrutura mínima, flexível e simplificada do Node.js que facilita o desenvolvimento de um aplicativo Web que pode lidar com vários tipos de solicitações, como GET, PUT, POST e DELETE. O Express vem com um gerador de aplicativo que criará automaticamente uma arquitetura de arquivos para o seu aplicativo.

Para criar um projeto com o Express. js:

1. Abra a linha de comando (prompt de comando, PowerShell ou o que preferir).
2. Crie uma nova pasta de projeto: `mkdir ExpressProjects` e insira esse diretório: `cd ExpressProjects`
3. Use o Express para criar um modelo de projeto HelloWorld: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Estamos usando o comando `npx` aqui para executar o pacote de nós Express. js sem realmente instalá-lo (ou ao instalá-lo temporariamente dependendo de como você deseja considerar). Se você tentar usar o comando `express` ou verificar a versão do Express instalado usando: `express --version`, você receberá uma resposta informando que o Express não pode ser encontrado. Se você quiser instalar o Express globalmente para usar repetidamente, use: `npm install -g express-generator`. Você pode exibir uma lista dos pacotes que foram instalados pelo NPM usando `npm list`. Eles serão listados por profundidade (o número de diretórios aninhados). Os pacotes que você instalou terão uma profundidade de 0. As dependências desse pacote terão a profundidade 1, outras dependências terão a profundidade 2 e assim por diante. Para saber mais, confira [diferença entre NPX e NPM?](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) em StackOverflow.

4. Examine os arquivos e pastas que expressam incluídos abrindo o projeto no VS Code, com: `code .`

   Os arquivos gerados pelo Express criarão um aplicativo Web que usa uma arquitetura específica que pode parecer um pouco complicada no início. Você verá na janela do VS Code **Explorer** (Ctrl + Shift + E para exibir) que os seguintes arquivos e pastas foram gerados:

   - `bin`. Contém o arquivo executável que inicia o aplicativo. Dispara um servidor (na porta 3000, se nenhuma alternativa for fornecida) e define o tratamento básico de erros. 
   - `public`. Contém todos os arquivos acessados publicamente, incluindo arquivos JavaScript, folhas de estilo CSS, arquivos de fonte, imagens e outros ativos que as pessoas precisam quando se conectam ao seu site.
   - `routes`. Contém todos os manipuladores de rota para o aplicativo. Dois arquivos, `index.js` e `users.js`, são gerados automaticamente nessa pasta para servir como exemplos de como separar a configuração de rota do seu aplicativo.
   - `views`. Contém os arquivos usados pelo seu mecanismo de modelagem. O Express está configurado para procurar aqui uma exibição correspondente quando o método de renderização for chamado. O mecanismo de modelo padrão é o Jade, que foi preterido em favor do Pug, portanto, usamos o sinalizador `--view` para alterar o mecanismo de exibição (modelo). Você pode ver as opções do sinalizador `--view`, e de outros, usando `express --help`.
   - `app.js`. o ponto de partida do seu aplicativo. Ele carrega tudo e começa a atender às solicitações do usuário. Basicamente é o que mantém todas as diferentes partes unidas.
   - `package.json`. Contém a descrição do projeto, o gerenciador de scripts e o manifesto do aplicativo. Seu principal objetivo é rastrear as dependências do seu aplicativo e suas respectivas versões.

5. Agora você precisa instalar as dependências que o Express usa para criar e executar seu aplicativo HelloWorld Express (os pacotes usados para tarefas como executar o servidor, conforme definido no arquivo `package.json`). Dentro de VS Code, abra o terminal selecionando **exibir** > **terminal** (ou selecione CTRL + ', usando o caractere de acento grave), certifique-se de que você ainda está no diretório do projeto ' HelloWorld '. Instale as dependências do pacote expresso com:

```bash
npm install
```

6. Neste ponto, a estrutura está configurada para um aplicativo Web de várias páginas, com acesso a diversas APIs e métodos de utilitário HTTP e middleware, facilitando a criação de uma API robusta. Inicie o aplicativo expresso em um servidor virtual digitando:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> O `DEBUG=myapp:*` parte do comando acima significa que você está dizendo ao node. js que você deseja ativar o registro em log para fins de depuração. Lembre-se de substituir ' MyApp ' pelo nome do aplicativo. Você pode encontrar o nome do aplicativo no arquivo `package.json` sob a propriedade "Name". O uso de `npx cross-env` define a variável de ambiente `DEBUG` em qualquer terminal, mas você também pode defini-la com a maneira específica do terminal. O comando `npm start` está informando NPM para executar os scripts em seu arquivo de `package.json`.

7. Agora você pode exibir o aplicativo em execução abrindo um navegador da Web e acessando: **localhost: 3000**

   ![Captura de tela do aplicativo Express em execução em um navegador](../images/express-app.png)

8. Agora que seu aplicativo HelloWorld Express está sendo executado localmente em seu navegador, tente fazer uma alteração abrindo a pasta ' views ' no diretório do projeto e selecionando o arquivo ' index. Pug '. Uma vez aberto, altere `h1= title` para `h1= "Hello World!"` e selecione **salvar** (Ctrl + S). Exiba sua alteração atualizando a URL do **localhost: 3000** no seu navegador da Web.

9. Para interromper a execução do aplicativo expresso, em seu terminal, digite: **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Experimente usar um módulo Node.js

O Node.js tem várias ferramentas para ajudar no desenvolvimento de aplicativos Web do lado do servidor, algumas internas e muitas outras disponíveis via npm. Esses módulos podem ajudar em várias tarefas:

|Ferramenta               |Usada para                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulação de imagem, incluindo edição, redimensionamento, compactação e assim por diante, diretamente no seu código JavaScript |
|PDFKit             |Geração de PDF                                                                                            |
|validator.js       |Validação de cadeia de caracteres                                                                                         |
|imagemin, UglifyJS2|Minificação                                                                                              |
|spritesmith        |Geração de folha de Sprite                                                                                   |
|winston            |Registro em log                                                                                                  |
|commander.js       |Criação de aplicativos de linha de comando                                                                       |

Vamos usar o módulo interno do sistema operacional para obter algumas informações sobre o SO do seu computador:

1) Na linha de comando, abra a CLI do node. js. Você verá o aviso de `>` informando que você está usando o Node. js depois de inserir: `node`

2) Para identificar o sistema operacional que você está usando no momento (que deve retornar uma resposta informando que você está no Windows), digite: `os.platform()`

3) Para verificar a arquitetura da CPU, digite: `os.arch()`

4) Para exibir as CPUs disponíveis no seu sistema, digite: `os.cpus()`

5) Saia da CLI do Node.js inserindo `.exit` ou selecionando Ctrl+C duas vezes.

   > [!TIP]
   > Você pode usar o módulo do sistema operacional node. js para fazer coisas como verificar a plataforma e retornar uma variável específica da plataforma: Win32/. bat para o desenvolvimento do Windows, Darwin/. sh para Mac/UNIX, Linux, SunOS e assim por diante (por exemplo, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Próximas etapas

Neste guia, você aprendeu algumas coisas básicas sobre o que pode fazer com o Node. js, tentado usar a linha de comando do node. js em VS Code, criou um aplicativo Web simples com o Express. js e o executou localmente no navegador da Web e, em seguida, tentou usar alguns dos módulos internos do node. js. Para saber mais sobre como instalar e usar algumas estruturas da Web do node. js populares, vá para o próximo guia que aborda o Next. js (uma estrutura da Web renderizada pelo servidor com base em reagir), Nuxt. js (uma estrutura da Web renderizada pelo servidor com base em Vue) e Gatsby (a estrutura da Web renderizada estaticamente com base em reagir). Você também pode pular para aprender a trabalhar com bancos de dados MongoDB ou PostgreSQL ou contêineres do Docker.

- [Introdução às estruturas da Web do node. js no Windows](./web-frameworks.md)
- [Introdução à conexão de aplicativos node. js a um banco de dados](./databases.md)
- [Introdução ao uso de contêineres do Docker com node. js](./containers.md)
