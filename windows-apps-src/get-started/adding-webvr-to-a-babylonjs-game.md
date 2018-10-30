---
title: Adicionando suporte a WebVR a um jogo de Babylon. js 3D
description: Saiba como adicionar suporte a WebVR a um jogo de Babylon. js 3D existente.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, desenvolvimento da web, babylon, babylonjs, Babylon. js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 72681c3f91fc2dcbfcc4e4531359d6d668e18b80
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5765663"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Adicionando suporte a WebVR a um jogo de Babylon. js 3D

Se você criou um jogo 3D com Babylon. js e pensado que pode parecer ótima em realidade virtual (VR), siga as etapas simples para garantir que uma realidade neste tutorial.

Vamos adicionar suporte a WebVR ao jogo mostrado aqui. Vá em frente e conectar um controlador do Xbox para testá-lo!


<iframe height='300' scrolling='no' title='Jogo de dino Babylon. js usando Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon. js dino jogo usando Babylon.GUI</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

Isso é um jogo 3D que funcione bem em uma tela simples, mas o que sobre em VR?
Neste tutorial, nós o guiaremos pelas etapas alguns ele leva para obter esse e em execução com a WebVR. Vamos usar um fone de ouvido de [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) pode aproveitar o suporte adicionado para WebVR no Microsoft Edge. Após aplicarmos essas alterações para o jogo, você pode esperar que ele também funcionam em outras combinações de navegador/fone de ouvido que dão suporte a WebVR.



## <a name="prerequisites"></a>Pré-requisitos

