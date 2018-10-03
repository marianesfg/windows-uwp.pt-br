---
author: libbymc
title: Criar um app Web de página única com back-end da API REST
description: Usar tecnologias da Web populares para criar um Aplicativo Web Hospedado para a Microsoft Store
keywords: aplicativo web hospedado, HWA, API REST, aplicativo de página única, SPA
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.localizationpriority: medium
ms.openlocfilehash: 42f11cbdd749a44c4ba0d8bc1a0397a4f2882257
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4318230"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>Criar um app Web de página única com back-end da API REST

**Criar um Aplicativo Web Hospedado para a Microsoft Store com tecnologias da Web fullstack populares**

![Jogo de memória simples como um aplicativo Web de página única](images/fullstack.png)

Este tutorial de duas partes oferece um tour rápido pelo desenvolvimento moderno da Web fullstack à medida que você cria um jogo de memória simples que funciona no navegador e como um Aplicativo Web Hospedado para a Microsoft Store. Na Parte I, você criará um serviço API REST simples para o back-end do jogo. Armazenando a lógica do jogo na nuvem como um serviço de API, você preserva o estado do jogo para que o usuário possa continuar no mesmo jogo em dispositivos diferentes. Na Parte II, você criará a interface do usuário front-end como um aplicativo Web de página única com layout dinâmico.

