---
Description: Use os controles ListView e GridView para exibir e manipular conjuntos de dados, como uma galeria de imagens ou um conjunto de mensagens de email.
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
ms.openlocfilehash: 2ff5d0831e918c0399bccb1dac9bb4fca8a6d408
ms.sourcegitcommit: c079388634cbd328d0d43e7a6185e09bb4bca65b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71939649"
---
# <a name="list-view-and-grid-view"></a>Exibição de lista e exibição de grade

A maioria dos aplicativos manipula e exibe conjuntos de dados, como uma galeria de imagens ou um conjunto de mensagens de email. A estrutura da IU XAML fornece controles ListView e GridView que tornam mais fácil exibir e manipular dados em seu aplicativo.  

> **APIs importantes**: [Classe ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), [Classe GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [Propriedade ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), [Propriedade Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items)

> [!NOTE]
> Os controles ListView e GridView são derivados da classe [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase), portanto, eles têm a mesma funcionalidade, mas exibem os dados de modo diferente. Neste artigo, ao falarmos sobre exibição de lista, as informações se aplicam aos controles ListView e GridView, a menos que especificado de outra forma. Poderemos nos referir a classes, como ListView ou ListViewItem, mas o prefixo *List* poderá ser substituído por *Grid* para o equivalente a grade correspondente (GridView ou GridViewItem). 

ListView e GridView fornecem muitos benefícios para trabalhar com coleções. Ambos são fáceis de serem implementados e oferecem uma interface do usuário básica; interação; e rolagem e ainda são facilmente personalizáveis. O ListView e o GridView podem ser associados a fontes de dados dinâmicos existentes ou dados embutidos em código fornecidos no próprio XAML/code-behind. 

Esses dois controles são flexíveis para muitos casos de uso, mas, de maneira geral, funcionam melhor com coleções nas quais todos os itens devem ter a mesma estrutura e aparência básicas, assim como o mesmo comportamento de interação – por exemplo, todos eles devem desempenhar a mesma ação quando clicados (abrir um link, navegar, etc.).


## <a name="differences-between-listview-and-gridview"></a>Diferenças entre ListView e GridView

### <a name="listview"></a>ListView
O ListView exibe dados empilhados verticalmente em uma única coluna. O ListView funciona melhor para itens que têm texto como ponto focal e para coleções que devem ser lidas de cima para baixo (ou seja, em ordem alfabética). Alguns casos de uso comuns para o ListView incluem listas de mensagens e resultados da pesquisa.

![Uma exibição de lista com dados agrupados](images/listview-grouped-example-resized-final.png)

### <a name="gridview"></a>GridView
O controle GridView apresenta uma coleção de itens em linhas e colunas, que podem ser roladas verticalmente. Os dados são empilhados horizontalmente até preencherem as colunas e, em seguida, passam para a próxima linha. O GridView funciona melhor para itens que têm imagens como ponto focal e para coleções que podem ser lidas lado a lado ou que não estão classificadas em uma ordem específica. Um caso de uso comum para o GridView é uma galeria de fotos ou produtos.

![Exemplo de uma biblioteca de conteúdo](images/gridview-simple-example-final.png)

## <a name="which-collection-control-should-you-use-a-comparison-with-itemsrepeater"></a>Qual controle de coleção você deve usar? Uma comparação com ItemsRepeater

ListView e GridView são controles que funcionam de maneira imediata para exibir qualquer coleção, com sua própria experiência do usuário e interface do usuário interna. O controle [ItemsRepeater](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) também é usado para exibir coleções, mas foi criado como um bloco de construção para criar um controle personalizado que atende às necessidades exatas de interface do usuário. As diferenças mais importantes que devem afetar qual controle você acaba usando são listadas abaixo:

-   ListView e GridView são controles ricos em recursos que exigem pouca personalização, mas que têm muito a oferecer. O ItemsRepeater é um bloco de construção para criar seu próprio controle de layout e não tem os mesmos recursos e funcionalidades internos – ele exige que você implemente recursos ou interações necessários.
-   ItemsRepeater deverá ser usado se você tiver uma interface do usuário altamente personalizada que não possa ser criada usando ListView ou GridView ou se você tiver uma fonte de dados que requeira um comportamento altamente diferente para cada item.


Saiba mais sobre o ItemsRepeater lendo suas páginas [Diretrizes](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/items-repeater) e [Documentação da API](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abri-lo e ver o <a href="xamlcontrolsgallery:/item/ListView">ListView</a> ou <a href="xamlcontrolsgallery:/item/GridView">GridView</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-listview-or-gridview"></a>Criar um ListView ou GridView

Tanto o ListView quanto o GridView são tipos [ItemsControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol) para que possam obter uma coleção de itens de qualquer tipo. Um ListView ou um GridView deve ter itens em sua coleção [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) antes que possa mostrar qualquer coisa na tela. Para preencher a exibição, você pode adicionar os itens diretamente à coleção [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou definir a propriedade [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) como uma fonte de dados. 

> [!IMPORTANT]
> Você pode usar Items ou ItemsSource para popular a lista, mas não pode usar os dois ao mesmo tempo. Se você definir a propriedade ItemsSource e adicionar um item em XAML, o item adicionado será ignorado. Se você definir a propriedade ItemsSource e adicionar um item à coleção Items no código, uma exceção será gerada.

Em muitos exemplos deste artigo, a coleção `Items` é preenchida diretamente para simplificar. No entanto, é mais comum que os itens de uma lista sejam provenientes de uma fonte dinâmica, como uma lista de livros de um banco de dados online. Use a propriedade `ItemsSource` para essa finalidade. 

### <a name="add-items-to-a-listview-or-gridview"></a>Adicionar Items a um ListView ou GridView

Você pode adicionar itens à coleção [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) do ListView ou GridView usando o XAML ou código para produzir o mesmo resultado. Normalmente, você adicionará itens por meio de XAML se tiver um pequeno número de itens que não mudam e são facilmente definidos ou se gerar os itens em código no tempo de execução. 

<u>Método 1: Adicionar itens à coleção Items</u>
#### <a name="option-1-add-items-through-xaml"></a>Opção 1: Adicionar itens por meio de XAML
```xml
<!-- No corresponding C# code is needed for this example. -->

<ListView x:Name="Fruits"> 
   <x:String>Apricot</x:String> 
   <x:String>Banana</x:String> 
   <x:String>Cherry</x:String> 
   <x:String>Orange</x:String> 
   <x:String>Strawberry</x:String> 
</ListView>  
```


#### <a name="option-2-add-items-through-c"></a>Opção 2: Adicionar itens por meio de C#

##### <a name="c-code"></a>Código em C#:
```csharp
// Create a new ListView and add content. 
ListView Fruits = new ListView(); 
Fruits.Items.Add("Apricot"); 
Fruits.Items.Add("Banana"); 
Fruits.Items.Add("Cherry"); 
Fruits.Items.Add("Orange"); 
Fruits.Items.Add("Strawberry");
 
// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file).
FruitsPanel.Children.Add(Fruits); 
```

##### <a name="corresponding-xaml-code"></a>Código XAML correspondente:
```xml
<StackPanel Name="FruitsPanel"></StackPanel>
```
As duas opções acima resultarão no mesmo ListView, mostrado abaixo:

![Uma exibição de lista simples](images/listview-basic-code-example2.png)
<br/>
<u> Método 2: Adicionar itens definindo ItemsSource</u>

Geralmente, você usa um ListView ou GridView para exibir dados de uma fonte, como um banco de dados ou a Internet. Para popular um ListView/GridView de uma fonte de dados, defina sua propriedade [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) como uma coleção de itens de dados. Esse método funcionará melhor se ListView ou GridView armazenar objetos de classe personalizados, conforme mostrado nos exemplos abaixo.

#### <a name="option-1-set-itemssource-in-c"></a>Opção 1: Definir ItemsSource em C#
Aqui, o ItemsSource da exibição de lista está definido em código diretamente como uma instância de uma coleção. 

##### <a name="c-code"></a>Código em C#:
```csharp 
// Class defintion should be provided within the namespace being used, outside of any other classes.

this.InitializeComponent();

// Instead of adding hard coded items to an ObservableCollection as shown below, 
//the data could be pulled asynchronously from a database or the internet.
ObservableCollection<Contact> Contacts = new ObservableCollection<Contact>();

// Contact objects are created by providing a first name, last name, and company for the Contact constructor.
// They are then added to the ObservableCollection Contacts.
Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));

// Create a new ListView (or GridView) for the UI, add content by setting ItemsSource
ListView ContactsLV = new ListView();
ContactsLV.ItemsSource = Contacts;

// Add the ListView to a parent container in the visual tree (that you created in the corresponding XAML file)
ContactPanel.Children.Add(ContactsLV);
```

##### <a name="xaml-code"></a>Código XAML:
```xml
<StackPanel x:Name="ContactPanel"></StackPanel>
```

#### <a name="option-2-set-itemssource-in-xaml"></a>Opção 2: Definir ItemsSource em XAML
Você também pode associar a propriedade ItemsSource a uma coleção no XAML. Aqui, o ItemsSource está associado a uma propriedade pública denominada `Contacts` que expõe a coleção de dados privados da página, `_contacts`.

**XAML**
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}"/>
```

**C#**
```csharp
// Class defintion should be provided within the namespace being used, outside of any other classes.
// These two declarations belong outside of the main page class.
private ObservableCollection<Contact> _contacts = new ObservableCollection<Contact>();

public ObservableCollection<Contact> Contacts
{
    get { return this._contacts; }
}

// This method should be defined within your main page class.
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    // Instead of hard coded items, the data could be pulled 
    // asynchronously from a database or the internet.
    Contacts.Add(new Contact("John", "Doe", "ABC Printers"));
    Contacts.Add(new Contact("Jane", "Doe", "XYZ Refridgerators"));
    Contacts.Add(new Contact("Santa", "Claus", "North Pole Toy Factory Inc."));
}
```

As duas opções acima resultarão no mesmo ListView, mostrado abaixo. O ListView mostra apenas a representação de cadeia de caracteres de cada item porque não fornecemos um modelo de dados.

![Uma exibição de lista simples com ItemsSource definido](images/listview-basic-code-example-final.png)

> [!IMPORTANT]
> Sem nenhum modelo de dados definido, os objetos de classe personalizados só serão exibidos no ListView com seu valor de cadeia de caracteres se tiverem um método [ToString()](https://docs.microsoft.com/uwp/api/windows.foundation.istringable.tostring) definido.

 A próxima seção detalhará como apresentar visualmente itens de classe simples e personalizados corretamente em um ListView ou GridView.

Para saber mais sobre vinculação de dados, consulte [Visão geral de vinculação de dados](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).

> [!NOTE]
> Se você precisar mostrar dados agrupados em seu ListView, deverá associar-se a um [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource). O CollectionViewSource age como um proxy para a classe da coleção em XAML e habilita o suporte a agrupamento. Para saber mais, consulte a [CollectionViewSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource).

## <a name="customizing-the-look-of-items-with-a-datatemplate"></a>Personalizar a aparência de itens com um DataTemplate

Um modelo de dados em um ListView ou GridView define como os itens/dados são visualizados. Por padrão, um item de dados é exibido no ListView como a representação de cadeia de caracteres do objeto de dados ao qual ele está associado. É possível mostrar a representação de cadeia de caracteres de uma propriedade específica do item de dados definindo [DisplayMemberPath](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.displaymemberpath) como essa propriedade.

No entanto, você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens no ListView/GridView são exibidos, crie um [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo estático definido embutido. 

> [!NOTE]
> Ao usar a [extensão de marcação x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) em um DataTemplate, você precisa especificar o DataType (`x:DataType`) no DataTemplate.

#### <a name="simple-listview-data-template"></a>Modelo de dados ListView simples
Neste exemplo, o item de dados é uma cadeia de caracteres simples. Um DataTemplate é definido embutido dentro da definição do ListView para adicionar uma imagem à esquerda da cadeia de caracteres e mostrar a cadeia de caracteres em azul petróleo. Este é o mesmo ListView criado usando o Método 1 e a Opção 1 mostrados acima.

**XAML**
```XML
<!--No corresponding C# code is needed for this example.-->
<ListView x:Name="FruitsList">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="x:String">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="47"/>
                                <ColumnDefinition/>
                            </Grid.ColumnDefinitions>
                            <Image Source="Assets/placeholder.png" Width="32" Height="32"
                                HorizontalAlignment="Left" VerticalAlignment="Center"/>
                            <TextBlock Text="{x:Bind}" Foreground="Teal" FontSize="14" 
                                Grid.Column="1" VerticalAlignment="Center"/>
                        </Grid>
                    </DataTemplate>
                </ListView.ItemTemplate>
                <x:String>Apricot</x:String>
                <x:String>Banana</x:String>
                <x:String>Cherry</x:String>
                <x:String>Orange</x:String>
                <x:String>Strawberry</x:String>
            </ListView>

```

Veja qual é a aparência dos itens de dados quando são exibidos com esse modelo de dados em um ListView:

![Itens ListView com um modelo de dados](images/listview-w-datatemplate1-final.png)

#### <a name="listview-data-template-for-custom-class-objects"></a>Modelo de dados ListView para objetos de classe personalizados
Neste exemplo, o item de dados é um objeto Contact. Um DataTemplate é definido embutido dentro da definição do ListView para adicionar uma imagem de contato à esquerda do Nome do contato e da empresa. Esse ListView foi criado usando o Método 2 e a Opção 2 mencionados acima.
```xml
<ListView x:Name="ContactsLV" ItemsSource="{x:Bind Contacts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Contact">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="*"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
                <Image Grid.Column="0" Grid.RowSpan="2" Source="Assets/grey-placeholder.png" Width="32"
                    Height="32" HorizontalAlignment="Center" VerticalAlignment="Center"></Image>
                <TextBlock Grid.Column="1" Text="{x:Bind Name}" Margin="12,6,0,0" 
                    Style="{ThemeResource BaseTextBlockStyle}"/>
                <TextBlock  Grid.Column="1" Grid.Row="1" Text="{x:Bind Company}" Margin="12,0,0,6" 
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Veja qual é a aparência dos itens de dados quando são exibidos usando esse modelo de dados em um ListView:

![Itens de classe personalizados ListView com um modelo de dados](images/listview-customclass-datatemplate-final.png)

Os modelos de dados são a principal maneira de definir a aparência do seu ListView. Eles também poderão causar um impacto significativo no desempenho se a lista armazenar um grande número de itens.  

Seu modelo de dados pode ser definido embutido dentro da definição ListView/GridView (mostrado acima) ou separadamente em uma seção Recursos. Se definido fora do próprio ListView/GridView, o DataTemplate deverá receber um atributo [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) e ser atribuído à propriedade [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) do ListView/GridView usando essa chave.

Para saber mais e exemplos de como usar modelos de dados e contêineres de itens para definir a aparência dos itens em sua lista ou grade, confira os [contêineres de e modelos de item](item-containers-templates.md). 

## <a name="change-the-layout-of-items"></a>Alterar o layout dos itens

Quando você adiciona itens a um ListView ou GridView, o controle encapsula automaticamente cada item de um contêiner de itens e, em seguida, dispõe todos os contêineres. A forma como esses contêineres de itens são dispostos depende do [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) do controle.  
- Por padrão, a **ListView** usa um [ItemsStackPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel), que gera uma lista vertical, como esta.

![Uma exibição de lista simples](images/listview-simple.png)

- **GridView** usa um [ItemsWrapGrid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemswrapgrid), que adiciona itens horizontalmente, encapsula e rola verticalmente, da seguinte maneira.

![Uma exibição de grade simples](images/gridview-simple.png)

Você pode modificar o layout dos itens ajustando as propriedades no painel de itens ou substituir o painel padrão por outro painel.

> [!NOTE]
> Tenha cuidado para não desabilitar a virtualização se alterar o ItemsPanel. O **ItemsStackPanel** e o **ItemsWrapGrid** dão suporte à virtualização, portanto, é seguro usá-los. Ao usar qualquer outro painel, talvez você desabilite a virtualização e degrade o desempenho da exibição de lista. Para obter mais informações, consulte os artigos sobre exibição de lista em [Desempenho](https://docs.microsoft.com/windows/uwp/debug-test-perf/performance-and-xaml-ui). 

Este exemplo mostra como fazer o **ListView** dispor seus contêineres de itens em uma lista horizontal alterando a propriedade [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemsstackpanel.orientation) do **ItemsStackPanel**.
Como a exibição de lista rola verticalmente por padrão, você também precisa ajustar algumas propriedades no [ScrollViewer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer) interno da exibição de lista para fazê-la rolar horizontalmente.
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode) para **Habilitado** ou **Automático**
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility) para **Automático** 
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode) para **Desabilitado** 
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) para **Oculto** 

