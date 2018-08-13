---
title: Adicionando suporte a WebVR a um jogo Babylon.js 3D
description: Saiba como adicionar WebVR suporte a um jogo de Babylon.js 3D existente.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr, borda, desenvolvimento de web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018646"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Adicionando suporte a WebVR a um jogo Babylon.js 3D

Se você tiver criado um jogo 3D com Babylon.js e pensou que ele seria excelente na realidade virtual (VR), siga as etapas simples neste tutorial fazer com que uma realidade.

Vamos adicionar WebVR suporte ao jogo mostrado aqui. Vá em frente e conecta um controlador Xbox testá-lo!


<iframe height='300' scrolling='no' title='Jogo de dino Babylon.js usando Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>jogo de dino Babylon.js usando Babylon.GUI</a> pelo Microsoft borda Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

Este é um jogo 3D que funciona bem em uma tela plana, mas o que sobre na VR?
Neste tutorial, vamos examinar algumas etapas ele leva para fazer isso para cima e em execução com WebVR. Usaremos um fone de ouvido [Realidade misto do Windows](https://developer.microsoft.com/en-us/windows/mixed-reality) que pode canalizar o suporte adicionado para WebVR no Microsoft Edge. Depois aplicamos essas alterações ao jogo, você pode esperar que ele também para trabalhar em outras combinações de navegador/headset que oferecem suporte a WebVR.



## <a name="prerequisites"></a>Pré-requisitos

- Um editor de texto (como o [Código do Visual Studio](https://code.visualstudio.com/download))
- Um controlador de Xbox que está conectado ao seu computador
- Atualização do Windows 10 para Criadores
- Um computador com o [mínimas especificações necessárias para executar a realidade misto do Windows](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Um dispositivo de realidade misto do Windows (opcional) 



## <a name="getting-started"></a>Introdução

A maneira mais simples para começar é a visitar o [web do Windows tutoriais GitHub repo](https://github.com/Microsoft/Windows-tutorials-web), pressione a verde **Clone ou fazer download** emoticon e selecione **Abrir no Visual Studio**.

![botão clone ou botão baixar](images/3dclone.png)

Se não quiser clonar o projeto, você pode baixá-lo como um arquivo zip.
Em seguida, você terá duas pastas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) e [depois](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). A pasta "before" é o nosso jogo antes de qualquer recurso VR é adicionado, e a pasta "depois" é o jogo terminar com suporte VR.

O antes e depois de pastas contêm esses arquivos:
-   **texturas /** - uma pasta contendo quaisquer imagens usadas no jogo.
-   **css /** - uma pasta contendo o CSS para o jogo.
-   **js /** - uma pasta que contém os arquivos JavaScript. O arquivo js é nosso jogo e os outros arquivos são as bibliotecas usadas.
-   **modelos /** - uma pasta que contém os modelos 3D. Para este jogo temos apenas um modelo para o dinossauro.
-   **index** - página da Web que hospeda o processador do jogo. Abrindo esta página no Microsoft Edge inicia o jogo.

Você pode testar as duas versões do jogo, abrindo arquivos seus respectivos index no Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>O Portal de realidade misto

Se você estiver familiarizado com realidade misto do Windows e tiver a atualização do Windows 10 criadores instalado em um computador com uma placa gráfica compatível, tente abrir o aplicativo do **Portal de realidade misto** no menu Iniciar no Windows 10.

![Pesquisa de Portal de realidade mista](images/mixed-reality-portal.png)

Se você atendidas todos os requisitos, você pode ativar os recursos de desenvolvedor e simular um fone de ouvido realidade misto do Windows conectada ao seu computador. Se você estiver sorte de contar com um headset real os próximos, conecte-o e execute a instalação.

> [!IMPORTANT]
> O Portal de realidade misto devem ser aberto em todos os momentos durante este tutorial.

Agora você está pronto para experiência WebVR com Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>UI 2D em um mundo virtual

>[!NOTE]
> Captura a pasta [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obter a amostra de início.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) é uma biblioteca de VR amigável, permitindo que você se funcionam bem para VR de interfaces do usuário interativo para criar um simples, e não-VR exibe.
Uma extensão do Babylon.js, o `GUI` biblioteca é usada throuhout da amostra para criar elementos 2D.


Um texto 2D `GUI` elemento pode ser criado com algumas linhas, dependendo de quantos atributos que você deseja ajustar.
O trecho de código a seguir já está em nosso exemplo [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , mas vamos passo a passo, o que está acontecendo.
Primeiro fizermos uma [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para estabelecer a GUI que cobrirá. O exemplo define como sendo `CreateFullScreenUI()`, que significa que o nosso UI abordará a tela inteira. Com `AdvancedDynamicTexture` criado, em seguida, fizermos uma caixa de texto 2D que aparece ao iniciar o jogo usando `GUI.Rectanlge()` e `GUI.TextBlock()`.


Este código é adicionado dentro [**js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


Essa interface do usuário é visível depois de criado, mas pode ser alternado ativada ou desativada com `isVisible` dependendo o que está acontecendo no jogo.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detectando headsets

É uma boa prática para aplicativos VR ter dois tipos de câmeras, para que os vários cenários podem ser suportados. Para este jogo, vai suportamos uma câmera que requer um fone de ouvido de trabalho para ser conectadas e outro que não usa nenhum headset. Para determinar qual do jogo usará, deve primeiro verificamos para ver se um headset foi detectado. Para fazer isso, usaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Adicione este código acima `window.addEventListener('DOMContentLoaded')` em **js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Com as informações armazenadas no `headset` variável, agora poderemos escolher a câmera ideal para o usuário.


## <a name="creating-and-selecting-the-initial-camera"></a>Criando e selecionando a câmera inicial

Com Babylon.js, os WebVR pode ser adicionado rapidamente usando o [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Nesta câmera pode levar a entrada do teclado e permite que você use um fone de ouvido VR para controlar sua rotação "cabeça".


### <a name="step-1-checking-for-headsets"></a>Etapa 1: Verificação de headsets

Para nossa câmera fallback, usaremos o [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) que está sendo usado no jogo original.

Verificaremos nosso `headset` variável para determinar se é possível usar o `WebVRFreeCamera` câmera.

Substituir `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` com o código a seguir.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>Etapa 2: Ativando o WebVRFreeCamera
Para ativar essa câmera na maioria dos navegadores, o usuário deve executar alguma interação que solicita a experiência virtual.
Vamos nos conectar essa funcionalidade até um clique do mouse.


Cole o código em `createScene()` funcionar após `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Um clique no jogo agora cria um prompt semelhante ao seguinte, ou exibe o jogo de Headset imediatamente se o usuário aceitou o prompt antes.

![prompt imersivo](images/immersiveview.png)

Podemos também pode adicionar um trecho de código que exibirá o o `UniversalCamera` exibir antes que podemos alternar para nosso `WebVRFreeCamera`, permitindo que o usuário a ser analisado o jogo, em vez de uma janela azul. 

Adicione a seguinte após `engine.runRenderLoop(function () {`.
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>Etapa 3: Adicionando suporte a gamepad

Desde que o `WebVRFreeCamera` inicialmente não oferece suporte a gamepads, podemos vai mapear nosso botões gamepad para as teclas de direção do teclado. Faremos isso por aprofundar-se no `inputs` propriedade da câmera. Adicionando os códigos correspondentes para a esquerda pente analógico para cima, para baixo, esquerda e direita fazer a correspondência com as teclas de direção, nosso gamepad é novamente em ação.


Adicione este código abaixo do `scene.onPointerDown = function() {...}` chamada.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Etapa 4: Faça uma tentativa!

Se podemos abrir **index** com nosso headset e na tomada de controlador de jogo, um clique à esquerda da janela do jogo azul alternará nosso jogo para o modo VR! Prossiga e colocar em seu fone de ouvido fazer check-out os resultados. 


<iframe height='300' scrolling='no' title='Jogo de dino Babylon.js usando Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>jogo de dino Babylon.js usando Babylon.GUI - WebVR</a> pelo Microsoft borda Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusão

Parabéns! Agora você tem um jogo Babylon.js completo com suporte de WebVR. A partir daqui, você pode tirar o que você aprendeu construir um jogo ainda melhor, ou criar desativa este.
