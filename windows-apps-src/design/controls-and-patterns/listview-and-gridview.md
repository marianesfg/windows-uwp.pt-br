---
Description: Use ListView and GridView controls to display and manipulate sets of data, such as a gallery of images or a set of email messages.
title: Exibição de lista e exibição de grade
label: List view and grid view
template: detail.hbs
ms.date: 05/20/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f8532ba0-5510-4686-9fcf-87fd7c643e7b
pm-contact: predavid
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8c748efaa242fb6fc59c6346aa9c893bc35fde5c
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7719773"
---
# <a name="list-view-and-grid-view"></a>Exibição de lista e exibição de grade

A maioria dos aplicativos manipula e exibe conjuntos de dados, como uma galeria de imagens ou um conjunto de mensagens de email. A estrutura da IU XAML fornece controles ListView e GridView que tornam mais fácil exibir e manipular dados em seu aplicativo.  

> **APIs importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx), [propriedade ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx), [propriedade Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx)

Os controles ListView e GridView são derivados da classe ListViewBase, portanto, eles têm a mesma funcionalidade, mas exibem dados de modo diferente. Neste artigo, ao falarmos sobre ListView, as informações se aplicam aos controles ListView e GridView, a menos que especificado de outra forma. Poderemos nos referir a classes, como ListView ou ListViewItem, mas o prefixo "List" poderá ser substituído por "Grid" para o equivalente a grade correspondente (GridView ou GridViewItem). 

## <a name="is-this-the-right-control"></a>Este é o controle correto?

O ListView exibe dados empilhados verticalmente em uma única coluna. Ele é frequentemente usado para mostrar uma lista ordenada de itens, como uma lista de emails ou resultados de pesquisa. 

![Uma exibição de lista com dados agrupados](images/simple-list-view-phone.png)

O controle GridView apresenta uma coleção de itens em linhas e colunas, que podem ser roladas verticalmente. Os dados são empilhados horizontalmente até preencherem as colunas e, em seguida, passam para a próxima linha. Ele é frequentemente usado quando você precisa mostrar uma visualização rica de cada item que ocupa mais espaço, como uma galeria de fotos. 

![Exemplo de uma biblioteca de conteúdo](images/controls_list_contentlibrary.png)

