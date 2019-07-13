---
title: Adicionando suporte a WebVR a um jogo Babylon.js 3D
description: Saiba como adicionar suporte a WebVR a um jogo Babylon.js 3D existente.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, desenvolvimento da web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 5f212e4e06035134b0ac5b5ea69381ed0d985783
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321161"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Adicionando suporte a WebVR a um jogo Babylon.js 3D

Se você criou um jogo 3D com o Babylon.js e achou que ele pode funcionar perfeitamente em VR (realidade virtual), siga as etapas simples deste tutorial para fazer disso uma realidade.

Adicionaremos suporte a WebVR ao jogo mostrado aqui. Vá em frente e conecte um controlador do Xbox para experimentar!


<iframe height='300' scrolling='no' title='Jogo de dinossauro do Babylon.js usando Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Jogo de dinossauro do Babylon.js usando Babylon.GUI</a> da Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

Este é um jogo 3D que funciona bem em tela plana, mas e em VR?
Neste tutorial, percorreremos as etapas necessárias para colocá-lo em funcionamento com WebVR. Usaremos um headset do [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) que pode tirar proveito do suporte adicional a WebVR no Microsoft Edge. Após aplicarmos essas alterações ao jogo, espera-se que ele também funcione em outras combinações de navegador/headset compatíveis com WebVR.



## <a name="prerequisites"></a>Pré-requisitos

