---
author: jken
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: Desenvolvendo para realidade misturada
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: Realidade misturada, Hololens, realidade aumentada, focar, voz, controlador
ms.author: jken
ms.date: 2/5/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: 0f392d1d6c8eaa309e1774e8a98671a7743fdc9d
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6665016"
---
# <a name="designing-for-mixed-reality"></a>Desenvolvendo para realidade misturada

Projete seu aplicativo para se encaixar bem na realidade misturada e aproveite os novos métodos de entrada.

## <a name="overview"></a>Visão geral

[Realidade misturada](https://developer.microsoft.com/windows/mixed-reality/mixed_reality) é o resultado da mistura do mundo físico com o mundo digital. As experiências do espectro de realidade misturada incluem de um lado dispositivos extremos, como as HoloLens (um dispositivo que mistura conteúdo gerado por computador com o mundo real), e do outro uma exibição completamente imersiva de Realidade virtual (como visto com o headset do Windows Mixed Reality). Consulte [Tipos de aplicativos de realidade misturada](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps) para obter exemplos de como as experiências poderão variar.

Quase todos os aplicativos UWP existentes serão executados no ambiente de realidade misturada, como aplicativos 2D sem alterações, embora a experiência do usuário possa ser melhorada seguindo algumas das diretrizes neste tópico.

![Visualização da realidade misturada](images/MR-01.png)

Tanto as HoloLens quanto os headsets do Windows Mixed Reality oferecem suporte a aplicativos sendo executados na plataforma UWP, e ambos suportam dois tipos de experiência distintos. 

### <a name="2d-vs-immersive-experience"></a>2D comparado a experiência imersiva

Um aplicativo imersivo assume a tela inteira visível ao usuário, posicionando-o no centro de uma visualização criada pelo aplicativo. Por exemplo, um jogo imersivo pode colocar o usuário na superfície de um planeta alienígena ou um aplicativo de tour guia pode colocar o usuário em uma vila da América do Sul. Criar um aplicativo imersivo requer elementos gráficos 3D ou vídeo estereográfico capturado. Aplicativos imersivos geralmente são desenvolvidos usando um mecanismo de jogo de terceiros como Unity ou com DirectX.

Se você estiver criando aplicativos imersivos, você deve visitar o [Centro de desenvolvimento do Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) para obter mais informações.

Um aplicativo 2D é executado como uma janela tradicional simples em modo de exibição do usuário. Nas HoloLens, isso significa uma exibição fixada na parede ou em um ponto no espaço na sala de estar ou escritório dos usuários. Em um headset do Windows Mixed Reality, o aplicativo está fixado em uma parede no [ambiente de realidade misturada](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home) (às vezes chamado de *Casa no Penhasco*).

![Vários aplicativos em execução em realidade misturada](images/MR-multiple.png)


Esses aplicativos 2D não tomam a exibição inteira: eles são colocados dentro dela. Vários aplicativos 2D podem existir no ambiente de uma só vez.

O restante deste tópico aborda considerações de design para a experiência 2D.

## <a name="launching-2d-apps"></a>Iniciando aplicativos 2D

![Menu Iniciar de realidade misturada](images/MR-start-options.png)

Todos os apps são iniciados a partir do Menu Iniciar, mas também é possível criar um objeto 3D para atuar como um iniciador de aplicativo. Veja [esse vídeo](https://www.youtube.com/watch?v=TxIslHsEXno) para obter detalhes.

## <a name="the-2d-app-input-overview"></a>A visão geral de entrada do aplicativo 2D

Teclados e mouses são suportados nas plataformas HoloLens e de realidade misturada. Você pode emparelhar um teclado e mouse diretamente com as HoloLens via Bluetooth. Aplicativos de realidade misturada oferecem suporte a mouse e teclado conectados ao computador host. Ambos podem ser úteis em situações onde um nível de controle fino é necessário.

Também há suporte para outros métodos de entrada mais naturais, e eles podem ser particularmente úteis quando o usuário não estiver sentado em uma mesa com um teclado real em sua frente ou quando controle fino é necessário.

Sem nenhuma codificação ou hardware extra, os aplicativos usarão focar - o vetor que seu usuário está vendo - como um ponteiro de mouse ao trabalhar com aplicativos 2D. Ele é implementado como se um ponteiro do mouse estivesse passando sobre algo na cena virtual.

Em uma interação típica, seu usuário olhará um controle em seu aplicativo, fazendo-o realçar. O usuário então disparará uma ação usando um gesto (nas HoloLens) ou um controlador, ou fornecendo um comando de voz. Se o usuário selecionar um campo de entrada de texto, o teclado de software será exibido. 


![O teclado pop-up na realidade misturada](images/MR-keyboard.png)

É importante observar que todas essas interações ocorrerão automaticamente com nenhum código extra de sua parte, como consequência de executar na plataforma UWP. Entrada de HoloLens e headset de realidade misturada aparecerá como entrada touch ao aplicativo 2D. Isso significa que muitos aplicativos UWP serão executados e poderão ser usados na realidade misturada, por padrão. 

Dito isso, com algum trabalho extra, é possível melhorar consideravelmente a experiência. Por exemplo, [controle de voz](https://developer.microsoft.com/windows/mixed-reality/voice_design) pode ser especialmente eficaz. Os ambientes das HoloLens e de realidade misturada dão suporte a comandos de voz para iniciar e interagir com aplicativos, e incluem suporte de voz que aparecerá como uma extensão natural dessa abordagem. Consulte [Interações de controle por voz]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions) para obter mais informações sobre como adicionar suporte de voz ao seu aplicativo UWP. 


### <a name="selecting-the-right-controller"></a>Selecionando o controlador correto

![Controladores de movimento de realidade misturada](images/MR-controllers.png)

Vários métodos de entrada novos foram projetados principalmente para uso com de realidade misturada, especificamente:

* [Gestos de mão](https://developer.microsoft.com/windows/mixed-reality/gestures) (HoloLens somente, mas apenas usado para iniciar aplicativos 2D)
* [Suporte de Gamepad](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (ambos ambientes)
* [Dispositivo clicker](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories) (HoloLens somente)
* [Controladores de movimento](https://developer.microsoft.com/windows/mixed-reality/motion_controllers) (dispositivos de realidade misturada somente, mostrados acima.)

Esses controladores fazem a interação com objetos virtuais se tornar natural e precisa. Algumas das interações você obtém gratuitamente. Por exemplo, o HoloLens seleciona gesto ou clicando na tecla Windows do controlador de movimento ou gatilho gerará a resposta de entrada esperada, novamente, sem codificação de sua parte.

Em outras ocasiões, você desejará adicionar código para tirar proveito das informações extras e entradas que são disponibilizadas. Por exemplo, os controladores de movimento podem ser usados para manipular objetos com um nível de controle fino, se você escrever um código que leva em consideração sua posição e pressionamento de botão.

> [!NOTE]
> Em resumo: a orientação principal deve ser sempre para fornecer ao usuário o método de entrada mais natural e sem atrito possível.


## <a name="2d-app-design-considerations-functionality"></a>Considerações de design de aplicativo 2D: funcionalidade

Ao criar um aplicativo UWP que possivelmente será usado em uma plataforma de realidade misturada, há várias coisas para se ter em mente.

* Arrastar e soltar podem não funcionar bem quando usados com controladores de movimento, consoles ou gestos. Se seu aplicativo depende muito de arrastar e soltar, você precisará fornecer um método alternativo para dar suporte a essa ação, como apresentar uma caixa de diálogo confirmando se objetos devem ser movidos para um novo local.

* Fique atento com a mudança de som. Se seu aplicativo gera efeitos de som, a origem do som aparecerá como sendo a localização fixa do seu aplicativo no mundo virtual. Conforme o usuário move para fora do aplicativo, o som reduzirá. Consulte [Som espacial](https://developer.microsoft.com/windows/mixed-reality/spatial_sound) para obter mais informações.

* Considere o campo de visão e forneça funcionalidades. Nem todos os dispositivo fornecerão um campo tão grande de visualização quanto um monitor de computador. Consulte [Quadro holográfico](https://developer.microsoft.com/windows/mixed-reality/holographic_frame) para obter detalhes completos. Além disso, o usuário pode estar distante de um aplicativo em execução. Ou seja, o aplicativo pode aparecer fixo na parede em um local diferente do mundo (real ou virtual). Seu aplicativo pode precisar chamar a atenção dos usuários ou levar em consideração que a exibição completa não está visível em todos os momentos. Notificações do sistema estão disponíveis, mas outra maneira de obter a atenção do usuário pode ser gerar um som ou alerta de [fala](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs).

* Um aplicativo 2D recebe automaticamente uma [barra de aplicativos](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box) para permitir que o usuário mova e dimensione-a no ambiente virtual. Os modos de exibição podem ser redimensionados verticalmente ou redimensionados mantendo a mesma taxa de proporção.


## <a name="2d-app-design-considerations-uiux"></a>Considerações de design de aplicativo 2D: UI/UX

* Controles XAML que implementam o [Sistema de Design Fluente](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/), como o [Modo de exibição de navegação](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview) e efeitos como [Acrílico](https://docs.microsoft.com/windows/uwp/design/style/acrylic) tudo funciona especialmente bem nos aplicativos de realidade misturada 2D.

* Teste o tamanho do texto e das janelas do aplicativo em um dispositivo de realidade misturada, ou no mínimo, no simulador de realidade misturada. Seu aplicativo terá um tamanho de janela padrão de 853 x 480 pixels efetivos. Use tamanhos de fonte maiores (recomenda-se um tamanho de ponto de aproximadamente 32) e leia [Atualizando seu aplicativo universal existente para as Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens). O artigo [Tipografia](https://developer.microsoft.com/windows/mixed-reality/typography) aborda esse tópico em detalhes. Ao trabalhar no Visual Studio, há um configuração de editor de design do XAML para um aplicativo 2D de HoloLens de 57 pol., que fornece uma exibição com a escala e dimensões corretas. 

![Texto exibido em aplicativos de realidade misturada deve ser grande.](images/MR-text.png)

* [Seu foco é o mouse](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting). Quando o usuário analisa algo, ele atua como um evento de **foco por toque**, então simplesmente olhar para um objeto poderá disparar um pop-up de forma inadvertida ou outras interações indesejadas. Talvez seja necessário detectar se o aplicativo está sendo executado na realidade misturada e alterar esse comportamento. Consulte **Suporte de tempo de execução** abaixo. 

* Quando um usuário foca em direção a algo ou aponta com um controlador de movimento, um evento **foco por toque** ocorrerá. Isso consiste em uma **PointerPoint** onde **PointerType** é **Touch**, mas **IsInContact** é **false**. Quando ocorre alguma forma de confirmação (por exemplo, botão A do gamepad é pressionado, um dispositivo clicker é pressionado, um gatilho de controlador de movimento é pressionado, ou o reconhecimento de voz coordena "Selecionar"), um **pressionamento de toque** ocorre, com o **PointerPoint** tendo **IsInContact** se torna **true**. Consulte [Interações de toque](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions) para obter mais informações sobre esses eventos de entrada.

* Lembre-se, focar não é tão preciso quanto apontar com o mouse. Destinos ou botões de mouse menores podem causar frustração em seus usuários, então redimensione os controles de acordo. Se eles são projetados para toque, funcionarão na realidade misturada, mas você pode decidir ampliar alguns botões em tempo de execução. Consulte [Atualizando seu aplicativo universal existente para as Hololens](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens).

* As HoloLens definem a cor preta como ausência de luz. Ela simplesmente não é processada, permitindo que o "mundo real" se mostre através dela. Seu aplicativo não deve usar preto porque isso pode causar confusão. Em um headset de realidade misturada, preto é preto.

* As HoloLens não oferecem suporte a temas de cores em aplicativos, e padroniza como azul para garantir a melhor experiência para os usuários. Para obter mais conselhos sobre como selecionar cores, você deve consultar [este tópico](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials), que aborda o uso de cor e material em designs de realidade misturada.


## <a name="other-points-to-consider"></a>Outros pontos que devem ser considerados

* Embora a [Ponte da Desktop](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) possa ajudar a trazer aplicativos de área de trabalho (Win32) existentes ao Windows 10 e a Microsoft Store, ela não pode criar aplicativos que rodem em HoloLens ou em realidade misturada nesse momento.




## <a name="runtime-support"></a>Suporte de tempo de execução

É possível para seu aplicativo determinar se ele está em execução em um dispositivo de realidade misturada no tempo de execução e usá-lo como uma oportunidade para redimensionar controles ou, de outra forma, otimizar o aplicativo para uso em um headset.

Aqui está um breve trecho de código que redimensiona o texto dentro de um controle TextBlock XAML somente se o aplicativo está sendo usado em um dispositivo de realidade misturada.

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>Artigos relacionados


* [Limitações atuais para aplicativos que usam APIs do shell](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [Criando aplicativos 2D](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens: criação de aplicativos 2D de UWP para Microsoft HoloLens](https://channel9.msdn.com/Events/Build/2016/B854)
* [XAML condicional](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


