---
author: Jwmsft
Description: Provides a list by function of some of the controls that you can use in your apps.
title: Controles por função
ms.assetid: 8DB4347B-91D6-4659-91F2-80ECF7BBB596
label: Controls by function
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50d2d5d6dd53ffcb14ed6223e2fd0f85324a8438
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5754893"
---
# <a name="controls-by-function"></a>Controles por função

A estrutura da IU XAML para Windows fornece uma biblioteca extensiva de controles que dão suporte ao desenvolvimento da interface do usuário. Alguns desses controles têm uma representação visual, outros funcionam como contêineres de outros controles ou conteúdos, como imagens e mídias. 

Você também pode ver vários controles da IU do Windows em ação baixando o [Amostra de noções básicas de interface do usuário XAML](http://go.microsoft.com/fwlink/p/?LinkId=619992).

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/NavigationView">Abrir o aplicativo e ver o NavigationView em ação</a> </p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>


Aqui está uma lista por função dos controles XAML comuns que você pode usar em seu aplicativo.

## <a name="appbars-and-commands"></a>Appbars e comandos

### <a name="app-bar"></a>Barra de aplicativos
Uma barra de ferramentas para exibir comandos específicos a aplicativos. Consulte Barra de comandos.

Referência: [AppBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbar.aspx) 

### <a name="app-bar-button"></a>Botão da barra de aplicativos
Um botão para mostrar os comandos usando a definição da barra de aplicativos.

![Ícones dos botões da barra de aplicativos](images/controls/app-bar-buttons.png) 

Referência: [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [SymbolIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.symbolicon.aspx), [BitmapIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.bitmapicon.aspx), [FontIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.fonticon.aspx), [PathIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pathicon.aspx) 

Design e instruções: [Guia de controle da barra de aplicativos e barra de comandos](app-bars.md) 

Código de amostra: [amostra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-separator"></a>Separador da barra de aplicativos
Visualmente separa grupos de comandos em uma barra de comandos.

Referência: [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) 

Código de amostra: [Amostra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="app-bar-toggle-button"></a>Botão de alternância da barra de aplicativos
Um botão para alternar comandos em uma barra de comandos.

Referência: [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx) 

Código de amostra: [amostra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

### <a name="command-bar"></a>Barra de comandos
Uma barra de aplicativos especializada que lida com o redimensionamento de elementos de botão da barra de aplicativos.

![Controle de barra de comandos](images/command-bar-compact.png)

```xaml
<CommandBar>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
</CommandBar>
```
Referência: [CommandBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.commandbar.aspx) 

Design e instruções: [Guia de controle da barra de aplicativos e barra de comandos](app-bars.md)

Código de amostra: [Amostra de comandos XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="buttons"></a>Botões

### <a name="button"></a>Botão
Um controle que responde a uma entrada de usuário e dispara um evento **Click**.

![Um botão padrão](images/controls/button.png)

```xaml
<Button x:Name="button1" Content="Button" 
        Click="Button_Click" />
```

Referência: [Button](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx) 

Design e instruções: [Guia de controle de botões](buttons.md) 

### <a name="hyperlink"></a>Hyperlink
Consulte o botão Hiperlink.

### <a name="hyperlink-button"></a>Botão Hiperlink
Um botão que aparece como texto marcado e abre o URI especificado em um navegador.

![Botão Hiperlink](images/controls/hyperlink-button.png)

```xaml
<HyperlinkButton Content="www.microsoft.com" 
                 NavigateUri="http://www.microsoft.com"/>
```

Referência: [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.hyperlinkbutton.aspx) 

Design e instruções: [Guia do controle de Hyperlinks](hyperlinks.md)

### <a name="repeat-button"></a>Botão Repetir
Um botão que aciona o evento **Click** repetidamente, desde o momento em que é pressionado até ser liberado. 

![Um controle de botão de repetição](images/controls/repeat-button.png) 

```xaml
<RepeatButton x:Name="repeatButton1" Content="Repeat Button" 
              Click="RepeatButton_Click" />
```

Referência: [RepeatButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 

Design e instruções: [Guia de controle de botões](buttons.md) 

## <a name="collectiondata-controls"></a>Controles de coleção/dados

### <a name="flip-view"></a>Exibição virando a página
Um controle que apresenta um conjunto de itens pelo qual o usuário pode navegar, um por vez.

```xaml
<FlipView x:Name="flipView1" SelectionChanged="FlipView_SelectionChanged">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

Referência: [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx) 

Design e instruções: [Guia do controle de exibição de inversão](flipview.md) 

### <a name="grid-view"></a>Exibição em grade
Um controle que apresenta um conjunto de itens em linhas e colunas, que podem ser roladas verticalmente.

```xaml
<GridView x:Name="gridView1" SelectionChanged="GridView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</GridView>
```

Referência: [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) 

Design e instruções: [Listas](lists.md) 

Código de amostra: [Amostra de ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

### <a name="items-control"></a>Controle de itens
Um controle que apresenta um conjunto de itens em uma interface do usuário especificada, por modelo de dados. 

```xaml
<ItemsControl/>
```

Referência: [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) 

### <a name="list-view"></a>Exibição de lista
Um controle que apresenta um conjunto de itens em uma lista que pode ser rolada verticalmente.

```xaml
<ListView x:Name="listView1" SelectionChanged="ListView_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
</ListView>
```

Referência: [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) 

Design e instruções: [Listas](lists.md) 

Código de amostra: [Amostra de ListView](http://go.microsoft.com/fwlink/p/?LinkId=619900)

## <a name="date-and-time-controls"></a>Controles de data e hora

### <a name="calendar-date-picker"></a>Seletor de data do calendário
Um controle que permite ao usuário selecionar uma data usando uma exibição de calendário suspenso.

![Um seletor de data do calendário com exibição de calendário aberto](images/controls/calendar-date-picker-open.png)

```xaml
<CalendarDatePicker/>
```

Referência: [CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx) 

Design e instruções: [Controles de calendário, data e hora](date-and-time.md)
 
### <a name="calendar-view"></a>Exibição de calendário
Uma exibição de calendário configurável que permite ao usuário selecionar uma ou várias datas.

```xaml
<CalendarView/>
```

Referência: [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) 

Design e instruções: [Controles de calendário, data e hora](date-and-time.md) 

### <a name="date-picker"></a>Seletor de data
Um controle que permite ao usuário selecionar uma data.

![Controle seletor de data](images/controls/date-picker.png)

```xaml
<DatePicker Header="Arrival Date"/>
```

Referência: [DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx) 

Design e instruções: [Controles de calendário, data e hora](date-and-time.md)
 
### <a name="time-picker"></a>Seletor de hora
Um controle que permite ao usuário definir um valor de tempo.

![Controle TimePicker](images/controls/time-picker.png) 

```xaml
<TimePicker Header="Arrival Time"/>
```

Referência: [TimePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx) 

Design e instruções: [Controles de calendário, data e hora](date-and-time.md)

## <a name="flyouts"></a>Submenus

### <a name="context-menu"></a>Menu de contexto
Consulte Submenu de menu e Menu pop-up.

### <a name="flyout"></a>Submenu
Exibe uma mensagem que requer interação do usuário. Ao contrário de uma caixa de diálogo, um submenu desdobrável não cria uma janela separada e não bloqueia outra interação do usuário.

![Controle de submenu](images/controls/flyout.png)

```xaml
<Flyout>
    <StackPanel>
        <TextBlock Text="All items will be removed. Do you want to continue?"/>
        <Button Click="DeleteConfirmation_Click" Content="Yes, empty my cart"/>
    </StackPanel>
</Flyout>
```

Referência: [Submenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flyout.aspx) 

Design e instruções: [submenus](dialogs-and-flyouts/flyouts.md) 

### <a name="menu-flyout"></a>Submenu de menu
Exibe temporariamente uma lista de comandos ou opções relacionadas com o que o usuário está fazendo atualmente.

![Controle de submenu de menu](images/controls/menu-flyout.png) 

```xaml
<MenuFlyout>
    <MenuFlyoutItem Text="Reset" Click="Reset_Click"/>
    <MenuFlyoutSeparator/>
    <ToggleMenuFlyoutItem Text="Shuffle" 
                          IsChecked="{Binding IsShuffleEnabled, Mode=TwoWay}"/>
    <ToggleMenuFlyoutItem Text="Repeat" 
                          IsChecked="{Binding IsRepeatEnabled, Mode=TwoWay}"/>
</MenuFlyout>
```

Referência: [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyout.aspx), [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) 

Design e instruções: [Menus e menus de contexto](menus.md) 

Código de amostra: [Amostra de Menu de contexto de XAML](http://go.microsoft.com/fwlink/p/?LinkId=620021)

### <a name="popup-menu"></a>Menu pop-up
Um menu personalizado que apresenta os comandos especificados por você.

Referência: [PopupMenu](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.popups.popupmenu.aspx) 

Design e instruções: [caixas de diálogo](dialogs-and-flyouts/dialogs.md) 

### <a name="tooltip"></a>Dica de ferramenta
Uma janela pop-up que exibe as informações de um elemento. 
 
![Controle de dica de ferramenta](images/controls/tool-tip.png)

```xaml
<Button Content="Button" Click="Button_Click" 
        ToolTipService.ToolTip="Click to perform action" />
```

Referência: [ToolTip](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltip.aspx), [ToolTipService](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.tooltipservice.aspx) 

Design e instruções: Diretrizes para dicas de ferramentas 

## <a name="images"></a>Imagens

### <a name="image"></a>Imagem
Um controle que apresenta uma imagem.

```xaml
<Image Source="Assets/Logo.png" />
```

Referência: [Image](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.image.aspx) 

Design e instruções: [Image e ImageBrush](images-imagebrushes.md) 

Código de amostra: [Amostra de imagens XAML](http://go.microsoft.com/fwlink/p/?linkid=226867)

## <a name="graphics-and-ink"></a>Tinta e elementos gráficos

### <a name="inkcanvas"></a>InkCanvas
Um controle que recebe e exibe traços de tinta.

```xaml
<InkCanvas/>
```

Referência: [InkCanvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.inkcanvas.aspx) 

### <a name="shapes"></a>Formas
Vários objetos gráficos de modo retido que podem ser apresentados como elipses, retângulos, linhas, caminhos de Bezier, etc.

![Um polígono](images/controls/shapes-polygon.png) 
![Um caminho](images/controls/shapes-path.png) 

```xaml
<Ellipse/>
<Path/>
<Rectangle/>
```

Referência: [Formas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.aspx) 

Como: [Desenhar formas](../../graphics/drawing-shapes.md) 

Código de amostra: [Amostra de desenho baseado em vetor em XAML](http://go.microsoft.com/fwlink/p/?linkid=226866)

## <a name="layout-controls"></a>Controles de layout

### <a name="border"></a>Borda
Um controle de contêiner que desenha uma borda, uma tela de fundo ou ambas em outro objeto.

![Uma borda ao redor de 2 retângulos](images/controls/border.png) 

```xaml
<Border BorderBrush="Blue" BorderThickness="4" 
        Height="108" Width="64" 
        Padding="8" CornerRadius="4">
    <Canvas>
        <Rectangle Fill="Orange"/>
        <Rectangle Fill="Green" Margin="0,44"/>
    </Canvas>
</Border>
```

Referência: [Borda](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.border.aspx)

### <a name="canvas"></a>Canvas
Um painel de layout que dá suporte ao posicionamento absoluto de elementos filho relativos ao canto superior esquerdo da tela.
 
![Painel de layout de tela](images/controls/canvas.png) 

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

Referência: [Canvas](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx)
 
### <a name="grid"></a>Grade
Um painel de layout que dá suporte à organização de elementos filho em linhas e colunas.

![Painel de layout de grade](images/controls/grid.png) 

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="50"/>
        <ColumnDefinition Width="50"/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```

Referência: [Grid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx)
 
### <a name="panning-scroll-viewer"></a>Visualizador de rolagem de movimento panorâmico
Consulte Visualizador de rolagem.

### <a name="relativepanel"></a>RelativePanel
Um painel que permite posicionar e alinhar os objetos filho em relação uns aos outros ou ao painel pai.

![Painel de layout do painel relativo](images/controls/relative-panel.png) 

```xaml
<RelativePanel>
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

Referência: [RelativePanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx)

### <a name="scroll-bar"></a>Barra de rolagem
Consulte Visualizador de rolagem. (ScrollBar é um elemento do ScrollViewer. Você normalmente não o usa como um controle independente.)

Referência: [ScrollBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.scrollbar.aspx)
 
### <a name="scroll-viewer"></a>Visualizador de rolagem
Um controle de contêiner que permite aplicar panorâmica e zoom ao conteúdo.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10" 
              HorizontalScrollMode="Enabled" 
              HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

Referência: [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx)

Design e instruções: [Guia de controles de rolagem e movimento panorâmico](scroll-controls.md) 

Código de amostra: [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=238577)

### <a name="stack-panel"></a>Painel da pilha
Um painel de layout que organiza os elementos filho em uma única linha que pode ser orientada horizontal ou verticalmente.

![Controle de layout do painel de pilha](images/controls/stack-panel.png) 

```xaml
<StackPanel>
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue"/>
    <Rectangle Fill="Green"/>
    <Rectangle Fill="Orange"/>
</StackPanel>
```

Referência: [StackPanel](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx)
 
### <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid
Um painel de layout que dá suporte à organização de elementos filho em linhas e colunas. Cada elemento filho pode se propagar em várias linhas e colunas.

![Painel de layout de grade de encapsulamento de tamanho variável](images/controls/variable-sized-wrap-grid.png) 

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" Height="80" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" Width="80" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" Height="80" Width="80" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```

Referência: [VariableSizedWrapGrid](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx)

### <a name="viewbox"></a>Viewbox
Um controle de contêiner que dimensiona o conteúdo em um tamanho especificado.

![Controle do Viewbox](images/controls/view-box.png) 

```xaml
<Viewbox MaxWidth="25" MaxHeight="25">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="75" MaxHeight="75">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
<Viewbox MaxWidth="150" MaxHeight="150">
    <Image Source="Assets/Logo.png"/>
</Viewbox>
```

Referência: [Viewbox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.viewbox.aspx)
 
### <a name="zooming-scroll-viewer"></a>Visualizador de rolagem de zoom
Consulte Visualizador de rolagem.

## <a name="media-controls"></a>Controles de mídia

### <a name="audio"></a>Áudio
Veja Elemento de mídia.

### <a name="media-element"></a>Elemento de mídia
Um controle que reproduz conteúdo de áudio e vídeo.

```xaml
<MediaElement x:Name="myMediaElement"/>
```

Referência: [MediaElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediaelement.aspx) 

Design e instruções: [Guia de controle do elemento de mídia](media-playback.md)

### <a name="mediatransportcontrols"></a>MediaTransportControls
Um controle que fornece controles de reprodução para um MediaElement.

![Elemento de mídia com controles de transporte](images/controls/media-transport-controls.png) 

```xaml
<MediaTransportControls MediaElement="myMediaElement"/>
```

Referência: [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.mediatransportcontrols.aspx) 

Design e instruções: [Guia de controle do elemento de mídia](media-playback.md) 

Código de amostra: [Amostra de controles de transporte de mídia](http://go.microsoft.com/fwlink/p/?LinkId=620023)

### <a name="video"></a>Vídeo
Veja Elemento de mídia.

## <a name="navigation"></a>Navegação

### <a name="navigationview"></a>NavigationView

Um contêiner adaptável e o modelo de navegação flexível que implementa o painel de navegação esquerdo, a navegação superior e o padrão das guias.

Referência: [NavigationView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.navigationview)

Design e instruções: [Guia do controle NavigationView](navigationview.md)

### <a name="splitview"></a>SplitView

Um controle de contêiner com dois modos de exibição; um modo de exibição para o conteúdo principal e outro modo de exibição que é normalmente usado para um menu de navegação.

![Controle de modo de exibição dividido](images/controls/split-view.png) 

```xaml
<SplitView>
    <SplitView.Pane>
        <!-- Menu content -->
    </SplitView.Pane>
    <SplitView.Content>
        <!-- Main content -->
    </SplitView.Content>
</SplitView>
```

Referência: [SplitView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) 

Design e instruções: [Guia do controle de modo de exibição dividido](split-view.md)

### <a name="web-view"></a>Modo de exibição da Web

Um controle de contêiner que hospeda conteúdo da Web.

```xaml
<WebView x:Name="webView1" Source="http://dev.windows.com" 
         Height="400" Width="800"/>
```

Referência: [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 

Design e instruções: diretrizes para modos de exibição da Web 

Código de amostra: [Amostra de controle WebView em XAML](http://go.microsoft.com/fwlink/p/?linkid=238582)

### <a name="semantic-zoom"></a>Zoom semântico

Um controle de contêiner que permite aplicar zoom entre dois modos de exibição de um conjunto de itens.

```xaml
<SemanticZoom>
    <ZoomedInView>
        <GridView></GridView>
    </ZoomedInView>
    <ZoomedOutView>
        <GridView></GridView>
    </ZoomedOutView>
</SemanticZoom>
```

Referência: [SemanticZoom](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.semanticzoom.aspx) 

Design e instruções: [Guia de controle de zoom semântico](semantic-zoom.md)

Código de amostra: [Amostra de SemanticZoom e agrupamento de GridView em XAML](http://go.microsoft.com/fwlink/p/?linkid=226564)

## <a name="progress-controls"></a>Controles de progresso

### <a name="progress-bar"></a>Barra de progresso
Um controle que indica o progresso exibindo uma barra.

![Controle da barra de progresso](images/controls/progress-bar-determinate.png)

Um barra de progresso que mostra um valor específico.

```xaml
<ProgressBar x:Name="progressBar1" Value="50" Width="100"/>
```

![Controle da barra de progresso indeterminado](images/controls/progress-bar-indeterminate.png)

Uma barra de progresso que mostra um progresso indeterminado.

```xaml
<ProgressBar x:Name="indeterminateProgressBar1" IsIndeterminate="True" Width="100"/>
```

Referência: [ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx) 

Design e instruções: [Guia de controles de progresso](progress-controls.md) 

### <a name="progress-ring"></a>Anel de progresso
Um controle que indica progresso indeterminado exibindo um anel. 

![Controle do anel de progresso](images/controls/progress-ring.png) 

```xaml
<ProgressRing x:Name="progressRing1" IsActive="True"/>
```

Referência: [ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx) 

Design e instruções: [Guia de controles de progresso](progress-controls.md) 

## <a name="text-controls"></a>Controles de texto

### <a name="auto-suggest-box"></a>Caixa de sugestão automática
Uma caixa de entrada de texto que fornece o texto sugerido à medida que o usuário digita.

![Caixa de pesquisa de sugestão automática](images/controls/auto-suggest-box.png) 

Referência: [AutoSuggestBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)

Design e instruções: [Controles de texto](text-controls.md), [Guia do controle de caixa de sugestão automática](auto-suggest-box.md)

Código de amostra: [Amostra de migração de AutoSuggestBox](http://go.microsoft.com/fwlink/p/?LinkId=619996)

### <a name="multi-line-text-box"></a>Caixa de texto com várias linhas
Consulte Caixa de texto.

### <a name="password-box"></a>Caixa de senha
Um controle para inserir senhas.

 ![Uma caixa de senha](images/controls/password-box.png)

```xaml
<PasswordBox x:Name="passwordBox1" 
             PasswordChanged="PasswordBox_PasswordChanged" />
```

Referência: [PasswordBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx) 

Design e instruções: [Controles de texto](text-controls.md), [Guia de controle de caixa de senha](password-box.md) 

Código de amostra: [Amostra de exibição de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238579), [Amostra de edição de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)

### <a name="rich-edit-box"></a>Caixa de edição com formato
Um controle que permite que um usuário edite documentos rich text com conteúdo como texto formatado, hiperlinks e imagens.

```xaml
<RichEditBox />
```

Referência: [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) 

Design e instruções: [controles de texto](text-controls.md), [Guia de controle de caixa de edição com formato](rich-edit-box.md)

Código de amostra: [Amostra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="search-box"></a>Caixa de pesquisa
Consulte a Caixa de sugestão automática.

### <a name="single-line-text-box"></a>Caixa de texto com linha única
Consulte Caixa de texto.

### <a name="static-textparagraph"></a>Texto/parágrafo estático
Consulte Bloco de texto.

### <a name="text-block"></a>Bloco de texto
Um controle que exibe texto.

![Controle do bloco de texto](images/controls/text-block.png) 

```xaml
<TextBlock x:Name="textBlock1" Text="I am a TextBlock"/>
```

Referência: [TextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx), [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx) 

Design e instruções: [Controles de texto](text-controls.md), [Guia do controle TextBlock](text-block.md), [Guia de controle do bloco de Rich text](rich-text-block.md)

Código de amostra: [Amostra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

### <a name="text-box"></a>Caixa de texto
Um campo de texto sem formatação com uma única linha ou com várias linhas.

![Controle da caixa de texto](images/controls/text-box.png) 

```xaml
<TextBox x:Name="textBox1" Text="I am a TextBox" 
         TextChanged="TextBox_TextChanged"/>
```

Referência: [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) 

Design e instruções: [controles de texto](text-controls.md), [Guia de controle de caixa de texto](text-box.md) 

Código de amostra: [Amostra de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)

## <a name="selection-controls"></a>Controles de seleção

### <a name="check-box"></a>Caixa de seleção
Representa um controle que um usuário pode marcar e desmarcar.

![Os 3 estados de uma caixa de seleção](images/templates-checkbox-states-default.png)

```xaml
<CheckBox x:Name="checkbox1" Content="CheckBox" 
          Checked="CheckBox_Checked"/>
```

Referência: [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 

Design e instruções: [Guia do controle de caixa de seleção](checkbox.md) 

### <a name="combo-box"></a>Caixa de combinação
Uma lista suspensa de itens da qual os usuários podem selecionar.

![Abrir caixa de combinação](images/controls/combo-box-open.png) 

```xaml
<ComboBox x:Name="comboBox1" Width="100"
          SelectionChanged="ComboBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ComboBox>
```

Referência: [ComboBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.combobox.aspx) 

Design e instruções: [Listas](lists.md) 

### <a name="list-box"></a>Caixa de listagem
Um controle que apresenta uma lista embutida de itens, na qual o usuário pode fazer seleções. 

![Controle de caixa de listagem](images/controls/list-box.png)

```xaml
<ListBox x:Name="listBox1" Width="100"
         SelectionChanged="ListBox_SelectionChanged">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
</ListBox>
```

Referência: [ListBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listbox.aspx) 

Design e instruções: [Listas](lists.md) 

### <a name="radio-button"></a>Botão de opção
Um controle que permite ao usuário selecionar uma única opção em uma lista de opções. Quando os botões de opção são agrupados, eles se tornam mutuamente exclusivos.

![Controles de botão de opção](images/controls/radio-button.png)

```xaml
<RadioButton x:Name="radioButton1" Content="RadioButton 1" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
<RadioButton x:Name="radioButton2" Content="RadioButton 2" GroupName="Group1" 
             Checked="RadioButton_Checked" IsChecked="True"/>
<RadioButton x:Name="radioButton3" Content="RadioButton 3" GroupName="Group1" 
             Checked="RadioButton_Checked"/>
```

Referência: [RadioButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.radiobutton.aspx) 

Design e instruções: [Guia do controle de botão de opção](radio-button.md)
 
### <a name="slider"></a>Controle deslizante
Um controle que permite que o usuário selecione de uma lista de valores movendo um controle de Thumb paralelo a uma faixa.

![Controle deslizante](images/controls/slider.png)

```xaml
<Slider x:Name="slider1" Width="100" ValueChanged="Slider_ValueChanged" />
```

Referência: [Controle Deslizante](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.slider.aspx) 

Design e instruções: [Guia do controle deslizante](slider.md) 

### <a name="toggle-button"></a>Botão de alternância
Um botão que pode ser alternado entre dois estados.

```xaml
<ToggleButton x:Name="toggleButton1" Content="Button" 
              Checked="ToggleButton_Checked"/>
```

Referência: [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)

Design e instruções: [Guia do controle de alternância](toggles.md) 

### <a name="toggle-switch"></a>Switch de alternância
Uma opção que pode ser alternada entre dois estados.

![Controle do comutador de alternância](images/controls/toggle-switch.png) 

```xaml
<ToggleSwitch x:Name="toggleSwitch1" Header="ToggleSwitch" 
              OnContent="On" OffContent="Off" 
              Toggled="ToggleSwitch_Toggled"/>
```

Referência: [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.toggleswitch.aspx) 

Design e instruções: [Guia do controle de alternância](toggles.md) 
