---
title: Criar um jogo UWP em JavaScript
description: Um jogo simples UWP para a Microsoft Store, escrito em JavaScript e CreateJS
ms.date: 02/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: 4ef4255c8ea1eb9db34f652e5974a4f341e5500d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80759445"
---
# <a name="create-a-uwp-game-in-javascript"></a>Criar um jogo UWP em JavaScript

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Um jogo simples 2D UWP para a Microsoft Store, escrito em JavaScript e CreateJS


![Folha de sprite do Walking Dino](images/JS2D_1.png)


## <a name="introduction"></a>Introdução


Publicar um aplicativo da Microsoft Store significa que você pode compartilhá-lo com milhões de pessoas (ou vendê-lo!), em muitos dispositivos diferentes.  

Para publicar seu aplicativo na Microsoft Store, ele deve ser escrito como um aplicativo UWP (Plataforma Universal do Windows). Entretanto, a UWP é extremamente flexível e oferece suporte a uma ampla variedade de linguagens e estruturas. Para provar isso, este exemplo é um jogo simples escrito em JavaScript, que usa várias bibliotecas de CreateJS e demonstra como desenhar sprites, criar um loop de jogo, oferecer suporte ao teclado e ao mouse e se adaptar a diferentes tamanhos de tela.

Este projeto foi desenvolvido com JavaScript usando o Visual Studio. Com algumas pequenas alterações, também pode ser hospedado em um site ou adaptado para outras plataformas. 

**Observação**: este não é um jogo completo (ou bom!); ele foi projetado para demonstrar o uso de JavaScript e uma biblioteca de terceiros para deixar um aplicativo pronto para ser publicado na Microsoft Store. 


## <a name="requirements"></a>Requisitos

Para jogar com este projeto, você precisará do seguinte:

