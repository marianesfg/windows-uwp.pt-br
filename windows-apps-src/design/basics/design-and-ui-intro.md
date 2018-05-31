---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: Introdução ao design de aplicativos UWP (Plataforma Universal do Windows) (aplicativos do Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817274"
---
#  <a name="introduction-to-uwp-app-design"></a>Introdução ao design de aplicativos UWP

As orientações sobre design da Plataforma Universal do Windows (UWP) são um recurso que ajuda você a projetar e criar aplicativos belos e refinados.

Não é uma lista de regras prescritivas – é um documento dinâmico, projetado para se adaptar ao nosso [Sistema Design Fluente](../fluent-design-system/index.md) em evolução, bem como às necessidades de nossa comunidade de criação de aplicativos.

Esta introdução fornece uma visão geral dos recursos de design universais que são incluídos em todos os aplicativos UWP, ajudando você a criar interfaces de usuário (UI) que se adequam perfeitamente a vários dispositivos.

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>Dimensionamento e pixels efetivos

Os aplicativos UWP ajustam automaticamente o tamanho de controles, fontes e outros elementos da interface do usuário para que eles sejam legíveis e a interação com eles seja fácil em [todos os dispositivos compatíveis com aplicativos UWP](../devices/index.md).

Quando seu aplicativo é executado em um dispositivo, o sistema usa um algoritmo para normalizar a forma como os elementos da interface do usuário são exibidos na tela. Esse algoritmo de dimensionamento leva em conta a distância de visualização e a densidade da tela (pixels por polegada) para otimizar o tamanho percebido (em vez do tamanho físico). O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a 3 m de distância seja tão legível para o usuário quanto uma fonte de 24 px no telefone de 5" a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/1910808-hig-uap-toolkit-03.png)

Devido ao modo como o sistema de dimensionamento funciona, ao criar seu aplicativo UWP, você está criando em pixels efetivos, não pixels físicos reais. Pixels efetivos (epx) são uma unidade virtual de medição, e eles são usados para expressar dimensões de layout e espaçamento, independente da densidade de tela. (Em nossas diretrizes, epx, ep e px são intercambiáveis.)

Você pode ignorar a densidade de pixels e a resolução de tela real ao projetar. Crie o projeto para obter a resolução efetiva (a resolução em pixels efetivos) de uma classe de tamanho (para obter detalhes, consulte o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Ao criar modelos de tela em programas de edição de imagens, defina o DPI para 72 e defina as dimensões da imagem para a resolução efetiva da classe de tamanho pretendida. Para obter uma lista de classes de tamanho e resoluções efetivas, consulte o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).


## <a name="layout"></a>Layout

### <a name="margins-spacing-and-positioning"></a>Margens, espaçamento e posicionamento 
![grade](images/4epx.png)

Quando o sistema escalona a interface do usuário do seu aplicativo, ele faz isso em múltiplos de 4.

Como um resultado, os tamanhos, margens e posições dos **elementos de interface do usuário devem estar sempre em múltiplos de 4 epx **. Isso resulta em melhor renderização alinhando com pixels inteiros. Isso também garante que os elementos de interface do usuário tenham bordas agudas e nítidas. 

Observe que esse requisito não se aplica ao texto; o texto pode ter qualquer tamanho e posição. Para obter orientações sobre como alinhar o texto com outros elementos da interface de usuário, consulte o [Guia de tipografia UWP](../style/typography.md).

![dimensionando na grade](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>Padrões de layout

![Um padrão comum de layout](images/page-components.png)

A interface do usuário é composta por três tipos de elementos: 
1. Os **elementos de navegação** ajudam os usuários a escolher o conteúdo que desejam exibir. Consulte [noções básicas de navegação](navigation-basics.md).
2. Os **elementos de comando** iniciam ações, como manipulação, gravação ou compartilhamento de conteúdo. Consulte [Noções básicas de comando](commanding-basics.md).
3. Os **elementos de conteúdo** exibem o conteúdo do aplicativo. Consulte [noções básicas de conteúdo](content-basics.md).

### <a name="adaptive-behavior"></a>Comportamento adaptável
![comportamento adaptável para telefone e desktop](../controls-and-patterns/images/patterns_masterdetail.png)

Apesar de a interface do usuário do seu aplicativo ser legível e utilizável em todos os dispositivos do Windows, convém personalizar a interface do usuário do seu aplicativo para dispositivos específicos e tamanhos de tela. Para obter diretrizes mais específicas, consulte [tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) e [técnicas de design responsivo](../layout/responsive-design.md).

## <a name="type"></a>Tipo

Por padrão, os aplicativos UWP usam a fonte **Segoe UI** e a rampa de tipo UWP inclui sete classes de tipografia, buscando a abordagem mais eficiente em todos os tamanhos de exibição. 

![Rampa de tipos](images/type-ramp.png)

Para obter detalhes sobre a rampa de tipos, consulte o [guia de tipografia UWP ](../style/typography.md). Para saber como usar os diferentes níveis de rampa de tipos UWP em seu aplicativo, consulte [recursos de tema](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp).

## <a name="color"></a>Cor

A cor fornece uma maneira intuitiva de comunicar informações aos usuários. Ele pode ser usado para sinalizar interatividade, enviar comentários às ações do usuário, transmitir informações de estado e dar à sua interface uma noção de continuidade visual. 

O Windows 10 oferece uma paleta de cores universal compartilhada de 48 cores, que é aplicada entre os aplicativos shell e UWP. 

![paleta de cores universal do windows](images/colors.png)

O sistema aplica automaticamente a cor ao seu aplicativo UWP com a cor de destaque do sistema. Quando os usuários escolhem uma cor de destaque da paleta de cores em suas configurações, a cor aparece como parte do tema de seu sistema. Dependendo das preferências do usuário, a cor de destaque do sistema também pode ser exibida na tela inicial, nos blocos da tela inicial, na barra de tarefas e barras de título. 

![selecione a cor de destaque nas configurações](images/selectcolor.png)

Dentro de seu aplicativo UWP, os hiperlinks e estados selecionados dentro de controles comuns refletirão a cor de destaque do sistema.

![cor de destaque do sistema nos controles](images/accentcolor.png)

Aplicativos UWP podem impedir que a cor de destaque do sistema sejam exibidas nos controles usando [estilo leve](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou criando [controles personalizados](../controls-and-patterns/control-templates.md).

Para obter diretrizes adicionais sobre como usar cores em seu aplicativo UWP, consulte o artigo [Cor](../style/color.md).

### <a name="themes"></a>Temas

Os usuários podem escolher entre um tema claro, escuro, ou de alto contraste. Você pode optar por alterar a aparência do seu aplicativo com base na preferência do tema do usuário, ou recusar.

O tema claro é mais adequado para tarefas de produtividade e leitura.

O tema escuro permite um contraste mais visível em aplicativos centrados em mídia e cenários que incluem uma grande quantidade de vídeos ou imagens. Nesses cenários, é mais provável que a principal tarefa seja assistir a filmes do que leitura, e que ela aconteça em condições ambientes de baixa luminosidade.

![Mostrar calculadora em temas claros e escuros](images/light-dark.png)

O tema de alto contraste usa uma pequena paleta de cores contrastantes que deixa a interface mais fácil de ver.
![Calculadora mostrada no tema claro e no tema Preto em Alto Contraste.](../accessibility/images/high-contrast-calculators.png)


Para saber mais sobre o uso de temas e a rampa de cores UWP em seu aplicativo, veja [recursos de tema](../controls-and-patterns/xaml-theme-resources.md).

## <a name="icons"></a>Ícones
![ícones](images/icons.png)

Ícones são um tipo de linguagem visual que se tornaram parte integrante de nossas vidas diárias. Eles permitem que expressemos conceitos e ações de maneira visualmente atraente, economizemos espaço de tela e servem como uma maneira de navegar por nossa vida digital. 

Todos os aplicativos UWP têm acesso aos ícones na [fonte Segoe MDL2](../style/segoe-ui-symbol-font.md). Esses ícones dependem de formulários estabelecidos que são conhecidos e facilmente identificáveis para todos, mas eles também foram modernizados para dar a sensação de que foram feitos manualmente.

Se você quiser criar seus próprios ícones, consulte [ícones para aplicativos UWP](../style/icons.md).

## <a name="tiles"></a>Blocos
![blocos no menu Iniciar](images/tiles.png)

Os blocos são exibidos no menu Iniciar e fornecem uma prévia do que está acontecendo em seu aplicativo. A energia deles vem do conteúdo por trás deles e da inteligência e habilidade com a qual são oferecidos. 

Os aplicativos UWP têm quatro tamanhos de bloco (pequeno, médio, largo e grande) que podem ser personalizados com o ícone e a identidade do aplicativo. Para obter orientações sobre design de blocos para seu aplicativo UWP, consulte [Diretrizes para blocos e ativos de ícones](../shell/tiles-and-notifications/app-assets.md).

## <a name="controls"></a>Controles
![controle de botão](images/1910808-hig-uap-toolkit-01.png)

A UWP fornece um conjunto de controles comuns que têm garantia de funcionar bem em todos os dispositivos com Windows. Esses controles incluem tudo, desde controles simples, como botões e elementos de texto, até controles sofisticados que podem gerar listas de um conjunto de dados e um modelo.

Os controles UWP automaticamente refletem o tema do sistema tema e a cor de destaque, trabalha com todos os tipos de entrada e dimensiona para todos os dispositivos. E eles também são altamente personalizáveis, – você pode alterar a cor de primeiro plano de um controle ou personalizar completamente sua aparência. 

Para obter uma lista completa de controles UWP e os padrões que você pode criar com base neles, consulte a [seção controles e padrões](../controls-and-patterns/index.md).

## <a name="input"></a>Entrada
![entradas](../input/images/input-interactions/icons-inputdevices03.png)

Aplicativos UWP dependem de interações inteligentes. Você pode criar uma interação de clique sem precisar saber nem definir se o clique vem de um mouse, de uma caneta, ou do toque de um dedo. No entanto, você também pode criar seus aplicativos para [modos de entrada específicos e dispositivos](../input/input-primer.md).

## <a name="accessibility"></a>Acessibilidade
![pessoas de design inclusivo](images/inclusive.png)

Por último, mas não menos importante, acessibilidade significa tornar a experiência de seu aplicativo aberta para todos os usuários. Ele é relevante para todos, não apenas para pessoas com deficiências. Todos podem se beneficiar de experiências do usuário realmente inclusivas - consulte [usabilidade para aplicativos UWP](../usability/index.md) para ver como tornar seu aplicativo fácil de usar para todos. Você também pode querer considerar [os recursos de acessibilidade](../accessibility/accessibility-overview.md) para usuários com visão, audição e mobilidade limitada. 

Se a acessibilidade estiver integrada no seu design desde o início, então [tornar seu aplicativo acessível](../accessibility/accessibility-in-the-store.md) deve levar muito pouco tempo e esforço extra.

## <a name="tools-and-design-toolkits"></a>Ferramentas e kits de ferramentas de design
Agora que você sabe sobre os recursos de design básico, que tal começar a criação de seu aplicativo UWP?

Fornecemos uma variedade de ferramentas para ajudá-lo no processo de projeção:

* Consulte nossa [página de kits de ferramentas de design](../downloads/index.md) para ter acesso a kits de ferramentas do XD, Illustrator, Photoshop, Framer e Sketch, bem como ferramentas de design adicionais e download de fontes. 

* Para configurar sua máquina para que ela escreva códigos para aplicativos UWP, consulte nosso artigo [Introdução &gt; Prepare-se para começar](../../get-started/get-set-up.md). 

* Para obter inspiração sobre como implementar a interface do usuário para UWP, dê uma olhada em nossos [aplicativos UWP de exemplo](https://developer.microsoft.com/windows/samples) completos.

## <a name="next-fluent-design-system"></a>A seguir: o Sistema de Design Fluente
Se você quiser saber mais sobre os princípios por trás do Design Fluente (sistema de design da Microsoft) e ver mais recursos que você pode incorporar em seu aplicativo UWP, continue para o [sistema de Design Fluente](../fluent-design-system/index.md).