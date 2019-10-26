---
description: Conheça o Design Fluente e como incorporá-lo a seus aplicativos.
title: Sistema de Design Fluente para Windows
keywords: layout do aplicativo uwp, plataforma universal do Windows, design do aplicativo, interface, sistema de design fluente
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: f3a2575b17cc4228d7c4db273845478aecf65f29
ms.sourcegitcommit: f0936ce8e88d78b1af99998794a8765094f6a487
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72915102"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Os criadores de aplicativos do Sistema de Design Fluente para Windows

![Cabeçalho de Design Fluente](images/fluent/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Introdução

O sistema de Design Fluente é nosso sistema para criar interfaces do usuário adaptáveis, empáticas e bonitas.

## <a name="principles"></a>Princípios

**Adaptável: as experiências fluentes sentem-se naturais em cada dispositivo**

Experiências Fluent se adaptam ao ambiente. Uma experiência Fluent parece confortável em um tablet, um computador desktop e um Xbox – ela também funciona muito bem em um headset de realidade misturada. E quando você adiciona mais hardware, como um monitor adicional para o seu computador, uma experiência Fluent tira proveito dele.

**Empática: as experiências fluentes são intuitivas e poderosas**

Experiências Fluent ajustam-se ao comportamento e intenção&mdash;elas compreendem e preveem o que é necessário. Elas unem pessoas e ideias, sejam em lados opostos do mundo ou estando um de frente ao outro.

**Linda: experiências fluentes estão envolventes e envolventes**

Ao incorporar elementos do mundo físico, uma experiência Fluent toca em algo fundamental. Ele usa luz, sombra, movimento, profundidade e textura para organizar as informações de maneira que parece intuitiva e instintiva.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Aplicando o Design Fluente a seu aplicativo com a UWP

![Logotipo do Design Fluente](images/fluent/fluentdesign_header.png)

Nossas diretrizes de design explicam como aplicar os princípios de Design Fluente a aplicativos. Que tipo de aplicativos? Embora muitas das nossas diretrizes possam ser aplicadas a qualquer plataforma, criamos a UWP (Plataforma Universal do Windows) para dar suporte ao Design Fluente.

Recursos de Design Fluente estão integrados ao UWP. Alguns desses recursos&mdash;como pixels efetivos e o sistema de entrada universal&mdash;são automáticos. Você não precisa escrever nenhum código extra para utilizá-los. Outras características, como o acrílico, são opcionais; você as adiciona a seu aplicativo escrevendo um código para incluí-los.

> Estamos trazendo os controles da UWP até a área de trabalho para que você possa melhorar a aparência e a funcionalidade dos aplicativos atuais do WPF ou do Windows com recursos de Design Fluente. Para saber mais, veja [Hospedar controles UWP em aplicativos WPF e Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Além das orientações de design, nossos artigos de Design Fluente também mostram como escrever o código que faz com que seus designs se concretizem. A UWP usa XAML, uma linguagem baseada em marcação que torna mais fácil criar interfaces do usuário. Aqui está um exemplo:

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![](images/fluent/xaml-example.png)


> Se você não tem experiência em desenvolvimento de UWP, confira nossa [página de Introdução à UWP](/windows/uwp/get-started).

## <a name="find-a-natural-fit"></a>Encontrar uma opção natural

Como fazer com que um aplicativo pareça natural em uma variedade de dispositivos? Ao fazer isso, parece que ele foi projetado com cada dispositivo específico em mente. Um layout de interface do usuário que se adapte a diferentes tamanhos de tela&mdash;para que não haja desperdício de espaço (e também nenhuma lotação)&mdash;torna uma experiência natural, como se ela fosse projetada para esse dispositivo.

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**Design para os pontos de interrupção corretos**

Em vez de criar para cada tamanho de tela individual, focar em algumas larguras chave (também chamadas de "pontos de interrupção") pode simplificar bastante os designs e o código, além de deixar seu aplicativo com uma aparência ótima em telas pequenas a grandes.

[Saiba mais sobre tamanhos de tela e pontos de interrupção](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**Criar um layout responsivo**

Para que um aplicativo se sinta natural, ele deve adaptar seu layout a diferentes tamanhos de tela e dispositivos. Você pode usar dimensionamento automático, painéis de layout, Estados visuais e até mesmo definições de interface do usuário separadas em XAML para criar uma interface do usuário responsiva.

[Saiba mais sobre o design responsivo](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**Design de um espectro de dispositivos**

Aplicativos UWP podem ser executados em uma ampla variedade de dispositivos do Windows. É útil entender quais dispositivos estão disponíveis, para o que foram feitos e como os usuários interagem com eles.

[Saiba mais sobre os dispositivos UWP](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**Otimizar para a entrada correta**

Aplicativos UWP suportam automaticamente mouse, teclado, caneta e interações comuns por toque&mdash;você não precisa fazer mais nada. Mas você pode aprimorar seu aplicativo com suporte otimizado para entradas específicas, como caneta e Surface Dial.

[Saiba mais sobre entradas e interações](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Torne-a intuitiva

Uma experiência parece intuitiva quando ela se comporta da forma que o usuário espera. Usando controles estabelecidos e padrões e tirar proveito do suporte da plataforma para globalização e acessibilidade, você cria uma experiência sem esforço que ajuda os usuários a serem mais produtivos.

Demonstrar empatia tem a ver com fazer a coisa certa no momento certo.

Experiências Fluent usam controles e padrões de forma consistente, para que eles se comportem das maneiras que o usuário aprendeu a esperar. Experiências Fluent são acessíveis para pessoas com uma ampla variedade de características físicas e incorporam recursos de globalização para que pessoas em todo o mundo possam usá-los.

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**Fornecer a navegação à direita**

Crie uma experiência sem esforço usando a estrutura de aplicativo e os componentes de navegação corretos.

[Saiba mais sobre navegação](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**Ser interativo**

Botões, barras de comando, atalhos de teclado e menus de contexto permitem que os usuários interajam com seu aplicativo; Elas são as ferramentas que alteram uma experiência estática em algo dinâmico.

[Saiba mais sobre a linha de comando](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**Usar o controle certo para o trabalho**

Controles são os blocos de construção da interface do usuário; usar o controle correto ajuda você a criar uma interface do usuário que se comporta da maneira que os usuários esperam. A UWP oferece mais de 45 controles, desde botões simples a controles de dados avançados.

[Saiba mais sobre os controles UWP](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![imagem inclusiva](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**Ser inclusivo** Um aplicativo bem projetado pode ser acessado por pessoas com deficiências. Com codificação extra, você pode compartilhar seu aplicativo com pessoas ao redor do mundo.

[Saiba mais sobre a usabilidade](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Seja envolvente e imersivo

Design Fluente não tem a ver com efeitos extravagantes. Ele incorpora efeitos físicos que realmente melhoram a experiência do usuário, porque simulam experiências que nossos cérebros estão programados para processar de maneira eficiente.

## <a name="use-light"></a>Usar luz

A luz atrai nossa atenção. Ela cria atmosfera e um senso de lugar, além de ser uma ferramenta prática para iluminar a informação.

Adicione luz a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**Realce de revelação**

[Revelar destaque](/windows/uwp/design/style/reveal) usa a luz para criar elementos interativos. A luz ilumina os elementos com os quais o usuário pode interagir, revelando fronteiras ocultas. Revelação é habilitada automaticamente em alguns controles, como o modo de exibição de lista e exibição de grade. Você pode ativá-lo em outros controles pela aplicação de nossos estilos de realce de revelação predefinidos.
:::row-end:::

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**Foco de revelação**

[Revelar foco](/windows/uwp/design/style/reveal-focus) usa luz para chamar a atenção para o elemento que atualmente está em foco de entrada.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Criar uma sensação de profundidade

Vivemos em um mundo tridimensional. Ao incorporar intencionalmente a profundidade na interface do usuário, transformamos uma interface 2D plana em algo especial&mdash;algo que apresenta informações e conceitos de forma eficiente criando uma hierarquia visual. Ela reinventa o modo como as coisas se relacionam entre si em um ambiente físico com camadas

Adicione profundidade a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**Paralaxe**

O recurso [Paralaxe](/windows/uwp/design/motion/parallax) cria a ilusão de profundidade, fazendo com que itens em primeiro plano pareçam se mover mais rapidamente do que itens na tela de fundo.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporar movimento

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essas sensações nos nossos projetos, levando as pessoas a fazerem a transição de uma tarefa para a outra com facilidade cinematográfica.

Adicione movimento a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![gif de continuidade](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**Animações conectadas**

O recurso [Animações conectadas](/windows/uwp/design/motion/connected-animation) ajuda o usuário a manter o contexto pela criação de uma transição suave entre as cenas.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Crie-o com o material correto

As coisas que nos cercam no mundo real são sensoriais e revigorantes. Elas se curvam, alongam-se, saltam, estilhaçam-se e deslizam. Essas qualidades materiais se convertem em ambientes digitais, fazendo com que as pessoas desejem acessar e tocar nossos projetos.

Adicione material a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrílico**

O recurso [Acrílico](/windows/uwp/design/style/acrylic) é um material translúcido que permite ao usuário ver camadas de conteúdo, estabelecendo uma hierarquia de elementos de interface do usuário.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de ferramentas de design e amostras de código

Quer começar a criar seus próprios aplicativos com Design Fluente? Nossos kits de ferramentas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer e esboço ajudarão a impulsionar seus designs e nossas amostras ajudarão a realizar codificação mais rápido.

:::row:::
    :::column:::
        ![imagem FPO](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**Página de exemplos de kits de desenvolvimento e amostras**

Confira nossa [página de exemplos e kits de ferramentas de design](/windows/uwp/design/downloads/)
:::row-end:::