> [!IMPORTANT]
> Esses exemplos são mostrados com a largura da exibição de lista irrestrita, para que as barras de rolagem horizontais não sejam mostradas. Se você executar esse código, poderá definir `Width="180"` no ListView para mostrar as barras de rolagem.

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

A lista resultante tem esta aparência.

![Uma exibição de lista horizontal](images/listview-horizontal2-final.png)

 No próximo exemplo, o **ListView** dispõe os itens em uma lista de encapsulamento vertical usando um **ItemsWrapGrid** em vez de um **ItemsStackPanel**. 
 
> [!IMPORTANT]
> A altura da exibição de lista deve ser restrita para forçar o controle a encapsular os contêineres.

**XAML**
```xml
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
    <x:String>Apricot</x:String>
    <x:String>Banana</x:String>
    <x:String>Cherry</x:String>
    <x:String>Orange</x:String>
    <x:String>Strawberry</x:String>
</ListView>
```

A lista resultante tem esta aparência.

![Uma exibição de lista com layout de grade](images/listview-itemswrapgrid2-final.png)

Se você mostrar os dados agrupados em sua exibição de lista, o ItemsPanel determinará como os grupos de itens, e não os itens individuais, estão dispostos. Por exemplo, se o ItemsStackPanel horizontal mostrado anteriormente for usado para mostrar os dados agrupados, os grupos serão organizados horizontalmente, mas os itens em cada grupo ainda serão empilhados verticalmente, conforme mostrado aqui.

