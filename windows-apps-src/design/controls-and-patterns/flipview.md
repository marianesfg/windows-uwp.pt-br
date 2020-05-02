---
Description: Exibe imagens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, uma imagem por vez.
title: Diretrizes para controles de exibição de inversão
ms.assetid: A4E05D92-1A0E-4CDD-84B9-92199FF8A8A3
label: Flip view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ef17fdc69f7a9f25831c5c17419768ff32874f80
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80080944"
---
# <a name="flip-view"></a>Exibição de inversão

Use um recurso exibição de inversão para procurar imagens ou outros itens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, um item por vez. Em dispositivos sensíveis ao toque, deslizar o dedo em um item move a coleção. Com um mouse, os botões de navegação aparecem na passagem do mouse. No teclado, as teclas de seta movem a coleção.

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | A Biblioteca de interface do usuário do Windows 2.2 ou posterior inclui um novo modelo para esse controle que usa cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). WinUI é um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs de plataforma:** [Classe FlipView](/uwp/api/windows.ui.xaml.controls.flipview), [propriedade ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [propriedade ItemTemplate](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

O recurso exibição de inversão é melhor para examinar imagens em coleções pequenas a médias (até 25 itens ou algo assim). Exemplos de tais coleções incluem itens em uma página de detalhes do produto ou fotos em um álbum de fotos. Embora não recomendemos o modo de exibição invertido em coleções maiores, o controle é comum para a visualização de imagens individuais em um álbum de fotos.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/FlipView">abri-lo e ver o FlipView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

A navegação horizontal, começando pelo item mais à esquerda e invertendo à direita, é o layout típico para um modo de exibição invertido. Esse layout funciona bem na orientação retrato ou paisagem em todos os dispositivos:

![Exemplo de layout horizontal de inversão de modo de exibição](images/controls_flipview_horizonal.jpg)

O recurso exibição de inversão também pode ser procurado verticalmente:

![Exemplo do recurso exibição de inversão vertical](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>Criar um recurso exibição de inversão

FlipView é um [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol) e, por isso, pode conter uma coleção de itens de qualquer tipo. Para popular a exibição, adicione itens à coleção de [**Itens**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou defina a propriedade [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) como uma fonte de dados.

Por padrão, o item de dados é exibido no recurso exibição de inversão como a representação do objeto de dados ao qual ele está associado. Para especificar exatamente como os itens em exibição de inversão são exibidos, crie um [**DataTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) para definir o layout dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo definido embutido. Você atribui o DataTemplate à propriedade [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) do FlipView.

### <a name="add-items-to-the-items-collection"></a>Adicionar itens à coleção Items

Você pode adicionar itens à coleção [**Items**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) usando XAML ou código. Normalmente, você adiciona itens dessa maneira quando tem um pequeno número de itens que não mudam e são facilmente definidos no XAML ou ao gerar os itens em código no tempo de execução. Este é um recurso exibição de inversão com itens definidos embutidos.

```xaml
<FlipView x:Name="flipView1">
    <Image Source="Assets/Logo.png" />
    <Image Source="Assets/SplashScreen.png" />
    <Image Source="Assets/SmallLogo.png" />
</FlipView>
```

```csharp
// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.Items.Add("Item 1");
flipView1.Items.Add("Item 2");

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

Quando você adiciona itens a um recurso exibição de inversão, eles são colocados automaticamente no contêiner [**FlipViewItem**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipViewItem). Para alterar como um item é exibido, você pode aplicar um estilo ao contêiner de item definindo a propriedade [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle). 

Quando você define os itens em XAML, eles também são adicionados automaticamente à coleção Items.

### <a name="set-the-items-source"></a>Definir a origem de itens

Geralmente, você usa um recurso de exibição de inversão para exibir dados de uma origem, como um banco de dados ou a Internet. Para popular um recurso exibição de inversão em uma fonte de dados, você define sua propriedade [**ItemsSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) como uma coleção de itens de dados.

Aqui, o ItemsSource do recurso exibição de inversão está definido diretamente no código em uma instância de uma coleção.

```csharp
// Data source.
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

// Create a new flip view, add content, 
// and add a SelectionChanged event handler.
FlipView flipView1 = new FlipView();
flipView1.ItemsSource = itemsList;
flipView1.SelectionChanged += FlipView_SelectionChanged;

// Add the flip view to a parent container in the visual tree.
stackPanel1.Children.Add(flipView1);
```

Você também pode associar a propriedade ItemsSource a uma coleção em XAML. Para saber mais, consulte [Vinculação de dados com XAML](../../data-binding/data-binding-quickstart.md).

Aqui, o ItemsSource está associado a um [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) denominado `itemsViewSource`. 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**Observação**&nbsp;&nbsp;É possível preencher uma exibição de inversão adicionando itens à coleção Items ou definindo a propriedade ItemsSource, mas você não pode usar as duas formas ao mesmo tempo. Se você definir a propriedade ItemsSource e adicionar um item em XAML, o item adicionado será ignorado. Se você definir a propriedade ItemsSource e adicionar um item à coleção Items no código, uma exceção será gerada.

### <a name="specify-the-look-of-the-items"></a>Especificar a aparência dos itens

Por padrão, o item de dados é exibido no recurso exibição de inversão como a representação do objeto de dados ao qual ele está associado. Você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens são exibidos no recurso exibição de inversão, você cria o [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo definido embutido. O DataTemplate é atribuído à propriedade [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) do controle FlipView.

Neste exemplo, o ItemTemplate de um FlipView é definido embutido. Uma sobreposição é adicionada à imagem para exibir o nome da imagem. 

```XAML
<FlipView x:Name="flipView1" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

Esta é a aparência do layout definido pelo modelo de dados.

Modelo de recurso exibição de inversão.

### <a name="set-the-orientation-of-the-flip-view"></a>Definir a orientação do recurso exibição de inversão

Por padrão, o recurso exibição de inversão inverte horizontalmente. Para fazê-lo inverter verticalmente, use um painel de pilha com uma orientação vertical como o [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) do recurso exibição de inversão.

Este exemplo mostra como usar o painel de pilha com uma orientação vertical como o ItemsPanel de um FlipView.

```XAML
<FlipView x:Name="flipViewVertical" Width="480" Height="270" 
          BorderBrush="Black" BorderThickness="1">
    
    <!-- Use a vertical stack panel for vertical flipping. -->
    <FlipView.ItemsPanel>
        <ItemsPanelTemplate>
            <VirtualizingStackPanel Orientation="Vertical"/>
        </ItemsPanelTemplate>
    </FlipView.ItemsPanel>
    
    <FlipView.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Image Width="480" Height="270" Stretch="UniformToFill"
                       Source="{Binding Image}"/>
                <Border Background="#A5000000" Height="80" VerticalAlignment="Bottom">
                    <TextBlock Text="{Binding Name}" 
                               FontFamily="Segoe UI" FontSize="26.667" 
                               Foreground="#CCFFFFFF" Padding="15,20"/>
                </Border>
            </Grid>
        </DataTemplate>
    </FlipView.ItemTemplate>
</FlipView>
```

Esta é a aparência do recurso exibição de inversão com uma orientação vertical.

![Exemplo do recurso exibição de inversão vertical](images/controls_flipview_vertical.jpg)

## <a name="adding-a-context-indicator"></a>Adicionando um indicador de contexto

Um indicador de contexto em um recurso exibição de inversão fornece um ponto de referência útil. Os pontos em um indicador de contexto padrão não são interativos. Conforme visto neste exemplo, o melhor posicionamento costuma ser centralizado e abaixo da galeria:

![Exemplo de um indicador de página](images/controls_pageindicator.png)

Para coleções maiores (10 a 25 itens), considere usar um indicador que ofereça mais contexto, como uma tira de filme de miniaturas. Diferentemente de um indicador de contexto que usa pontos simples, cada miniatura na tira de filme mostra uma pequena versão da imagem correspondente e deve poder ser selecionada:

![Exemplo de indicador de contexto](images/controls_contextindicator.jpg)

Para conferir um código de exemplo que mostra como adicionar um indicador de contexto a um FlipView, consulte [Amostra de FlipView em XAML](https://code.msdn.microsoft.com/windowsapps/XAML-FlipView-control-0ae45312).

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   O recurso exibição de inversão funciona melhor para coleções de até aproximadamente 25 itens.
-   Evite usar um controle de modo exibição invertido em coleções grandes, já que o movimento repetitivo de inversão em cada item pode ser entediante. Uma exceção seria para álbuns de fotos, que costumam ter centenas ou milhares de imagens. Álbuns de fotos quase sempre alternam para um modo de exibição invertido depois que uma foto é selecionada no layout do modo de exibição de grade. Para outras coleções grandes, considere o [Modo de exibição de lista ou de grade](lists.md).
-   Para indicadores de contexto:
    -   A ordem dos pontos (ou qualquer marcador visual que você escolher) funciona melhor quando centralizado e abaixo de uma galeria de movimento panorâmico horizontal.
    -   Caso você queira um indicador de contexto em uma galeria com movimento panorâmico vertical, ele funciona melhor centralizado e à direita das imagens.
    -   O ponto realçado indica o item atual. Normalmente, o ponto realçado é branco e os outros pontos são cinza.
    -   O número de pontos pode variar, mas não pode haver muitos pontos para que o usuário não tenha dificuldade para encontrar seu local – 10 pontos costuma ser o número máximo a ser mostrado.

## <a name="globalization-and-localization-checklist"></a>Lista de verificação de globalização e localização

<table>
<tr>
<th>Considerações de bidirecional</th><td>Use o espelhamento padrão para idiomas RTL. Os controles de avançar e voltar devem ser baseados na direção do idioma, portanto para idiomas RTL, o botão direito deve navegar para trás e o botão esquerdo deve navegar para a frente.</td>
</tr>

</table>

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Diretrizes para listas](lists.md)
- [**Classe FlipView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)
