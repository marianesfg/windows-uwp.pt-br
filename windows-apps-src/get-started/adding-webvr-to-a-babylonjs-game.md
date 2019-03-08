---
title: Adicionando suporte a WebVR a um jogo 3D do Babylon.js
description: Saiba como adicionar suporte a WebVR a um jogo 3D do Babylon.js existente.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, desenvolvimento da web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638551"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Adicionando suporte a WebVR a um jogo 3D do Babylon.js

Se você criou um jogo 3D com o Babylon.js e achou que ele pode funcionar perfeitamente na realidade virtual (VR), siga as etapas simples deste tutorial para tornar isso uma realidade.

Adicionaremos o suporte a WebVR ao jogo mostrado aqui. Vá em frente e conecte um controle Xbox para experimentar!


<iframe height='300' scrolling='no' title='Jogo de dino Babylon. js usando Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon. js dino jogo usando Babylon.GUI</a> pelo Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

Este é um jogo 3D que funciona perfeitamente em tela plana, mas e em VR?
Neste tutorial, conduziremos você pelas etapas necessárias para que ele funcione com a WebVR. Usaremos um fone de ouvido [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) que pode aproveitar o suporte adicional para WebVR no Microsoft Edge. Após aplicarmos essas alterações ao jogo, espera-se também que ele funcione em outras combinações de navegador/fone de ouvido compatíveis com a WebVR.



## <a name="prerequisites"></a>Pré-requisitos