Usaremos algumas das tecnologias da Web mais populares, incluindo o [tempo de execução Node.js](https://nodejs.org/en/) e o [Express](http://expressjs.com/) para o desenvolvimento do lado do servidor, a estrutura da IU [Bootstrap](http://getbootstrap.com/), o mecanismo de modelo [Pug](https://www.npmjs.com/package/pug) e o [Swagger](http://swagger.io/tools/) para a criação de APIs RESTful. Você também adquirirá experiência com o [Portal do Azure](https://ms.portal.azure.com/) para hospedagem na nuvem e trabalho com o editor [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver esses recursos no computador, siga estes links de download:

 - [Node.js](https://nodejs.org/en/download/) - Não deixe de selecionar a opção para adicionar o Node ao CAMINHO.

 - [Gerador Express](http://expressjs.com/en/starter/generator.html)- Após instalar o Node, instale o Express executando o `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

Se você quiser concluir as etapas finais de hospedagem do serviço de API e do aplicativo de jogo no Microsoft Azure, você precisará [criar uma conta gratuita do Azure](https://azure.microsoft.com/en-us/free/), caso ainda não tenha feito isso.

Se você decidir alça em (ou adiar) a parte do Azure, basta ignorar as seções finais das partes I e II, que abrangem a hospedagem do Azure e o empacotamento do aplicativo para a Microsoft Store. O serviço de API e o aplicativo Web criado ainda serão executados localmente (em `http://localhost:8000` e `http://localhost:3000`, respectivamente) no computador.

## <a name="part-i-build-a-rest-api-backend"></a>Parte I: Compilar um back-end da API REST

Primeiro, criaremos uma API de jogo de memória simples para ativar nosso aplicativo Web de jogo de memória. Usaremos o [Swagger](http://swagger.io/) para definir a API e gerar código scaffold e uma interface do usuário da Web para teste manual.

Se você quiser ignorar esta parte e passar diretamente para a [Parte II: Criar um aplicativo Web de página única](#part-ii-build-a-single-page-web-appl), este é o [código final da Parte I](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend). Siga as instruções do *LEIAME* para que o código seja executado localmente ou consulte *5. Hospedar o serviço de API no Azure e habilitar o CORS* para executá-lo no Azure.

### <a name="game-overview"></a>Visão geral do jogo

*Memória* (também conhecido como [*Concentration*](https://en.wikipedia.org/wiki/Concentration_(game)) e [*Pelmanism*](https://en.wikipedia.org/wiki/Pelmanism_(system))), é um jogo simples que consiste em uma série de pares de cartas. As cartas são colocados viradas para baixo na mesa e o jogador inspeciona os valores das cartas, duas ao mesmo tempo, procurando correspondências. Após cada rodada, as cartas são viradas para baixo novamente, a não ser que um par correspondente seja encontrado; nesse caso, essas duas cartas são retiradas do jogo. O objetivo do jogo é encontrar todos os pares de cartas na menor quantidade de rodadas.

Para fins de instrução, a estrutura de jogo que usaremos é muito simples: é um único jogo e um único jogador. No entanto, a lógica do jogo acontece no lado do servidor (e não no lado do cliente) para preservar o estado do jogo; assim, você pode continuar no mesmo jogo em dispositivos diferentes.

A estrutura de dados de um jogo de memória consiste simplesmente em uma matriz de objetos JavaScript, cada um representando uma única carta, com índices na matriz atuando como IDs de carta. No servidor, cada objeto de carta tem um valor e um sinalizador **cleared**. Por exemplo, um tabuleiro com duas correspondências (quatro cartas) pode ser gerado aleatoriamente e serializado desta forma.

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

Quando a matriz do tabuleiro for passada para o cliente, as chaves de valor serão removidas da matriz para evitar trapaças (por exemplo, a inspeção do corpo da resposta HTTP por meio das ferramentas de navegador F12). Esta será a aparência do novo jogo para um cliente que está chamando o ponto de extremidade REST **GET /game**:

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

Falando em pontos de extremidade, o serviço de jogo de memória consiste em três APIs REST.

#### <a name="post-new"></a>POST /new
Inicializa um novo tabuleiro de jogo do tamanho especificado (N° de correspondências).

| Parâmetro | Descrição |
|-----------|-------------|
| int *size* |Número de pares correspondentes a serem embaralhados no "tabuleiro" do jogo. Exemplo: `http://memorygameapisample/new?size=2`|

| Resposta | Descrição |
|----------|-------------|
| 200 OK | o novo jogo de memória do tamanho solicitado está pronto.|
| 400 BAD REQUEST| O tamanho solicitado está fora do intervalo aceitável.|


#### <a name="get-game"></a>GET /game
Recupera o estado atual do tabuleiro do jogo de memória.

*Sem parâmetros*

| Resposta | Descrição |
|----------|-------------|
| 200 OK | Retorna a matriz JSON de objetos de carta. Cada carta tem uma propriedade **cleared** propriedade indicando se sua correspondência já foi encontrada. As cartas correspondentes também reportam **value**. Exemplo: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
Especifica uma carta a ser revelada e procura uma correspondência para a carta revelada anteriormente.

| Parâmetro | Descrição |
|-----------|-------------|
| int *card* | ID da carta (índice na matriz de tabuleiro do jogo) da carta a ser revelada. Cada "suposição" completa consiste em duas cartas especificadas (por exemplo, duas chamadas para **/guess** com valores *card* válidos e exclusivos). Exemplo: `http://memorygameapisample/guess?card=0`|

| Resposta | Descrição |
|----------|-------------|
| 200 OK | Retorna o JSON com **id** e **value** da carta especificada. Exemplo: `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  Erro com a carta especificada. Consulte o corpo da resposta HTTP para obter mais detalhes.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. Especifique a API e gere stubs de código

Usaremos o [Swagger](http://swagger.io/) para transformar o design da API de jogo de memória em um código funcional de servidor Node.js. Saiba como você definir as [APIs de jogo de memória como metadados do Swagger](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json). Usaremos isso para gerar stubs de código de servidor.

1. Crie uma nova pasta (no diretório local do *GitHub*, por exemplo) e baixe o arquivo [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) arquivo que contém as definições de API de jogo de memória. Assegure que o nome da pasta não contém espaços.

2. Abra seu shell favorito ([ou use o terminal integrado do Visual Studio Code!](https://code.visualstudio.com/docs/editor/integrated-terminal)) para essa pasta e execute o seguinte comando do NPM (Gerenciador de Pacotes do Nó) para instalar a ferramenta de scaffolding de código [Yeoman](http://yeoman.io/) (yo) e o gerador Swagger para seu ambiente Node global (**-g**):

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. Agora podemos gerar o código scaffold de servidor usando o Swagger:

    ```
    yo swaggerize
    ```

4. O comando **swaggerize** fará várias perguntas.
    - Caminho (ou URL) para balancear documento: **api.json**
    - Estrutura: **express**
    - O que você deseja para chamar este projeto (InsiraSeuNomedePastaAqui): **[enter]**

    Responda a qualquer outra pergunta que desejar; as informações são principalmente para fornecer ao arquivo *package.json* as informações de contato para que você possa distribuir seu código como um pacote NPM.

5. Por fim, instale todas as dependências (listadas no *package.json*) do novo projeto e o suporte à [interface do usuário do Swagger](http://swagger.io/swagger-ui/).

    ```
    npm install
    npm install swaggerize-ui
    ```

    Inicie o VS Code, clique em **File** > **Open Folder...** e vá para o diretório MemoryGameAPI. Este é o servidor de API Node.js que você acabou de criar! Ele usa a estrutura de aplicativo Web popular [ExpressJS](http://expressjs.com/en/4x/api.html) para estruturar e executar seu projeto.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. Personalize o código de servidor e a depuração da configuração

O arquivo *server. js* na raiz do projeto atua como a função "principal" do servidor. Abra-o no VS Code e copie o conteúdo a seguir. As linhas modificadas do código gerado serão comentadas com explicação adicional.

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

Depois disso, é hora de executar o servidor! Vamos configurar o Visual Studio Code para depuração do nó durante esse processo. Selecione o ícone de painel **Debug** (Ctrl+Shift+D) e, em seguida, o ícone de engrenagem (Abrir launch.json) e modifique as "configurações" para:

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

Pressione F5 e abra o navegador em [http://localhost:8000](http://localhost:8000). A página abrirá a interface do usuário do Swagger da API do jogo de memória e, nesse local, você poderá expandir os detalhes e os campos de entrada de cada um dos métodos. Você pode até mesmo tentar chamar as APIs, embora suas respostas contenham apenas dados simulados (fornecidos pelo módulo [Swagmock](https://www.npmjs.com/package/swagmock)). É hora de adicionar a lógica do jogo para tornar essas APIs reais.

### <a name="3-set-up-your-route-handlers"></a>3. Configure os manipuladores de rotas

O arquivo do Swagger (config\swagger.json) ensina o servidor a manipular várias solicitações HTTP de cliente por meio do mapeamento de cada caminho de URL definido para um arquivo de manipulador (em \handlers) e cada método definido para esse caminho (por exemplo, **GET**, **POST**) para uma `operationId` (função) nesse arquivo manipulador.

Nessa camada do programa, adicionaremos uma verificação de entrada antes de passar as várias solicitações de cliente para nosso modelo de dados. Baixe (ou copie e cole):

 - Este código [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) para o arquivo **handlers\game.js**
 - Este código [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) para o arquivo **handlers\guess.js**
 - Este código [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) para o arquivo **handlers\new.js**

 Você pode ler os comentários nesses arquivos para obter mais detalhes sobre as alterações, mas basicamente eles procuram erros básicos de entrada (por exemplo, o cliente solicita um novo jogo com menos de uma correspondência) e enviam mensagens de erro descritivas quando necessário. Os manipuladores também roteiam solicitações de cliente válidas para os arquivos de dados correspondentes (em \data) para processamento adicional. Vamos trabalhar neles a seguir.

### <a name="4-set-up-your-data-model"></a>4. Configure o modelo de dados

É hora de trocar o serviço de simulação de dados de espaço reservado por um modelo de dados real do tabuleiro do jogo de memória.

Essa camada do programa representa as própria cartas de memória e fornece a maior parte da lógica do jogo, incluindo o "embaralhamento" da série de cartas de um novo jogo, identificando pares de cartas correspondentes e controlando o estado do jogo. Copie e cole:

 - Este código [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) para o arquivo **data\game.js**
 - Este código [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) para o arquivo **data\guess.js**
 - Este código [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) para o arquivo **data\new.js**

Para simplificar, estamos armazenando o tabuleiro do jogo em uma variável global (`global.board`) no servidor do nó. Mas, na verdade, você usaria o armazenamento em nuvem (como o Google [Cloud Datastore](https://cloud.google.com/datastore/) ou o Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/)) para torná-lo um serviço de API de jogo de memória viável que dá suporte a vários jogadores e jogos simultaneamente.

Verifique se você salvou todas as alterações no VS Code, acione o servidor novamente (F5 no VS Code ou `npm start` no shell e vá até [http://localhost:8000](http://localhost:8000)) para testar a API do jogo.

Cada vez que você pressionar o botão **Try it out!** em uma das operações **/game**, **/guess** ou **/new**, verifique o **Corpo da Resposta** e o **Código da Resposta** resultante abaixo para certificar-se de que tudo está funcionando como esperado.

Tente: 

1. Criar um novo jogo `size=2`.

    ![Inicie um novo jogo de memória na interface do usuário do Swagger](images/swagger_new.png)

2. Supor alguns valores.

    ![Adivinhe uma carta da interface do usuário do Swagger](images/swagger_guess.png)

3. Verificar o tabuleiro do jogo enquanto o jogo estiver em andamento.

    ![Verifique o estado do jogo na interface do usuário do Swagger](images/swagger_game.png)

Se tudo parecer em ordem, o serviço de API está pronto para ser hospedado no Azure! Se você estiver tendo problemas, tente comentar as linhas a seguir em \data\game.js.

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

Com essa alteração, o método **GET /game** retornará todos os valores de carta (incluindo os que não foram limpos). Este é um meio de depuração útil enquanto você cria o front-end para o jogo de memória.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (Opcional) Hospede o serviço de API no Azure e habilite o CORS

Os documentos do Azure conduzirão você pelo(a):

 - [Registro de um novo *Aplicativo de API* com o Portal do Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [Configuração da implantação do Git no aplicativo de API](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git) e
 - [Implantação do código do aplicativo de API no Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

Ao registrar o aplicativo, tente diferenciar seu *Nome de aplicativo* (para evitar conflitos de nomenclatura com outras pessoas que estão solicitando variações na URL *http://memorygameapi.azurewebsites.net*).

Se você fez isso e o Azure já está atendendo à interface do usuário do Swagger, há apenas uma etapa para o back-end do jogo de memória. No [Portal do Azure](https://portal.azure.com), selecione o *Serviço de Aplicativo* recém-criado e selecione ou procure a opção **CORS** (Compartilhamento de Recursos Entre Origens). Em **Origens Permitidas**, adicione um asterisco (`*`) e clique em **Salvar**. Isso lhe permite fazer chamadas entre origens para o serviço de API no front-end do jogo memória à medida que você o desenvolve no computador local. Depois que você finalizar o front-end do jogo de memória e implantá-lo no Azure, poderá substituir essa entrada com a URL específica do aplicativo Web.

### <a name="going-further"></a>Aprofundamento

Para tornar a API do jogo de memória um serviço back-end viável para um aplicativo de produção, você estenderá o código para oferecer suporte a vários jogadores e jogos. Para isso, você provavelmente precisará inserir a [autenticação](http://swagger.io/docs/specification/authentication/) (para o gerenciamento das identidades do jogador), um [banco de dados NoSQL](https://docs.microsoft.com/en-us/azure/documentdb/) (para o controlar de jogos e jogadores) e alguns [testes de unidade](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis) básicos na API.

Aqui estão alguns recursos úteis para aprofundamento:

 - [Depuração avançada do Node.js com o Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Web do Azure + documentos móveis](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Documentos do Azure DocumentDB](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>Parte II: Criar um aplicativo web de página única

Agora que você criou (ou [baixou](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) o [back-end da API REST](#part-i-build-a-rest-api-backend) na Parte I, você está pronto para criar o front-end do jogo de memória de página única com o [Node](https://nodejs.org/en/), o [Express](http://expressjs.com/) e o [Bootstrap ](http://getbootstrap.com/).

A Parte II deste tutorial lhe proporcionará a experiência com o: 

* [Node.js](https://nodejs.org/en/): para criar o servidor que está hospedando o jogo
* [jQuery](http://jquery.com/): uma biblioteca JavaScript
* [Express](http://expressjs.com/): para a estrutura do aplicativo Web
* [Pug](https://pugjs.org/): (antigo Jade) para o mecanismo de modelagem
* [Bootstrap](http://getbootstrap.com/): para o layout dinâmico
* [Visual Studio Code](https://code.visualstudio.com/): para criação, exibição de markdown e depuração do código

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Crie um aplicativo Node.js usando o Express

Vamos começar criando o projeto Node.js com o Express.

1. Abra um prompt de comando e navegue até o diretório no qual deseja armazenar o jogo. 
2. Use o gerador Express para criar um novo aplicativo chamado *memória* por meio do mecanismo de modelagem *Pug *.

    ```
    express --view=pug memory
    ```

3. No diretório de memória, instale as dependências listadas no arquivo package.json. O arquivo package.json é criado na raiz do projeto. Esse arquivo contém os módulos necessários ao aplicativo Node.js.  

    ```
    cd memory
    npm install
    ```

    Após a execução desse comando, você verá uma pasta chamada node_modules que contém todos os módulos necessários a esse aplicativo. 

4. Agora, execute o aplicativo.

    ```
    npm start
    ```

5. Para exibir p aplicativo, acesse [http://localhost:3000/](http://localhost:3000/).

    ![Uma captura de tela de http://localhost:3000/](./images/express.png)

6. Altere o título padrão do aplicativo Web editando index.js no diretório memory\routes. Altere `Express` na linha abaixo para `Memory Game` (ou outro título de sua preferência).

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. Para atualizar o aplicativo para ver o novo título, interrompa-o pressionando **Crtl + C**, **Y** no prompt de comando e reinicie-o com `npm start`.

### <a name="2-add-client-side-game-logic-code"></a>2. Adicione o código da lógica do jogo do lado do cliente
Você encontrará os arquivos necessários a essa metade do tutorial na pasta [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start). Se você se perder, o código final está disponível na pasta [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final). 

1. Copie o arquivo scripts.js da pasta [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) e cole-o em memory\public\javascripts. Esse arquivo contém toda a lógica necessária para executar o jogo, incluindo:

    * Criação/inicialização de um novo jogo.
    * Restauração de um jogo armazenado no servidor.
    * Desenho do tabuleiro do jogo e das cartas com base na seleção do usuário.
    * Virada das cartas.

2. Em script.js, vamos começar modificando a função `newGame()`. Essa função:

    * Manipula o tamanho da seleção de jogo do usuário.
    * Busca a [matriz do tabuleiro do jogo](#part-i-build-a-rest-api-backend) no servidor.
    * Chama a função `drawGameBoard()` para colocar o tabuleiro do jogo na tela.

    Adicione o código a seguir a `newGame()` após o comentário `// Add code from Part 2.2 here`.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    Esse código recupera o valor no menu suspenso com `id="selectGameSize"` (que criaremos mais tarde) e o armazena em uma variável (`size`).  Usamos a função [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) para analisar o valor de cadeia de caracteres na lista suspensa para retornar um número inteiro e, assim possamos passar o `size` do tabuleiro de jogo solicitado ao servidor. 

    Usamos o método [`/new`](#part-i-build-a-rest-api-backend) criado na Parte I deste tutorial para postar o tamanho de tabuleiro de jogo escolhido no servidor. O método retorna uma matriz de cartas JSON única e valores `true/false` que indicam se as cartas têm correspondência. 

3. Em seguida, preencha a função `restoreGame()` que restaura o último jogo reproduzido. Para simplificar, o aplicativo sempre carrega o último jogo reproduzido. Se não houver um jogo armazenado no servidor, use o menu suspenso para iniciar um novo jogo. 

    Copie e cole este código em `restoreGame()`.

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    Agora o jogo buscará o estado de jogo no servidor. Para obter mais informações sobre o método [`/game`](#part-i-build-a-rest-api-backend) sendo usado nesta etapa, consulte a Parte I deste tutorial. Se estiver usando o Azure (ou outro serviço) para hospedar a API de back-end, substitua o endereço *localhost* acima pela sua URL de produção.

4. Agora, vamos criar a função `drawGameBoard()`.  Essa função:

    * Detecta o tamanho do jogo e aplica estilos CSS específicos.
    * Gera as cartas em HTML.
    * Adiciona as cartas à página.

    Copie e cole este código na função `drawGameBoard()` de *scripts.js*:

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. Em seguida, precisamos concluir a função `flipCard()`.  Essa função manipula a maior parte da lógica do jogo, incluindo a obtenção dos valores das cartas selecionadas no servidor por meio do método [`/guess`](#part-i-build-a-rest-api-backend) desenvolvido na Parte I do tutorial. Não esqueça de substituir o endereço *localhost* pela URL de produção, se você estiver hospedagem o back-end da API REST na nuvem.

    Na função `flipCard()`, remova o comentário deste código:

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> Se você estiver usando o Visual Studio Code, selecione todas as linhas de código cujo comentário você deseja remova e pressione Crtl + K, U

Aqui, usamos [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) e o método **PUT** [`/guess`](#part-i-build-a-rest-api-backend) criado na Parte I. 

Esse código é executado na ordem a seguir.

* O `id`do primeiro cartão selecionado pelo usuário é adicionada à matriz selectedCards[] como primeiro valor: `selectedCards[0]` 
* O valor (`id`) em `selectedCards[0]` é postado no servidor por meio do método [`/guess`](#part-i-build-a-rest-api-backend)
* O servidor responde com o `value` dessa placa (um número inteiro)
* Um [glyphicon Bootstrap](http://getbootstrap.com/components/) é adicionado à parte de trás da carta cuja `id` é `selectedCards[0]`
* O `value` da primeira carta (do servidor) é armazenado na matriz `selectedCardsValues[]`: `selectedCardsValues[0]`. 

A segunda suposição do usuário segue a mesma lógica. Se as cartas que o usuário selecionou tiverem as mesmas IDs (por exemplo, `selectedCards[0] == selectedCards[1]`), as cartas serão correspondentes! A classe CSS `.matched` é adicionada às cartas correspondentes (tornando-as verdes) e as cartas permanecem viradas.

Agora, precisamos adicionar lógica para verificar se o usuário achou a correspondência de todas as cartas e ganhou o jogo. Na função `flipCard()`, adicione as seguintes linhas de código abaixo do comentário `//check if the user won the game`. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

Se o número de cartas viradas corresponder ao tamanho do tabuleiro do jogo (por exemplo, `cardsFlipped == gameBoardSize`), não há mais nenhuma carta a ser virada e o usuário ganhou o jogo. Adicionaremos um HTML simples a `div` com `id="game-board"` para que o usuário saiba que ganhou e pode jogar novamente.  

### <a name="3-create-the-user-interface"></a>3. Crie a interface do usuário 
Agora vamos conferir todo esse código em ação, criando a interface do usuário. Neste tutorial, usamos o mecanismo de modelagem [Pug](https://pugjs.org/) (antigo Jade).  *Pug* é uma sintaxe limpa que faz distinção de espaços em branco na escrita de HTML. Veja um exemplo a seguir. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

torna-se

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Substitua o arquivo layout.pug em memory\views pelo arquivo layout.pug fornecido, na pasta Start. Em layout.pug, você verá links para:

    * Bootstrap
    * jQuery
    * Um arquivo CSS
    * O arquivo JavaScript que acabamos de modificar

2. Abra o arquivo index.pug no diretório memory\views.
Esse arquivo estende o arquivo layout.pug e renderizará nosso jogo. Em layout.pug, cole as seguintes linhas de código:

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> Lembre-se: o Pug faz distinção de espaços em branco. Verifique se todos os recuos estão corretos!

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. Use o sistema de grade do Bootstrap para criar um layout dinâmico
O [sistema de grade](http://getbootstrap.com/css/#grid) do Bootstrap é um sistema de grade fluido que dimensiona uma grade como alterações de visor de um dispositivo. As cartas neste jogo usam classes de sistema de grade predefinidas do Bootstrap no layout, incluindo:
* `.container-fluid`: especifica o contêiner fluido da grade
* `.row-fluid`: especifica as linhas fluidas
* `.col-xs-3`: especifica o número de colunas

O sistema de grade do Bootstrap permite que um sistema de grade recolha uma coluna vertical, como você veria em um menu de navegação em um dispositivo móvel.  No entanto, como queremos que nosso jogo sempre tenha colunas, usamos a classe predefinida `.col-xs-3`, que mantém a grade sempre horizontal. 

O sistema de grade permite até 12 colunas. Como queremos apenas quatro colunas no nosso jogo, usamos a classe `.col-xs-3`. Essa classe especifica que cada uma de nossas colunas deve possuir a largura de 3 das 12 colunas disponíveis mencionadas anteriormente. Esta imagem mostra uma grade de 12 colunas e uma grade de quatro colunas, como a utilizada neste jogo.

![Grade Bootstrap com 12 colunas e quatro colunas](./images/grid.png)

1. Abra scripts.js e localize a função `drawGameBoard()`.  No bloco de código no qual geramos o HTML de cada placa, você pode identificar o elemento `div` com `class="col-xs-3"`? 

2. Em index.pug, vamos adicionar as classes Bootstrap predefinidas mencionadas anteriormente para criar nosso layout fluido.  Altere index.pug ao seguinte.

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. Adicione uma animação de virada de carta com transformações CSS
Substitua o arquivo style.css em memory\public\stylesheets pelo arquivo style.css da pasta Start.

A adição de um movimento de virada por meio de [Transformações CSS](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms) confere às cartas um movimento 3D realista. As cartas do jogo são criadas através da estrutura HTML a seguir e adicionadas de forma programática ao tabuleiro do jogo (na função `drawGameBoard()` mostrada anteriormente).

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. Para começar, dê [perspectiva](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) ao contêiner pai da animação (`.flipContainer`).  Isso dá a ilusão de profundidade em seus elementos filho: quanto maior o valor, mais afastado do usuário o elemento aparecerá. Vamos adicionar a perspectiva a seguir à classe `.flipContainer` em style.css.

    ``` css
    perspective: 1000px; 
    ```

2. Agora, adicione as propriedades a seguir à classe `.cards` em style.css. O `div` `.cards` é o elemento que fará a animação de virada de carta, mostrando a parte da frente ou de trás da carta. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    A propriedade [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) estabelece um contexto de renderização 3D, e os filhos da classe `.cards` `.front` e `.back` são membros do espaço 3D. A adição da propriedade [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) especifica o número de segundos até o término da animação. 

3.  Usando a propriedade [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform), podemos girar a carta em torno do eixo y.  Adicione a seguinte CSS a `cards.flip`.

    ``` css
    transform: rotateY(180deg);
    ```

    O estilo definido em `cards.flip` é ativado e desativado na função `flipCard` por meio de [`.toggleClass()`](http://api.jquery.com/toggleClass/). 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    Agora, quando um usuário clica em uma carta, esta é girada 180 graus.

### <a name="6-test-and-play"></a>6. Teste e jogue
Parabéns! Você concluiu a criação do aplicativo Web! Vamos testá-lo. 

1. Abra um prompt de comando no diretório de memória e digite o seguinte comando: `npm start`

2. No navegador, vá para [http://localhost:3000/](http://localhost:3000/) e jogue!

3. Se você encontrar algum erro, poderá usar as ferramentas de depuração Node.js do Visual Studio Code pressionando F5 no teclado e digitando `Node.js`. Para obter mais informações sobre a depuração no Visual Studio Code, confira este [artigo ](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations). 

    Você também pode comparar seu código com o código fornecido na pasta Final.

4. Para interromper o jogo, no prompt de comando, digite **Ctrl + C**, **Y **. 

### <a name="going-further"></a>Aprofundamento

Agora você pode implantar o aplicativo no Azure (ou em qualquer outro serviço de hospedagem de nuvem) para testar nos diferentes fatores forma de dispositivo, como o celular, tablet e desktop. (Não esqueça de testar em diferentes navegadores também!) Depois que o aplicativo estiver pronto para produção, você poderá empacotá-lo facilmente como um *Aplicativo Web Hospedado* (HWA) da *Plataforma Universal do Windows* (UWP) e distribuí-lo na Microsoft Store.

Estas são as etapas básicas para publicação na Microsoft Store:

 1. Criar uma conta de [Desenvolvedor do Windows](https://developer.microsoft.com/en-us/store/register).
 2. Usar a [lista de verificação](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions) do envio de aplicativo.
 3. Envie o aplicativo para [certificação](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)

Aqui estão alguns recursos úteis para aprofundamento:

 - [Implantar o projeto de desenvolvimento de aplicativos para sites do Azure](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [Converter o aplicativo Web em um aplicativo UWP (Plataforma Universal do Windows)](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [Publicar aplicativos do Windows](https://developer.microsoft.com/en-us/store/publish-apps)
