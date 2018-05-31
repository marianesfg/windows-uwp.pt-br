---
description: Conheça o Design Fluente e como incorporá-lo nos apps.
title: Sistema de Design Fluente para aplicativos UWP
author: mijacobs
keywords: layout do aplicativo uwp, plataforma universal do Windows, design do app, interface, sistema de design fluent
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638784"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>O Sistema de Design Fluente para aplicativos UWP

## <a name="introduction"></a>Introdução

<img src="images/fluentdesign-app-header.jpg" alt=" " />

A interface do usuário está evoluindo. Ele está se expandindo para incluir novas dimensões e interfaces, de 2D a 3D e muito mais, do teclado e mouse ao foco, caneta e toque.  

O Sistema de Design Fluente é um conjunto de recursos UWP inovadores combinado a práticas recomendadas para criação de aplicativos que funcionam perfeitamente em todos os tipos de dispositivos com Windows.

É nosso sistema para a criação de interfaces do usuário adaptáveis, compreensivas e bonitas. 

**Adaptável: Experiências de Fluent parecem naturais em cada dispositivo**

Experiências de Fluent se adaptam ao ambiente. Uma experiência de Fluent parece confortável em um tablet, um computador e um Xbox&mdash;, ela também funciona muito bem em um headset de realidade misturada. E quando você adiciona mais hardware, como um monitor adicional para o seu computador, uma experiência de Fluent tira proveito dele. 

**Compreensivo: Experiências de Fluent são intuitivas e potentes**

Experiências de Fluent se ajustam ao comportamento e intenção&mdash;elas compreendem e preveem o que é necessário. Elas unem pessoas e ideias, sejam em lados opostos do mundo ou estando um de frente ao outro. 

Demonstrar empatia tem a ver com fazer a coisa certa no momento certo. 

Experiências de Fluent usam controles e padrões de forma consistente, para que eles se comportem de maneiras que o usuário aprendeu a esperar. Experiências de Fluent são acessíveis para pessoas com uma ampla variedade de características físicas e incorporam recursos de globalização para que pessoas em todo o mundo possam usá-los. 

**Lindo: Experiências de Fluent são envolventes e imersivas** 

Ao incorporar elementos do mundo físico, uma experiência de Fluent toca em algo fundamental. Ele usa luz, sombra, movimento, profundidade e textura para organizar as informações de maneira que parece intuitiva e instintiva. 

Design Fluente não tem a ver com efeitos extravagantes. Ele incorpora efeitos físicos que realmente melhoram a experiência do usuário, porque simulam experiências que nossos cérebros estão programados para processar de maneira eficiente. 

## <a name="applying-fluent-design-to-your-app"></a>Aplicação de Design Fluente ao seu aplicativo

Recursos de Design Fluente estão integrados ao UWP. Alguns desses recursos&mdash;como pixels efetivos e o sistema de entrada universal&mdash;são automáticos. Você não precisa escrever nenhum código extra para utilizá-los. Outras características, como o acrílico, são opcionais; você as inclui em seu aplicativo escrevendo um código para incluí-los. 