Para obter uma comparação mais detalhada e orientação sobre qual controle usar, consulte [Listas](lists.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo e ver o <a href="xamlcontrolsgallery:/item/ListView">ListView</a> ou <a href="xamlcontrolsgallery:/item/GridView">GridView</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-list-view"></a>Criar um modo de exibição de lista

Modo de exibição de lista é um [ItemsControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.aspx) e, por isso, pode conter uma coleção de itens de qualquer tipo. Ele deve ter itens em sua coleção [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) para mostrar algo na tela. Para popular a exibição, você pode adicionar itens diretamente à coleção [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) ou definir a propriedade [ItemsSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) como uma fonte de dados. 

**Importante**&nbsp;&nbsp;Você pode usar Items ou ItemsSource para popular a lista, mas não pode usar os dois ao mesmo tempo. Se você definir a propriedade ItemsSource e adicionar um item em XAML, o item adicionado será ignorado. Se você definir a propriedade ItemsSource e adicionar um item à coleção Items no código, uma exceção será gerada.

> **Observação**&nbsp;&nbsp;Em muitos exemplos deste artigo, a coleção **Items** é preenchida diretamente para simplificar. No entanto, é mais comum que os itens de uma lista sejam provenientes de uma fonte dinâmica, como uma lista de livros de um banco de dados online. Você usa a propriedade **ItemsSource** para essa finalidade. 

### <a name="add-items-to-the-items-collection"></a>Adicionar itens à coleção Items

Você pode adicionar itens à coleção [Items](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.items.aspx) usando XAML ou código. Normalmente, você adiciona itens dessa maneira quando tem um pequeno número de itens que não mudam e são facilmente definidos no XAML ou ao gerar os itens em código no tempo de execução. 

Veja a seguir uma exibição de lista com itens definidos embutidos em XAML. Quando você define os itens em XAML, eles também são adicionados automaticamente à coleção Items.

**XAML**
```xaml
<ListView x:Name="listView1"> 
   <x:String>Item 1</x:String> 
   <x:String>Item 2</x:String> 
   <x:String>Item 3</x:String> 
   <x:String>Item 4</x:String> 
   <x:String>Item 5</x:String> 
</ListView>  
```

Veja a seguir a exibição de lista criada em código. A lista resultante é igual à criada anteriormente em XAML.

**C#**
```csharp
// Create a new ListView and add content. 
ListView listView1 = new ListView(); 
listView1.Items.Add("Item 1"); 
listView1.Items.Add("Item 2"); 
listView1.Items.Add("Item 3"); 
listView1.Items.Add("Item 4"); 
listView1.Items.Add("Item 5");
 
// Add the ListView to a parent container in the visual tree. 
stackPanel1.Children.Add(listView1); 
```

O ListView tem a aparência a seguir.

![Uma exibição de lista simples](images/listview-simple.png)

### <a name="set-the-items-source"></a>Definir a origem de itens

Geralmente, você usa uma exibição de lista para exibir dados de uma fonte, como um banco de dados ou a Internet. Para popular uma exibição de lista em uma fonte de dados, defina sua propriedade [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) como uma coleção de itens de dados.

Aqui, o ItemsSource da exibição de lista está definido em código diretamente como uma instância de uma coleção.

**C#**
```csharp 
// Instead of hard coded items, the data could be pulled 
// asynchronously from a database or the internet.
ObservableCollection<string> listItems = new ObservableCollection<string>();
listItems.Add("Item 1");
listItems.Add("Item 2");
listItems.Add("Item 3");
listItems.Add("Item 4");
listItems.Add("Item 5");

// Create a new list view, add content, 
ListView itemListView = new ListView();
itemListView.ItemsSource = listItems;

// Add the list view to a parent container in the visual tree.
stackPanel1.Children.Add(itemListView);
```

Você também pode associar a propriedade ItemsSource a uma coleção no XAML. Para saber mais sobre vinculação de dados, consulte [Visão geral de vinculação de dados](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

Aqui, o ItemsSource está associado a uma propriedade pública denominada `Items` que expõe a coleção de dados privados da página.

**XAML**
```xaml
<ListView x:Name="itemListView" ItemsSource="{x:Bind Items}"/>
```

**C#**
```csharp
private ObservableCollection<string> _items = new ObservableCollection<string>();

public ObservableCollection<string> Items
{
    get { return this._items; }
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Items.Add("Item 1");
    Items.Add("Item 2");
    Items.Add("Item 3");
    Items.Add("Item 4");
    Items.Add("Item 5");
}
```

Se você precisar mostrar dados agrupados em sua exibição de lista, deverá associar a um [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx). O CollectionViewSource age como um proxy para a classe da coleção em XAML e habilita o suporte a agrupamento. Para saber mais, consulte a [CollectionViewSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.data.collectionviewsource.aspx).

## <a name="data-template"></a>Modelo de dados

Um modelo de dados de um item define como os dados são visualizados. Por padrão, o item de dados aparece na exibição de lista como a representação em cadeia de caracteres do objeto de dados ao qual ele está associado. Você pode mostrar a representação em cadeia de caracteres de uma propriedade específica do item de dados definindo [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) como essa propriedade.

No entanto, você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens aparecem na exibição de lista, você cria um [DataTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.datatemplate.aspx). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo estático definido embutido. Você atribui o DataTemplate à propriedade [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) do controle de lista.

Neste exemplo, o item de dados é uma cadeia de caracteres simples. Você usa um DataTemplate para adicionar uma imagem à esquerda da cadeia de caracteres e mostra a cadeia em azul petróleo.

> **Observação**&nbsp;&nbsp;ao usar a [extensão de marcação x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) em um DataTemplate, você precisa especificar o DataType (`x:DataType`) no DataTemplate.

**XAML**
```XAML
<ListView x:Name="listView1">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="47"/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Image Source="Assets/placeholder.png" Width="32" Height="32" 
                       HorizontalAlignment="Left"/>
                <TextBlock Text="{x:Bind}" Foreground="Teal" 
                           FontSize="15" Grid.Column="1"/>
            </Grid> 
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

Veja a seguir a aparência dos itens de dados quando exibidos com este modelo de dados.

![Itens de exibição de lista com um modelo de dados](images/listview-itemstemplate.png)

Os modelos de dados são a principal maneira de definir a aparência de sua exibição de lista. Eles também poderão causar um impacto significativo no desempenho se sua lista exibir um grande número de itens. Neste artigo, vamos usar dados de cadeia de caracteres simples na maioria dos exemplos e não especificar um modelo de dados. Para obter mais informações e exemplos de como usar modelos de dados e contêineres de itens para definir a aparência dos itens em sua lista ou grade, consulte [Contêineres de e modelos de item](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Alterar o layout dos itens

Quando você adiciona itens a uma exibição de lista ou grade, o controle encapsula automaticamente cada item de um contêiner de itens e, em seguida, dispõe todos os contêineres. A forma como esses contêineres de itens são dispostos depende do [ItemsPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx) do controle.  
- Por padrão, **ListView** usa um [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), que gera uma lista vertical, como esta.

![Uma exibição de lista simples](images/listview-simple.png)

- **GridView** usa um [ItemsWrapGrid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemswrapgrid.aspx), que adiciona itens horizontalmente, encapsula e rola verticalmente, da seguinte maneira.

![Uma exibição de grade simples](images/gridview-simple.png)

Você pode modificar o layout dos itens ajustando as propriedades no painel de itens ou substituir o painel padrão por outro painel.

> Observação&nbsp;&nbsp;Tenha cuidado para não desabilitar a virtualização se alterar o ItemsPanel. O **ItemsStackPanel** e o **ItemsWrapGrid** dão suporte à virtualização, portanto, é seguro usá-los. Ao usar qualquer outro painel, talvez você desabilite a virtualização e degrade o desempenho da exibição de lista. Para obter mais informações, consulte os artigos sobre exibição de lista em [Desempenho](https://msdn.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui). 

Este exemplo mostra como fazer uma **ListView** dispor seus contêineres de itens em uma lista horizontal alterando a propriedade [Orientação ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.orientation.aspx) do **ItemsStackPanel**.
Como a exibição de lista rola verticalmente por padrão, você também precisa ajustar algumas propriedades no [ ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) interno para fazê-la rolar horizontalmente.
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx) para **Habilitado** ou **Automático**
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx) para **Automático** 
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx) para **Desabilitado** 
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) para **Oculto** 

> **Observação**&nbsp;&nbsp;Esses exemplos são mostrados com a largura da exibição de lista irrestrita, para que as barras de rolagem horizontais não sejam mostradas. Se você executar esse código, poderá definir `Width="180"` no ListView para mostrar as barras de rolagem.

**XAML**
```xaml
<ListView Height="60" 
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

A lista resultante tem esta aparência.

![Uma exibição de lista horizontal](images/listview-horizontal.png)

 No próximo exemplo, o **ListView** dispõe os itens em uma lista de encapsulamento vertical usando um **ItemsWrapGrid** em vez de um **ItemsStackPanel**. 
 
> **Observação**&nbsp;&nbsp;A altura da exibição de lista deve ser restrita para forçar o controle a encapsular os contêineres.

**XAML**
```xaml
<ListView Height="100"
          ScrollViewer.HorizontalScrollMode="Enabled" 
          ScrollViewer.HorizontalScrollBarVisibility="Auto"
          ScrollViewer.VerticalScrollMode="Disabled"
          ScrollViewer.VerticalScrollBarVisibility="Hidden">
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

A lista resultante tem esta aparência.

![Uma exibição de lista com layout de grade](images/listview-itemswrapgrid.png)

Se você mostrar dados agrupados em sua exibição de lista, o ItemsPanel determinará como os grupos de itens são dispostos, não como os itens individuais, estão dispostos. Por exemplo, se o ItemsStackPanel horizontal mostrado anteriormente é usado para mostrar dados agrupados, os grupos são organizados horizontalmente, mas os itens em cada grupo são ainda empilhados verticalmente, conforme mostrado aqui.

![Uma exibição de lista horizontal agrupada](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Interação e seleção de itens

Você pode escolher entre várias maneiras de permitir que um usuário interaja com uma exibição de lista. Por padrão, um usuário pode selecionar um único item. Você pode alterar a propriedade [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) para habilitar a multisseleção ou desabilitar a seleção. É possível definir a propriedade [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) para que um usuário clique em um item em vez de selecioná-lo para invocar uma ação (como um botão).

> **Observação**&nbsp;&nbsp;Tanto o ListView quanto o GridView usam a enumeração [ListViewSelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewselectionmode.aspx) para suas propriedades SelectionMode. IsItemClickEnabled é **Falso** por padrão, portanto, você precisa defini-lo para apenas habilitar o modo de clique.

Esta tabela mostra as maneiras como um usuário pode interagir com uma exibição de lista e como você pode responder à interação.

Para habilitar essa interação: | Use estas configurações: | Manipule este evento: | Para esta propriedade para obter o item selecionado:
----------------------------|---------------------|--------------------|--------------------------------------------
Nenhuma interação | [SelectionMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectionmode.aspx) = **Nenhum**, [IsItemClickEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isitemclickenabled.aspx) = **Falso** | N/A | N/A 
Seleção única | SelectionMode = **Único**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx), [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx)  
Seleção múltipla | SelectionMode = **Múltiplo**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Seleção estendida | SelectionMode = **Estendido**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx) | [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx)  
Clicar | SelectionMode = **Nenhum**, IsItemClickEnabled = **Verdadeiro** | [ItemClick](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.itemclick.aspx) | N/A 

> **Observação**&nbsp;&nbsp;A partir do Windows 10, você pode habilitar IsItemClickEnabled para disparar um evento ItemClick enquanto SelectionMode também está definido como Único, Múltiplo ou Estendido. Se você fizer isso, o evento ItemClick será gerado primeiro e, em seguida, o evento SelectionChanged será acionado. Em alguns casos, como se você estivesse navegando para outra página no manipulador de eventos ItemClick, o evento SelectionChanged não é gerado e o item não é selecionado.

Você pode definir essas propriedades em XAML ou em código, conforme mostrado aqui.

**XAML**
```xaml
<ListView x:Name="myListView" SelectionMode="Multiple"/>

<GridView x:Name="myGridView" SelectionMode="None" IsItemClickEnabled="True"/> 
```

**C#**
```csharp
myListView.SelectionMode = ListViewSelectionMode.Multiple; 

myGridView.SelectionMode = ListViewSelectionMode.None;
myGridView.IsItemClickEnabled = true;
```

### <a name="read-only"></a>Somente leitura

É possível definir a propriedade SelectionMode como **ListViewSelectionMode.None** para desabilitar a seleção de itens. Isso coloca o controle no modo somente leitura, para ser usado para exibição de dados, mas não para interação. O próprio controle não está desabilitado, somente a seleção de itens está desabilitada.

### <a name="single-selection"></a>Seleção única

Esta tabela descreve as interações com o teclado, o mouse e de toque quando o SelectionMode está definido como **Único**.

Tecla modificadora | Interação
-------------|------------
Nenhum(a) | <li>Um usuário pode selecionar um único item usando a barra de espaço, um clique do mouse ou um toque.</li>
Ctrl | <li>Um usuário pode desmarcar um único item usando a barra de espaço, um clique do mouse ou um toque.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>

Quando SelectionMode está definido como **Único**, você pode obter o item de dados selecionado a partir da propriedade [SelectedItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selecteditem.aspx). Você pode obter o índice na coleção do item selecionado com a propriedade [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectedindex.aspx). Se nenhum item estiver selecionado, SelectedItem será **nulo** e SelectedIndex será -1. 
 
Se você tentar definir um item que não está na coleção **Items** como o **SelectedItem**, a operação será ignorada e o SelectedItem será **nulo**. No entanto, se você tentar definir o **SelectedIndex** como um índice fora do intervalo de **Items** na lista, ocorrerá uma exceção **ArgumentException**. 

### <a name="multiple-selection"></a>Seleção múltipla

Esta tabela descreve as interações com o teclado, o mouse e de toque quando o SelectionMode está definido como **Múltiplo**.

Tecla modificadora | Interação
-------------|------------
Nenhum(a) | <li>Um usuário pode selecionar vários itens usando a barra de espaço, um clique do mouse ou um toque para alternar a seleção do item focalizado.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>
Shift | <li>Um usuário pode selecionar vários itens adjacentes clicando ou tocando no primeiro item da seleção e, em seguida, no último item da seleção.</li><li>Com as teclas de seta, um usuário pode criar uma seleção contígua começando com o item selecionado quando Shift é pressionada.</li>

### <a name="extended-selection"></a>Seleção estendida

Esta tabela descreve as interações com o teclado, o mouse e de toque quando o SelectionMode está definido como **Estendido**.

Tecla modificadora | Interação
-------------|------------
Nenhum(a) | <li>O comportamento é o mesmo que da seleção **Único**.</li>
Ctrl | <li>Um usuário pode selecionar vários itens usando a barra de espaço, um clique do mouse ou um toque para alternar a seleção do item focalizado.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>
Shift | <li>Um usuário pode selecionar vários itens adjacentes clicando ou tocando no primeiro item da seleção e, em seguida, no último item da seleção.</li><li>Com as teclas de seta, um usuário pode criar uma seleção contígua começando com o item selecionado quando Shift é pressionada.</li>

Quando SelectionMode está definido como **Múltiplo** ou **Estendido**, você pode obter os itens de dados selecionados a partir da propriedade [SelectedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selecteditems.aspx). 

As propriedades **SelectedIndex**, **SelectedItem** e **SelectedItems** são sincronizadas. Por exemplo, se você definir SelectedIndex como -1, SelectedItem será definido como **nulo** e SelectedItems ficará em branco. Se você definir SelectedItem como **nulo**, SelectedIndex será definido como -1 e SelectedItems ficará em branco.

No modo de seleção múltipla, **SelectedItem** contém o item que foi selecionado primeiro, e **Selectedindex** contém o índice do item que foi selecionado primeiro. 

### <a name="respond-to-selection-changes"></a>Responder a alterações de seleção

Para responder às alterações de seleção em uma exibição de lista, manipule o evento [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.selectionchanged.aspx). No código do manipulador de eventos, você pode obter a lista de itens selecionados a partir da propriedade [SelectionChangedEventArgs.AddedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.addeditems.aspx). Você pode obter todos os itens que foram desmarcados a partir da propriedade [SelectionChangedEventArgs.RemovedItems](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.selectionchangedeventargs.removeditems.aspx). As coleções AddedItems e RemovedItems contêm, no máximo, 1 item, a menos que o usuário selecione um intervalo de itens mantendo pressionada a tecla Shift.

Este exemplo mostra como manipular o evento **SelectionChanged** e acessar as diversas coleções de itens.

**XAML**
```xaml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
    <TextBlock x:Name="selectedItem"/>
    <TextBlock x:Name="selectedIndex"/>
    <TextBlock x:Name="selectedItemCount"/>
    <TextBlock x:Name="addedItems"/>
    <TextBlock x:Name="removedItems"/>
</StackPanel> 
```

**C#**
```csharp
private void ListView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (listView1.SelectedItem != null)
    {
        selectedItem.Text = 
            "Selected item: " + listView1.SelectedItem.ToString();
    }
    else
    {
        selectedItem.Text = 
            "Selected item: null";
    }
    selectedIndex.Text = 
        "Selected index: " + listView1.SelectedIndex.ToString();
    selectedItemCount.Text = 
        "Items selected: " + listView1.SelectedItems.Count.ToString();
    addedItems.Text = 
        "Added: " + e.AddedItems.Count.ToString();
    removedItems.Text = 
        "Removed: " + e.RemovedItems.Count.ToString();
}
```

### <a name="click-mode"></a>Modo de clique

Você pode alterar a exibição de lista para que um usuário clique em itens como botões em vez de selecioná-los. Por exemplo, isso é útil quando o seu aplicativo navega para uma nova página quando o seu usuário clica em um item em uma lista ou grade. Para habilitar esse comportamento:
- Defina **SelectionMode** como **Nenhum**.
- Defina **IsItemClickEnabled** como **verdadeiro**.
- Manipule o evento **ItemClick** para fazer algo quando o usuário clicar em um item.

Veja a seguir uma exibição de lista com itens clicáveis. O código no manipulador de eventos ItemClick navega para uma nova página.

**XAML**
```xaml
<ListView SelectionMode="None"
          IsItemClickEnabled="True" 
          ItemClick="ListView1_ItemClick">
    <x:String>Page 1</x:String>
    <x:String>Page 2</x:String>
    <x:String>Page 3</x:String>
    <x:String>Page 4</x:String>
    <x:String>Page 5</x:String>
</ListView>
```

**C#**
```csharp
private void ListView1_ItemClick(object sender, ItemClickEventArgs e)
{
    switch (e.ClickedItem.ToString())
    {
        case "Page 1":
            this.Frame.Navigate(typeof(Page1));
            break;

        case "Page 2":
            this.Frame.Navigate(typeof(Page2));
            break;

        case "Page 3":
            this.Frame.Navigate(typeof(Page3));
            break;

        case "Page 4":
            this.Frame.Navigate(typeof(Page4));
            break;

        case "Page 5":
            this.Frame.Navigate(typeof(Page5));
            break;

        default:
            break;
    }
}
```

### <a name="select-a-range-of-items-programmatically"></a>Selecionar um intervalo de itens de forma programática

Às vezes, você precisa manipular a seleção de itens de uma exibição de lista de modo programático. Por exemplo, você pode ter um botão **Selecionar tudo** para permitir que o usuário selecione todos os itens de uma lista. Nesse caso, geralmente, não é muito eficiente adicionar e remover itens da coleção SelectedItems individualmente. Cada alteração de itens provoca a ocorrência de um evento SelectionChanged, e quando você trabalha com os itens diretamente em vez de trabalhar com valores de índice, o item é desvirtualizado.

Os métodos [SelectAll](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectall.aspx), [SelectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectrange.aspx) e [DeselectRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.deselectrange.aspx) proporcionam uma forma mais eficiente de modificar a seleção do que a propriedade SelectedItems. Esses métodos selecionam ou desmarcam usando intervalos de índices de itens. Os itens virtualizados permanecem virtualizados, pois somente o índice é usado. Todos os itens do intervalo especificado são selecionados (ou desmarcados), independentemente do estado de seleção original. O evento SelectionChanged ocorre somente uma vez para cada chamada desses métodos.

> **Importante**&nbsp;&nbsp;Você deve chamar esses métodos apenas quando a propriedade SelectionMode for definida como Múltiplo ou Estendido. Se você chamar SelectRange quando SelectionMode for Único ou Nenhum, uma exceção será gerada.

Quando você selecionar itens usando intervalos de índice, use a propriedade [SelectedRanges](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.selectedranges.aspx) para obter todos os intervalos selecionados na lista.

Se o ItemsSource implementar [IItemsRangeInfo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.iitemsrangeinfo.aspx), e você usar esses métodos para modificar a seleção, as propriedades **AddedItems** e **RemovedItems** não serão definidas no SelectionChangedEventArgs. Definir essas propriedades requer a desvirtualização do objeto de item. Use a propriedade **SelectedRanges** para obter os itens.

Você pode selecionar todos os itens de uma coleção chamando o método SelectAll. No entanto, não há nenhum método correspondente para desmarcar todos os itens. Você pode desmarcar todos os itens chamando DeselectRange e transmitindo um [ItemIndexRange](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.aspx) com um valor de [FirstIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.firstindex.aspx) de 0 e um valor de [Length](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.itemindexrange.length.aspx) equivalente ao número de itens da coleção. 

**XAML**
```xaml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Item 1</x:String>
        <x:String>Item 2</x:String>
        <x:String>Item 3</x:String>
        <x:String>Item 4</x:String>
        <x:String>Item 5</x:String>
    </ListView>
</StackPanel>
```

**C#**
```csharp
private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.SelectAll();
    }
}

private void DeselectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (listView1.SelectionMode == ListViewSelectionMode.Multiple ||
        listView1.SelectionMode == ListViewSelectionMode.Extended)
    {
        listView1.DeselectRange(new ItemIndexRange(0, (uint)listView1.Items.Count));
    }
}
```

Para obter informações sobre como alterar a aparência dos itens selecionados, consulte [Contêineres e modelos de item](item-containers-templates.md).

### <a name="drag-and-drop"></a>Arrastar e soltar

Os controles ListView e GridView permitem arrastar e soltar itens dentro deles mesmos e entre si e outros controles ListView e GridView. Para obter mais informações sobre como implementar o padrão arrastar e soltar, consulte [Arrastar e soltar](https://msdn.microsoft.com/windows/uwp/design/input/drag-and-drop). 

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo de XAML ListView e GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) -demonstra os controles ListView e GridView.
- [Exemplo de XAML arrastar e soltar](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) -demonstra arrastar e soltar com o controle ListView.
- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Arrastar e soltar](https://msdn.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
