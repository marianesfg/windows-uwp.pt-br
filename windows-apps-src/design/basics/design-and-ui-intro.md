---
author: mijacobs
Description: The universal design features included in every UWP app help you build apps that scale beautifully across a range of devices.
title: Introdução ao design de aplicativos UWP (Plataforma Universal do Windows) (aplicativos do Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: mijacobs
ms.date: 05/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 952db87d0dabdb927a472de17f0c0d7b345bde4e
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3382474"
---
# <a name="introduction-to-uwp-app-design"></a>Introdução ao design de aplicativos UWP

![aplicativo de iluminação de exemplo](images/introUWP-header.jpg)

As orientações sobre design da Plataforma Universal do Windows (UWP) são um recurso que ajuda você a projetar e criar aplicativos belos e refinados.

Não é uma lista de regras prescritivas – é um documento dinâmico, projetado para se adaptar ao nosso [Sistema Design Fluente](../fluent-design-system/index.md) em evolução, bem como às necessidades de nossa comunidade de criação de aplicativos.

Esta introdução fornece uma visão geral dos recursos de design universais que são incluídos em todos os aplicativos UWP, ajudando você a criar interfaces de usuário (UI) que se adequam perfeitamente a vários dispositivos.

## <a name="effective-pixels-and-scaling"></a>dimensionamento e pixels efetivos

Primeiro, os aplicativos UWP funcionam em todos os [dispositivos Windows 10](../devices/index.md), da sua TV ao seu tablet ou computador. Como isso afeta a interface do usuário do aplicativo?

![mesmo aplicativo em vários dispositivos](images/universal-image-1.jpg)

Felizmente, os aplicativos UWP ajustam automaticamente o tamanho de elementos da interface do usuário para que eles sejam legíveis e a interação com eles seja fácil em todos os dispositivos e tamanhos de tela.

Quando seu aplicativo é executado em um dispositivo, o sistema usa um algoritmo para normalizar a forma como os elementos da interface do usuário são exibidos na tela. Esse algoritmo de dimensionamento leva em conta a distância de visualização e a densidade da tela (pixels por polegada) para otimizar o tamanho percebido (em vez do tamanho físico). O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a 3 m de distância seja tão legível para o usuário quanto uma fonte de 24 px no telefone de 5" a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/scaling-chart.png)

Devido ao modo como o sistema de dimensionamento funciona, ao criar seu aplicativo UWP, você está criando em pixels efetivos, não pixels físicos reais. Pixels efetivos (epx) são uma unidade virtual de medição, e eles são usados para expressar dimensões de layout e espaçamento, independente da densidade de tela. (Em nossas diretrizes, epx, ep e px são intercambiáveis.)

Você pode ignorar a densidade de pixels e a resolução de tela real ao projetar. Crie o projeto para obter a resolução efetiva (a resolução em pixels efetivos) de uma classe de tamanho (para obter detalhes, consulte o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Ao criar modelos de tela em programas de edição de imagens, defina o DPI para 72 e defina as dimensões da imagem para a resolução efetiva da classe de tamanho pretendida. Para obter uma lista de classes de tamanho e resoluções efetivas, consulte o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Múltiplos de quatro

:::row:::
    ::: extensão da coluna::: os tamanhos, margens e posições dos elementos de interface do usuário devem estar sempre em **múltiplos de 4 epx** em seus aplicativos UWP.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Layout

Como os aplicativos UWP são dimensionados automaticamente para todos os dispositivos, a criação de um aplicativo UWP para qualquer dispositivo segue a mesma estrutura. Vamos começar desde o início da interface do usuário do seu aplicativo UWP.

### <a name="windows-frames-and-pages"></a>Windows, quadros e páginas

:::row:::
    :::column:::
        Quando um aplicativo UWP é lançado em qualquer dispositivo Windows 10, ele abre em uma [janela](/uwp/api/Windows.UI.Xaml.Controls.Window) com um [quadro](/uwp/api/Windows.UI.Xaml.Controls.Frame), que pode navegar entre instâncias de [página](/uwp/api/Windows.UI.Xaml.Controls.Page) .
    :::column-end:::
    :::column:::
        ![Quadro](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        Você pode pensar da interface do usuário do seu aplicativo como uma coleção de páginas. Cabe a você decidir o que deve ir em cada página e as relações entre elas.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Layout de página

Como essas páginas devem se parecer? A maioria das páginas segue uma estrutura comum para oferecer consistência, permitindo que os usuários naveguem facilmente entre as páginas do seu aplicativo. As páginas normalmente contêm três tipos de elementos da interface do usuário:

- Os elementos de [Navegação](navigation-basics.md) ajudam os usuários a selecionar o conteúdo que desejam exibir.
- O elementos de [Comando](commanding-basics.md) iniciam ações, como manipulação, gravação ou compartilhamento de conteúdo.
- Os elementos de [Conteúdo](content-basics.md) exibem o conteúdo do aplicativo.

![Um padrão comum de layout](../layout/images/page-components.svg)

Para saber mais sobre como implementar padrões comuns de aplicativo UWP, consulte o artigo de [Layout de página](../layout/page-layout.md).

Você também pode usar o [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) no Visual Studio para começar com um layout para seu aplicativo.

## <a name="controls"></a>Controles

A plataforma de design da UWP fornece um conjunto de controles comuns que funcionam em todos os dispositivos do Windows e seguem os princípios do nosso [Sistema de design fluente](../fluent-design-system/index.md). Esses controles incluem tudo, desde controles simples, como botões e elementos de texto, até controles sofisticados que podem gerar listas de um conjunto de dados e um modelo.

![controles UWP](../style/images/color/windows-controls.svg)

Para obter uma lista completa de controles UWP e os padrões que você pode criar com base neles, consulte a [seção controles e padrões](../controls-and-patterns/index.md).

## <a name="style"></a>Estilo

Os controles comuns automaticamente refletem o tema do sistema tema e a cor de destaque, trabalha com todos os tipos de entrada e dimensiona para todos os dispositivos. Assim, eles refletem o Sistema de design fluente: são adaptáveis, fáceis de usar e com ótima aparência. Os controles comuns usam iluminação, movimento e profundidade nos estilos padrão, portanto, ao usá-los, você incorpora nosso Sistema de design fluente ao seu aplicativo.

Os controles comuns são altamente personalizáveis: você pode alterar a cor de primeiro plano de um controle ou personalizar completamente sua aparência. Para substituir os estilos padrão nos controles, use o [estilo leve](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou crie os [controles personalizados](../controls-and-patterns/control-templates.md) em XAML.

![gif de cor de destaque](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
        Seu aplicativo UWP interage com a experiência mais ampla do Windows com blocos e notificações no [Shell](../shell/tiles-and-notifications/creating-tiles.md)do Windows.

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entradas

:::row:::
    :::column:::
        Aplicativos UWP dependem de interações inteligentes. Você pode criar uma interação de clique sem precisar saber nem definir se o clique vem de um mouse, de uma caneta, ou do toque de um dedo. No entanto, você também pode criar seus aplicativos para [modos de entrada específicos](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![entradas](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Dispositivos

![dispositivos](../layout/images/size-classes.svg)

Da mesma forma, enquanto a UWP dimensiona automaticamente seu aplicativo para diferentes dispositivos, você também pode [otimizar seu aplicativo UWP para dispositivos específicos](../devices/index.md).

## <a name="usability"></a>Usabilidade

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Por último, mas não menos importante, a usabilidade significa tornar a experiência de seu aplicativo aberta para todos os usuários. Todos podem se beneficiar de experiências do usuário realmente inclusivas - consulte [usabilidade para aplicativos UWP](../usability/index.md) para ver como tornar seu aplicativo fácil de usar para todos.

Se estiver projetando para um público internacional, confira [Globalização e localização](../globalizing/globalizing-portal.md).

Você também pode querer considerar [os recursos de acessibilidade](../accessibility/accessibility-overview.md) para usuários com visão, audição e mobilidade limitada. Se a acessibilidade estiver integrada no seu design desde o início, então [tornar seu aplicativo acessível](../accessibility/accessibility-in-the-store.md) deve levar muito pouco tempo e esforço extra.

## <a name="tools-and-design-toolkits"></a>Ferramentas e kits de ferramentas de design

Agora que você sabe sobre os recursos de design básico, que tal começar a criação de seu aplicativo UWP?

Fornecemos uma variedade de ferramentas para ajudá-lo no processo de projeção:

- Consulte nossa [página de kits de ferramentas de design](../downloads/index.md) para ter acesso a kits de ferramentas do XD, Illustrator, Photoshop, Framer e Sketch, bem como ferramentas de design adicionais e download de fontes.

- Para configurar sua máquina para que ela escreva códigos para aplicativos UWP, consulte nosso artigo [Introdução &gt; Prepare-se para começar](../../get-started/get-set-up.md).

- Para obter inspiração sobre como implementar a interface do usuário para UWP, dê uma olhada em nossos [aplicativos UWP de exemplo](https://developer.microsoft.com/windows/samples) completos.

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>A seguir: o Sistema de Design Fluente

Se você quiser saber mais sobre os princípios por trás do Design Fluente (sistema de design da Microsoft) e ver mais recursos que você pode incorporar em seu aplicativo UWP, continue para o [sistema de Design Fluente](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artigos relacionados

- [O que é um aplicativo UWP?](../../get-started/universal-application-platform-guide.md)
- [Sistema de Design Fluente](../fluent-design-system/index.md)
- [Visão geral da plataforma XAML](../../xaml-platform/index.md)
