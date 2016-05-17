---
author: martinekuan
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: Criar um aplicativo Hello, world (JS)
description: Este tutorial ensina a usar JavaScript e HTML para criar um aplicativo Hello, world simples segmentado para a Plataforma Universal do Windows (UWP) no Windows 10.
---
# Criar um aplicativo "Hello, world" (JS)

Este tutorial ensina a usar JavaScript e HTML para criar um aplicativo "Hello, world" simples segmentado para a Plataforma Universal do Windows (UWP) no Windows 10. Com um único projeto no Microsoft Visual Studio, você pode compilar um aplicativo que seja executado em qualquer dispositivo do Windows 10. Aqui, nosso foco é criar um aplicativo que seja executado igualmente bem em dispositivos móveis e desktops.

**Importante**   Este tutorial deve ser usado com o Microsoft Visual Studio 2015 e o Windows 10. Ele não funcionará corretamente com versões anteriores.

Aqui, você aprenderá a:

-   Criar um novo projeto
-   Adicionar conteúdo HTML à página inicial
-   Manipular a entrada de toque, caneta e mouse
-   Executar o projeto na área de trabalho local e no emulador do telefone no Visual Studio.
-   Criar seus próprios estilos personalizados
-   Usar um controle da Biblioteca do Windows para JavaScript

##Antes de começar...


