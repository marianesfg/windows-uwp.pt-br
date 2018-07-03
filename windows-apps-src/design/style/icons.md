---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Ícones
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983569"
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP

![Imagem de cabeçalho do ícone](images/icons/header-icons.png)

Os ícones fornecem uma abreviação visual para uma ação, um conceito ou um produto. Ao compactar o significado em uma imagem simbólica, os ícones podem transpor barreiras de idioma e ajudar a conservar um recurso extremamente valioso: o espaço de tela. 

Ícones podem ser exibidos em aplicativos e fora deles: 

::: linha:::::: coluna::: **Ícones no aplicativo**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::fim da linha:::

Este artigo descreve os ícones no seu aplicativo. Para saber mais sobre ícones fora do aplicativo (ícones de aplicativos), veja o [artigo de ícones de aplicativo e bloco](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quando usar os ícones

Ícones podem economizar espaço, mas quando você deve usá-los? 

:::linha::: :::coluna::: ![fazer](images/do.svg) ![imagem padrão dos ícones](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::fim da linha:::

:::linha::: :::coluna::: ![fazer](images/do.svg) ![ícone de carrinho de compras](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::fim da linha:::



## <a name="using-the-right-type-of-icon"></a>Usar o tipo correto de ícone

Há muitas maneiras de criar um ícone. Você pode usar uma fonte de símbolo como Segoe MDL2 Assets. Você pode criar sua própria imagem com base em vetor. Você ainda pode usar uma imagem de bitmap, embora isso não seja recomendado. Veja um resumo das diferentes maneiras de adicionar um ícone ao aplicativo. 

### <a name="use-a-predefined-icon"></a>Use um ícone predefinido.
:::linha::: :::coluna::: A Microsoft fornece mais de 1000 ícone na forma da fonte Segoe MDL2 Assets. Talvez não seja intuitivo obter um ícone a partir de uma fonte, mas nossa tecnologia de exibição de fonte proporciona a nitidez ideal em qualquer tela, em qualquer resolução e em qualquer tamanho. :::final da coluna::: :::coluna::: ![imagem ícone pré-definida](images/icons/predefined-icon.png) :::fim da coluna::: :::fim da linha:::

### <a name="use-a-font"></a>Use uma fonte.
:::linha::: :::coluna::: Você não precisa usar a fonte Segoe MDL2 Assets, é possível usar qualquer fonte que o usuário instalou no sistema, como Wingdings ou Webdings.
:::final da coluna::: :::coluna::: ![imagem de wingdings](images/icons/wingdings.png) :::fim da coluna::: :::fim da linha:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Use um arquivo de Elementos gráficos vetoriais escaláveis (SVG).
:::linha::: :::coluna::: os recursos SVG são ideais para ícones, pois sempre têm aparência nítida em qualquer tamanho ou resolução. A maioria dos aplicativos de desenho pode exportar para SVG. :::final da coluna::: :::coluna::: ![imagem de SVG](images/icons/icon-scale.gif) :::fim da coluna::: :::fim da linha:::

### <a name="use-geometry-objects"></a>Use objetos geométricos.
:::linha::: :::coluna::: Como os arquivos SVG, as geometrias são um recurso com base em vetor para que fiquem sempre nítidos. No entanto, é complicado criar um objeto geométrico, pois você precisa especificar individualmente cada ponto e curva. É uma boa ideal somente se você precisar modificar o ícone enquanto o aplicativo está em execução (para animá-lo, por exemplo). Para ver instruções, consulte [Mover e desenhar comandos para geometrias](../../xaml-platform/move-draw-commands-syntax.md). :::final da coluna::: :::coluna::: ![imagem de objetos geométricos](images/icons/geometry-objects.png) :::fim da coluna::: :::fim da linha:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Você também pode usar uma imagem bitmap, como PNG, GIF ou JPEG, embora isso não seja recomendado.
:::linha::: :::coluna::: As imagens bitmap são criada com um tamanho específico para poderem ser dimensionadas dependendo do tamanho desejado do ícone e da resolução da tela. Quando a imagem é redimensionada (reduzida), ela pode aparecer desfocada; Quando ele for ampliada, ela pode aparecer irregular e pixelada. Se precisa usar uma imagem bitmap, recomendamos usar um PNG ou GIF em vez de JPEG. :::final de coluna::: :::coluna::: ![não fazer](images/dont.svg) ![Imagem Bitmap](images/icons/bitmap-image.png) :::final da coluna::: :::final da linha:::

## <a name="make-the-icon-do-something"></a>Faça o ícone fazer algo

Depois que você tiver um ícone, a próxima etapa é fazê-lo realizar algo, associando-o a uma ação de comando ou navegação. A melhor maneira para fazer isso é adicionar o ícone a um botão ou uma barra de comandos. 

![Imagem da barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Criar um botão de ícone

Você pode colocar um ícone em um botão padrão. Como você pode usar botões em vários lugares, isso confere um pouco mais de flexibilidade de exibição para o ícone de ação. 

Existem algumas maneiras de adicionar um ícone a um botão:

:::linha::: :::extensão da coluna="2"::: <b>Etapa 1</b><br>
        Defina a família de fontes do botão como `Segoe MDL2 Assets` e a propriedade do conteúdo para o valor unicode do glifo que você deseja usar: :::fim da coluna::: :::coluna::: ![Etapa 1 Criar um botão de ícone](images/icons/create-icon-step-1.svg) :::fim da coluna::: :::fim da linha:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::linha::: :::extensão da coluna="2"::: <b>Etapa 2</b><br>
        Você pode usar um dos objetos do elemento de ícone: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) ou [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Isso oferece mais tipos de ícones para você escolher e permite combinar ícones e outros tipos de conteúdo, como texto, se você quiser: ::::fina da coluna::: :::coluna::: ![Etapa 2 Criar um botão de ícone](images/icons/icon-text-step-2.svg) :::final da coluna::: :::final da linha:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Criar uma série de ícones em uma barra de comandos

:::linha::: :::extensão da coluna:::Quando você tem uma série de comandos que funcionam juntos, como cortar/copiar/colar ou um conjunto de comandos de desenho para um programa de edição de fotos, coloque-os juntos em uma [barra de comando](../controls-and-patterns/app-bars.md). Uma barra de comandos usa um ou mais botões da barra de aplicativos ou botões de alternância da barra de aplicativos, cada um representando uma ação. Cada botão tem uma propriedade de [Ícone](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que você pode usar para controlar qual ícone é exibido. Existem diversas maneiras de especificar o ícone. :::final da coluna::: :::coluna::: ![Exemplo de uma barra de comando com ícones](images/icons/create-icon-command-bar.svg) :::fim da coluna::: :::fim da linha:::

A maneira mais fácil é usar a lista de ícones predefinidos fornecida: apenas especifique o nome do ícone, como "Voltar" ou "Parar", e o sistema o desenhará: 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
Para ver a lista completa de nomes de ícone, consulte a [Enumeração de símbolo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Há outras maneiras de fornecer ícones para um botão em uma barra de comandos:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon) - o ícone tem por base um glifo de uma família de fontes especificada.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon) - o ícone tem por base um arquivo de imagem bitmap com um **Uri** especificado.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) - o ícone tem por base os dados de [Caminho](/uwp/api/windows.ui.xaml.shapes.path).

Para saber mais sobre as barras de comandos, consulte o [artigo de barra de comandos](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes de ativos de bloco e ícone](../shell/tiles-and-notifications/app-assets.md)