![Uma exibição de lista horizontal agrupada](images/listview-horizontal-groups.png)

## <a name="item-selection-and-interaction"></a>Interação e seleção de itens

Você pode escolher entre várias maneiras de permitir que um usuário interaja com uma exibição de lista. Por padrão, um usuário pode selecionar um único item. Você pode alterar a propriedade [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) para habilitar a multisseleção ou desabilitar a seleção. É possível definir a propriedade [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) para que o usuário clique em um item para invocar uma ação (como um botão), em vez de selecioná-lo.

> **Observação**&nbsp;&nbsp;Tanto o ListView quanto o GridView usam a enumeração [ListViewSelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewselectionmode) para as propriedades SelectionMode. IsItemClickEnabled é **Falso** por padrão, portanto, você precisa defini-lo para apenas habilitar o modo de clique.

Esta tabela mostra as maneiras como um usuário pode interagir com uma exibição de lista e como você pode responder à interação.

Para habilitar essa interação: | Use estas configurações: | Manipule este evento: | Para esta propriedade para obter o item selecionado:
----------------------------|---------------------|--------------------|--------------------------------------------
Nenhuma interação | [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) = **Nenhum**, [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) = **Falso** | N/D | N/D 
Seleção única | SelectionMode = **Único**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem), [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)  
Seleção múltipla | SelectionMode = **Múltiplo**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Seleção estendida | SelectionMode = **Estendido**, IsItemClickEnabled = **Falso** | [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) | [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems)  
Clique | SelectionMode = **Nenhum**, IsItemClickEnabled = **Verdadeiro** | [ItemClick](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.itemclick) | N/D 