-   Vamos passar diretamente para as etapas usadas para a criação de um aplicativo universal simples. É altamente recomendável que você leia e entenda as informações da visão geral em [Novidades no Windows 10](https://dev.windows.com/whats-new-windows-10-dev-preview) e [O que é um Aplicativo Universal do Windows](whats-a-uwp.md) antes de começar este tutorial.
-   Para concluir este tutorial, você precisa do Windows 10 e do Visual Studio 2015. Consulte [Prepare-se para começar](get-set-up.md) para saber mais.
-   Também pressupomos que você esteja usando o layout de janela padrão no Visual Studio. Se você alterar o layout padrão, poderá redefini-lo no menu **Janela** usando o comando **Redefinir Layout da Janela**.

##Etapa 1: crie um novo projeto no Visual Studio.


Criaremos um novo aplicativo chamado `HelloWorld`. Este é o procedimento:

1.  Inicie o Visual Studio 2015.

    A tela inicial do Visual Studio 2015 é exibida.

    (De agora em diante, chamaremos o Visual Studio 2015 simplesmente de Visual Studio.)

2.  No menu **Arquivo**, selecione **Novo** > **Projeto**

    A caixa de diálogo **Novo Projeto** será exibida. O painel esquerdo da caixa de diálogo permite selecionar o tipo dos modelos que serão exibidos.

3.  No painel esquerdo, expanda **Instalado > Modelos > JavaScript > Windows** e selecione o grupo de modelos **Universal**. O painel central da caixa de diálogo exibe uma lista de modelos de projeto para aplicativos UWP (Plataforma Universal do Windows).

    ![A janela Novo Projeto ](images/js-tut-newproject.png)

    Para este tutorial, usaremos o modelo **Aplicativo em Branco**. Esse modelo cria um aplicativo UWP básico que pode ser compilado e executado, mas que não contém controles de interface do usuário nem dados. Você adicionará controles e dados ao aplicativo ao longo desses tutoriais.

4.  No painel central, selecione o modelo **Aplicativo em Branco (Universal do Windows)**.

    O modelo **Aplicativo em Branco** cria um aplicativo UWP básico que é compilado e executado, mas não contém controles de interface do usuário ou dados. Você adicionará controles ao aplicativo ao longo deste tutorial.

5.  Na caixa de texto **Nome**, digite "HelloWorld".
6.  Clique em **OK** para criar o projeto.

    O Visual Studio criará seu projeto e o exibirá no **Gerenciador de Soluções**

    ![Visual Studio Solution Explorer para o projeto HelloWorld](images/js-tut-helloworld.png)

Apesar de ser um modelo básico, o **Aplicativo em Branco** contém vários arquivos:

-   Um arquivo de manifesto (package.appxmanifest) que descreve o aplicativo (nome, descrição, bloco, página inicial, tela inicial etc.) e lista os arquivos que ele contém.
-   Um conjunto de imagens de logotipo (images/Square150x150Logo.scale-200.png, images/Square44x44Logo.scale-200.png, and images/Wide310x150Logo.scale-200.png) para exibir no menu Iniciar.
-   Uma imagem (images/StoreLogo.png) para representar o aplicativo na Windows Store.
-   Uma tela inicial (images/SplashScreen.scale-200.png) para ser mostrada quando o aplicativo é iniciado.
-   Uma página inicial (default.html) e um arquivo JavaScript correspondente (default.js) que são executados quando o aplicativo é iniciado.

Para exibir e editar os arquivos, clique duas vezes no arquivo no **Gerenciador de Soluções**

Esses arquivos são essenciais para todos os aplicativos UWP em JavaScript. Eles fazem parte de qualquer projeto criado no Visual Studio.

##Etapa 2: inicie o aplicativo


Neste ponto, você criou um aplicativo muito simples. Este é um bom momento para compilar, implantar e iniciar seu aplicativo e verificar sua aparência. Você pode depurar o aplicativo no computador local, em um simulador ou emulador, ou em um dispositivo remoto. Aqui está o menu do dispositivo de destino no Visual Studio.

![Lista suspensa de destinos de dispositivo para depuração do aplicativo](images/uap-debug.png)

### Inicie o aplicativo em um dispositivo da área de trabalho

Por padrão, o aplicativo é executado no computador local. O menu do dispositivo de destino fornece várias opções para depurar seu aplicativo em dispositivos da família de dispositivos da área de trabalho.

-   **Simulador**
-   **Computador local**
-   **Computador remoto**

**Para iniciar a depuração no computador local.**

1.  No menu do dispositivo de destino (![Start debugging menu](images/startdebug-full.png)) na barra de ferramentas **Padrão**, verifique se **Computador Local** está selecionado. (Esta é a seleção padrão.)
2.  Clique no botão **Iniciar depuração** (![botão Iniciar depuração](images/startdebug-sm.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Iniciar Depuração**

   –ou–

   Pressione F5.

O aplicativo é aberto em uma janela, e uma tela inicial padrão aparece primeiro. A tela inicial é definida por uma imagem (SplashScreen.png) e uma cor da tela de fundo (especificada no arquivo de manifesto do aplicativo).

A tela inicial desaparecerá, e o aplicativo será exibido em seguida. Ele conterá uma tela preta com o texto "O conteúdo vai aqui".

![O aplicativo HelloWorld em um computador](images/helloworld-1-js.png)

Pressione a tecla Windows para abrir o menu **Iniciar** e exibir todos os aplicativos. Observe que implantar o aplicativo localmente adiciona seu bloco ao menu **Iniciar**. Para executar o aplicativo novamente (não no modo de depuração), toque ou clique no bloco no menu **Iniciar**.

Ele ainda não faz muita coisa, mas parabéns! Você criou seu primeiro aplicativo UWP!

**Para parar a depuração**

-   Clique no botão **Parar Depuração** (![botão Parar depuração](images/stopdebug.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Parar depuração**

   –ou–

   Feche a janela do aplicativo.

### Iniciar o aplicativo em um emulador de dispositivo móvel

Seu aplicativo é executado em qualquer dispositivo do Windows 10, portanto vamos ver sua aparência em um Windows Phone.

Além das opções para depurar em um dispositivo da área de trabalho, o Visual Studio fornece opções para implantar e depurar seu aplicativo em um dispositivo móvel físico conectado ao computador, ou em um emulador de dispositivo móvel. Você pode escolher entre emuladores para dispositivos com diferentes configurações de memória e exibição.

-   **Dispositivo**
-   **Emulador <SDK version> WVGA de 4 polegadas e 512 MB**
-   **Emulador <SDK version> WVGA de 4 polegadas e 1 GB**
-   etc. (Diversos emuladores em outras configurações)

É recomendável testar o aplicativo em um dispositivo com tela pequena e memória limitada, portanto use a opção **Emulator 10.0.10240.0 WVGA 4 inch 512MB**.
**Para iniciar a depuração em um emulador de dispositivo móvel**

1.  No menu do dispositivo de destino (![Menu Iniciar depuração](images/startdebug-full.png)) na barra de ferramentas **Padrão**, escolha **Emulador 10.0.10240.0 WVGA de 4 polegadas e 512 MB**
2.  Clique no botão **Iniciar depuração** (![botão Iniciar depuração](images/startdebug-sm.png)) na barra de ferramentas.

   –ou–

   No menu **Depurar**, clique em **Iniciar Depuração**

   
O Visual Studio inicia o emulador selecionado e, em seguida, implanta e inicia o aplicativo. No emulador do dispositivo móvel, o aplicativo tem a seguinte aparência.

![Tela inicial do aplicativo no dispositivo móvel](images/helloworld-1-js-phone.png)

## Etapa 3: modifique a página inicial

Um dos arquivos que o Visual Studio criou é default.html, a página inicial de seu aplicativo. Quando o aplicativo for executado, exibirá o conteúdo da página inicial. A página inicial também contém referências aos arquivos de código e às folhas de estilo do aplicativo. Veja a seguir a página inicial que o Visual Studio criou para você:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
</head>
<body class="win-type-body">
    <p>Content goes here</p>
</body>
</html>
```

Adicionaremos novo conteúdo ao arquivo default.html. Assim como qualquer outro arquivo HTML, o conteúdo deve ser adicionado no elemento [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011). Você pode usar elementos HTML5 para criar o aplicativo (com [algumas exceções](https://msdn.microsoft.com/library/windows/apps/Hh465380)). Isso significa que você pode usar elementos HTML5 como [**h1**](https://msdn.microsoft.com/library/windows/apps/Hh441078), [**p**](https://msdn.microsoft.com/library/windows/apps/Hh453431), [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017), [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) e [**img**](https://msdn.microsoft.com/library/windows/apps/Hh466114)

**Para modificar sua página inicial**

1.  Substitua o conteúdo existente no elemento [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011) por um cabeçalho de primeiro nível com a mensagem "Hello, world!", um texto perguntando o nome do usuário, um elemento [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) para aceitar o nome do usuário, um [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) e um elemento [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133). Atribua IDs ao **input**, ao **button** e ao **div**

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  Execute o aplicativo no computador local. Ele terá a aparência a seguir.

![O aplicativo HelloWorld com novo conteúdo](images/helloworld-2-js.png)

   Você pode digitar no elemento [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271), mas ao clicar no [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) agora, nada vai acontecer. Alguns objetos, como **button**, podem enviar mensagens quando ocorrem determinados eventos. Essas mensagens de evento permitem executar uma ação em resposta ao evento. Você coloca código para responder ao evento em um método do manipulador de eventos.

   Nas próximas etapas, criaremos um manipulador de eventos para o [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) que exibe uma saudação personalizada. Adicionaremos o código do manipulador de eventos ao arquivo default.js.

##Etapa 4: crie um manipulador de eventos

Quando criamos o novo projeto, o Visual Studio automaticamente criou um arquivo /js/default.js. Esse arquivo contém código para manipular o ciclo de vida do aplicativo. Também é nele que você adicionará código para criar interatividade no arquivo default.html.

Abra o arquivo default.js.

Antes de adicionarmos nosso próprio código, vejamos as primeiras e últimas linhas do código no arquivo:

```javascript
(function () {
    "use strict";

     // Omitted code 

 })(); 
```

Você deve estar se perguntando o que é isso. Essas linhas de código encapsulam o restante do código de default.js em uma função anônima autoexecutável. Uma função anônima de autoexecução torna mais fácil evitar conflitos de nomenclatura ou a modificação acidental de valores que não deveriam ser modificados. Isso também mantém identificadores desnecessários fora do namespace global, o que contribui com o desempenho. Parece um pouco estranho, mas trata-se de uma boa prática de programação.

A próxima linha de código ativa o [modo estrito](https://msdn.microsoft.com/en-us/library/windows/apps/br230269.aspx) para o código JavaScript. O modo estrito fornece verificações de erro adicionais para o código. Por exemplo, ele impede o uso de variáveis declaradas de modo implícito ou a atribuição de valor a propriedades somente leitura.

Observe o restante do código em default.js. Ele manipula os eventos [**activated**](https://msdn.microsoft.com/library/windows/apps/BR212679) e [**checkpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) do aplicativo. Esses eventos serão descritos com mais detalhes mais tarde. Por enquanto, apenas lembre-se de que o evento **activated** é acionado quando o aplicativo é iniciado.

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    app.start();
})();
```

Agora definiremos um manipulador de eventos para o [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017). Nosso novo manipulador de eventos obtém o nome do usuário por meio do controle `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) e o usa para gerar uma saudação no elemento `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) criado na seção anterior.

### Usando eventos que funcionam com a entrada de toque, mouse e caneta

Em um aplicativo UWP, você não precisa se preocupar com as diferenças entre toque, mouse e outras formas de entrada de ponteiro. Você pode simplesmente usar os eventos que já conhece, como [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312), e eles funcionarão com todas as formas de entrada.

**Dica**   O aplicativo também pode usar os novos eventos *MSPointer\** e *MSGesture\**, que funcionam com a entrada por toque, mouse e caneta e podem fornecer informações adicionais sobre o dispositivo que disparou o evento. Para saber mais, veja [Respondendo à interação do usuário](https://msdn.microsoft.com/library/windows/apps/Hh700412) e [Gestos, manipulações e interações](https://msdn.microsoft.com/library/windows/apps/Hh761498)

Agora, prosseguiremos para criar o manipulador de eventos.

**Para criar o manipulador de eventos**

1.  No default.js, depois do manipulador de eventos [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) e antes da chamada de [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705), crie uma função de manipulador de eventos [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) chamada `buttonClickHandler` que receberá um único parâmetro chamado
```javascript
    function buttonClickHandler(eventInfo) {
     
        }
```

2.  Dentro do manipulador de eventos, recupere o nome do usuário a partir do controle `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) e use-o para criar uma saudação. Use o `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) para exibir o resultado.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString; 
        }
 ```

Você adicionou o manipulador de eventos a default.js. Agora, você precisa registrá-lo.

## Etapa 5: registre o manipulador de eventos quando seu aplicativo for iniciado


Tudo o que precisamos fazer agora é registrar o manipulador de eventos no botão. A maneira recomendada de registrar um manipulador de eventos é chamar [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) do código. Um bom momento para registrar o manipulador de eventos é quando o aplicativo for ativado. Felizmente, o Visual Studio gerou um código no arquivo default.js que manipula a ativação do aplicativo: o manipulador de eventos [**app.onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679). Observemos esse código.

```javascript
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };
```

Dentro do manipulador [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679), o código verifica qual tipo de ativação ocorreu. Existem vários tipos de ativação diferentes. Por exemplo, seu aplicativo foi ativado quando o usuário iniciou o aplicativo e quando o usuário deseja abrir um arquivo que está associado ao seu aplicativo. (Para saber mais, veja [Ciclo de vida do aplicativo](https://msdn.microsoft.com/library/windows/apps/Mt243287)

Estamos interessados na ativação de [**launch**](https://msdn.microsoft.com/library/windows/apps/BR224693). Um aplicativo é *iniciado* sempre que não está em execução e um usuário o ativa.

```javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
```

Se a ativação for de inicialização, o código verificará como o aplicativo foi desligado na última vez que foi executado.

```javascript
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
```

Em seguida, chama [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)

```javascript
            args.setPromise(WinJS.UI.processAll());
        }
    };
