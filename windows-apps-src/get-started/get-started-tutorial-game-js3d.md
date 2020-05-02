---
title: Tutorial de Introdução – um jogo 3D UWP em JavaScript
description: Um jogo UWP para a Microsoft Store, escrito em JavaScript com three.js
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: b4ce91e32b14bdf81b40b24e810e0bd86bcaa99b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67321086"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>Criar um jogo 3D em JavaScript usando three.js

## <a name="introduction"></a>Introdução

Para os desenvolvedores da Web ou para quem é hábil com JavaScript, desenvolver aplicativos UWP com JavaScript é uma maneira fácil para levar os seus aplicativos para o mundo. Não é preciso se preocupar em aprender uma linguagem como C# ou C++!

Para este exemplo, vamos aproveitar a biblioteca do **three.js**. Essa biblioteca segue o WebGL, uma API que é usada para renderizar gráficos 2D e 3D para navegadores da Web. O **three.js** pega essa API complicada e a simplifica, tornando muito mais fácil o desenvolvimento em 3D. 


Quer ter uma prévia do aplicativo que faremos antes de continuar a ler? Veja na CodePen!

<iframe height='300' scrolling='no' title='Jogo final do dinossauro' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja na Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>Jogo final do dinossauro</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE] 
> Observação: este não é um jogo completo; ele foi projetado para demonstrar o uso de JavaScript e uma biblioteca de terceiros para deixar um aplicativo pronto para ser publicado na Microsoft Store.


## <a name="requirements"></a>Requisitos