- Um editor de texto (como o [Visual Studio Code](https://code.visualstudio.com/download))
- Um controlador do Xbox conectado ao computador
- Atualização do Windows 10 para Criadores
- Um computador com as [especificações mínimas necessárias à execução de Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Um dispositivo Windows Mixed Reality (opcional) 



## <a name="getting-started"></a>Introdução

O modo mais simples de começar é visitar o [repositório Windows-tutorials-web do GitHub](https://github.com/Microsoft/Windows-tutorials-web), pressionar o botão verde **Clonar ou baixar** e selecionar **Abrir no Visual Studio**.

![botão clonar ou baixar](images/3dclone.png)

Se não quiser clonar o projeto, você poderá baixá-lo como um arquivo zip.
Você terá duas pastas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) e [depois](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). A pasta "antes" é nosso jogo antes da adição de qualquer recurso de VR e a pasta "depois" é o jogo final, com suporte para VR.

As pastas antes e depois contêm estes arquivos:
-   **textures/** – uma pasta que contém as imagens usadas no jogo.
-   **css/** – uma pasta que contém o CSS do jogo.
-   **js/** – uma pasta que contém os arquivos JavaScript. O arquivo main.js é nosso jogo e os outros arquivos são as bibliotecas utilizadas.
-   **models/** – uma pasta que contém os modelos 3D. Neste jogo, temos apenas um modelo, para o dinossauro.
-   **index.html** – a página da Web que hospeda o renderizador do jogo. Abrir esta página no Microsoft Edge inicia o jogo.

Você pode testar as duas versões do jogo abrindo seus respectivos arquivos index.html no Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>O Portal de Realidade Misturada

Se você não estiver familiarizado com o Windows Mixed Reality e tiver a Atualização do Windows 10 para Criadores instalada em um computador com placa gráfica compatível, tente abrir o aplicativo **Portal de Realidade Misturada** no menu Iniciar do Windows 10.

![Pesquisa no Portal de Realidade Misturada](images/mixed-reality-portal.png)

Se tiver cumprido todos os requisitos, você poderá ligar os recursos de desenvolvedor e simular um headset do Windows Mixed Reality conectado ao computador. Se você tiver a sorte de ter um headset real próximo, conecte-o e execute a instalação.

> [!IMPORTANT]
> O Portal de Realidade Misturada deve estar aberto continuamente durante este tutorial.

Agora, você está pronto para experimentar a WebVR com o Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface do usuário 2D em um mundo virtual

>[!NOTE]
> Acesse a pasta [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obter a amostra inicial.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) é uma biblioteca compatível com VR, que permite criar interfaces do usuário simples e interativas que funcionam bem para telas de VR e não VR.
Uma extensão do Babylon.js, a biblioteca `GUI` é usada em toda a amostra para criar elementos 2D.


Um elemento de texto 2D `GUI` pode ser criado com algumas linhas, dependendo de quantos atributos você deseja ajustar.
O snippet de código a seguir já está em nossa amostra [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before), mas vamos ver passo a passo o que está acontecendo.
Primeiro, criamos um objeto [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) para estabelecer o que a GUI abrangerá. A amostra o define como `CreateFullScreenUI()`, o que significa que nossa interface do usuário abrangerá a tela inteira. Com o `AdvancedDynamicTexture` criado, fazemos uma caixa de texto 2D que aparece ao iniciar o jogo usando `GUI.Rectanlge()` e `GUI.TextBlock()`.


Esse código é adicionado dentro de [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Essa interface do usuário é visível depois de criada, mas pode ser ativada ou desativada com `isVisible`, dependendo do que está acontecendo no jogo.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detectando headsets

Uma prática recomendada para aplicativos VR é ter dois tipos de câmeras, a fim de que haja suporte para vários cenários. Neste jogo, daremos suporte a uma câmera que requer que um headset em funcionamento esteja conectado e a outra que não usa um headset. Para determinar qual delas o jogo usará, primeiro precisamos verificar se um headset foi detectado. Para fazer isso, usaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Adicione o código acima de `window.addEventListener('DOMContentLoaded')` em **main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Com as informações armazenadas na variável `headset`, poderemos escolher a câmera ideal para o usuário.


## <a name="creating-and-selecting-the-initial-camera"></a>Criando e selecionando a câmera inicial

Com o Babylon.js, a WebVR pode ser adicionada rapidamente usando a [`WebVRFreeCamera`](https://doc.babylonjs.com/api/classes/babylon.webvrfreecamera). Essa câmera pode receber entrada de teclado e permite que você use um headset VR para controlar a rotação da "cabeça".


### <a name="step-1-checking-for-headsets"></a>Etapa 1: Procurar headsets

Como câmera reserva, usaremos a [`UniversalCamera`](https://doc.babylonjs.com/api/classes/babylon.universalcamera) que é usada atualmente no jogo original.

Verificaremos nossa variável `headset` para determinar se podemos usar a câmera `WebVRFreeCamera`.

Substitua `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` pelo código a seguir.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>Etapa 2: Ativar a WebVRFreeCamera
Para ativar esta câmera na maioria dos navegadores, o usuário precisa interagir com a experiência virtual.
Vamos conectar essa funcionalidade a um clique do mouse.


Cole o código na função `createScene()` após `camera.applyGravity = true;`.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Agora, um clique no jogo criará um prompt semelhante ao seguinte ou exibirá o jogo no headset imediatamente, caso o usuário tenha aceitado o prompt antes.

![prompt imersivo](images/immersiveview.png)

Também podemos adicionar um trecho de código que mostrará a exibição `UniversalCamera` antes de mudarmos para nosso `WebVRFreeCamera`, permitindo que o usuário veja o jogo em vez de uma janela azul. 

Adicione o seguinte após `engine.runRenderLoop(function () {`.
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

### <a name="step-3-adding-gamepad-support"></a>Etapa 3: Adicionar suporte para gamepad

Como a `WebVRFreeCamera` inicialmente não dá suporte a gamepads, mapearemos os botões de gamepad para as teclas de seta do teclado. Faremos isso investigando a propriedade `inputs` da câmera. Adicionando os códigos correspondentes ao direcional analógico para cima, para baixo, para a esquerda e para a direita de modo que correspondam às teclas de seta, nosso gamepad volta a funcionar.


Adicione este código abaixo da chamada `scene.onPointerDown = function() {...}`.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Etapa 4: Experimentar!

Se abrirmos **index.html** com nosso headset e controlador de jogo conectados, um clique com o botão esquerdo do mouse na janela azul do jogo colocará o jogo no modo VR! Vá em frente e coloque o headset para conferir os resultados. 


<iframe height='300' scrolling='no' title='Jogo de dinossauro do Babylon.js usando Babylon.GUI – WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Jogo de dinossauro do Babylon.js usando Babylon.GUI – WebVR</a> da Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusão

Parabéns! Agora você tem um jogo de Babylon.js completo com suporte para WebVR. Daqui, você pode aproveitar o que aprendeu para criar um jogo ainda melhor ou um jogo completamente diferente deste.