- Um editor de texto (como o [Visual Studio Code](https://code.visualstudio.com/download))
- Um controlador do Xbox que está conectado ao computador
- Atualização do Windows 10 para Criadores
- Um computador com o [mínimas especificações necessárias para executar o Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Um dispositivo Windows Mixed Reality (opcional) 



## <a name="getting-started"></a>Introdução

A maneira mais simples de começar é visitar o [repositório do GitHub tutoriais-Windows-web](https://github.com/Microsoft/Windows-tutorials-web), pressione a verde **Clone ou baixar** botão e selecione **aberto no Visual Studio**.

![botão clone ou botão baixar](images/3dclone.png)

Se não quiser clonar o projeto, você pode baixá-lo como um arquivo zip.
Em seguida, você terá duas pastas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) e [depois](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). A pasta "antes" é nosso jogo antes de quaisquer recursos VR são adicionados, e a pasta "após" é o jogo concluído com suporte VR.

O antes e depois de pastas contêm esses arquivos:
-   **texturas /** - uma pasta que contém as imagens usadas no jogo.
-   **css /** - uma pasta que contém o CSS para o jogo.
-   **js /** - uma pasta que contém os arquivos JavaScript. O arquivo main. js é nosso jogo, e os outros arquivos são as bibliotecas usadas.
-   **modelos /** - uma pasta que contém os modelos 3D. Para esse jogo, temos apenas um modelo, para o dinossauro.
-   **index. HTML** - a página da Web que hospeda o renderizador do jogo. Abrir esta página no Microsoft Edge inicia o jogo.

Você pode testar as duas versões do jogo, abrindo arquivos seus respectivos index. HTML no Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>O Portal de realidade misturada

Se você não estiver familiarizado com o Windows Mixed Reality e tiver o Windows 10 para criadores instalada em um computador com uma placa gráfica compatível, tente abrir o aplicativo do **Portal de realidade misturada** do menu Iniciar no Windows 10.

![Pesquisa de Portal de realidade mista](images/mixed-reality-portal.png)

Se você atender a todos os requisitos, você pode ativar recursos de desenvolvedor e simular um fone de ouvido Windows Mixed Reality conectado ao computador. Se você estiver sorte de ter um headset real nas proximidades, conecte-o e execute a instalação.

> [!IMPORTANT]
> O Portal de realidade misturada deve ser aberto em todos os momentos durante este tutorial.

Agora você está pronto para experimentar WebVR com o Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface do usuário 2D em um mundo virtual

>[!NOTE]
> Prenda a pasta [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obter a amostra de starter.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) é uma biblioteca de VR amigável, permitindo que você para criar simples, o usuário interativo interfaces que funcionam bem para VR e não de VR exibe.
Uma extensão Babylon. js, o `GUI` biblioteca é usado throuhout a amostra para criar elementos 2D.


Um texto 2D `GUI` elemento pode ser criado com algumas linhas dependendo de quantos atributos você querer ajustar.
O trecho de código a seguir já está em nosso exemplo de [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , mas vamos passo a passo que está acontecendo.
Fazemos primeiro um [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para estabelecer a GUI que abordará. O exemplo define como sendo `CreateFullScreenUI()`, que significa nossa interface do usuário abordará a tela inteira. Com `AdvancedDynamicTexture` criado, podemos fazer em seguida, uma caixa de texto 2D que aparece ao iniciar o jogo usando `GUI.Rectanlge()` e `GUI.TextBlock()`.


Esse código é adicionado em [**Main. js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Essa interface do usuário fica visível quando criado, mas pode ser ativada ou desativada com `isVisible` dependendo do que está acontecendo no jogo.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detectar fones de ouvido

É uma boa prática para aplicativos de VR ter dois tipos de câmeras para que podem oferecer suporte a vários cenários. Para esse jogo, nós vai dar suporte a uma câmera que requer um fone de ouvido de trabalho para ser conectado e outra que não usa nenhum fone de ouvido. Para determinar qual o jogo usará, podemos deve verificar primeiro para ver se um fone de ouvido foi detectado. Para fazer isso, usaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Adicione este código acima `window.addEventListener('DOMContentLoaded')` em **Main. js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Com as informações armazenadas no `headset` variável, agora poderemos escolher a câmera que seja ideal para o usuário.


## <a name="creating-and-selecting-the-initial-camera"></a>Criando e selecionando a câmera inicial

Com Babylon. js, a WebVR pode ser adicionada rapidamente usando o [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Câmera pode levar a entrada do teclado e permite que você use um fone de ouvido VR para controlar sua rotação "head".


### <a name="step-1-checking-for-headsets"></a>Etapa 1: Verificar se há fones de ouvido

Para nossa câmera fallback, vamos usar o [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) que atualmente é usado no jogo original.

Vamos verificar nosso `headset` variável para determinar se podemos usar o `WebVRFreeCamera` câmera.

Substitua `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` com o código a seguir.
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
Para ativar a câmera na maioria dos navegadores, o usuário deve executar alguma interação que solicita a experiência virtual.
Vamos nos conectar essa funcionalidade até um clique do mouse.


Cole o código dentro de `createScene()` funcionam após `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Um clique no jogo agora criará um prompt com o seguinte, ou exibe o jogo no fone de ouvido imediatamente se o usuário aceitou o prompt antes.

![prompt imersivo](images/immersiveview.png)

Também adicionamos um trecho de código que exibirá a `UniversalCamera` exibir antes de podemos alternar para nosso `WebVRFreeCamera`, permitindo que o usuário examinar o jogo em vez de uma janela azul. 

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

### <a name="step-3-adding-gamepad-support"></a>Etapa 3: Adicionar suporte a gamepad

Desde que o `WebVRFreeCamera` inicialmente não dá suporte a gamepads, nós vai mapear os botões de gamepad para as teclas de seta do teclado. Faremos isso por aprofundarmos o `inputs` propriedade da câmera. Adicionando os códigos correspondentes para o joystick analógico esquerdo para cima, para baixo, esquerda e direita fazer a correspondência com as teclas de seta, nosso gamepad é novamente em ação.


Adicione este código abaixo do `scene.onPointerDown = function() {...}` chamar.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Etapa 4: Experimentá-lo!

Se abrimos **index. HTML** com nosso fone de ouvido e controlador de jogo conectado, um clique esquerdo da janela do jogo azul alternará nosso jogo para o modo VR! Vá em frente e colocar em seu fone de ouvido conferir os resultados. 


<iframe height='300' scrolling='no' title='Jogo de dino Babylon. js usando Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon. js dino jogo usando Babylon.GUI - WebVR</a> dos documentos do Microsoft Edge (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusão

Parabéns! Agora você tem um jogo de Babylon. js completo com suporte a WebVR. A partir daqui, você pode tirar o que aprendeu para criar um jogo ainda melhor ou compilar desativar esta.
