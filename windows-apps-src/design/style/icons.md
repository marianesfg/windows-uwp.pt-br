---
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Ícones
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0f881c470d92ea1eafb08f24e6cf1fd06f4fc75e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7855474"
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP

![Imagem de cabeçalho do ícone](images/icons/header-icons.png)

Os ícones fornecem uma abreviação visual para uma ação, um conceito ou um produto. Ao compactar o significado em uma imagem simbólica, os ícones podem transpor barreiras de idioma e ajudar a conservar um recurso extremamente valioso: o espaço de tela. 

Ícones podem ser exibidos em aplicativos e fora deles: 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::row-end:::

Este artigo descreve os ícones no seu aplicativo. Para saber mais sobre ícones fora do aplicativo (ícones de aplicativos), veja o [artigo de ícones de aplicativo e bloco](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quando usar os ícones

Ícones podem economizar espaço, mas quando você deve usá-los? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Usar o tipo correto de ícone

Há muitas maneiras de criar um ícone. Você pode usar uma fonte de símbolo como Segoe MDL2 Assets. Você pode criar sua própria imagem com base em vetor. Você ainda pode usar uma imagem de bitmap, embora isso não seja recomendado. Veja um resumo das diferentes maneiras de adicionar um ícone ao aplicativo. 

### <a name="use-a-predefined-icon"></a>Use um ícone predefinido.
:::row:::
    :::column:::
        Microsoft provides over 1000 icons in the form of the Segoe MDL2 Assets font. It might not be intuitive to get an icon from a font, but our font display technology means these icons will look crisp and sharp on any display, at any resolution, and at any size. 
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Use uma fonte.
:::row:::
    :::column:::
        You don't have to use the Segoe MDL2 Assets font--you can use any font the user has installed on their system, such as Wingdings or Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Use um arquivo de Elementos gráficos vetoriais escaláveis (SVG).
:::row:::
    :::column:::
        SVG resources are ideal for icons, because they always look sharp at any size or resolution. Most drawing applications can export to SVG. 
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Use objetos geométricos.
:::row:::
    :::column:::
        Like SVG files, geometries are a vector-based resource, so they always look sharp. However, creating a geometry is complicated because you have to individually specify each point and curve. It's really only a good choice if you need to modify the icon while your app is running (to animate it, for example). For instructions, see [Move and draw commands for geometries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Você também pode usar uma imagem bitmap, como PNG, GIF ou JPEG, embora isso não seja recomendado.
:::row:::
    :::column:::
        Bitmap images are created at a specific size, so they have to be scaled up or down depending on how large you want the icon to be and the resolution of the screen. When the image is scaled down (shrunk), it can appear blurry; when it's scaled up, it can appear blocky and pixelated. If you have to use a bitmap image we recommend using a PNG or GIF over a JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Faça o ícone fazer algo

Quando você tiver um ícone, a próxima etapa é fazê-lo realizar algo, associando-o com o comando ou uma ação de navegação. A melhor maneira de fazer isso é adicionar o ícone a um botão ou uma barra de comandos. 

![Imagem da barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Criar um botão de ícone

Você pode colocar um ícone em um botão padrão. Como você pode usar botões em vários lugares, isso confere um pouco mais de flexibilidade de exibição para o ícone de ação. 

Existem algumas maneiras de adicionar um ícone a um botão:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
        Set the button's font family to `Segoe MDL2 Assets` and its content property to the unicode value of the glyph you want to use:
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
        You can use one of the icon element objects: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon),
        [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), 
        [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), or
        [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). This gives you more types of icons to choose from, and enables you to combine icons and other types of content, such as text, if you want:
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Criar uma série de ícones em uma barra de comandos

:::row:::
    :::column span:::
        When you have a series of commands that go together, such as cut/copy/paste or a set of drawing commands for a photo-editing program, put them together in a [command bar](../controls-and-patterns/app-bars.md). A command bar takes one or more app bar buttons or app bar toggle buttons, each of which represents an action. Each button has an [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) property you use to control which icon it displays. There are a variety of ways to specify the icon. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

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