Para jogar com este projeto, você precisará do seguinte:
-   Um computador com Windows (ou uma máquina virtual) executando a versão atual do Windows 10.
-   Uma cópia do Visual Studio. O Visual Studio Community Edition gratuito pode ser baixado na [home page do Visual Studio](https://visualstudio.com/).
Este projeto usa a biblioteca do **three.js** do JavaScript. **three.js** é lançada sob licença do MIT. Essa biblioteca já está presente no projeto (procure `js/libs` no modo de exibição do Gerenciador de Soluções). Mais informações sobre essa biblioteca podem ser encontradas na página inicial do [**three.js**](https://threejs.org/).

## <a name="getting-started"></a>Introdução

O código-fonte completo para o aplicativo está armazenado no [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d).

O modo mais simples de começar é visitar o GitHub, clicar no botão verde Clone ou no botão baixar e selecionar Abrir no Visual Studio. 

![botão clone ou botão baixar](images/3dclone.png)

Se não quiser clonar o projeto, você poderá baixá-lo como um arquivo zip.
Depois que a solução tiver sido carregada no Visual Studio, você verá vários arquivos, inclusive:
-   Imagens/ - uma pasta que contém os diversos ícones exigidos pelos aplicativos UWP.
- css/ - uma pasta que contém o CSS a ser usado.
-   js/ - uma pasta que contém os arquivos JavaScript. O arquivo main.js é o nosso jogo enquanto os outros arquivos são as bibliotecas de terceiros.
-   modelos/ - uma pasta que contém os modelos 3D. Para esse jogo, só temos um modelo para o dinossauro.
-   index.html - a página da Web que hospeda o renderizador do jogo.

Agora você pode executar o jogo!

Pressione F5 para iniciar o aplicativo. Uma janela deve se abrir, solicitando que você clique na tela. Você também verá um dinossauro se movendo em segundo plano. Vá em frente e feche o jogo, e vamos começar examinando o aplicativo e seus principais componentes.

> [!NOTE] 
> Algo deu errado? Verifique se você instalou o Visual Studio com suporte para Web. Você pode verificar criando um novo projeto – se não houver suporte para JavaScript, você precisará instalar o Visual Studio novamente e verificar a caixa de Microsoft Web Developer Tools.

## <a name="walkthrough"></a>visão geral

Ao iniciar esse jogo, você verá um aviso para clicar na tela. A [API de bloqueio de ponteiro](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) é usada para permitir que você possa examinar ao redor com o mouse. A movimentação é obtida pressionando-se as teclas W, A, S, D ou as teclas de setas.
O objetivo deste jogo é ficar longe do dinossauro. Quando o dinossauro estiver próximo o suficiente de você, ele vai começar a caçá-lo até que saia do raio de alcance ou fique perto demais e perca o jogo.

### <a name="1-setting-up-your-initial-html-file"></a>1. Configurar seu arquivo HTML inicial

Você precisará adicionar um pouco de HTML dentro de **index.html** para começar. Esse arquivo é a página da Web padrão que contém o nosso aplicativo.

Agora, vamos configurá-lo com as bibliotecas que usaremos e o `div` (denominado `container`) que usaremos para renderizar os gráficos. Nós também vamos defini-lo para apontar para nosso **main.js** (nosso código de jogo).


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


Agora que temos nosso iniciador HTML pronto para ser usado, vamos para **main.js** criar alguns elementos gráficos!

### <a name="2-creating-your-scene"></a>2. Criar sua cena

Na seção do passo a passo, vamos adicionar a base do jogo.

Começaremos elaborando um `scene`. Um `scene` em **three.js** é onde sua câmera, objetos e luzes serão adicionados. Você também precisará de um renderizador que vai capturar o que a sua câmera vê na cena e exibir.

Em **main.js**, faremos uma função que faz tudo isso chamada `init()` que chama algumas funções adicionais:

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

Outras funções que precisamos criar incluem:
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- Funções de conversão de unidade

#### <a name="createmazecubes"></a>createMazeCubes()

A função `createMazeCubes()` adicionará um cubo simples à nossa cena. Posteriormente, usaremos a função para adicionar muitos cubos e criar o nosso labirinto.

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

A função `addLights()` é uma função simples que agrupa a criação de nossas luzes e as adiciona à cena.

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

A função `onWindowResize` é chamada sempre que nosso ouvinte de eventos ouvir que um evento `resize` foi disparado. Isso ocorre sempre que o usuário ajusta o tamanho da janela. Se isso acontecer, queremos garantir que a imagem permaneça proporcional e possa ser vista em toda a janela.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

A última coisa de que precisaremos é nossa função `animate()`, que também chamará a função `render()`. A função [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) é usada para atualizar constantemente nosso renderizador. Posteriormente, usaremos essas funções para atualizar nosso renderizador com animações interessantes como a movimentação do labirinto.

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>Funções de conversão de unidade

Em **three.js**, as rotações são medidas em radianos. Para facilitar as coisas para nós, adicionaremos algumas funções para podermos converter facilmente entre graus e radianos. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

Quem se lembra de que 30 graus correspondem a 0,523 radianos? É muito mais simples empregar `degreesToRadians(30)` para obter o valor de rotação que é usado na função `createMazeCubes()`.

___

Foi bastante código para absorver, mas agora temos um belo cubo que é renderizado para o `container`! Veja os resultados na CodePen.

Você pode copiar e colar todo o JavaScript neste CodePen como apoio no caso de encontrar problemas ou editá-lo para ajustar algumas luzes e alterar algumas cores. 

<iframe height='300' scrolling='no' title='Luz, câmera, cubo!' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Confira a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>Luz, câmera, cubo!</a> por Documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="3-making-the-maze"></a>3. Criar o labirinto

Embora olhar para um cubo seja de tirar o fôlego, um labirinto inteiro feito de cubos é ainda melhor! É um segredo bem conhecido da comunidade de jogos que uma das maneiras mais rápidas de criar um nível é inserindo cubos em toda parte com uma matriz de 2D.
 
![labirinto feito com uma matriz de 2D](images/dinomap.png)

Colocar 1 onde estão os cubos e 0 onde há espaço vazio permite uma maneira simples e manual de criar e ajustar o labirinto.

Conseguimos isso substituindo nossa antiga função `createMazeCubes()` por uma que usa um loop aninhado para criar e posicionar vários cubos. Também criaremos um nome de matriz `collidableObjects` e adicionaremos os cubos a ele para detecção de colisão posteriormente neste tutorial:

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

Agora que sabemos quantos cubos estão sendo usados ​​(e qual o tamanho deles), podemos usar a variável `mapSize` ​​calculada para definir as dimensões do plano horizontal:

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

A última parte do labirinto que vamos adicionar são as paredes do perímetro para encaixar tudo. Usaremos um loop para criar dois planos (nossas paredes) ao mesmo tempo, usando a variável `mapSize` que calculamos em `createGround()` para determinar qual largura elas devem ter. As novas paredes também serão adicionadas à nossa matriz `collidableObjects` para detecção futura de colisão:

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


Não se esqueça de adicionar uma chamada para `createGround()` e `createPerimWalls` depois de `createMazeCubes()` na sua função `init()` para que sejam compilados!
___

Agora vamos ter um belo labirinto para olhar, mas realmente não é possível perceber como ele é interessante, pois nossa câmera está parada em um só lugar. É hora de subir este jogo de nível e adicionar alguns controles de câmera.

Fique à vontade para testar tudo no CodePen, como alterar as cores dos cubos ou remover o chão, comentando `createGround()` na função `init()`.


<iframe height='300' scrolling='no' title='Compilar labirintos' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>Compilar labirintos</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. Permitir que o jogador olhe ao redor

Agora é hora de entrar nesse labirinto e começar a olhar ao redor. Para fazer isso, usaremos a biblioteca **PointerLockControls.js** e nossa câmera.

A biblioteca **PoinerLockControls.js** usa o mouse para girar a câmera na direção em que o mouse é movido, permitindo que o jogador olhe ao redor. 

Primeiro vamos adicionar alguns novos elementos ao nosso arquivo **index**:

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

Você também precisará de todo o CSS na CodePen na parte inferior desta seção. Ele deve ser colado no seu arquivo **stylesheet.css**.

Voltando para o **main.js**, adicione algumas novas variáveis globais; `controls` para armazenar o controlador, `controlsEnabled` para rastrear o estado do controlador, e `blocker` para capturar o elemento `blocker` no **index.html**:

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


Agora, em nossa função `init()` podemos criar um novo objeto `PoinerLockControls`, passá-lo à nossa `camera` e adicionar a `camera` (acessada com `controls.getObject()`).

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

A câmera está agora conectada, mas precisamos de alguma forma deixar o mouse e o controlador interagirem para que possamos olhar em volta. 

Nessa situação, a [API de bloqueio de ponteiro](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) entra em ação, permitindo que conectemos os movimentos do mouse com a nossa câmera. A API de bloqueio de ponteiro também faz o mouse desaparecer para uma experiência mais imersiva. Ao pressionar Esc, nós interrompemos a conexão entre o mouse e a câmera e fazemos o mouse reaparecer. Adicionar as funções `getPointerLock()` e `lockChange()` vai nos ajudar a fazer exatamente isso.

A função `getPointerLock()` escuta quando ocorre um clique do mouse. Após o clique, nosso jogo renderizado (no elemento `container`) tenta obter o controle do mouse. Também adicionamos um ouvinte de eventos para detectar quando o jogador ativa ou desativa o bloqueio que, em seguida, chama `lockChange()`. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

Nossa função `lockChange()` precisa desativar ou ativar os controles e o elemento `blocker`. Podemos determinar o estado do bloqueio do ponteiro verificando se o destino da propriedade [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) para os eventos do mouse está definido para o nosso `container`.

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

Agora, podemos adicionar uma chamada para `getPointerLock()` logo antes da nossa função `init()`.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

Neste ponto, temos a capacidade de **olhar** ao redor, mas o fator real de 'assombro' é a capacidade de nos **movermos**. As coisas vão ficar um pouco matemáticas com vetores, mas o que são os elementos gráficos 3D sem um pouco de matemática?

<iframe height='300' scrolling='no' title='Olhar ao redor' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>Olhar ao redor</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="5-adding-player-movement"></a>5. Adicionar movimento do jogador

Para descobrir como fazer nosso jogador se mover, temos que voltar às nossas aulas de cálculo. Queremos aplicar velocidade (movimento) a `camera` ao longo de um determinado vetor (direção).

Vamos adicionar mais algumas variáveis ​​globais para saber em qual direção o jogador está se movendo e definir um vetor de velocidade inicial:

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

No início da função `init()`, defina `clock` para um novo objeto `Clock`. Vamos usar isso para acompanhar a alteração no tempo (delta) para renderizar novos quadros. Você também precisará adicionar uma chamada para `listenForPlayerMovement()`, que coleta a entrada do usuário. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

Nossa função `listenForPlayerMovement()` é o que vai inverter nossos estados de direção. Na parte inferior da função, temos dois ouvintes de eventos que aguardam que as teclas sejam pressionadas e liberadas. Depois que um desses eventos é disparado, vamos verificar em seguida se é uma chave que queremos para disparar ou interromper o movimento.

Para esse jogo, nós configuramos para que o jogador possa se movimentar com as teclas W, A, S, D ou as teclas de seta.

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

Agora que podemos determinar em qual direção o usuário deseja ir (que agora está armazenada como `true` em um dos sinalizadores de direção global), é hora de alguma ação. Essa ação acontece na forma da função `animatePlayer()`.

Essa função será chamada de dentro de `animate()`, passando em `delta` para obter a alteração de tempo entre quadros para que nosso movimento não pareça fora de sincronia durante as mudanças na taxa de quadros:

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

Agora é hora da parte divertida! Nosso vetor de impulso (`playerVeloctiy`) tem três parâmetros, `(x, y, z)`, com `y` sendo o impulso vertical. Como não vamos fazer nenhum salto neste jogo, só vamos trabalhar com os parâmetros `x` e `z`. Inicialmente, esse vetor é definido como (0, 0, 0).

Como visto no código abaixo, uma série de verificações é feita para ver qual sinalizador de direção é invertido para `true`. Assim que tivermos a direção, adicionamos ou subtraímos de `x` e `y` para aplicar o impulso nessa direção. Se nenhuma chave de movimento estiver sendo pressionada, o vetor será definido para `(0, 0, 0)`.


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

Por fim, aplicamos quaisquer que sejam os valores atualizados de `x` e `y` à câmera como traduções para fazer com que o jogador realmente se mova.

---

Parabéns! Agora, você tem uma câmera de jogador controlada que pode se movimentar e olhar ao redor. Nós ainda passamos direto pelas paredes, mas isso é algo para nos preocuparmos mais tarde. Em seguida, adicionaremos nosso dinossauro.

<iframe height='300' scrolling='no' title='Mover-se' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>Mover-se</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!NOTE]
> Se você usar esses controles em seu aplicativo UWP, poderá experimentar um atraso de movimento e eventos `keyUp` não registrados. Estamos analisando isso e esperamos corrigir essa parte do exemplo em breve!

### <a name="6-load-that-dino"></a>6. Carregar o dinossauro!

Se você clonou ou baixou este repositório de projetos, verá uma pasta `models` com `dino.json` dentro. Esse arquivo JSON é um modelo de dinossauro 3D que foi feito e exportado do Blender.


Teremos que adicionar mais variáveis ​​globais para carregar esse dinossauro:

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

Agora que temos nosso `JSONLoader` criado, passaremos para o nosso **dino.json** e um retorno de chamada com a geometria e os materiais reunidos do arquivo.
Carregar o dinossauro é uma tarefa assíncrona, ou seja, nada será renderizado até que o dinossauro esteja completamente carregado. Em nosso **index.html**, alteramos a cadeia de caracteres no elemento `instructions` para `"Loading..."` para informar o jogador de que as coisas estão em andamento.

Depois que o dinossauro for carregado, atualize o elemento `instructions` com as instruções reais do jogo e mova a função `animate()` do fim de `init()` para o final da função retorno de chamada visto abaixo:

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

Agora nosso modelo de dinossauro está carregado. Veja só isso!

<iframe height='300' scrolling='no' title='Adicionar o dinossauro' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>Adicionar o dinossauro</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="7-move-that-dino"></a>7. Mover o dinossauro!

Criar IA para um jogo pode se tornar extremamente complexo, então, neste exemplo, faremos com que esse dinossauro tenha um comportamento de movimento simples. Nosso dinossauro vai se mover em linha reta, passando pelas paredes e sumindo na neblina distante.

Para fazer isso, primeiro adicione a variável global `dinoVelocity`.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 Em seguida, chame a função `animateDino()` da função `animation()` e adicione o código abaixo:

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

Observar o dinossauro sumir na distância não é muito divertido, mas depois de adicionarmos a detecção de colisão, as coisas ficarão mais interessantes.

<iframe height='300' scrolling='no' title='Mover o dinossauro - nenhuma colisão' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>Mover o dinossauro - nenhuma colisão</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. Detecção de colisão para o jogador

Agora temos o jogador e o dinossauro se movimentando, mas ainda há aquele problema chato de que todos passam pelas paredes. Quando começamos a adicionar nossos cubos e paredes anteriormente neste tutorial, nós os enviamos para a matriz `collidableObjects`. Essa matriz é o que usaremos para saber se um jogador está perto demais de algo que não pode atravessar.

Vamos usar raycasters para determinar quando um cruzamento está prestes a ocorrer. Você pode imaginar um raycaster como um feixe de laser que sai da câmera em alguma direção especificada, relatando de volta se bateu em um objeto e exatamente a qual distância ele está.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

Vamos criar uma nova função chamada `detectPlayerCollision()` que retornará `true` se o jogador estiver próximo demais de um objeto colidente.
Para o jogador, vamos aplicar um raycaster, alterando a direção para a qual ele está apontando dependendo de em qual direção ele está indo.

Para fazer isso, nós criamos `rotationMatrix`, uma matriz indefinida. Como podemos verificar em qual direção estamos indo, vamos acabar com uma `rotationMatrix` definida, ou indefinida se você estiver indo em frente.
Se definida, a `rotationMatrix` será aplicada à direção dos controles. 

Um raycaster será criado, começando na câmera e indo na direção da `cameraDirection`.


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

Nossa função `detectPlayerCollision()` depende da função auxiliar `rayIntersect()`.
Isso levará um raycaster e um valor que representa a proximidade a que podemos chegar de um objeto na matriz `collidableObjects` antes de determinar que ocorreu uma colisão.

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

Agora que podemos determinar quando uma colisão está prestes a ocorrer, nós podemos aprimorar nossa função `animatePlayer()`:

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

Agora nós temos uma detecção de colisão do jogador, então vá em frente e tente atravessar alguma parede!

<iframe height='300' scrolling='no' title='Mover o jogador - colisão' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>Mover o jogador - colisão</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. Detecção de colisão e animação para o dinossauro

Está na hora impedir nosso dinossauro de passar pelas paredes, e em vez disso, fazê-lo ir em uma direção aleatória quando estiver próximo demais um objeto colidente.

Primeiro, vamos descobrir quando nosso dinossauro está prestes a colidir. 

Será necessário definir outra variável global para a distância de colisão:

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

Agora que especificamos a que distância queremos que nosso dinossauro colida, vamos adicionar uma função semelhante à `detectPlayerCollision()`, mas um pouco mais simples.
A função `detectDinoCollision` é simples porque sempre temos um raycaster saindo diretamente da frente do dinossauro. Não há necessidade de girá-lo como para a colisão do jogador.

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

Vamos dar uma espiada em como nossa função `animateDino()` final será quando conectada com a detecção de colisão:


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

Queremos que nosso dinossauro gire -90, 90 ou 180 graus. Para fazer isso direto, criamos acima a matriz `directionMultiples` que produzirá esses números quando multiplicada por 90.
Para tornar aleatória a seleção dos graus de rotação, adicionamos a função auxiliar `getRandomInt()` para obter um valor de 0, 1 ou 2, que representará um índice aleatório da matriz.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

Depois de fazer isso, multiplicamos o índice aleatório da matriz por 90 para obter o grau de rotação (convertido em radianos).
A adição desse valor para a rotação `y` do dinossauro com `dino.rotation.y += randomDirection;` faz com que o dinossauro agora faça voltas aleatórias após a colisão.


---

Nós conseguimos! Agora temos um dinossauro com IA que pode se mover pelo nosso labirinto!

<iframe height='300' scrolling='no' title='Mover o dinossauro - colisão' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>Mover o dinossauro - colisão</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="10-starting-the-chase"></a>10. Começar a caçada

Quando o dinossauro estiver a uma determinada distância do jogador, queremos que comece a caçá-lo. Como este é apenas um exemplo, não existe nenhum algoritmo avançado aplicado para o dinossauro rastrear o jogador. Em vez disso, o dinossauro vai olhar para o jogador e andar na direção dele. Isso funciona muito bem em uma parte aberta do labirinto, mas o dinossauro fica preso quando há uma parede no caminho.

Na nossa função `animate()`, adicionaremos uma variável booliana que é determinada pelo que é retornado por `triggerChase()`:

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

Nossa função `triggerChase` vai confirmar se o jogador está ao alcance de caça do dinossauro e, depois fazer o dinossauro sempre ficar de frente para o jogador, o que permite que ele se movimente na direção do jogador. 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

A segunda metade de `triggerChase` lida com a exibição de algum texto que informa ao jogador a que distância o dinossauro está. Também introduzimos `CATCHOFFSET` para especificar qual distância deve ser `0`. Se não tivéssemos o deslocamento, `0` seria bem em cima do jogador, o que não cria um final muito cinematográfico.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

Neste ponto, temos um dinossauro selvagem que começa a seguir o jogador quando você se aproxima demais e que não para até sua posição ser em cima do jogador.
A etapa final é adicionar algumas condições de fim de jogo assim que o dinossauro estiver a `CATCHOFFSET` unidades de distância.

<iframe height='300' scrolling='no' title='A perseguição' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>A caça</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) na <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="11-ending-the-game"></a>11. Finalizar o jogo