* Um computador com Windows (ou uma máquina virtual) executando a versão atual do Windows 10.
* Uma cópia do Visual Studio. O Visual Studio Community Edition gratuito pode ser baixado na [home page do Visual Studio](https://visualstudio.com).

Este projeto usa a estrutura do CreateJS JavaScript. O CreateJS é um conjunto gratuito de ferramentas, lançado sob uma licença do MIT e projetado para facilitar a criação de jogos baseados em sprites. As bibliotecas do CreateJS já estão presentes no projeto (procure por *js/easeljs-0.8.2.min.js* e *js/preloadjs-0.6.2.min.js* no modo de exibição do Gerenciador de Soluções). Mais informações sobre o CreateJS podem ser encontradas na [página inicial do CreateJS](https://www.createjs.com).

**Observação**: O Visual Studio 2019 não oferece suporte ao desenvolvimento de aplicativos UWP JavaScript. Para desenvolver um aplicativo UWP JavaScript deve-se usar o Visual Studio 2017.

## <a name="getting-started"></a>Introdução

O código-fonte completo para o aplicativo está armazenado no [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d).

O modo mais simples de começar é visitar o GitHub, clicar no botão verde **Clone ou baixar** e selecionar **Abrir no Visual Studio**. 

![Clonar o repositório](images/JS2D_2.png)

Você também pode baixar o projeto como um arquivo zip ou usar alguma outra maneira padrão de trabalhar com [projetos do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

Depois que a solução tiver sido carregada no Visual Studio, você verá vários arquivos, inclusive:

* Imagens/ – uma pasta contendo os vários ícones exigidos pelos aplicativos UWP, bem como o SpriteSheet do jogo e alguns outros bitmaps.
* js/ – uma pasta que contém os arquivos JavaScript. O arquivo main.js é o nosso jogo, os outros arquivos são EaselJS e PreloadJS.
* index.html – a página da Web que contém o objeto de tela que hospeda os elementos gráficos do jogo.

Agora você pode executar o jogo!

Pressione **F5** para iniciar o aplicativo em execução. Você deve ver uma janela aberta e nosso dinossauro familiar em uma paisagem idílica (apesar de esparsa). Vamos agora examinar o aplicativo, explicar algumas partes importantes e desbloquear o restante dos recursos à medida que avançamos.

![Apenas um dinossauro comum com um gato ninja nas costas](images/JS2D_3.png)

**Observação**: Algo deu errado? Verifique se você instalou o Visual Studio com suporte para Web. Você pode verificar criando um novo projeto - se não houver suporte para JavaScript, você precisará instalar o Visual Studio novamente e verificar a caixa de *Microsoft Web Developer Tools*.

## <a name="walkthough"></a>Explicação passo a passo

Se você começou o jogo com F5, provavelmente está se perguntando o que está acontecendo. E a resposta é "não muito", pois muito do código é atualmente comentado. Até agora, tudo o que você verá é o dinossauro e um pedido pouco eficiente para pressionar Espaço. 

### <a name="1-setting-the-stage"></a>1. Configurar o cenário

Se você abrir e examinar o arquivo **index.html**, verá que está praticamente vazio. Esse arquivo é a página da Web padrão que contém nosso aplicativo, e executa apenas duas ações importantes. Primeiro, ele inclui o código-fonte JavaScript para as bibliotecas do **EaselJS** e **PreloadJS** CreateJS e também **main.js** (nosso próprio arquivo de código-fonte).
Segundo, ele define uma marcação de &lt;tela&gt;, que é onde todos os nossos elementos gráficos vão aparecer. Uma &lt;tela&gt; é um componente padrão do documento HTML5. Podemos nomeá-la (gameCanvas) para que nosso código em **main.js** possa fazer referência a ele. A propósito, se pretende criar seu próprio jogo JavaScript do zero, você também precisará copiar os arquivos **EaselJS** e **PreloadJS** em sua solução e, em seguida, criar um objeto de tela.

O EaselJS nos fornece um novo objeto chamado *palco*. O palco está vinculado à tela e é usado para exibir imagens e texto. Qualquer objeto que queremos exibir no palco precisa primeiro ser adicionado como um filho do palco, desta forma:

```
    stage.addChild(myObject);
```

Você verá essa linha de código aparecer várias vezes em **main.js**

Falando nisso, agora é um bom momento para abrir o **main.js**.

### <a name="2-loading-the-bitmaps"></a>2. Carregar os bitmaps

O EaselJS fornece vários tipos diferentes de objetos gráficos. Podemos criar formas simples (como o retângulo azul usado para o céu), ou bitmaps (como as nuvens que estamos prestes a adicionar), objetos de texto e sprites. Sprites usa um (SpriteSheet)[https://createjs.com/docs/easeljs/classes/SpriteSheet.html ]: um único bitmap contendo várias imagens. Por exemplo, usamos este SpriteSheet para armazenar o quadro diferente de animação de dinossauro:

![Folha de sprite do Walking Dino](images/JS2D_4.png)

Fazemos o dinossauro andar, definindo os quadros diferentes e a velocidade em que devem ser animados neste código:

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

Agora, vamos adicionar algumas nuvens fofinhas ao cenário. Assim que o jogo estiver rodando, elas vão se mover pela tela. A imagem para a nuvem já está na solução, na pasta *images*.

Examine o **main.js** até encontrar a função **init**. Ela é chamada quando o jogo começa, e é onde começamos a configurar todos os nossos objetos gráficos.

Localize o código a seguir e remova os comentários (\\) da linha que faz referência à imagem da nuvem.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

O JavaScript precisa de um pouco de ajuda quando se trata de carregar recursos, como imagens, e por isso estamos usando um recurso da biblioteca CreateJS que pode pré-carregar imagens, chamado de [LoadQueue](https://www.createjs.com/docs/preloadjs/classes/LoadQueue.html). Não podemos saber ao certo quanto tempo levará para que as imagens sejam carregadas, portanto, usamos o LoadQueue para cuidar disso. Depois que as imagens estiverem disponíveis, a fila indicará que estão prontas. Para fazer isso, primeiro criamos um novo objeto que lista todas as nossas imagens e, em seguida, criamos um objeto LoadQueue. Você verá no código abaixo como ele está configurado para chamar uma função chamada **loadingComplete()** quando tudo estiver pronto.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

Quando a função **loadingComplete()** é chamada, as imagens estão carregadas e prontas para uso. Você verá uma seção comentada que cria as nuvens, agora seu bitmap está disponível. Remova os comentários, para que tenha esta aparência:

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
Esse código cria três objetos de nuvem, cada um usando nossa imagem pré-carregada, define sua localização e os adiciona ao cenário.

Execute o aplicativo novamente (pressione F5) e você verá nossas nuvens aparecerem.

### <a name="3-moving-the-clouds"></a>3. Mover as nuvens

Agora, vamos fazer nuvens se moverem. O segredo para mover nuvens – e mover qualquer coisa, na verdade – é configurar uma função [ticker](https://www.createjs.com/docs/easeljs/classes/Ticker.html) que é chamada repetidamente várias vezes por segundo. Toda vez que essa função é chamada, ela redesenha os elementos gráficos em um local ligeiramente diferente.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">Confira a Caneta <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS – Animar nuvens</a> dos documentos do Microsoft Edge (<a href="https://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) na <a href="https://codepen.io">CodePen</a>.</p>
  O código para fazer isso já está no arquivo **main.js**, fornecido pela biblioteca do CreateJS, EaselJS. Ele é semelhante ao seguinte:

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

Esse código chamará uma função chamada **gameLoop()** entre 30 e 60 quadros por segundo. A velocidade exata depende da velocidade do computador.

Procure a função **gameLoop()** e mais para baixo, em direção ao final, você verá uma função chamada **animateClouds()** . Edite-a para que não seja comentada.

```
    // Move clouds
    animateClouds();
```

Se verificar a definição dessa função, você verá como ela pega uma nuvem por vez e altera sua coordenada x. Se a x-ordenada está fora da lateral da tela, ela é redefinida para a extrema direita. Cada nuvem também se move a uma velocidade um pouco diferente.

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

Se você executar o aplicativo agora, verá que as nuvens começaram a se mover. Por fim, temos movimento!

### <a name="4-adding-keyboard-and-mouse-input"></a>4. Adicionar entrada de mouse e teclado

Um jogo em que você não pode interagir não é um jogo. Portanto, vamos permitir que o jogador use o teclado ou o mouse para fazer algo. Novamente na função **loadingComplete()** , você verá o seguinte. Remova os comentários.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

Agora temos duas funções sendo chamadas sempre que o jogador apertar uma tecla ou clicar no mouse. Ambos os eventos chamarão **userDidSomething()** , uma função que analisa a variável gamestate para decidir o que o jogo está fazendo no momento, e o que precisa ser feito em seguida como resultado.

Gamestate é um padrão de design comum usado em jogos. Tudo o que acontece, acontece na função **gameLoop()** , chamada pelo temporizador do ticker. O gameLoop() monitora se o jogo está reproduzindo ou em um "estado de fim de jogo" ou em um "estado pronto para reproduzir" ou qualquer outro estado definido pelo autor, usando uma variável. Esta variável de estado é testada em uma instrução de comutador, e isso define quais outras funções são chamadas. Assim, se o estado for definido como "reproduzindo", as funções para fazer o dinossauro pular e fazer os barris se moverem serão chamadas. Se o dinossauro for morto por algo, a variável gamestate será definida como "estado fim de jogo" e a mensagem "Fim de jogo!" será exibida. Se você estiver interessado em padrões de design de jogo, o livro [Game Programming Patterns](https://gameprogrammingpatterns.com/) é muito útil.

Tente executar o aplicativo novamente e, finalmente você poderá começar a jogar. Pressione espaço (ou clique com o mouse ou toque na tela) para começar. 

Você verá um barril vir rolando na sua direção: pressione espaço ou clique novamente no momento certo, e o dinossauro vai pular. Se o momento for errado, seu jogo será encerrado.

O barril é animado da mesma maneira que as nuvens (embora ele fique mais rápido a cada vez), e nós verificamos a posição do dinossauro e do barril para garantir que eles não tenham colidido:

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

Se o dinossauro não estiver saltando e o barril estiver perto, o código muda a variável de estado para o estado que chamamos de *GameOver*. Como você pode imaginar, *GameOver* interrompe o jogo.

E, portanto, a mecânica principal de nosso jogo é concluída.

### <a name="5-resizing-support"></a>5. Redimensionar o suporte

Estamos quase no fim agora! Mas antes de concluirmos, há um problema irritante para resolver primeiro. Quando o jogo estiver em execução, tente redimensionar a janela. Você verá que o jogo rapidamente se torna muito confuso, pois os objetos não estão mais onde deveriam estar. Podemos cuidar disso criando um manipulador para o evento de redimensionamento de janela gerado quando o jogador redimensiona a janela, ou quando o dispositivo for girado de paisagem para retrato.

O código para fazer isso já está presente (na verdade, nós o chamamos quando o jogo é iniciado, para garantir que o tamanho de janela padrão funciona porque, quando um aplicativo UWP é iniciado, você não tem certeza de qual será o tamanho da janela).

Simplesmente remova o comentário desta linha para chamar a função quando o evento de tamanho da tela for acionado:

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

Se executar o aplicativo novamente, você agora deverá poder redimensionar a janela e obter resultados melhores.

## <a name="publishing-to-the-microsoft-store"></a>Publicar na Microsoft Store

Agora que você tem um aplicativo UWP, é possível publicá-lo na Microsoft Store (supondo que você o aperfeiçoou primeiro!) 

Há algumas etapas no processo.

1. É preciso estar [registrado](https://developer.microsoft.com/store/register) como Desenvolvedor do Windows.
2. É preciso usar a [lista de verificação](https://docs.microsoft.com/windows/uwp/publish/app-submissions) de envio de aplicativo.
3. O aplicativo deve ser enviado para [certificação](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process).

Para obter mais detalhes, confira [Publicar seu aplicativo UWP](https://docs.microsoft.com/windows/uwp/publish/).

## <a name="suggestions-for-other-features"></a>Sugestões para outros recursos.

O que fazer em seguida? Aqui estão algumas sugestões de recursos para adicionar ao seu aplicativo premiado (em breve).

1. Efeitos sonoros. A biblioteca CreateJS inclui suporte para som com uma biblioteca chamada [SoundJS](https://www.createjs.com/soundjs).
2. Suporte de Gamepad. Há uma [API disponível](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345).
3. Torne-o um jogo muito, muito melhor! Essa parte é você quem decide, mas há vários recursos disponíveis online. 

## <a name="other-links"></a>Outros links

* [Criar um jogo simples do Windows com JavaScript](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [Selecionar um mecanismo de jogo de HTML/JS](https://html5gameengine.com/)
* [Usar CreateJS no seu jogo baseado em JS](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [Cursos de desenvolvimento de jogos no LinkedIn Learning](https://www.linkedin.com/learning/topics/game-development)
