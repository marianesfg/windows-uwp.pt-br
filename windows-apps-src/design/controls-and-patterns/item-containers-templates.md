---
Description: Use os modelos para modificar a aparência dos itens em controles ListView ou GridView.
title: Contêineres e modelos de itens
label: Item containers and templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: d8eb818d-b62e-4314-a612-f29142dbd93f
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0834a905c50b92003c3aa78ff8226d35c25e5dd
ms.sourcegitcommit: ddc65c170834bcce524b5e1d36e6755eae1e3af2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83729859"
---
# <a name="item-containers-and-templates"></a>Contêineres e modelos de itens

 

Os controles **ListView** e **GridView** gerenciam como seus itens são organizados (horizontal, vertical, quebra automática etc…) e como o usuário interage com os itens, mas não como os itens individuais são mostrados na tela. A visualização dos itens é gerenciada por contêineres de itens. Quando você adiciona itens a uma exibição de lista, eles são colocados automaticamente em um contêiner. O contêiner de itens padrão de ListView é [ListViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewItem); de GridView, é [GridViewItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridViewItem).

> **APIs importantes**: [Classe ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [Classe GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [Classe ListViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem), [Classe GridViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem), [Propriedade ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate), [Propriedade ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)


> [!NOTE]
> Os controles ListView e GridView são derivados da classe [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase), portanto, eles têm a mesma funcionalidade, mas exibem os dados de modo diferente. Neste artigo, ao falarmos sobre exibição de lista, as informações se aplicam aos controles ListView e GridView, a menos que especificado de outra forma. Poderemos nos referir a classes, como ListView ou ListViewItem, mas o prefixo *List* poderá ser substituído por *Grid* no equivalente a grade correspondente (GridView ou GridViewItem). 

## <a name="listview-items-and-gridview-items"></a>Itens ListView e itens GridView
Conforme mencionado acima, os itens ListView são colocados automaticamente no contêiner ListViewItem e os itens GridView são colocados no contêiner GridViewItem. Esses contêineres de item são controles que têm seu próprio estilo e interação internos, mas também podem ser altamente personalizados. No entanto, antes da personalização, familiarize-se com as diretrizes e o estilo recomendados para o ListViewItem e o GridViewItem:

- **ListViewItems** – os itens são basicamente focados em texto e prolongados em forma. Os ícones ou as imagens podem ser exibidos à esquerda do texto.
- **GridViewItems** – geralmente os itens têm forma quadrada ou pelo menos uma forma de retângulo alongado. Os itens têm foco na imagem e podem exibir um texto ao seu redor ou sobreposto na imagem. 

## <a name="introduction-to-customization"></a>Introdução à personalização
Os controles de contêiner (como o ListViewItem e o GridViewItem) são compostos por duas partes importantes que são combinadas para criar os elementos visuais finais mostrados para um item: *modelo de dados* e o *modelo de controle*.

- **Modelo de dados** – atribua um [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) à propriedade [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) da exibição de lista para especificar como itens de dados individuais são mostrados.
- **Modelo de controle** – O modelo de controle fornece a parte da visualização do item pela qual a estrutura é responsável, como estados visuais. Você pode usar a propriedade [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) para modificar o modelo de controle. Normalmente, você faz isso para modificar as cores da exibição de lista para combinar com sua marca ou para alterar a forma como os itens selecionados são mostrados.

Esta imagem mostra como o modelo de controle e o modelo de dados são combinados para criar o visual final de um item.

![Modelos de controle e dados de exibição de lista](images/listview-visual-parts.png)

Este é o XAML que cria este item. Vamos explicar os modelos mais tarde.

```xaml
<ListView Width="220" SelectionMode="Multiple">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid Background="Yellow">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="54"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="44" Height="44"
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Black"
                           FontSize="14" Grid.Column="1"
                           VerticalAlignment="Center"
                           Padding="0,0,54,0"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Background" Value="LightGreen"/>
        </Style>
    </ListView.ItemContainerStyle>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

> [!IMPORTANT]
> Modelos de dados e de controle são usados para personalizar o estilo de muitos controles diferentes do ListView e do GridView. Eles incluem controles com seu próprio estilo interno, como FlipView, e controles de criação personalizada, como o ItemsRepeater. Embora o exemplo abaixo seja específico do ListView/GridView, os conceitos podem ser aplicados a muitos outros controles. 
 
## <a name="prerequisites"></a>Pré-requisitos

- Presumimos que você saiba como usar um controle de exibição de lista. Para obter mais informações, consulte o artigo [ListView e GridView](listview-and-gridview.md).
- Também presumimos que você entenda os estilos e modelos de controle, inclusive como usar um estilo embutido ou como um recurso. Para obter mais informações, consulte [Aplicando estilos a controles](xaml-styles.md) e [Modelos de controle](control-templates.md).

## <a name="the-data"></a>Os dados

Antes de nos aprofundarmos mais em como mostrar itens de dados em uma exibição de lista, precisamos entender os dados a serem mostrados. Neste exemplo, criamos um tipo de dados chamado `NamedColor`. Ele combina um nome de cor, um valor de cor e um **SolidColorBrush** da cor, que é exposto como 3 propriedades: `Name`, `Color` e `Brush`.
 
Preenchemos, então, uma **List** com um objeto `NamedColor` para cada cor designada na classe [Colors](https://docs.microsoft.com/uwp/api/windows.ui.colors). A lista é definida como o [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) para a exibição de lista.

Este é o código para definir a classe e popular a lista `NamedColors`.

**C#**
```csharp
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

namespace ColorsListApp
{
    public sealed partial class MainPage : Page
    {
        // The list of colors won't change after it's populated, so we use List<T>. 
        // If the data can change, we should use an ObservableCollection<T> intead.
        List<NamedColor> NamedColors = new List<NamedColor>();

        public MainPage()
        {
            this.InitializeComponent();

            // Use reflection to get all the properties of the Colors class.
            IEnumerable<PropertyInfo> propertyInfos = typeof(Colors).GetRuntimeProperties();

            // For each property, create a NamedColor with the property name (color name),
            // and property value (color value). Add it the NamedColors list.
            for (int i = 0; i < propertyInfos.Count(); i++)
            {
                NamedColors.Add(new NamedColor(propertyInfos.ElementAt(i).Name,
                                    (Color)propertyInfos.ElementAt(i).GetValue(null)));
            }

            colorsListView.ItemsSource = NamedColors;
        }
    }

    class NamedColor
    {
        public NamedColor(string colorName, Color colorValue)
        {
            Name = colorName;
            Color = colorValue;
        }

        public string Name { get; set; }

        public Color Color { get; set; }

        public SolidColorBrush Brush
        {
            get { return new SolidColorBrush(Color); }
        }
    }
}
```

## <a name="data-template"></a>Modelo de dados

Você especifica um modelo de dados para dizer à exibição de lista como seu item de dados deve ser mostrado. 

Por padrão, o item de dados aparece na exibição de lista como a representação em cadeia de caracteres do objeto de dados ao qual ele está associado. Se você mostrar os dados de 'NamedColors' em uma exibição de lista sem dizer à exibição de lista como eles deve aparecer, ela mostrará apenas o que o método **ToString** retornar, desta forma.

**XAML**
```xaml
<ListView x:Name="colorsListView"/>
```

![exibição de lista mostrando a representação em cadeia de caracteres dos itens](images/listview-no-template.png)

É possível mostrar a representação de cadeia de caracteres de uma propriedade específica do item de dados definindo [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) como essa propriedade. Aqui, você define DisplayMemberPath como a propriedade `Name` do item `NamedColor`.

**XAML**
```xaml
<ListView x:Name="colorsListView" DisplayMemberPath="Name" />
```

Agora a exibição de lista exibe os itens por nome, conforme mostrado aqui. É mais útil, mas não é muito interessante e deixa muitas informações ocultas.

![Exibição de lista mostrando a representação em cadeia de caracteres de uma propriedade de item](images/listview-display-member-path.png)

Você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens aparecem na exibição de lista, crie um [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo estático definido embutido. Atribua o DataTemplate à propriedade [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) do controle da lista.

> [!IMPORTANT]
> Não é possível usar **ItemTemplate** e **DisplayMemberPath** ao mesmo tempo. Se as duas propriedades forem definidas, ocorrerá uma exceção.

Aqui, você define um DataTemplate que mostra um elemento [Rectangle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.rectangle) na cor do item, além dos valores RGB e do nome da cor. 

> [!NOTE]
> Ao usar a [extensão de marcação x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) em um DataTemplate, você precisa especificar o DataType (`x:DataType`) no DataTemplate.

**XAML**
```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:NamedColor">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition MinWidth="54"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition Width="32"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Grid.RowDefinitions>
                    <RowDefinition/>
                    <RowDefinition/>
                </Grid.RowDefinitions>
                <Rectangle Width="44" Height="44" Fill="{x:Bind Brush}" Grid.RowSpan="2"/>
                <TextBlock Text="{x:Bind Name}" Grid.Column="1" Grid.ColumnSpan="4"/>
                <TextBlock Text="{x:Bind Color.R}" Grid.Column="1" Grid.Row="1" Foreground="Red"/>
                <TextBlock Text="{x:Bind Color.G}" Grid.Column="2" Grid.Row="1" Foreground="Green"/>
                <TextBlock Text="{x:Bind Color.B}" Grid.Column="3" Grid.Row="1" Foreground="Blue"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Veja a seguir a aparência dos itens de dados quando são exibidos com este modelo de dados.

![Itens de exibição de lista com um modelo de dados](images/listview-data-template-0.png)

> [!IMPORTANT]
> Por padrão, o ListViewItems têm seu conteúdo alinhado à esquerda. O [HorizontalContentAlignmentProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment#Windows_UI_Xaml_Controls_Control_HorizontalContentAlignment) dele, por exemplo, é definido como Left. Se você tiver vários elementos horizontalmente adjacentes dentro de um ListViewItem, como elementos empilhados horizontalmente ou colocados na mesma linha de grade, todos eles serão alinhados à esquerda e serão separados apenas por sua margem definida. 
<br/><br/> Para ter elementos espalhados que preencham todo o corpo de um ListItem, é necessário definir o HorizontalContentAlignmentProperty como [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) usando um [Setter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter) dentro do seu ListView:

```xaml
<ListView.ItemContainerStyle>
    <Style TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>
</ListView.ItemContainerStyle>
```


Você pode querer mostrar os dados em uma GridView. Este é outro modelo de dados que mostra os dados de maneira mais adequada para um layout de grade. Desta vez, o modelo de dados é definido como um recurso em vez de ser embutido com o XAML da GridView.


**XAML**
```xaml
<Page.Resources>
    <DataTemplate x:Key="namedColorItemGridTemplate" x:DataType="local:NamedColor">
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
                <ColumnDefinition Width="32"/>
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="96"/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
    
            <Rectangle Width="96" Height="96" Fill="{x:Bind Brush}" Grid.ColumnSpan="3" />
            <!-- Name -->
            <Border Background="#AAFFFFFF" Grid.ColumnSpan="3" Height="40" VerticalAlignment="Top">
                <TextBlock Text="{x:Bind Name}" TextWrapping="Wrap" Margin="4,0,0,0"/>
            </Border>
            <!-- RGB -->
            <Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
            <TextBlock Text="{x:Bind Color.R}" Foreground="Red"
                   Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.G}" Foreground="Green"
                   Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
            <TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
                   Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
            <!-- HEX -->
            <Border Background="Gray" Grid.Row="2" Grid.ColumnSpan="3">
                <TextBlock Text="{x:Bind Color}" Foreground="White" Margin="4,0,0,0"/>
            </Border>
        </Grid>
    </DataTemplate>
</Page.Resources>

...

<GridView x:Name="colorsGridView" 
          ItemTemplate="{StaticResource namedColorItemGridTemplate}"/>
```

Quando os dados são mostrados em uma grade usando este modelo de dados, eles têm esta aparência.

![Itens de exibição de grade com um modelo de dados](images/gridview-data-template.png)

### <a name="performance-considerations"></a>Considerações sobre desempenho

Os modelos de dados são a principal maneira de definir a aparência de sua exibição de lista. Eles também poderão causar um impacto significativo no desempenho se sua lista exibir um grande número de itens. 

Uma instância de cada elemento XAML em um modelo de dados é criada para cada item da exibição de lista. Por exemplo, o modelo de grade do exemplo anterior tem 10 elementos XAML (1 Grid, 1 Rectangle, 3 Borders, 5 TextBlocks). Uma GridView que mostra 20 itens na tela usando esse modelo de dados cria pelo menos 200 elementos (20 x 10 = 200). Reduzir o número de elementos em um modelo de dados pode reduzir significativamente o número total de elementos criados para a exibição de lista. Para saber mais, confira a [otimização das interfaces do usuário ListView e GridView: redução de contagem de elemento por item](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

 Considere esta seção do modelo de dados de grade. Vejamos algumas coisas que reduzem o número de elementos.

**XAML**
```xaml
<!-- RGB -->
<Border Background="Gainsboro" Grid.Row="1" Grid.ColumnSpan="3"/>
<TextBlock Text="{x:Bind Color.R}" Foreground="Red"
           Grid.Column="0" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.G}" Foreground="Green"
           Grid.Column="1" Grid.Row="1" HorizontalAlignment="Center"/>
<TextBlock Text="{x:Bind Color.B}" Foreground="Blue" 
           Grid.Column="2" Grid.Row="1" HorizontalAlignment="Center"/>
```

 - Primeiro, o layout usa um único Grid. Você pode ter um Grid de uma coluna e colocar estes 3 TextBlocks em um StackPanel, mas em um modelo de dados que é criado muitas vezes, é preciso procurar formas de evitar a inserção de painéis de layout dentro de outros painéis de layout.
 - Segundo, você pode usar um controle Border para renderizar uma tela de fundo sem colocar de fato itens dentro do elemento Border. Um elemento Border pode ter apenas um elemento filho. Então, você precisa adicionar um painel de layout para hospedar os 3 elementos TextBlock dentro do elemento Border no XAML. Ao não tornar os TextBlocks filhos do elemento Border, você elimina a necessidade de um painel para conter os TextBlocks.
 - Por fim, você poderia colocar os TextBlocks dentro de um StackPanel e definir as propriedades da borda no StackPanel em vez de usar um elemento Border explícito. No entanto, o elemento Border é um controle mais leve do que um StackPanel. Então, ele tem menos impacto no desempenho quando renderizado muitas vezes.

### <a name="using-different-layouts-for-different-items"></a>Usar diferentes layouts para diferentes itens


## <a name="control-template"></a>Modelo de controle
O modelo de controle de um item contém os elementos visuais que exibem o estado, como seleção, posição do ponteiro e foco. Esses elementos visuais são renderizados acima ou abaixo do modelo de dados. Alguns dos elementos visuais padrão comuns desenhados pelo modelo de controle ListView são mostrados aqui.

- Foco – um retângulo cinza-claro desenhado abaixo o modelo de dados.  
- Seleção – um retângulo azul-claro desenhado abaixo do modelo de dados. 
- Foco do teclado – um [visual de foco de alta visibilidade](/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals) desenhado sobre o modelo de item.

![Elementos visuais de estado da exibição de lista](images/listview-state-visuals.png)

A exibição de lista combina os elementos do modelo de dados e do modelo de controle para criar os elementos visuais finais renderizados na tela. Aqui, os elementos visuais de estado são mostrados no contexto de uma exibição de lista.

![Exibição de lista com itens em diferentes estados](images/listview-states.png)

### <a name="listviewitempresenter"></a>ListViewItemPresenter

Conforme observado anteriormente sobre modelos de dados, o número de elementos XAML criados para cada item pode ter um impacto significativo no desempenho de uma exibição de lista. Como o modelo de dados e o modelo de controle são combinados para exibir cada item, o número real de elementos necessários para exibir um item inclui os elementos em ambos os modelos.

Os controles ListView e GridView são otimizados para reduzir o número de elementos XAML criados por item. O elementos visuais **ListViewItem** são criados pelo [ListViewItemPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter), um elemento XAML especial que exibe elementos visuais complexos para foco, seleção e outros estados visuais sem a sobrecarga de muitos elementos da interface de usuário.
 
> [!NOTE]
> Em aplicativos UWP para Windows 10, **ListViewItem** e **GridViewItem** usam **ListViewItemPresenter**; o GridViewItemPresenter está preterido e você não deve usá-lo. ListViewItem e GridViewItem definem valores de propriedade diferentes em ListViewItemPresenter para obter aparências padrão diferentes.)

Para modificar a aparência do contêiner de item, use a propriedade [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) e forneça um [Style](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style) com o [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.targettype) definido como **ListViewItem** ou **GridViewItem**.

Neste exemplo, você pode adicionar preenchimento a ListViewItem para criar um espaço entre os itens na lista.

```xaml
<ListView x:Name="colorsListView">
    <ListView.ItemTemplate>
        <!-- DataTemplate XAML shown in previous ListView example -->
    </ListView.ItemTemplate>

    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="Padding" Value="0,4"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

Agora a exibição de lista tem esta aparência com um espaço entre os itens.

![Itens da exibição de lista com preenchimento aplicado](images/listview-data-template-1.png)

No estilo ListViewItem padrão, a propriedade **ContentMargin** de ListViewItemPresenter tem um [TemplateBinding](https://docs.microsoft.com/windows/uwp/xaml-platform/templatebinding-markup-extension) à propriedade **Padding** (`<ListViewItemPresenter ContentMargin="{TemplateBinding Padding}"/>`) de ListViewItem. Quando definimos a propriedade Padding, esse valor realmente está sendo passado para a propriedade ContentMargin de ListViewItemPresenter.

Para modificar outras propriedades ListViewItemPresenter que não estejam associadas às propriedades ListViewItems por modelo, você precisa recriar o modelo de ListViewItem com um novo ListViewItemPresenter em que seja possível modificar as propriedades. 

> [!NOTE]
> Os estilos padrão ListViewItem e GridViewItem definem muitas propriedades em ListViewItemPresenter. Você sempre deve começar com uma cópia do estilo padrão e modificar apenas as propriedades necessárias. Caso contrário, os elementos visuais provavelmente não aparecerão conforme o esperado porque algumas propriedades não serão definidas corretamente.

**Fazer uma cópia do modelo padrão no Visual Studio**
 
1. Abra o painel Estrutura de Tópicos do Documento (**Exibir > Outras Janelas > Estrutura de Tópicos do Documento**).
2. Selecione o elemento de lista ou grade para modificar. Neste exemplo, você modifica o elemento `colorsGridView`.
3. Clique com o botão direito do mouse e selecione **Editar Modelos Adicionais > Editar Contêiner de Itens Gerados (ItemContainerStyle) > Editar uma Cópia**.
    ![Editor do Visual Studio](images/listview-itemcontainerstyle-vs.png)
4. Na caixa de diálogo Criar Recurso de Estilo, insira um nome para o estilo. Neste exemplo, você usa `colorsGridViewItemStyle`.
    ![Visual Studio Create Style Resource dialog(images/listview-style-resource-vs.png)

Uma cópia do estilo padrão é adicionada ao seu aplicativo como um recurso e a propriedade **GridView.ItemContainerStyle** é definida como esse recurso, conforme mostrado neste XAML. 

```xaml
<Style x:Key="colorsGridViewItemStyle" TargetType="GridViewItem">
    <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
    <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}" />
    <Setter Property="Background" Value="Transparent"/>
    <Setter Property="Foreground" Value="{ThemeResource SystemControlForegroundBaseHighBrush}"/>
    <Setter Property="TabNavigation" Value="Local"/>
    <Setter Property="IsHoldingEnabled" Value="True"/>
    <Setter Property="HorizontalContentAlignment" Value="Center"/>
    <Setter Property="VerticalContentAlignment" Value="Center"/>
    <Setter Property="Margin" Value="0,0,4,4"/>
    <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
    <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="GridViewItem">
                <ListViewItemPresenter 
                    CheckBrush="{ThemeResource SystemControlForegroundBaseMediumHighBrush}" 
                    ContentMargin="{TemplateBinding Padding}" 
                    CheckMode="Overlay" 
                    ContentTransitions="{TemplateBinding ContentTransitions}" 
                    CheckBoxBrush="{ThemeResource SystemControlBackgroundChromeMediumBrush}" 
                    DragForeground="{ThemeResource ListViewItemDragForegroundThemeBrush}" 
                    DragOpacity="{ThemeResource ListViewItemDragThemeOpacity}" 
                    DragBackground="{ThemeResource ListViewItemDragBackgroundThemeBrush}" 
                    DisabledOpacity="{ThemeResource ListViewItemDisabledThemeOpacity}" 
                    FocusBorderBrush="{ThemeResource SystemControlForegroundAltHighBrush}" 
                    FocusSecondaryBorderBrush="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    HorizontalContentAlignment="{TemplateBinding HorizontalContentAlignment}" 
                    PointerOverForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    PressedBackground="{ThemeResource SystemControlHighlightListMediumBrush}" 
                    PlaceholderBackground="{ThemeResource ListViewItemPlaceholderBackgroundThemeBrush}" 
                    PointerOverBackground="{ThemeResource SystemControlHighlightListLowBrush}" 
                    ReorderHintOffset="{ThemeResource GridViewItemReorderHintThemeOffset}" 
                    SelectedPressedBackground="{ThemeResource SystemControlHighlightListAccentHighBrush}" 
                    SelectionCheckMarkVisualEnabled="True" 
                    SelectedForeground="{ThemeResource SystemControlForegroundBaseHighBrush}" 
                    SelectedPointerOverBackground="{ThemeResource SystemControlHighlightListAccentMediumBrush}" 
                    SelectedBackground="{ThemeResource SystemControlHighlightAccentBrush}" 
                    VerticalContentAlignment="{TemplateBinding VerticalContentAlignment}"/>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>

...

<GridView x:Name="colorsGridView" ItemContainerStyle="{StaticResource colorsGridViewItemStyle}"/>
```

Agora você pode modificar as propriedades no ListViewItemPresenter para controlar a caixa de seleção, o posicionamento de itens e as cores do pincel para estados visuais. 

#### <a name="inline-and-overlay-selection-visuals"></a>Elementos visuais de seleção embutidos e de sobreposição

O ListView e GridView indicam os itens selecionados de maneiras diferentes dependendo do controle e do [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode). Para saber mais sobre a seleção da exibição de lista, veja [ListView e GridView](listview-and-gridview.md). 

Quando **SelectionMode** é definido como **Multiple**, uma caixa de seleção é mostrada como parte do modelo de controle do item. Você pode usar a propriedade [SelectionCheckMarkVisualEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled) para desativar a caixa de seleção no modo de seleção Múltiplo. No entanto, essa propriedade é ignorada em outros modos de seleção. Portanto, você não pode ativar a caixa de seleção no modo de seleção Estendida ou Única.

Você pode definir a propriedade [CheckMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter.checkmode) para especificar se a caixa de seleção é mostrada usando o estilo de sobreposição ou o estilo embutido.

- **Embutido**: esse estilo mostra a caixa de seleção à esquerda do conteúdo e as colore a tela de fundo do contêiner do item para indicar a seleção. Este é o estilo padrão para ListView.
- **Sobreposição**: esse estilo exibe a caixa de seleção na parte superior do conteúdo e colore apenas a borda do contêiner do item para indicar a seleção. Este é o estilo padrão para GridView.

Esta tabela mostra os elementos visuais padrão usados para indicar a seleção.

SelectionMode:&nbsp;&nbsp; | Single/Extended | Vários
---------------|-----------------|---------
Embutido | ![Seleção única ou estendida embutida](images/listview-single-selection.png) | ![Seleção múltipla embutida](images/listview-multi-selection.png)
Sobreposição | ![Seleção única ou estendida sobreposta](images/gridview-single-selection.png) | ![Seleção múltipla sobreposta](images/gridview-multi-selection.png)

> [!NOTE]
> Neste e nos próximos exemplos, itens de dados da cadeia de caracteres simples são mostrados sem modelos de dados para enfatizar os elementos visuais fornecidos pelo modelo de controle.

Também há várias propriedades de pincel para alterar as cores da caixa de seleção. Vamos dar uma olhada nelas em seguidas com outras propriedades de pincel.

#### <a name="brushes"></a>Pincéis 

Muitas das propriedades especificam os pincéis usados para diferentes estados visuais. Você pode querer modificá-las para combinar com a cor de sua marca. 

Esta tabela mostra os estados visuais comum e de seleção para ListViewItem e os pincéis usados para renderizar os elementos visuais para cada estado. As imagens mostram os efeitos dos pincéis sobre os estilos visuais de seleção embutida e sobreposta.

> [!NOTE]
> Nesta tabela, os valores de cor modificados para os pincéis são cores nomeadas em código e as cores são selecionadas para tornar mais aparente onde elas são aplicadas no modelo. Estas não são as cores padrão dos estados visuais. Se você modificar as cores padrão em seu aplicativo, use os recursos de pincel para modificar os valores de cor como feito no modelo padrão.

Nome do estado/pincel | Estilo embutido | Estilo de sobreposição
------------|--------------|--------------
<b>Normal</b><ul><li><b>CheckBoxBrush="Red"</b></li></ul> | ![Seleção normal de itens embutidos](images/listview-item-normal.png) | ![Seleção normal de itens sobrepostos](images/gridview-item-normal.png)
<b>PointerOver</b><ul><li><b>PointerOverForeground="DarkOrange"</b></li><li><b>PointerOverBackground="MistyRose"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Ponteiro sobre seleção de itens embutidos](images/listview-item-pointerover.png) | ![Ponteiro sobre seleção de itens sobrepostos](images/gridview-item-pointerover.png)
<b>Pressed</b><ul><li><b>PressedBackground="LightCyan"</b></li><li>PointerOverForeground="DarkOrange"</li><li>CheckBoxBrush="Red"</li></ul> | ![Seleção de itens embutidos pressionados](images/listview-item-pressed.png) | ![Seleção de itens sobrepostos pressionados](images/gridview-item-pressed.png)
<b>Selected</b><ul><li><b>SelectedForeground="Navy"</b></li><li><b>SelectedBackground="Khaki"</b></li><li><b>CheckBrush="Green"</b></li><li>CheckBoxBrush="Red" (somente embutido)</li></ul> | ![Seleção de itens embutidos selecionados](images/listview-item-selected.png) | ![Seleção de itens sobrepostos selecionados](images/gridview-item-selected.png)
<b>PointerOverSelected</b><ul><li><b>SelectedPointerOverBackground="Lavender"</b></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (somente sobreposição)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (somente embutido)</li></ul> | ![Ponteiro sobre seleção de itens embutidos selecionados](images/listview-item-pointeroverselected.png) | ![Ponteiro sobre seleção de itens sobrepostos selecionados](images/gridview-item-pointeroverselected.png)
<b>PressedSelected</b><ul><li><b>SelectedPressedBackground="MediumTurquoise"</b></li></li><li>SelectedForeground="Navy"</li><li>SelectedBackground="Khaki" (somente sobreposição)</li><li>CheckBrush="Green"</li><li>CheckBoxBrush="Red" (somente embutido)</li></ul> | ![Seleção de itens embutidos pressionados selecionados](images/listview-item-pressedselected.png) | ![Seleção de itens sobrepostos pressionados selecionados](images/gridview-item-pressedselected.png)
<b>Focused</b><ul><li><b>FocusBorderBrush="Crimson"</b></li><li><b>FocusSecondaryBorderBrush="Gold"</b></li><li>CheckBoxBrush="Red"</li></ul> | ![Seleção de itens embutidos focalizados](images/listview-item-focused.png) | ![Seleção de itens sobrepostos focalizados](images/gridview-item-focused.png)

ListViewItemPresenter tem outras propriedades de pincel para estados de espaços reservados de dados e arrastar. Se você usa o carregamento incremental ou arrastar e soltar em sua exibição de lista, considere se é necessário também modificar essas propriedades adicionais do pincel. Veja a classe ListViewItemPresenter para obter a lista completa das propriedades que você pode modificar. 

### <a name="expanded-xaml-item-templates"></a>Modelos de item XAML expandidos

Se você precisar fazer modificações mais do que o que é permitido pelas propriedades **ListViewItemPresenter** – se precisar alterar a posição da caixa de seleção, por exemplo – poderá usar os modelos *ListViewItemExpanded* ou *GridViewItemExpanded*. Esses modelos são incluídos com os estilos padrão em generic.xaml. Eles seguem o padrão XAML usual de criação de todos os elementos visuais de UIElements individuais.

Como mencionado anteriormente, o número de UIElements em um modelo de item tem um impacto significativo no desempenho de sua exibição de lista. A substituição de ListViewItemPresenter pelos modelos XAML expandidos aumenta consideravelmente o número de elementos e não é recomendada quando a exibição de lista mostrará um grande número de itens ou quando o desempenho for uma preocupação.

> [!NOTE]
> **ListViewItemPresenter** terá suporte apenas quando o [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) da exibição de lista for um [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid) ou [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel). Se você alterar o ItemsPanel para usar [VariableSizedWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid), [WrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.wrapgrid) ou [StackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel), o modelo de item será alternado automaticamente para o modelo XAML expandido. Para saber mais, confira a [otimização das interfaces do usuário ListView e GridView](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview).

Para personalizar um modelo XAML expandido, você precisa fazer uma cópia dele em seu aplicativo e definir a propriedade **ItemContainerStyle** para sua cópia.

**Para copiar o modelo expandido**
1. Defina a propriedade ItemContainerStyle conforme mostrado aqui para ListView ou GridView.
    ```xaml
    <ListView ItemContainerStyle="{StaticResource ListViewItemExpanded}"/>
    <GridView ItemContainerStyle="{StaticResource GridViewItemExpanded}"/>
    ```
2. No painel Propriedades do Visual Studio, expanda a seção Diversos e localize a propriedade ItemContainerStyle. (Certifique-se de que ListView ou GridView esteja selecionado.)
3. Clique no marcador de propriedade para a propriedade ItemContainerStyle. (É a caixa pequena ao lado de TextBox. Está colorida em verde para mostrar que está definida como um StaticResource). O menu Propriedade é aberto.
4. No menu de propriedades, clique em **Converter em Novo Recurso**. 
    
    ![Menu de propriedades do Visual Studio](images/listview-convert-resource-vs.png)
5. Na caixa de diálogo Criar Recurso de Estilo, insira um nome para o recurso e clique em OK.

Uma cópia do modelo expandido de generic.xaml será criada em seu aplicativo, que pode ser modificada conforme necessário.


## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [ListView e GridView](listview-and-gridview.md)