Percorremos um longo caminho desde um cubo simples, e agora é a hora de finalizar.

Vamos primeiro definir uma variável para acompanhar se o jogo acabou ou não:

```javascript
var gameOver = false;
```

Agora precisamos atualizar nossa função `animate()` pela última vez para verificar se o dinossauro está perto demais do jogador.
Se o dinossauro estiver muito próximo, iniciaremos uma nova função chamada `caught()` e impediremos que o jogador e o dinossauro se movam. Se não, continuaremos como usual e deixaremos o jogador e o dinossauro se movimentarem.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

Se o dinossauro capturar o jogador, `caught()` vai exibir nosso elemento `blocker` e atualizar o texto para indicar que o jogo foi perdido.
A variável `gameOver` ​​também está definida para `true`, o que agora nos permite saber que o jogo acabou.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


Agora que sabemos se o jogo acabou ou não, podemos adicionar uma verificação de fim de jogo à nossa função `lockChange()`.
Quando o usuário pressiona Esc depois que o jogo é encerrado, podemos adicionar `location.reload` para reiniciar o jogo.

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

Pronto! Foi uma jornada e tanto, mas agora temos um jogo feito com **three.js**.

Volte para o topo da página para conferir a [CodePen final](#introduction)!


## <a name="publishing-to-the-microsoft-store"></a>Publicar na Microsoft Store
Agora que você tem um aplicativo UWP, é possível publicá-lo na Microsoft Store (supondo que você o aperfeiçoou primeiro!) Há algumas etapas no processo.

1.  É preciso estar [registrado](https://developer.microsoft.com/store/register) como Desenvolvedor do Windows.
2.  É preciso usar a [lista de verificação](https://docs.microsoft.com/windows/uwp/publish/app-submissions) de envio de aplicativo.
3.  O aplicativo deve ser enviado para [certificação](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process).
Para obter mais detalhes, confira [Publicar seu aplicativo UWP](https://docs.microsoft.com/windows/uwp/publish/).