```    

[
            **WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) é chamado independentemente de o aplicativo já ter sido desligado anteriormente ou de ser a primeira vez que está sendo iniciado. O **WinJS.UI.processAll** está contido em uma mensagem para o método [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609), que verifica se a tela não é retirada até que a página do aplicativo esteja pronta.

**Dica**   A função [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) verifica o arquivo default.html para localizar controles WinJS e os inicializa. Até agora, ainda não adicionamos esses controles, mas é recomendável manter esse código para o caso de você desejar adicioná-los posteriormente.

Um bom local para registrar os manipuladores de eventos de controles que não sejam WinJS é logo após a chamada de [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)

**Para registrar o manipulador de eventos**

-   No manipulador de eventos [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) em default.js, recupere `helloButton` e use [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) para registrar o manipulador de eventos para o evento [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312). Adicione este código após a chamada de [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)

```javascript
   app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll());

             // Retrieve the button and register our event handler. 
                var helloButton = document.getElementById("helloButton");
                helloButton.addEventListener("click", buttonClickHandler, false);
            }
        };
```    

Aqui está o código completo para o arquivo atualizado default.js:

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());

            // Retrieve the button and register our event handler. 
            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    function buttonClickHandler(eventInfo) {
        var userName = document.getElementById("nameInput").value;
        var greetingString = "Hello, " + userName + "!";
        document.getElementById("greetingOutput").innerText = greetingString;
    }

    app.start();
})();
```

