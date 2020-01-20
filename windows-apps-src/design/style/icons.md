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
ms.openlocfilehash: e30e9b2bed5cb4c0b7876ff1c597bb7d1243008a
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684161"
---
# <a name="icons-for-uwp-apps"></a>Ícones para aplicativos UWP

![Imagem de cabeçalho de ícones](images/icons/header-icons.png)

Os ícones fornecem uma abreviação visual para uma ação, um conceito ou um produto. Ao compactar o significado em uma imagem simbólica, os ícones podem transpor barreiras de idioma e ajudar a conservar um recurso extremamente valioso: o espaço na tela. 

Ícones podem ser exibidos dentro e fora de aplicativos: 

:::row:::
    :::column:::
        **Ícones dentro do aplicativo**

        ![ícones dentro do aplicativo](images/icons/inside-icons.png) Dentro de seu aplicativo, use ícones para representar uma ação, como copiar o texto ou navegar até a página de configurações.
    :::column-end:::
    :::column:::
**Ícones fora do aplicativo**

        ![ícones fora do aplicativo](images/icons/outside-icons.jpg) Fora do seu aplicativo, o Windows usa um ícone para representá-lo no menu Iniciar e na barra de tarefas. Se o usuário optar por fixar o seu aplicativo no menu Iniciar, o bloco de início do seu aplicativo pode apresentar o ícone do aplicativo. O ícone do seu aplicativo aparece na barra de título e você pode optar por criar uma tela inicial com o logotipo do aplicativo.
    :::column-end:::
:::row-end:::