- Um editor de texto (como [Visual Studio Code](https://code.visualstudio.com/download))
- Um controle Xbox conectado ao computador
- Atualização do Windows 10 para Criadores
- Um computador com as [especificações mínimas necessárias à execução do Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Um dispositivo Windows Mixed Reality (opcional) 



## <a name="getting-started"></a>Introdução

O modo mais simples de começar é visitando o [repositório Windows-tutorials-web do GitHub](https://github.com/Microsoft/Windows-tutorials-web), pressionando o botão verde **Clone or download** e selecionando **Open in Visual Studio**.

![botão clone ou botão baixar](images/3dclone.png)

Se não quiser clonar o projeto, você poderá baixá-lo como um arquivo zip.
Você terá duas pastas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) e [depois](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). A pasta "before" é nosso jogo antes da adição de qualquer recurso VR e a pasta "after" é o jogo final com suporte a VR.

As pastas before e after contêm estes arquivos:
-   **texturas /** – uma pasta que contém todas as imagens usadas no jogo.
-   **CSS /** – uma pasta que contém o CSS para o jogo.
-   **js /** – uma pasta que contém os arquivos JavaScript. O arquivo main.js é nosso jogo, e os outros arquivos são as bibliotecas utilizadas.
-   **modelos /** – uma pasta que contém os modelos 3D. Neste jogo, temos apenas um modelo, para o dinossauro.
-   **index. HTML** -a página da Web que hospeda o renderizador do jogo. Abrir esta página no Microsoft Edge inicia o jogo.

Você pode testar as duas versões do jogo, abrindo arquivos seus respectivos index. HTML no Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Portal de Realidade Misturada

Se você não estiver familiarizado com o Windows Mixed Reality e tiver a Atualização do Windows 10 para Criadores instalada em um computador com uma placa gráfica compatível, tente abrir o app **Portal de Realidade Misturada** no menu Iniciar do Windows 10.

![Pesquisa do Portal de Realidade Misturada](images/mixed-reality-portal.png)

Se você tiver cumprido todos os requisitos, poderá ativar os recursos de desenvolvedor e simular um fone de ouvido Windows Mixed Reality conectado ao computador. Se você tiver a sorte de ter um fone de ouvido real próximo, conecte-o e execute a instalação.

> [!IMPORTANT]
> O Portal de Realidade Misturada deve estar continuamente aberto durante este tutorial.

Agora você está pronto para experimentar o WebVR com o Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface do usuário 2D em um mundo virtual

>[!NOTE]
> Pegue o [ **antes de** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) pasta para obter o exemplo inicial.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) é uma biblioteca compatível com VR, permitindo que você para criar um simples, o usuário interativo interfaces que funcionam bem para VR e não-VR exibe.
Uma extensão do Babylon. js, o `GUI` biblioteca é usada throuhout exemplo para criar elementos 2D.


Um texto 2D `GUI` elemento pode ser criado com algumas linhas, dependendo de quantos atributos que você deseja ajustar.
O trecho de código a seguir já está em nossa [ **antes de** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) amostragem, mas vamos passo a passo, o que está acontecendo.
Primeiro a colocamos um [ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para estabelecer a GUI que abrangerá. O exemplo define isso como `CreateFullScreenUI()`, que significa que nossa interface do usuário abordará a tela inteira. Com o `AdvancedDynamicTexture` criou, em seguida, fazemos uma caixa de texto 2D que aparece ao iniciar o jogo usando `GUI.Rectanlge()` e `GUI.TextBlock()`.


Esse código é adicionado dentro [ **Main**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Essa interface do usuário é visível depois de criado, mas podem ser ativados ou desativados com `isVisible` dependendo do que está acontecendo no jogo.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detectando fones de ouvido

É recomendável para aplicativos de VR ter dois tipos de câmeras para que podem ter suporte a vários cenários. Nesse jogo, ofereceremos suporte a uma câmera que requer que um fone de ouvido de trabalho seja conectado, e outra que não usa fone de ouvido. Para determinar qual delas o jogo usará, primeiro precisamos verificar se um fone de ouvido foi detectado. Para fazer isso, usaremos [ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Adicione este código acima `window.addEventListener('DOMContentLoaded')` na **Main**.
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

Com o Babylon. js, a WebVR pode ser adicionado rapidamente usando o [ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera). Essa câmera pode receber entrada de teclado e permite que você use um fone de ouvido VR para controlar a rotação da "cabeça".


### <a name="step-1-checking-for-headsets"></a>Etapa 1: Verificação de fones de ouvido

Para nosso câmera fallback, estaremos usando o [ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera) que está sendo usado no jogo original.

Vamos verificar nossos `headset` variável para determinar se podemos usar o `WebVRFreeCamera` câmera.

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


### <a name="step-2-activating-the-webvrfreecamera"></a>Etapa 2: Ativando o WebVRFreeCamera
Para ativar esta câmera na maioria dos navegadores, o usuário deve passar por alguma experiência virtual.
Vamos nos conectar essa funcionalidade até um clique do mouse.


Cole o código na função `createScene()` após `camera.applyGravity = true;`.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Um clique no jogo agora cria um prompt semelhante ao seguinte ou exibe o jogo o fone de ouvido imediatamente se o usuário aceitar o prompt de antes.

![prompt imersivo](images/immersiveview.png)

Também podemos adicionar um trecho de código que exibirá a `UniversalCamera` exibir antes de que mudamos para o nosso `WebVRFreeCamera`, permitindo que o usuário examinar o jogo em vez de uma janela de azul. 

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

Uma vez que o `WebVRFreeCamera` inicialmente não dá suporte a gamepads, mapearemos nossos botões gamepad as teclas de direção do teclado. Faremos isso por examinando o `inputs` propriedade da câmera. Adicionando os códigos correspondentes do direcional analógico esquerdo para cima, para baixo, para a esquerda e para a direita de modo que correspondam às teclas de seta, nosso gamepad volta a funcionar.


Adicione este código a seguir o `scene.onPointerDown = function() {...}` chamar.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Etapa 4: Experimente!

Se abrimos **index. HTML** com nossos fone de ouvido e conectado controlador de jogo, um clique à esquerda da janela do jogo azul alternará nosso jogo para o modo VR! Vá em frente e coloque o fone de ouvido para conferir os resultados. 


<iframe height='300' scrolling='no' title='Jogo de dino Babylon. js usando Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon. js dino jogo usando Babylon.GUI - WebVR</a> pelo Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusão

Parabéns! Agora você tem um jogo de Babylon.js completo com suporte a WebVR. A partir daqui, você pode aproveitar o que aprendeu para criar um jogo ainda melhor ou um jogo completamente diferente deste.
