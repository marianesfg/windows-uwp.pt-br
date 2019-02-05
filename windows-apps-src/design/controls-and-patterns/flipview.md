---
Description: Displays images in a collection, such as photos in an album or items in a product details page, one image at a time.
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
ms.openlocfilehash: ac4e5bb7c761ad6661647cb88f831ffa652b6241
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048796"
---
# <a name="flip-view"></a>Exibição de inversão

 

Use um recurso exibição de inversão para procurar imagens ou outros itens em uma coleção, como fotos em um álbum ou itens em uma página de detalhes do produto, um item por vez. Em dispositivos sensíveis ao toque, deslizar o dedo em um item move a coleção. Com um mouse, os botões de navegação aparecem no foco do mouse. No teclado, as teclas de seta movem a coleção.

> **APIs importantes**: [classe FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx), [propriedade ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx), [propriedade ItemTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)


## <a name="is-this-the-right-control"></a>Este é o controle correto?

O recurso exibição de inversão é melhor para examinar imagens em coleções pequenas a médias (até 25 itens ou algo assim). Exemplos de tais coleções incluem itens em uma página de detalhes do produto ou fotos em um álbum de fotos. Embora não recomendemos o modo de exibição invertido em coleções maiores, o controle é comum para a visualização de imagens individuais em um álbum de fotos.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/FlipView">abrir o aplicativo e ver o FlipView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

A navegação horizontal, começando pelo item mais à esquerda e invertendo à direita, é o layout típico para um modo de exibição invertido. Esse layout funciona bem na orientação retrato ou paisagem em todos os dispositivos:

![Exemplo de layout horizontal de inversão de modo de exibição](images/controls_flipview_horizonal.jpg)

O recurso exibição de inversão também pode ser procurado verticalmente:

![Exemplo do recurso exibição de inversão vertical](images/controls_flipview_vertical.jpg)

## <a name="create-a-flip-view"></a>Criar um recurso exibição de inversão

FlipView é um [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx) e, por isso, pode conter uma coleção de itens de qualquer tipo. Para popular a exibição, adicione itens à coleção de [**Itens**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) ou defina a propriedade [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) como uma fonte de dados.

Por padrão, o item de dados é exibido no recurso exibição de inversão como a representação do objeto de dados ao qual ele está associado. Para especificar exatamente como os itens em exibição de inversão são exibidos, crie um [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.datatemplate.aspx) para definir o layout dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo definido embutido. Você atribui o DataTemplate à propriedade [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) do FlipView.

### <a name="add-items-to-the-items-collection"></a>Adicionar itens à coleção de itens

Você pode adicionar itens à coleção [**Items**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) usando XAML ou código. Normalmente, você adiciona itens dessa maneira quando tem um pequeno número de itens que não mudam e são facilmente definidos no XAML ou quando gera os itens em código no tempo de execução. Este é um recurso exibição de inversão com itens definidos embutidos.

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

Quando você adiciona itens a um recurso exibição de inversão, eles são colocados automaticamente no contêiner [**FlipViewItem**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipviewitem.aspx). Para alterar como um item é exibido, você pode aplicar um estilo ao contêiner de item definindo a propriedade [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle.aspx). 

Quando você define os itens no XAML, eles também são adicionados automaticamente à coleção de Items.

### <a name="set-the-items-source"></a>Definir a origem de itens

Geralmente, você usa um recurso de exibição de inversão para exibir dados de uma origem, como um banco de dados ou a Internet. Para popular um recurso exibição de inversão em uma fonte de dados, você define sua propriedade [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) como uma coleção de itens de dados.

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

Você também pode associar a propriedade ItemsSource a uma coleção no XAML. Para saber mais, veja [Vinculação de dados com XAML](../../data-binding/data-binding-quickstart.md).

Aqui, o ItemsSource está associado a um [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx) denominado `itemsViewSource`. 

```xaml
<Page.Resources>
    <!-- Collection of items displayed by this page -->
    <CollectionViewSource x:Name="itemsViewSource" Source="{Binding Items}"/>
</Page.Resources>

...

<FlipView x:Name="itemFlipView" 
          ItemsSource="{Binding Source={StaticResource itemsViewSource}}"/>
```

>**Observação**&nbsp;&nbsp;Você pode preencher um recurso exibição de inversão adicionando itens a sua coleção Items ou definindo sua propriedade ItemsSource, mas você não pode usar as duas formas ao mesmo tempo. Se você definir a propriedade ItemsSource e adicionar um item no XAML, o item será ignorado. Se você definir a propriedade ItemsSource e adicionar um item à coleção Items no código, uma exceção será gerada.

### <a name="specify-the-look-of-the-items"></a>Especificar a aparência dos itens

Por padrão, o item de dados é exibido no recurso exibição de inversão como a representação do objeto de dados ao qual ele está associado. Você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens são exibidos no recurso exibição de inversão, você cria o [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo definido embutido. O DataTemplate é atribuído à propriedade [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) do controle FlipView.

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

Por padrão, o recurso exibição de inversão inverte horizontalmente. Para fazê-lo inverter verticalmente, use um painel de pilha com uma orientação vertical como o [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) do recurso exibição de inversão.

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

Para conferir um código de exemplo que mostra como adicionar um indicador de contexto a um FlipView, consulte [Amostra de FlipView em XAML](https://go.microsoft.com/fwlink/p/?LinkID=311760).

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

- [Amostra do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Diretrizes para listas](lists.md)
- [**Classe FlipView**](https://msdn.microsoft.com/library/windows/apps/br242678)
