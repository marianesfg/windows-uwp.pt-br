---
Description: Ícones bons se harmonizam com a tipografia e com o restante da linguagem do design. Eles não misturam metáforas e comunicam apenas o que é necessário, com a máxima rapidez e simplicidade possível.
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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564542"
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP

![Imagem de cabeçalho do ícone](images/icons/header-icons.png)

Os ícones fornecem uma abreviação visual para uma ação, um conceito ou um produto. Ao compactar o significado em uma imagem simbólica, os ícones podem transpor barreiras de idioma e ajudar a conservar um recurso extremamente valioso: o espaço de tela. 

Ícones podem ser exibidos em aplicativos e fora deles: 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
Dentro de seu aplicativo, você pode usar ícones para representar uma ação, como copiar o texto ou navegando até a página de configurações.
    :::column-end:::
    :::column:::
**Ícones de fora do aplicativo**

        ![icons outside the app](images/icons/outside-icons.jpg)
Fora do seu aplicativo, o Windows usam um ícone para representar o aplicativo no menu Iniciar e na barra de tarefas. Se o usuário optar por fixar o seu aplicativo no menu Iniciar, o bloco de início do seu aplicativo pode caracterizar o ícone do aplicativo. Ícone do aplicativo aparece na barra de título e você pode optar por criar uma tela inicial com o logotipo do aplicativo.
    :::column-end:::
:::row-end:::

Este artigo descreve os ícones no seu aplicativo. Para saber mais sobre ícones fora do aplicativo (ícones de aplicativos), veja o [artigo de ícones de aplicativo e bloco](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quando usar os ícones

Ícones podem economizar espaço, mas quando você deve usá-los? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

Use um ícone para ações, como Recortar, copiar, colar e salvar ou para itens de navegação em um menu de navegação.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

Use um ícone se já existir para o conceito de que deseja representar. (Para ver se existe um ícone, verifique a lista de ícones do Segoe.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

Use um ícone se é fácil para o usuário entender o que significa o ícone e ele é simple o suficiente para ser claro em tamanhos pequenos.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

Não use um ícone se seu significado não está claro, ou se deixando claro requer uma forma complexa.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Usar o tipo correto de ícone

Há muitas maneiras de criar um ícone. Você pode usar uma fonte de símbolo como Segoe MDL2 Assets. Você pode criar sua própria imagem baseada em vetor. Você ainda pode usar uma imagem de bitmap, embora isso não seja recomendado. Veja um resumo das diferentes maneiras de adicionar um ícone ao aplicativo. 

### <a name="use-a-predefined-icon"></a>Use um ícone predefinido.
:::row:::
    :::column:::
A Microsoft fornece mais de 1000 ícones no formulário da fonte Segoe MDL2 ativos. Talvez não seja intuitivo obter um ícone a partir de uma fonte, mas nossa tecnologia de exibição de fonte proporciona a nitidez ideal em qualquer tela, em qualquer resolução e em qualquer tamanho. Para obter instruções, consulte [Segoe MDL2 ícones](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Use uma fonte.
:::row:::
    :::column:::
Você não precisa usar a fonte do Segoe MDL2 ativos – você pode usar qualquer fonte que o usuário instalou em seu sistema, como Wingdings ou Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Use um arquivo de Elementos gráficos vetoriais escaláveis (SVG).
:::row:::
    :::column:::
Recursos SVG são ideais para ícones, porque eles sempre aparecem perfeitos em qualquer tamanho ou resolução. A maioria dos aplicativos de desenho pode exportar para SVG. Para obter instruções, consulte [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Use objetos geométricos.
:::row:::
    :::column:::
Como os arquivos SVG, geometrias são um recurso baseado em vetor, para que eles sempre aparecem perfeitos. No entanto, é complicado criar um objeto geométrico, pois você precisa especificar individualmente cada ponto e curva. É uma boa ideal somente se você precisar modificar o ícone enquanto o aplicativo está em execução (para animá-lo, por exemplo). Para ver instruções, consulte [Mover e desenhar comandos para geometrias](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Você também pode usar uma imagem bitmap, como PNG, GIF ou JPEG, embora isso não seja recomendado.
:::row:::
    :::column:::
Imagens de bitmap são criadas em um tamanho específico, portanto, eles precisarão ser aumentados ou reduzidos, dependendo de quão grande que você deseja que o ícone esteja e a resolução da tela. Quando a imagem é redimensionada (reduzida), ela pode aparecer desfocada; Quando ele for ampliada, ela pode aparecer irregular e pixelada. Se precisa usar uma imagem bitmap, recomendamos usar um PNG ou GIF em vez de JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Faça o ícone fazer algo

Quando você tiver um ícone, a próxima etapa é para torná-lo a fazer algo, associando-a comando ou uma ação de navegação. A melhor maneira de fazer isso é adicionar o ícone para um botão ou uma barra de comandos. 

![Imagem da barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Criar um botão de ícone

Você pode colocar um ícone em um botão padrão. Como você pode usar botões em vários lugares, isso confere um pouco mais de flexibilidade de exibição para o ícone de ação. 

Existem algumas maneiras de adicionar um ícone a um botão:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
Defina a família de fontes do botão como `Segoe MDL2 Assets` e sua propriedade de conteúdo para o valor unicode de glifo que você deseja usar:
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
Você pode usar um dos objetos de elemento de ícone: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), ou [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Isso lhe dá mais tipos de ícones à sua escolha e permite que você combine ícones e outros tipos de conteúdo, como texto, se você quiser:
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
Quando você tem uma série de comandos que funcionam juntas, como Recortar/copiar/colar ou um conjunto de comandos para um programa de edição de fotos de desenho colocá-los juntos em uma [barra de comandos](../controls-and-patterns/app-bars.md). Uma barra de comandos usa um ou mais botões da barra de aplicativos ou botões de alternância da barra de aplicativos, cada um representando uma ação. Cada botão tem uma propriedade de [Ícone](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que você pode usar para controlar qual ícone é exibido. Existem diversas maneiras de especificar o ícone. 
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

* [Diretrizes para ativos de bloco e ícone](../shell/tiles-and-notifications/app-assets.md)