> **Observação**&nbsp;&nbsp;A partir do Windows 10, é possível habilitar o IsItemClickEnabled para disparar um evento ItemClick enquanto o SelectionMode também estiver definido como Único, Múltiplo ou Estendido. Se você fizer isso, o evento ItemClick será gerado primeiro e, em seguida, o evento SelectionChanged será acionado. Em alguns casos, como se você estivesse navegando para outra página no manipulador de eventos ItemClick, o evento SelectionChanged não é gerado e o item não é selecionado.

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
Nenhuma | <li>Um usuário pode selecionar um único item usando a barra de espaço, um clique do mouse ou um toque.</li>
Ctrl | <li>Um usuário pode desmarcar um único item usando a barra de espaço, um clique do mouse ou um toque.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>

Quando o SelectionMode é definido como **Único**, é possível obter o item de dados selecionado da propriedade [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem). Você pode obter o índice na coleção do item selecionado usando a propriedade [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex). Se nenhum item estiver selecionado, SelectedItem será **nulo** e SelectedIndex será -1. 
 
Se você tentar definir um item que não está na coleção **Items** como o **SelectedItem**, a operação será ignorada e o SelectedItem será **nulo**. No entanto, se você tentar definir o **SelectedIndex** como um índice fora do intervalo de **Items** na lista, ocorrerá uma exceção **ArgumentException**. 