> Para saber mais sobre os recursos básicos que são incluídos automaticamente em cada aplicativo UWP, consulte [Introdução ao artigo de design do aplicativo UWP](../basics/design-and-ui-intro.md). Se você for iniciante no desenvolvimento de UWP, é aconselhável conferir primeiro nosso [Introdução à página do UWP](https://developer.microsoft.com/windows/apps/getstarted). 

Para saber mais sobre os novos recursos que incorporam o Design Fluente no aplicativo, continue lendo.

## <a name="find-a-natural-fit"></a>Encontrar uma opção natural

Como fazer com que um aplicativo pareça natural em uma variedade de dispositivos? Ao fazer isso, parece que ele foi projetado com cada dispositivo específico em mente. Um layout de interface do usuário que se adapte a diferentes tamanhos de tela&mdash;para que não haja desperdício de espaço (e também nenhuma lotação)&mdash;torna uma experiência natural, como se ela fosse projetada para esse dispositivo. 

*  **Design para os pontos de interrupção certos**

    Em vez de criar para cada tamanho de tela individual, focar em algumas larguras chave (também chamadas de "pontos de interrupção") pode simplificar bastante os designs e o código, além de deixar seu aplicativo com uma aparência ótima em telas pequenas a grandes.

    [Saiba mais sobre tamanhos de tela e pontos de interrupção](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **Criar um layout dinâmico**

    Para um aplicativo parecer natural, ele precisa preencher o espaço de exibição disponível sem parecer muito preenchido. UWP fornece painéis que organizam o conteúdo em fluxos, grades e pilhas, e você pode aninhá-los dentro uns dos outros.

    [Saiba mais sobre painéis de layout da UWP](/windows/uwp/design/layout/layout-panels)

* **Design para uma variedade de dispositivos**

    Aplicativos UWP podem ser executados em uma ampla variedade de dispositivos do Windows. É útil entender quais dispositivos estão disponíveis, para o que foram feitos e como os usuários interagem com eles.

    [Saiba mais sobre dispositivos UWP](/windows/uwp/design/devices/)

* **Otimizar para a entrada certa**

    Aplicativos UWP suportam automaticamente mouse, teclado, caneta e interações comuns por toque&mdash;você não precisa fazer mais nada. Mas você pode aprimorar seu aplicativo com suporte otimizado para entradas específicas, como caneta e Surface Dial.

    [Saiba mais sobre entradas e interações](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>Torne-o intuitivo e eficiente

Uma experiência parece intuitiva quando ela se comporta da forma que o usuário espera. Usando controles estabelecidos e padrões e tirar proveito do suporte da plataforma para globalização e acessibilidade, você cria uma experiência sem esforço que ajuda os usuários a serem mais produtivos. 

* **Use o controle correto para o trabalho**

    Controles são os blocos de construção da interface do usuário; usar o controle correto ajuda você a criar uma interface do usuário que se comporta da maneira que os usuários esperam.  A UWP oferece mais de 45 controles, desde botões simples a controles de dados avançados. 

    [Saiba mais sobre os controles do UWP](/windows/uwp/design/controls-and-patterns/)

* **Ser inclusivo** 

    Um aplicativo bem projetado é acessível a todas as pessoas com deficiências. Com codificação extra, você pode compartilhar seu aplicativo com pessoas ao redor do mundo.

    [Saiba mais sobre Usabilidade](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>Seja envolvente e imersivo 

Torne seu aplicativo atraente, incorporando elementos físicos, como luz e movimento. 

## <a name="use-light"></a>Use luz

A luz atrai nossa atenção. Ela cria atmosfera e um senso de lugar, e é uma ferramenta prática para iluminar a informação.
        
Adicione luz a seu aplicativo do UWP:
        
* [Revelar destaque](../style/reveal.md) usa a luz para criar elementos interativos. A luz ilumina os elementos com os quais o usuário pode interagir, revelando fronteiras ocultas. Revelação é habilitada automaticamente em alguns controles, como o modo de exibição de lista e exibição de grade. Você pode ativá-lo em outros controles pela aplicação de nossos estilos de realce de revelação predefinidos. 

* [Revelar foco](../style/reveal-focus.md) usa luz para chamar a atenção para o elemento que atualmente está em foco de entrada.  

## <a name="create-a-sense-of-depth"></a>Criar uma sensação de profundidade

Vivemos em um mundo tridimensional. Ao incorporar intencionalmente a profundidade na interface do usuário, transformamos uma interface 2D plana em algo especial&mdash;algo que apresenta informações e conceitos de forma eficiente criando uma hierarquia visual. Ela reinventa o modo como as coisas se relacionam entre si em um ambiente físico com camadas

Adicione profundidade a seu aplicativo do UWP:

* O recurso [Acrílico](../style/acrylic.md) é um material translúcido que permite ao usuário ver camadas de conteúdo, estabelecendo uma hierarquia de elementos de interface do usuário.

* O recurso [Paralaxe](../motion/parallax.md) cria a ilusão de profundidade, fazendo com que itens em primeiro plano pareçam se mover mais rapidamente do que itens na tela de fundo.

## <a name="incorporate-motion"></a>Incorpore movimento

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essas sensações nos nossos projetos, levando as pessoas a fazerem a transição de uma tarefa para a outra com facilidade cinematográfica.

Adicione movimento a seu aplicativo do UWP:

* O recurso [Animações conectadas](../motion/connected-animation.md) ajuda o usuário a manter o contexto pela criação de uma transição suave entre as cenas. 

## <a name="build-it-with-the-right-material"></a>Crie-o com o material correto

As coisas que nos cercam no mundo real são sensoriais e revigorantes. Elas se curvam, esticam, saltam, destroem e deslizam. Essas qualidades materiais se convertem em ambientes digitais, fazendo com que as pessoas desejem acessar e tocar nossos projetos.

Adicione material a seu aplicativo do UWP: 
        
* O recurso [Acrílico](../style/acrylic.md)  é um material translúcido que permite ao usuário ver camadas de conteúdo, estabelecendo uma hierarquia de elementos de interface do usuário. 

## <a name="design-toolkits-and-code-samples"></a>Kits de ferramentas de design e amostras de código

Quer começar a criar seus próprios aplicativos com Design Fluente? Nossos kits de ferramentas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer e esboço ajudarão a impulsionar seus designs e nossas amostras ajudarão a realizar codificação mais rápido.

* Confira nossa [página de exemplos e kits de ferramentas de design](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />








