---
Description: Os recursos de design universais inclusos em todos os aplicativos UWP ajudam a compilar aplicativos que se adaptam perfeitamente em diversos dispositivos.
title: Introdução ao design de aplicativos UWP (Plataforma Universal do Windows) (aplicativos do Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2b0f5918b240bf5c28e49f2ede6f10dbeefcbbfc
ms.sourcegitcommit: e13f06042a28a8455a211b8693a009098e150cd1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68522096"
---
# <a name="introduction-to-uwp-app-design"></a>Introdução ao design de aplicativos UWP

![exemplo de aplicativo de iluminação](images/introUWP-header.jpg)

As orientações sobre design da UWP (Plataforma Universal do Windows) são um recurso que ajuda você a projetar e criar aplicativos belos e refinados.

Não é uma lista de regras limitadoras, é um documento dinâmico, projetado para se adaptar ao nosso [Sistema de Design Fluente](/windows/apps/fluent-design-system), que está em constante evolução, bem como às necessidades de nossa comunidade de criação de aplicativos.

Esta introdução fornece uma visão geral dos recursos de design universais inclusos em todos os aplicativos UWP, ajudando na criação de interfaces de usuário (IU) que se adequam perfeitamente a vários dispositivos.

## <a name="effective-pixels-and-scaling"></a>Dimensionamento e pixels eficazes

Os aplicativos UWP funcionam em todos os [dispositivos Windows 10](../devices/index.md), na TV, no tablet ou no computador. Então como projetar uma interface de usuário que tenha aparência satisfatória em uma enorme variedade de dispositivos e tamanhos de tela?

![o mesmo aplicativo em vários dispositivos](images/universal-image-1.jpg)

A UWP ajuda ao ajustar automaticamente os elementos da interface do usuário para que sejam legíveis e fáceis de interagir em todos os dispositivos e tamanhos de tela.

Quando seu aplicativo é executado em um dispositivo da plataforma Windows, o sistema usa um algoritmo para normalizar a forma como os os elementos da interface do usuário são exibidos na tela. Esse algoritmo de dimensionamento leva em conta a distância de visualização e a densidade da tela (pixels por polegada) para otimizar o tamanho percebido (em vez do tamanho físico). O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a aproximadamente 3 m de distância seja tão legível para o usuário quanto uma fonte de 24 px no telefone com tela de 5 polegadas a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/scaling-chart.png)

Devido ao modo como o sistema de dimensionamento funciona, ao criar seu aplicativo UWP, você está criando em pixels efetivos, não em pixels físicos reais. Pixels efetivos (epx) são uma unidade virtual de medição, e são usados para expressar espaçamento e dimensões de layout, independentemente da densidade de tela. (Em nossas diretrizes, epx, ep e px são intercambiáveis.)