### <a name="multiple-selection"></a>Seleção múltipla

Esta tabela descreve as interações com o teclado, o mouse e de toque quando o SelectionMode está definido como **Múltiplo**.

Tecla modificadora | Interação
-------------|------------
Nenhuma | <li>Um usuário pode selecionar vários itens usando a barra de espaço, um clique do mouse ou um toque para alternar a seleção do item focalizado.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>
Shift | <li>Um usuário pode selecionar vários itens adjacentes clicando ou tocando no primeiro item da seleção e, em seguida, no último item da seleção.</li><li>Com as teclas de seta, um usuário pode criar uma seleção contígua começando com o item selecionado quando Shift é pressionada.</li>

### <a name="extended-selection"></a>Seleção estendida

Esta tabela descreve as interações com o teclado, o mouse e de toque quando o SelectionMode está definido como **Estendido**.

Tecla modificadora | Interação
-------------|------------
Nenhuma | <li>O comportamento é o mesmo que da seleção **Único**.</li>
Ctrl | <li>Um usuário pode selecionar vários itens usando a barra de espaço, um clique do mouse ou um toque para alternar a seleção do item focalizado.</li><li>Com as teclas de seta, um usuário pode mover o foco independentemente da seleção.</li>
Shift | <li>Um usuário pode selecionar vários itens adjacentes clicando ou tocando no primeiro item da seleção e, em seguida, no último item da seleção.</li><li>Com as teclas de seta, um usuário pode criar uma seleção contígua começando com o item selecionado quando Shift é pressionada.</li>