Execute o aplicativo. Quando você inserir seu nome na caixa de texto e clicar no botão, o aplicativo exibirá uma saudação personalizada. Veja a aparência na máquina local e no emulador.

![Saudação personalizada do aplicativo HelloWorld](images/helloworld-3-js.png)

![Saudação personalizada do aplicativo HelloWorld](images/helloworld-3-js-phone.png)

**Observação**   Se você está se perguntando por que usamos [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) para registrar o evento no código em vez de definir o evento [**onclick**](https://msdn.microsoft.com/library/windows/apps/Hh441312) no HTML, veja [Codificando aplicativos básicos](https://msdn.microsoft.com/library/windows/apps/Hh780660) para obter uma explicação detalhada.

## Etapa 6: adicionar uma Biblioteca do Windows para controle JavaScript


Além dos controles HTML padrão, seu aplicativo pode usar qualquer um dos controles da Biblioteca do Windows para JavaScript, como [**WinJS.UI.DatePicker**](https://msdn.microsoft.com/library/windows/apps/BR211681), [**WinJS.UI.FlipView**](https://msdn.microsoft.com/library/windows/apps/BR211711), [**WinjS.UI.ListView**](https://msdn.microsoft.com/library/windows/apps/BR211837) e [**WinJS.UI.Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

Diferentemente dos controles HTML, os controles WinJS não possuem elementos de marcação dedicados: você não pode criar um controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) adicionando um elemento `<rating />`, por exemplo. Para adicionar um controle WinJS, você cria um elemento [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) e usa o atributo [**data-win-control**](https://msdn.microsoft.com/library/windows/apps/Hh440969) para especificar o tipo de controle desejado. Para adicionar um controle **Rating**, você define o atributo como "WinJS.UI.Rating".

Adicione um controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) ao seu aplicativo.

1.  No arquivo default.html, adicione um [**label**](https://msdn.microsoft.com/library/windows/apps/Hh453321) e um controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) após o `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133)

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body> 
    ```

2.  Execute o aplicativo no computador local. Observe o novo controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

   ![O aplicativo Hello, world com um controle de Biblioteca do Windows para JavaScript](images/helloworld-4-js.png)

> Para que o [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) seja carregada, sua página deve chamar o [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975). Como nosso aplicativo está usando um dos modelos do Visual Studio, seu default.js já inclui uma chamada ao **WinJS.UI.processAll**, conforme descrito anteriormente, para que você não tenha que adicionar nenhum código.

Agora, clicar no controle de [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) altera a classificação, mas não faz mais nada. Vamos usar um manipulador de eventos para fazer algo quando o usuário altera a classificação.

## Etapa 7: registrar um manipulador de eventos para um controle de Biblioteca do Windows para JavaScript


Registrar um manipulador de eventos para um controle WinJS é um pouco diferente de registrar um manipulador de eventos para um controle HTML padrão. Anteriormente, mencionamos que o manipulador de eventos [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) chama o método [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) para inicializar o WinJS em sua marcação. O **WinJS.UI.processAll** é encapsulado em uma chamada do método [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609).

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

Se [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) fosse um controle HTML padrão, você adicionaria seu manipulador de eventos após esta chamada ao [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975). Mas isso é um pouco mais complicado para um controle WinJS como nossa **Rating**. Como o **WinJS.UI.processAll** cria o controle de **Rating** para nós, não podemos adicionar o manipulador de eventos à **Rating** até que o **WinJS.UI.processAll** tenha concluído seu processamento.

Se [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) fosse um método típico, poderíamos registrar o manipulador de eventos [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) logo depois de chamá-lo. Mas o método **WinJS.UI.processAll** é assíncrono, então qualquer código que segui-lo pode ser executado antes de o **WinJS.UI.processAll** ser concluído. Então, o que devemos fazer? Usamos um objeto [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) para receber notificações quando o **WinJS.UI.processAll** é concluído.

Como em todos os métodos WinJS assíncronos, [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) retorna um objeto [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867). Uma **Promise** é uma "promessa" que algo ocorrerá no futuro; quando isto ocorre, diz-se que a **Promise** foi concretizada.

[
              Os objetos **Promise**
            ](https://msdn.microsoft.com/library/windows/apps/BR211867) têm um método [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) que consideram uma função "concluída" como um parâmetro. A **Promise** chama essa função quando é concluída.

Adicionando seu código a uma função "concluída" e a passando para o objeto [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) no método [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728), você pode ter certeza que seu código é executado após a conclusão do [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975).

1.  Vamos gerar o valor de classificação quando o usuário seleciona uma classificação. Em seu arquivo default.html, crie um elemento [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) para exibir o valor de classificação e dar a ele a **id** "ratingOutput".
```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  Em nosso arquivo default.js, crie um manipulador de eventos para o controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) no evento [**change**](https://msdn.microsoft.com/library/windows/apps/BR211891) denominado `ratingChanged`. O parâmetro [**eventInfo**](https://msdn.microsoft.com/library/windows/apps/Hh465776) contém uma propriedade **detail.tentativeRating** que fornece a nova classificação de usuário. Obtenha esse valor e o exiba no [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) de saída

```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating; 
        }
    ```

3.  Update the code in the [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler that calls [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) by adding a call to the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) method and passing it a `completed` function. In the `completed` function, retrieve the `ratingControlDiv` element that hosts the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) control. Then use the [**winControl**](https://msdn.microsoft.com/library/windows/apps/Hh770814) property to retrieve the actual **Rating** control. (This example defines the `completed` function inline.)

```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
    ```

4.  While it's fine to register event handlers for HTML controls after the call to [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), it's also OK to register them inside your `completed` function. For simplicity, let's go ahead and move all your event handler registrations inside the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) event handler.

Here's the updated [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler:

```javascript
       app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                    // Retrieve the button and register our event handler. 
                    var helloButton = document.getElementById("helloButton");
                    helloButton.addEventListener("click", buttonClickHandler, false);

                }));

            }
        };
```        

5.  Execute o aplicativo. Quando você seleciona um valor de classificação, ele gera um valor numérico abaixo do controle [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

![O aplicativo Hello world concluído em um computador](images/helloworld-5-js.png)

![O aplicativo Hello world concluído em um telefone](images/helloworld-5-js-phone.png)

## Resumo

Parabéns, você criou seu primeiro aplicativo para o Windows 10 e a UWP usando JavaScript e HTML!



<!--HONumber=May16_HO2-->


