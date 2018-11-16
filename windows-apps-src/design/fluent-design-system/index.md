---
description: Conheça o Design Fluente e como incorporá-lo nos apps.
title: Sistema de Design fluente para Windows
author: mijacobs
keywords: layout do aplicativo uwp, plataforma universal do Windows, design do app, interface, sistema de design fluent
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ab8e8c18a0b1db0991bf470f194f8774f2357b4
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974661"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Os criadores de aplicativo do sistema de Design fluente para Windows

![Cabeçalho de Design fluente](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Introdução

O sistema de Design fluente é nosso sistema para criar adaptáveis, abrangentes e interfaces do usuário bonitas.

## <a name="principles"></a>Princípios

**Adaptável: Experiências de Fluent parecem naturais em cada dispositivo**

Experiências de Fluent se adaptam ao ambiente. Uma experiência de Fluent parece confortável em um tablet, um computador desktop e um Xbox — também funciona muito bem em um headset de realidade misturada. E quando você adiciona mais hardware, como um monitor adicional para o seu computador, uma experiência de Fluent tira proveito dele.

**Compreensivo: Experiências de Fluent são intuitivas e potentes**

Experiências de Fluent se ajustam ao comportamento e intenção&mdash;elas compreendem e preveem o que é necessário. Elas unem pessoas e ideias, sejam em lados opostos do mundo ou estando um de frente ao outro.

**Lindo: Experiências de Fluent são envolventes e imersivas**

Ao incorporar elementos do mundo físico, uma experiência de Fluent toca em algo fundamental. Ele usa luz, sombra, movimento, profundidade e textura para organizar as informações de maneira que parece intuitiva e instintiva.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Aplicação de Design fluente ao seu aplicativo com UWP

![Logotipo do design fluente](images/fluentdesign_header.png)

Nossas diretrizes de design explicam como aplicar os princípios de Design fluente para aplicativos. Que tipo de aplicativos? Embora muitas das nossas diretrizes podem ser aplicadas a qualquer plataforma, criamos UWP (plataforma Universal do Windows) para dar suporte ao Design fluente.

Recursos de Design Fluente estão integrados ao UWP. Alguns desses recursos&mdash;como pixels efetivos e o sistema de entrada universal&mdash;são automáticos. Você não precisa escrever nenhum código extra para utilizá-los. Outras características, como o acrílico, são opcionais; você as inclui em seu aplicativo escrevendo um código para incluí-los.

> Estamos trazendo os controles da UWP até a área de trabalho para que você possa melhorar a aparência e a funcionalidade dos aplicativos atuais do WPF ou do Windows com recursos de Design Fluente. Para saber mais, consulte [controles de Host UWP em aplicativos WPF e Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Além de diretrizes de design, nossos artigos de Design fluente também mostram como escrever o código que faz com que seus designs de acontecer. UWP usa XAML, um idioma com base em marcação que torna mais fácil criar interfaces do usuário. Veja um exemplo:

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="24,16"/>
</Grid>
```

![](images/xaml-example.png)


> Se você for iniciante no desenvolvimento de UWP, Confira nosso [começar a usar página UWP](https://developer.microsoft.com/windows/apps/getstarted).

## <a name="find-a-natural-fit"></a>Encontrar uma opção natural

Como fazer com que um aplicativo pareça natural em uma variedade de dispositivos? Ao fazer isso, parece que ele foi projetado com cada dispositivo específico em mente. Um layout de interface do usuário que se adapte a diferentes tamanhos de tela&mdash;para que não haja desperdício de espaço (e também nenhuma lotação)&mdash;torna uma experiência natural, como se ela fosse projetada para esse dispositivo.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Torná-lo intuitiva

Uma experiência parece intuitiva quando ele se comporta da maneira que o usuário espera. Usando controles estabelecidos e padrões e tirar proveito do suporte da plataforma para globalização e acessibilidade, você cria uma experiência sem esforço que ajuda os usuários a serem mais produtivos.

Demonstrar empatia tem a ver com fazer a coisa certa no momento certo.

Experiências de Fluent usam controles e padrões de forma consistente, para que eles se comportem de maneiras que o usuário aprendeu a esperar. Experiências de Fluent são acessíveis para pessoas com uma ampla variedade de características físicas e incorporam recursos de globalização para que pessoas em todo o mundo possam usá-los.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Seja envolvente e imersivo

Design Fluente não tem a ver com efeitos extravagantes. Ele incorpora efeitos físicos que realmente melhoram a experiência do usuário, porque simulam experiências que nossos cérebros estão programados para processar de maneira eficiente.

## <a name="use-light"></a>Use luz

A luz atrai nossa atenção. Ela cria atmosfera e um senso de lugar, e é uma ferramenta prática para iluminar a informação.

Adicione luz a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Criar uma sensação de profundidade

Vivemos em um mundo tridimensional. Ao incorporar intencionalmente a profundidade na interface do usuário, transformamos uma interface 2D plana em algo especial&mdash;algo que apresenta informações e conceitos de forma eficiente criando uma hierarquia visual. Ela reinventa o modo como as coisas se relacionam entre si em um ambiente físico com camadas

Adicione profundidade a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>Incorpore movimento

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essas sensações nos nossos projetos, levando as pessoas a fazerem a transição de uma tarefa para a outra com facilidade cinematográfica.

Adicione movimento a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Crie-o com o material correto

As coisas que nos cercam no mundo real são sensoriais e revigorantes. Elas se curvam, esticam, saltam, destroem e deslizam. Essas qualidades materiais se convertem em ambientes digitais, fazendo com que as pessoas desejem acessar e tocar nossos projetos.

Adicione material a seu aplicativo do UWP:

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de ferramentas de design e amostras de código

Quer começar a criar seus próprios aplicativos com Design Fluente? Nossos kits de ferramentas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer e esboço ajudarão a impulsionar seus designs e nossas amostras ajudarão a realizar codificação mais rápido.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