Quando o SelectionMode é definido como **Múltiplo** ou **Estendido**, é possível obter os itens de dados selecionados da propriedade [SelectedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selecteditems). 

As propriedades **SelectedIndex**, **SelectedItem** e **SelectedItems** são sincronizadas. Por exemplo, se você definir SelectedIndex como -1, SelectedItem será definido como **nulo** e SelectedItems ficará em branco. Se você definir SelectedItem como **nulo**, SelectedIndex será definido como -1 e SelectedItems ficará em branco.

No modo de seleção múltipla, **SelectedItem** contém o item que foi selecionado primeiro, e **Selectedindex** contém o índice do item que foi selecionado primeiro. 

### <a name="respond-to-selection-changes"></a>Responder a alterações de seleção

Para responder às alterações de seleção em uma exibição de lista, manipule o evento [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). No código do manipulador de eventos, você pode obter a lista de itens selecionados da propriedade [SelectionChangedEventArgs.AddedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Você pode obter todos os itens que foram desmarcados da propriedade [SelectionChangedEventArgs.RemovedItems](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). As coleções AddedItems e RemovedItems contêm, no máximo, 1 item, a menos que o usuário selecione um intervalo de itens mantendo pressionada a tecla Shift.

Este exemplo mostra como manipular o evento **SelectionChanged** e acessar as diversas coleções de itens.

**XAML**
```xml
<StackPanel HorizontalAlignment="Right">
    <ListView x:Name="listView1" SelectionMode="Multiple" 
              SelectionChanged="ListView1_SelectionChanged">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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
```xml
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

Os métodos [SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectall), [SelectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectrange) e [DeselectRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.deselectrange) proporcionam uma forma mais eficiente de modificar a seleção do que a propriedade SelectedItems. Esses métodos selecionam ou desmarcam usando intervalos de índices de itens. Os itens virtualizados permanecem virtualizados, pois somente o índice é usado. Todos os itens do intervalo especificado são selecionados (ou desmarcados), independentemente do estado de seleção original. O evento SelectionChanged ocorre somente uma vez para cada chamada desses métodos.

> [!IMPORTANT]
> Você deverá chamar esses métodos apenas quando a propriedade SelectionMode for definida como Múltiplo ou Estendido. Se você chamar SelectRange quando SelectionMode for Único ou Nenhum, uma exceção será gerada.

Quando você selecionar itens usando intervalos de índice, use a propriedade [SelectedRanges](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectedranges) para obter todos os intervalos selecionados na lista.

Se o ItemsSource implementar [IItemsRangeInfo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.iitemsrangeinfo), e você usar esses métodos para modificar a seleção, as propriedades **AddedItems** e **RemovedItems** não serão definidas no SelectionChangedEventArgs. Definir essas propriedades requer a desvirtualização do objeto de item. Use a propriedade **SelectedRanges** para obter os itens.

Você pode selecionar todos os itens de uma coleção chamando o método SelectAll. No entanto, não há nenhum método correspondente para desmarcar todos os itens. Você pode desmarcar todos os itens chamando DeselectRange e transmitindo um [ItemIndexRange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange) com um valor [FirstIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.firstindex) de 0 e um valor [Length](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.itemindexrange.length) equivalente ao número de itens da coleção. Isso é mostrado no exemplo abaixo, junto com uma opção para selecionar todos os itens.

**XAML**
```xml
<StackPanel Width="160">
    <Button Content="Select all" Click="SelectAllButton_Click"/>
    <Button Content="Deselect all" Click="DeselectAllButton_Click"/>
    <ListView x:Name="listView1" SelectionMode="Multiple">
        <x:String>Apricot</x:String>
        <x:String>Banana</x:String>
        <x:String>Cherry</x:String>
        <x:String>Orange</x:String>
        <x:String>Strawberry</x:String>
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

Para saber mais sobre como alterar a aparência dos itens selecionados, confira os [contêineres e modelos de item](item-containers-templates.md).

### <a name="drag-and-drop"></a>Arrastar e soltar

Os controles ListView e GridView permitem arrastar e soltar itens dentro deles mesmos e entre si e outros controles ListView e GridView. Para obter mais informações sobre como implementar o padrão arrastar e soltar, consulte [Arrastar e soltar](https://docs.microsoft.com/windows/uwp/design/input/drag-and-drop). 

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo de XAML ListView e GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) - demonstra os controles ListView e GridView.
- [Exemplo de XAML arrastar e soltar](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlDragAndDrop) - demonstra o arrastar e soltar com o controle ListView.
- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Arrastar e soltar](https://docs.microsoft.com/windows/uwp/app-to-app/drag-and-drop)