Você pode ignorar a densidade de pixels e a resolução de tela real ao projetar. Crie o projeto para obter a resolução efetiva (a resolução em pixels efetivos) de uma classe de tamanho (para obter detalhes, consulte o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Ao criar modelos de tela em programas de edição de imagens, defina o DPI para 72 e defina as dimensões da imagem para a resolução efetiva da classe de tamanho pretendida. Para obter uma lista de classes de tamanho e resoluções eficazes, confira o artigo [Tamanhos de tela e pontos de interrupção](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Múltiplos de quatro

:::row:::
    :::column span:::
Os tamanhos, margens e posições dos elementos de interface do usuário devem estar sempre em **múltiplos de 4 epx** em aplicativos UWP.

A UWP é dimensionada em uma variedade de dispositivos com níveis de ajuste de dimensionamento de 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350% e 400%. A unidade base é 4 porque é o único inteiro que pode ser dimensionado por números não inteiros (por exemplo, 4 * 1,5 = 6). O uso de múltiplos de quatro alinha todos os elementos da interface do usuário com pixels inteiros e fazem os elementos da interface do usuário terem bordas nítidas. (Observe que esse requisito não se aplica ao texto; o texto pode ter qualquer tamanho e posição.)
    :::column-end:::
    :::column:::
![grade](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Layout

Como os aplicativos UWP são dimensionados automaticamente para todos os dispositivos, a criação de um aplicativo UWP para qualquer dispositivo segue a mesma estrutura. Vamos começar desde o início da interface do usuário do seu aplicativo UWP.

### <a name="windows-frames-and-pages"></a>Janelas, quadros e páginas

:::row:::
    :::column:::
Quando um aplicativo UWP é lançado em qualquer dispositivo Windows 10, ele é iniciado em uma [Janela](/uwp/api/windows.ui.xaml.window) com um [Quadro](/uwp/api/windows.ui.xaml.controls.frame), que pode navegar entre instâncias de [Página](/uwp/api/windows.ui.xaml.controls.page).
    :::column-end:::
    :::column:::
![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
É possível pensar na interface do usuário do aplicativo como uma coleção de páginas. Cabe a você decidir o que deve ir em cada página e as relações entre elas.

Para saber como é possível organizar suas páginas, confira [Noções básicas sobre navegação](navigation-basics.md).
    :::column-end:::
    :::column:::
![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Layout de página

Como deve ser a aparência dessas páginas? A maioria das páginas segue uma estrutura comum para oferecer consistência, permitindo que os usuários naveguem facilmente entre as páginas do seu aplicativo. As páginas normalmente contêm três tipos de elementos da interface do usuário:

- Os elementos [Navigation](navigation-basics.md) ajudam os usuários a escolher o conteúdo que desejam exibir.
- Os elementos [Command](commanding-basics.md) iniciam ações, como manipulação, gravação ou compartilhamento de conteúdo.
- Os elementos [Content](content-basics.md) exibem o conteúdo do aplicativo.

![Um padrão comum de layout](../layout/images/page-components.svg)

Para saber mais sobre como implementar padrões comuns de aplicativo UWP, confira o artigo [Layout de página](../layout/page-layout.md).

Também é possível usar o [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) no Visual Studio para começar com um layout para seu aplicativo.

## <a name="controls"></a>Controles

A plataforma de design da UWP fornece um conjunto de controles comuns que funciona em todos os dispositivos do Windows e segue os princípios do nosso [Sistema de Design Fluente](/windows/apps/fluent-design-system). Esses controles incluem tudo, desde controles simples, como botões e elementos de texto, a controles sofisticados que podem gerar listas a partir de um conjunto de dados e um modelo.

![Controles da UWP](../style/images/color/windows-controls.svg)

Para obter uma lista completa de controles UWP e dos padrões que você pode criar a partir deles, confira a [seção controles e padrões](../controls-and-patterns/index.md).

## <a name="style"></a>Estilo

Os controles comuns refletem automaticamente o tema e a cor de destaque do sistema, funcionam com todos os tipos de entrada e dimensionam para todos os dispositivos. Assim, eles refletem o Sistema de Design Fluente: são adaptáveis, fáceis de usar e têm ótima aparência. Os controles comuns usam iluminação, movimento e profundidade nos estilos padrão, portanto, ao usá-los, você incorpora nosso Sistema de Design Fluente ao seu aplicativo.

Os controles comuns são altamente personalizáveis: você pode alterar a cor de primeiro plano de um controle ou personalizar completamente sua aparência. Para substituir os estilos padrão nos controles, use o [estilo leve](../controls-and-patterns/xaml-styles.md#lightweight-styling) ou crie os [controles personalizados](../controls-and-patterns/control-templates.md) em XAML.

![Gif da cor de destaque](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
Seu aplicativo UWP interage com a experiência mais ampla do Windows com blocos e notificações no [Shell](../shell/tiles-and-notifications/creating-tiles.md) do Windows.

Os blocos são exibidos no menu Iniciar e quando seu aplicativo é iniciado e fornecem uma prévia do que está acontecendo em seu aplicativo. A energia deles vem do conteúdo por trás deles e da inteligência e habilidade com a qual são oferecidos.

Os aplicativos UWP têm quatro tamanhos de bloco (pequeno, médio, largo e grande) que podem ser personalizados com o ícone e a identidade do aplicativo. Para obter diretrizes sobre design de blocos para seu aplicativo UWP, consulte [Diretrizes para blocos e ativos de ícones](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
![blocos no menu Iniciar](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entradas

:::row:::
    :::column:::
Os aplicativos UWP dependem de interações inteligentes. É possível criar uma interação de clique sem precisar saber nem definir se o clique vem de um mouse, de uma caneta ou do toque de um dedo. No entanto, você também pode criar seus aplicativos para [modos de entrada específicos](../input/input-primer.md).
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

Por último, mas não menos importante, a usabilidade significa tornar a experiência de seu aplicativo acessível para todos os usuários. Todos podem se beneficiar de experiências do usuário realmente inclusivas, confira [usabilidade para aplicativos UWP](../usability/index.md) para ver como tornar seu aplicativo fácil de usar para todos.

Se estiver projetando para um público internacional, confira [Globalização e localização](../globalizing/globalizing-portal.md).

Considere também os [recursos de acessibilidade](../accessibility/accessibility-overview.md) para usuários com visão, audição e mobilidade limitada. Se a acessibilidade estiver integrada ao seu design desde o início, então [tornar seu aplicativo acessível](../accessibility/accessibility-in-the-store.md) deve demandar pouco tempo e esforço adicional.

## <a name="tools-and-design-toolkits"></a>Ferramentas e kits de ferramentas de design

Agora que você sabe sobre os recursos de design básicos, que tal começar a criar seu aplicativo UWP?

Fornecemos uma variedade de ferramentas para ajudá-lo no processo de design:

- Confira nossa [página de kits de ferramentas de design](../downloads/index.md) para ter acesso aos kits de ferramentas do XD, Illustrator, Photoshop, Framer e Sketch, além de ferramentas de design adicionais e baixar fontes.

- Para configurar sua máquina para que ela escreva códigos para aplicativos UWP, confira nosso artigo [Introdução e prepare-se para começar](../../get-started/get-set-up.md).

- Para obter inspiração sobre como implementar a interface do usuário para UWP, dê uma olhada em nossos [exemplos de aplicativos UWP](https://developer.microsoft.com/windows/samples) completos.

## <a name="video-summary"></a>Resumo do vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>Próximo: Sistema de Design Fluente

se você quiser saber mais sobre os princípios por trás do Design Fluente (sistema de design da Microsoft) e ver mais recursos que podem ser incorporados em seu aplicativo UWP, vá para o [Sistema de Design Fluente](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artigos relacionados

- [O que é um aplicativo UWP?](../../get-started/universal-application-platform-guide.md)
- [Sistema de Design Fluent](/windows/apps/fluent-design-system)
- [Visão geral da plataforma XAML](../../xaml-platform/index.md)