Este artigo descreve os ícones no seu aplicativo. Para saber mais sobre ícones fora do aplicativo (ícones de aplicativos), veja o [artigo sobre ícones de aplicativo e bloco](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Quando usar os ícones

Ícones podem economizar espaço, mas quando você deve usá-los? 

:::row:::
    :::column:::
        ![o que fazer](images/do.svg) ![imagem padrão dos ícones](images/icons/icons-standard.svg)<br>

Use ícones para ações, como recortar, copiar, colar e salvar, ou para itens de navegação em um menu de navegação.
    :::column-end:::
    :::column:::
        ![o que não fazer](images/dont.svg) ![imagem de conceito dos ícones](images/icons/icons-concept.svg)<br>

Use um ícone se já existir um para o conceito que deseja representar. (Para ver se existe um ícone, verifique a lista de ícones da Segoe.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![o que fazer](images/do.svg) ![carrinho de compras de ícone](images/icons/icon-shopping-cart.svg)<br>

Use um ícone se for fácil para o usuário entender o que ele significa e se for simples o bastante para ser entendido em tamanhos pequenos.
    :::column-end:::
    :::column:::
        ![o que não fazer](images/dont.svg) ![imagem de conceito dos ícones](images/icons/icon-bad-example.png)<br>

Não use um ícone se o significado não está claro, ou se para que fique claro seja necessário usar uma forma complexa.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Usar o tipo correto de ícone

Há muitas maneiras de criar um ícone. É possível: Usar uma fonte de símbolo como Segoe MDL2 Assets. Criar sua própria imagem com base em vetor. Usar uma imagem de bitmap, embora isso não seja recomendado. Veja um resumo das diferentes maneiras de adicionar um ícone ao aplicativo. 

### <a name="use-a-predefined-icon"></a>Use um ícone predefinido.
:::row:::
    :::column:::
A Microsoft fornece mais de 1.000 ícones na forma da fonte Segoe MDL2 Assets. Talvez não seja intuitivo obter um ícone a partir de uma fonte, mas nossa tecnologia de exibição de fonte proporciona a nitidez ideal em qualquer tela, em qualquer resolução e em qualquer tamanho. Para obter instruções, confira [Ícones Segoe MDL2](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![imagem de ícone predefinida](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Use uma fonte.
:::row:::
    :::column:::
Você não precisa usar a fonte Segoe MDL2 Assets, é possível usar qualquer fonte que o usuário tenha instalada no sistema, como Wingdings ou Webdings.
    :::column-end:::
    :::column:::
        ![imagem Wingdings](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Use um arquivo de Elementos Gráficos Vetoriais Escaláveis (SVG).
:::row:::
    :::column:::
Os recursos SVG são ideais para ícones, pois sempre têm aparência nítida em qualquer tamanho ou resolução. A maioria dos aplicativos de desenho pode exportar para SVG. Para obter instruções, confira [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![imagem SVG](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Use objetos geométricos.
:::row:::
    :::column:::
Como os arquivos SVG, objetos geométricos são um recurso baseado em vetor, então sempre têm aparência nítida. No entanto, é complicado criar um objeto geométrico, pois você precisa especificar individualmente cada ponto e curva. É uma boa opção somente se você precisar modificar o ícone enquanto o aplicativo está em execução (para animá-lo, por exemplo). Para ver instruções, confira [Mover e desenhar comandos para objetos geométricos](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Imagem de objetos geométricos](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>Também é possível usar uma imagem bitmap, como PNG, GIF ou JPEG, embora isso não seja recomendado.
:::row:::
    :::column:::
As imagens bitmap são criadas com um tamanho específico para poderem ser dimensionadas dependendo do tamanho desejado do ícone e da resolução da tela. Quando a imagem é redimensionada (reduzida), pode parecer desfocada; quando é ampliada, pode parecer irregular e pixelada. Se precisa usar uma imagem bitmap, recomendamos usar PNG ou GIF ao invés de JPEG. 
    :::column-end:::
    :::column:::
        ![o que não fazer](images/dont.svg) ![imagem Bitmap](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Faça o ícone fazer algo

Depois que você tiver um ícone, a próxima etapa é fazê-lo realizar algo, associando-o a uma ação de comando ou navegação. A melhor maneira para fazer isso é adicionar o ícone a um botão ou uma barra de comando. 

![Imagem da barra de comando](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Criar um botão de ícone

Você pode colocar um ícone em um botão padrão. Como é possível usar botões em vários lugares, isso confere um pouco mais de flexibilidade de exibição para o ícone de ação. 

Existem algumas maneiras de adicionar um ícone a um botão:

:::row:::
    :::column span="2":::
        <b>Etapa 1</b><br>
Defina a família de fontes do botão como `Segoe MDL2 Assets` e sua propriedade de conteúdo como o valor Unicode do glifo que deseja usar:
    :::column-end:::
    :::column:::
        ![Etapa 1 de criação de um botão de ícone](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Etapa 2</b><br>
É possível usar um dos objetos de elemento de ícone: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) ou [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Isso oferece mais tipos de ícones para escolher e permite combinar ícones e outros tipos de conteúdo, como texto, se você quiser:
    :::column-end:::
    :::column:::
        ![Etapa 2 de criação de um botão de ícone](images/icons/icon-text-step-2.svg)
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

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Criar uma série de ícones em uma barra de comando

:::row:::
    :::column span:::
Quando se tem uma série de comandos que funcionam juntos, como cortar/copiar/colar ou um conjunto de comandos de desenho para um programa de edição de fotos, coloque-os juntos em uma [barra de comando](../controls-and-patterns/app-bars.md). Uma barra de comando usa um ou mais botões ou botões de alternância da barra de aplicativos, cada um representando uma ação. Cada botão tem uma propriedade [Icon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que você pode usar para controlar qual ícone é exibido. Existem diversas maneiras de especificar o ícone. 
    :::column-end:::
    :::column:::
        ![Exemplo de uma barra de comandos com ícones](images/icons/create-icon-command-bar.svg)
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
Para ver a lista completa de nomes de ícone, confira a [enumeração Symbol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Há outras maneiras de fornecer ícones para um botão em uma barra de comando:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon): o ícone tem por base um glifo de uma família de fontes especificada.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): o ícone tem por base um arquivo de imagem bitmap com um **Uri** especificado.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): o ícone tem por base os dados de [Path](/uwp/api/windows.ui.xaml.shapes.path).

Para saber mais sobre as barras de comandos, confira o [artigo sobre a barra de comando](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Artigos relacionados

* [Diretrizes para ativos de bloco e ícone](../shell/tiles-and-notifications/app-assets.md)
