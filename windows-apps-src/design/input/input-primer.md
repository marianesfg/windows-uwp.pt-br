---
Description: As interações do usuário no aplicativo do Windows são uma combinação de fontes de entrada e saída (como mouse, teclado, caneta, toque, Touchpad, fala, Cortana, controlador, gesto, olhar e assim por diante), juntamente com vários modos ou modificadores que habilitam experiências estendidas (incluindo a roda do mouse e botões, botões de rosca e de segundo plano, teclado de toque e
title: Cartilha de interação
ms.assetid: 73008F80-FE62-457D-BAEC-412ED6BAB0C8
label: Interaction primer
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ef3adfd192acbef45ee341b133e4133e1f1ff586
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968152"
---
# <a name="interaction-primer"></a>Cartilha de interação

![tipos de entrada do windows](images/input-interactions/icons-inputdevices03.png)

As interações do usuário no aplicativo do Windows são uma combinação de fontes de entrada e saída (como mouse, teclado, caneta, toque, Touchpad, fala, **Cortana**, controlador, gesto, olhar e assim por diante), juntamente com vários modos ou modificadores que habilitam experiências estendidas (incluindo a roda do mouse e botões, botões de rosca e de segundo plano, teclado de toque e

A UWP usa um sistema de interação contextual "inteligente" que, na maioria dos casos, elimina a necessidade de lidar individualmente com os tipos exclusivos de entrada recebidos pelo seu aplicativo. Isso inclui manipular a entrada por touch, touchpad, mouse e caneta como um tipo de ponteiro genérico para dar suporte a gestos estáticos, como tocar ou pressionar e segurar, para gestos de manipulação como deslizar para movimento panorâmico ou para renderizar tinta digital.

Familiarize-se com cada tipo de dispositivo de entrada e seus comportamentos, recursos e limitações quando combinados com determinados fatores forma. Isso pode ajudar você a decidir se os controles e as funcionalidades da plataforma são suficientes para seu aplicativo, ou exigem que você forneça experiências de interação personalizadas.

## <a name="gaze"></a>Focar

Na **Atualização de abril de 2018 para o Windows 10**, introduziu o suporte para entrada usando o foco com dispositivos de entrada com acompanhamento de movimentos de olho e cabeça. 

> [!NOTE]
> O suporte para o hardware de acompanhamento com os olhos foi introduzido no **Windows 10 Fall Creators Update** juntamente com [Controle com os olhos](https://support.microsoft.com/help/4043921/windows-10-get-started-eye-control), um recurso interno que permite usar seus olhos para controlar o ponteiro virtual, digitar com o teclado virtual e se comunicar com pessoas usando a conversão de texto em fala.

### <a name="device-support"></a>Suporte a dispositivos

- Tablet
- Computadores e notebooks

### <a name="typical-usage"></a>Uso típico

Acompanhe o foco, a atenção e a presença do usuário com base na localização e na movimentação de seus olhos. Essa nova maneira poderosa de usar e interagir com os aplicativos do Windows é especialmente útil como uma tecnologia assistencial para usuários com neuro-muscular doenças (como ALS) e outras deficiências que envolvem funções imemparelhadas de capacidade ou núcleo. Entrada com foco também oferece oportunidades atraentes para jogos (incluindo a aquisição do alvo e acompanhamento) e aplicativos de produtividade tradicionais, quiosques e outros cenários interativos onde os dispositivos de entrada tradicionais (teclado, mouse, toque) não estão disponíveis ou onde pode ser útil para liberar as mãos do usuário para outras tarefas (por exemplo, segurar bolsas de compras).

### <a name="more-info"></a>Obter mais informações

[Interações de foco e rastreamento de olhos](gaze-interactions.md)

## <a name="surface-dial"></a>Surface Dial

Na **Atualização de Aniversário do Windows 10**, introduzimos a categoria de dispositivo de entrada chamada Windows Wheel. O Surface Dial é o primeiro nessa classe de dispositivo.

### <a name="device-support"></a>Suporte a dispositivos

- Tablet
- Computadores e notebooks

### <a name="typical-usage"></a>Uso típico

Com um fator forma com base em uma ação (ou gesto) girar, o Surface Dial destina-se como um dispositivo de entrada secundário para vários tipos de mídia que complementa ou modifica a entrada de um dispositivo principal. Na maioria dos casos, o dispositivo é manipulado pela mão não dominante de um usuário durante a execução de uma tarefa com a mão dominante (por exemplo, escrita à tinta com uma caneta).

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design do Surface Dial](windows-wheel-interactions.md)

## <a name="cortana"></a>Cortana

No Windows 10, a extensibilidade da **Cortana** permite lidar com comandos de voz de um usuário e iniciar um aplicativo para executar uma única ação.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT
-   Xbox
-   HoloLens

![cortana](images/input-interactions/icons-cortana01.png)

### <a name="typical-usage"></a>Uso típico

Um comando de voz é uma fala única, definida em um arquivo VCD (Definição de Comando de Voz), direcionada a um aplicativo instalado por meio da **Cortana**. O aplicativo pode ser iniciado em primeiro ou segundo plano, dependendo do nível e da complexidade da interação. Por exemplo, comandos de voz que exigem contexto adicional ou a entrada do usuário são mais bem manipulados em primeiro plano, enquanto os comandos básicos podem ser manipulados em segundo plano.

A integração da funcionalidade básica do seu aplicativo e o fornecimento de um ponto de entrada central para o usuário realizar a maioria das tarefas sem abrir o aplicativo diretamente permitem que a **Cortana** se torne uma ligação entre seu aplicativo e o usuário. Em muitos casos, isso pode economizar muito tempo e esforço do usuário. Para saber mais, consulte [Diretrizes de design da Cortana](https://docs.microsoft.com/cortana/skills/cortana-design-guidelines).

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design da Cortana](https://docs.microsoft.com/cortana/skills/cortana-design-guidelines)
 

## <a name="speech"></a>Speech

O controle por voz é uma forma eficiente e natural para as pessoas interagirem com aplicativos. É uma maneira fácil e precisa de se comunicar com aplicativos, e permite que as pessoas sejam produtivas e se mantenham informadas em diversas situações.

O controle por voz pode complementar ou, em muitos casos, ser o tipo de entrada principal, dependendo do dispositivo do usuário. Por exemplo, dispositivos como HoloLens e Xbox não dão suporte a tipos de entrada tradicionais (além de um teclado de software em situações específicas). Em vez disso, eles dependem da entrada e saída de voz (geralmente em combinação com outros tipos de entrada não tradicionais, como foco e gesto) para a maioria das interações do usuário.

A conversão de texto em fala (também conhecida como TTS ou sintetização de voz) é usada para informar ou direcionar o usuário.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT
-   Xbox
-   HoloLens

![controle por voz](images/input-interactions/icons-speech01.png)

### <a name="typical-usage"></a>Uso típico

Há três modos de interação de fala:

**Linguagem natural**

A linguagem natural é como interagimos verbalmente com as pessoas em geral. Nossa fala varia de acordo com a pessoa e a situação, e é geralmente entendida. Quando não é, geralmente usamos palavras e uma ordem de palavras diferentes para comunicar a mesma ideia.

As interações em linguagem natural com um aplicativo são semelhantes: falamos com o aplicativo através de nosso dispositivo como se ele fosse uma pessoa e esperamos que ele entenda e reaja adequadamente.

A linguagem natural é o modo mais avançado de interação de fala, e pode ser implementada e exposta pela **Cortana**.

**Comando e controle**

Comando e controle é o uso de comandos verbais para ativar controles e funcionalidades, como clicar em um botão ou selecionar um item de menu.

Como o comando e controle é essencial para uma experiência de usuário bem-sucedida, um único tipo de entrada geralmente não é recomendado. O controle por voz geralmente é uma das várias opções de entrada para um usuário com base em suas preferências ou recursos de hardware.

**Comandos**

O método de entrada de fala mais básico. Cada expressão é convertida em texto.

O ditado normalmente é usado quando um aplicativo não precisa compreender o significado ou a intenção.

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design de controle por voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
 

## <a name="pen"></a>Caneta

Uma caneta pode servir como um dispositivo apontador com precisão de pixel, como um mouse, e é o dispositivo ideal para entrada de tinta digital.

**Observe**  que há dois tipos de dispositivos de caneta: ativos e passivos.
  -   As canetas passivas não são eletrônicas, e emulam efetivamente a entrada touch de um dedo. Elas exigem uma exibição básica do dispositivo, que reconhece a entrada com base na pressão do contato. Como os usuários geralmente repousam a mão enquanto escrevem na superfície de entrada, os dados de entrada podem ficar poluídos devido a rejeição da palma da mão bem-sucedida.
  -   As canetas ativas são eletrônicas e podem funcionar com telas de dispositivos complexas para fornecer dados de entrada muito mais extensos (incluindo passagem do mouse ou dados de proximidade) ao sistema e seu aplicativo. A rejeição da palma da mão é muito mais robusta.

Quando nos referimos a dispositivos de caneta aqui, estamos fazendo referência a canetas ativas que fornecem dados de entrada avançados e são usados principalmente para interações precisas de tinta e apontamento.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT

![caneta](images/input-interactions/icons-pen01.png)

### <a name="typical-usage"></a>Uso típico

A plataforma de tinta do Windows, juntamente com uma caneta, oferece uma maneira natural de criar anotações manuscritas, desenhos e anotações. A plataforma dá suporte a captura de dados de tinta por entrada da digitalizador, geração de dados de tinta, renderização desses dados como traços de tinta no dispositivo de saída, gerenciamento dos dados de tinta e reconhecimento de manuscrito. Além de capturar os movimentos espaciais da caneta enquanto o usuário escreve ou desenha, seu aplicativo também pode coletar informações como pressão, forma, cor e opacidade, para oferecer experiências ao usuário que se aproximam bastante do ato de desenhar em papel com caneta esferográfica, lápis ou pincel.

A caneta e o touch apresentam divergências quando o assunto é a capacidade do touch de emular a manipulação direta de elementos da interface do usuário na tela usando gestos físicos executados nesses objetos (por exemplo, passar o dedo, deslizar o dedo, arrastar, girar etc.).

Você deve fornecer comandos de interface do usuário específicos à caneta, ou funcionalidades, para dar suporte a essas interações. Por exemplo, use os botões anterior e próximo (ou + e -) para permitir que os usuários percorram as páginas de conteúdo ou girem, redimensionem e ampliem objetos.

### <a name="more-info"></a>Obter mais informações

[Diretrizes para design de caneta](https://docs.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions)
 

## <a name="touch"></a>Touch

Com o touch, os gestos físicos de um ou mais dedos podem ser usados para emular a manipulação direta de elementos da interface do usuário (por exemplo, movimento panorâmico, girar, redimensionar ou mover), como um método alternativo de entrada (semelhante ao mouse ou à caneta), ou como um método de entrada complementar (para modificar aspectos da outra entrada, como borrar um traço de tinta desenhado com uma caneta). Experiências táteis como essa podem proporcionar sensações mais naturais do mundo real aos usuários conforme eles interagem com elementos em uma tela.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT

![touch](images/input-interactions/icons-touch01.png)

### <a name="typical-usage"></a>Uso típico

O suporte para entrada touch pode variar significativamente, dependendo do dispositivo.

Alguns dispositivos não dão suporte de nenhum tipo ao touch, alguns dispositivos dão suporte a um único contato touch, enquanto outros dão suporte a multi-touch (dois ou mais contatos).

A maioria dos dispositivos que dão suporte à entrada multi-touch normalmente reconhecem dez contatos simultâneos exclusivos.

Os dispositivos Surface Hub reconhecem 100 contatos touch simultâneos exclusivos.

Em geral, o touch é:

-   Usuário único, a menos que esteja sendo usado com um dispositivo de equipe da Microsoft, como o Surface Hub, onde a colaboração é enfatizada.
-   Não restrito à orientação do dispositivo.
-   Usado para todas as interações, incluindo entrada de texto (teclado virtual) e tinta (configurado pelo aplicativo).

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design de toque](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-user-interaction)
 

## <a name="touchpad"></a>Touchpad

Um touchpad combina a entrada multi-touch indireta com a entrada de precisão de um dispositivo apontador, como um mouse. Essa combinação torna o touchpad adequado para uma interface do usuário otimizada para touch e destinos menores de aplicativos de produtividade.

### <a name="device-support"></a>Suporte a dispositivos

-   Computadores e notebooks
-   IoT

![touchpad](images/input-interactions/icons-touchpad01.png)

### <a name="typical-usage"></a>Uso típico

Os touchpads normalmente dão suporte a um conjunto de gestos touch que oferecem suporte semelhante ao touch para manipulação direta de objetos e da interface do usuário.

Devido a essa convergência de experiências de interação compatíveis com touchpads, recomendamos também que você forneça comandos ou funcionalidades de interface do usuário estilo mouse em vez de depender somente do suporte interno para entrada por toque. Forneça comandos de interface do usuário, ou funcionalidades, específicos ao touchpad para dar suporte a essas interações.

Você deve fornecer comandos de interface do usuário, ou funcionalidades, específicos ao mouse para dar suporte a essas interações. Por exemplo, use os botões anterior e próximo (ou + e -) para permitir que os usuários percorram as páginas de conteúdo ou girem, redimensionem e ampliem objetos.

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design do touchpad](https://docs.microsoft.com/windows/uwp/input-and-devices/touch-interactions)
 

## <a name="keyboard"></a>Keyboard

Um teclado é o dispositivo principal de inserção de texto, e geralmente é indispensável para pessoas portadoras de determinadas deficiências ou usuários que o consideram um método mais rápido e mais eficiente de interagir com um aplicativo.

Com o [Continuum para telefone](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN), uma nova experiência para dispositivos móveis compatíveis com o Windows 10, os usuários podem conectar seus telefones a um mouse e um teclado para fazê-los funcionar como um notebook.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT
-   Xbox
-   HoloLens

![teclado](images/input-interactions/icons-keyboard01.png)

### <a name="typical-usage"></a>Uso típico

Os usuários podem interagir com aplicativos Universais do Windows por meio de um teclado de hardware e dois teclados de software: o OSK (teclado virtual) e o teclado virtual.

O OSK é um teclado de software visual que você pode usar em vez do teclado físico para digitar e inserir dados usando toque, mouse, caneta ou outro dispositivo apontador (uma tela sensível ao toque não é necessária). O OSK é fornecido para sistemas que não têm um teclado físico ou para usuários cujos problemas de mobilidade impedem o uso de dispositivos de entrada físicos tradicionais. O OSK emula a maior parte, se não toda a funcionalidade de um teclado de hardware.

O teclado virtual é um teclado de software visual usado para entrada de texto por touch. O teclado virtual não é uma substituição ao OSK, pois é usado apenas para entrada de texto (ele não emula o teclado de hardware) e só aparece quando um campo de texto ou outro controle de texto editável é focalizado. O teclado virtual não oferece suporte a comandos de aplicativo ou do sistema.

**Observe**  que o OSK tem prioridade sobre o teclado de toque, que não será mostrado se o OSK estiver presente.

Em geral, um teclado é:

-   Usuário único.
-   Não restrito à orientação do dispositivo.
-   Usado para entrada de texto, navegação, jogabilidade e acessibilidade.
-   Sempre disponível, de forma proativa ou reativa.

### <a name="more-info"></a>Obter mais informações

[Diretrizes de design do teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
 

## <a name="mouse"></a>Mouse

Um mouse é mais adequado para aplicativos de produtividade e interfaces do usuário de alta densidade, em que as interações dos usuários exigem precisão de pixel para direcionamentos e comandos.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT

![mouse](images/input-interactions/icons-mouse01.png)

### <a name="typical-usage"></a>Uso típico

A entrada do mouse pode ser modificada com a adição de várias teclas de teclado (Ctrl, Shift, Alt e assim por diante). Essas teclas podem ser combinadas com o botão esquerdo do mouse, o botão direito do mouse, o botão de rolagem e os botões X para um conjunto de comandos expandido otimizado para o mouse. (Alguns dispositivos de mouse da Microsoft tem dois botões adicionais, conhecidos como botões X. Geralmente são usados para navegar para a frente e para trás em navegadores da Web).

Assim como a caneta, o mouse e o touch apresentam divergências quando o assunto é a capacidade do touch de emular a manipulação direta de elementos da interface do usuário na tela usando gestos físicos executados nesses objetos (por exemplo, passar o dedo, deslizar o dedo, arrastar, girar etc.).

Você deve fornecer comandos de interface do usuário, ou funcionalidades, específicos ao mouse para dar suporte a essas interações. Por exemplo, use os botões anterior e próximo (ou + e -) para permitir que os usuários percorram as páginas de conteúdo ou girem, redimensionem e ampliem objetos.

### <a name="more-info"></a>Obter mais informações

[Diretrizes para design de mouse](https://docs.microsoft.com/windows/uwp/input-and-devices/mouse-interactions)
 

## <a name="gesture"></a>Gesto

Um gesto é qualquer forma de movimento do usuário que é reconhecida como entrada para controlar ou interagir com um aplicativo. Os gestos assumem várias formas, de simplesmente usar uma mão para indicar algo na tela a padrões específicos aprendidos de movimento e longas extensões de movimento contínuo em todo o corpo. Tome cuidado ao projetar gestos personalizados porque seu significado pode variar dependendo da cultura e da localidade.

### <a name="device-support"></a>Suporte a dispositivos

-   Computadores e notebooks
-   IoT
-   Xbox
-   HoloLens

![gesto](images/input-interactions/icons-gesture01.png)

### <a name="typical-usage"></a>Uso típico

Os eventos de gesto estático são disparados depois que uma interação é concluída.

- Os eventos de gesto estático incluem Tapped, DoubleTapped, RightTapped e Holding.

Os eventos de gestos de manipulação indicam uma interação contínua. Eles começam a ser acionados quando o usuário toca em um elemento, e continuam até o usuário retirar o dedo ou a manipulação ser cancelada.

- Os eventos de manipulação incluem interações multi-touch, como zoom, panorâmica ou giro, além de interações que usam dados de inércia e velocidade, como arrastar. (As informações fornecidas pelos eventos de manipulação não identificam a interação, mas fornecem dados como posição, delta de conversão e velocidade.)

- Os eventos de ponteiro, como PointerPressed e PointerMoved, fornecem detalhes de baixo nível para cada contato por toque, incluindo o movimento do ponteiro e a capacidade de diferenciar eventos de pressionamento e liberação.

Devido à convergência de experiências de interação compatíveis com o Windows, recomendamos também que você forneça comandos ou funcionalidades de interface do usuário estilo mouse em vez de depender somente do suporte interno para entrada por toque. Por exemplo, use os botões anterior e próximo (ou + e -) para permitir que os usuários percorram as páginas de conteúdo ou girem, redimensionem e ampliem objetos.


## <a name="gamepadcontroller"></a>Gamepad/controlador

O gamepad/controlador é um dispositivo altamente especializado geralmente exclusivo para jogar. No entanto, ele também é usado para emular a entrada do teclado básico e fornece uma experiência de navegação da interface do usuário muito parecida com o teclado.

### <a name="device-support"></a>Suporte a dispositivos

-   Computadores e notebooks
-   IoT
-   Xbox

![controlador](images/input-interactions/icons-controller01.png)

### <a name="typical-usage"></a>Uso típico

Jogar e interagir com um console especializado.


## <a name="multiple-inputs"></a>Várias entradas

Acomodar o máximo possível de usuários e dispositivos e projetar seus aplicativos para funcionar com o máximo possível de tipos de entrada (gesto, controle por voz, touch, touchpad, mouse e teclado) maximiza a flexibilidade, a usabilidade e a acessibilidade.

### <a name="device-support"></a>Suporte a dispositivos

-   Telefones e phablets
-   Tablet
-   Computadores e notebooks
-   Hub de Superfície
-   IoT
-   Xbox
-   HoloLens

![várias entradas](images/input-interactions/icons-inputdevices03-vertical.png)

### <a name="typical-usage"></a>Uso típico

Assim como as pessoas usam uma combinação de voz e gestos ao se comunicar uns com os outros, vários tipos e modos de entrada também podem ser úteis ao interagir com um aplicativo. No entanto, essas interações combinadas precisam ser o mais naturais e intuitivas possível porque também podem criar uma experiência confusa.





 

 
