---
author: Jwmsft
description: Use o código de exemplo de modo de exibição de árvore para criar uma árvore expansível.
title: Exibição em árvore
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.openlocfilehash: 41e17d299e9bac34e58f3c8ffdffecff19ddac18
ms.sourcegitcommit: e020e9a4d947368a68e4eeba1eea65e9b3a725af
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2018
ms.locfileid: "1924389"
---
# <a name="treeview"></a>TreeView

O controle XAML TreeView habilita uma lista hierárquica com nós em expansão e em colapso que contêm itens aninhados. Ele pode ser usado para ilustrar uma estrutura de pastas ou relacionamentos aninhados em sua interface do usuário.

> **APIs importantes**: [classe TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)

As APIs TreeView oferecem suporte aos seguintes recursos:

- Aninhamento de nível N
- Expansão/recolhimento de nós
- Seleção de único ou vários nós

## <a name="is-this-the-right-control"></a>Este é o controle correto?

- Use um TreeView quando os itens tiverem itens de lista aninhados e se for importante ilustrar a relação hierárquica dos itens para seus colegas e nós.

- Evite usar TreeView se realçar o relacionamento aninhado de um item não for prioridade. Para a maioria dos cenários detalhados, um modo de exibição de lista normal é apropriado

## <a name="treeview-ui"></a>Interface do usuário do TreeView

A visualização de árvore usa uma combinação de recuo e ícones pode ser usada para representar a relação aninhada entre nós de pasta/pai e nós que não sejam de pasta/filho. Nós recolhidos devem usar uma divisa que aponte para a direita, e os nós expandidos devem usar uma divisa que aponte para baixo.

![Uso do ícone chevron no TreeView](images/treeview_chevron.png)

Você pode incluir um ícone no modelo de dados do item de modo de exibição árvore para representar a nós. Se você fizer isso, você deve usar um ícone de pasta apenas para nós que representam pastas literais, como a estrutura de pastas em um disco.

![Os ícones chevron e pasta juntos em um TreeView](images/treeview_chevron_folder.png)

## <a name="create-a-tree-view"></a>Criar uma exibição de árvore

Para criar um modo de exibição de árvore, use um [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) controle e uma hierarquia de objetos [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Você pode criar a hierarquia de nó adicionando um ou mais nós raiz RootNodes coleção do controle TreeView. Cada TreeViewNode, em seguida, pode ter mais nós adicionados à sua coleção de filhos. Você pode aninhar nós de exibição de árvore para qualquer profundidade exige que você.

Aqui está um exemplo de uma exibição de árvore simples declarada em XAML. Você normalmente adiciona os nós no código, mas mostraremos a hierarquia XAML aqui porque ele pode ser útil para visualizar como a hierarquia de nós é criada.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

Na maioria dos casos, o modo de exibição de árvore exibe os dados de uma fonte de dados, portanto, você normalmente declara a raiz controle TreeView em XAML, mas adiciona os objetos TreeViewNode em código.

Esse modo de exibição de árvore é a mesma que foi criada anteriormente em XAML, mas os nós são criados no código.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Essas APIs estão disponíveis para gerenciar a hierarquia de dados do seu modo de exibição de árvore.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Uma exibição de árvore pode ter um ou mais nós raiz. Adicione um objeto TreeViewNode à coleção RootNodes para criar um nó raiz. O **pai** de uma raiz de nó é sempre **null**. A **profundidade** de uma raiz de nó é 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Adicione objetos TreeViewNode à coleção de filhos de um nó pai para criar hierarquia seu nó. Um nó é o **pai** de todos os nós na sua coleção **filhos**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **True** se o nó tem filhos percebidos. **False** indica uma pasta vazia ou um item. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Use essa propriedade se você estiver preenchendo nós conforme eles estiverem expandidos. Consulte _preencher um nó quando ele está se expandindo_ posteriormente neste artigo. |
| [Profundidade](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indica a distância do nó raiz que é um nó filho. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtém o TreeViewNode que possui o **filhos** que esse nó é parte da coleção. |

O modo de exibição de árvore usa o **HasChildren** e **HasUnrealizedChildren** propriedades para determinar se o ícone de expandir/recolher é exibido. Se qualquer propriedade for **true**, o ícone é exibido; Caso contrário, ele não é mostrado.

## <a name="tree-view-node-content"></a>Conteúdo do nó de modo de exibição de árvore

Você pode armazenar o item de dados que representa um nó de modo de exibição de árvore em sua propriedade [conteúdo](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Nos exemplos anteriores, o conteúdo era um valor de cadeia de caracteres simples. Aqui, um nó de modo de exibição de árvore representa pasta de imagens do usuário, então a biblioteca de imagens [StorageFolder](/uwp/api/windows.storage.storagefolder) é atribuído à propriedade Content do nó.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

Você pode fornecer um [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) para especificar exatamente como os itens aparecem na exibição da exibição de árvore.

> [!NOTE]
> No Windows 10, versão 1803, você precisa criar TreeView controlar e especificar um ItemTemplate personalizado se o seu conteúdo não é uma cadeia de caracteres. Para obter mais informações, consulte o exemplo completo no final deste artigo.

## <a name="interacting-with-a-tree-view"></a>Interagindo com um modo de exibição de árvore

Você pode configurar uma exibição de árvore para permitir que o usuário interagir com ele de várias maneiras diferentes:

- expandir ou recolher nós
- Seleção única ou vários itens
- clique para invocar um item

### <a name="expandcollapse"></a>Expandir/recolher

Qualquer nó de modo de exibição de árvore que tem filhos sempre pode ser expandido ou recolhido clicando o glifo expandir/recolher. Você também pode expandir ou recolher um nó de forma programática e responda quando um nó altera o estado.

#### <a name="expandcollapse-a-node-programmatically"></a>Expandir/recolher um nó de forma programática

Há 2 maneiras você pode expandir ou recolher um nó de modo de exibição de árvore em seu código.

- O [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) classe tem os métodos [recolher](/uwp/api/windows.ui.xaml.controls.treeview.collapse) e [expandir](/uwp/api/windows.ui.xaml.controls.treeview.expand). Quando você chama esses métodos, você passar o TreeViewNode que você deseja expandir ou recolher.

- Cada [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) tem a propriedade [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Você pode usar essa propriedade para verificar o estado de um nó ou defina-o para alterar o estado. Você também pode definir essa propriedade no XAML para definir o estado inicial de um nó.

### <a name="fill-a-node-when-its-expanding"></a>Preencher um nó quando ele está se expandindo

Talvez seja necessário mostrar um grande número de nós em sua exibição de árvore, ou talvez você não saiba antecipadamente quantos nós terá. O controle TreeView não for virtualizado, portanto, você pode gerenciar recursos de preenchimento de cada nó conforme ele é expandido, e removendo os nós filho quando ele é recolhido.

Manipular o [expandindo](/uwp/api/windows.ui.xaml.controls.treeview.expand) evento e use a propriedade [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) para adicionar filhos a um nó quando ele está sendo expandido. A propriedade HasUnrealizedChildren indica se o nó precisa ser preenchido, ou se sua coleção de filhos já foi preenchida. É importante lembrar que o TreeViewNode não defina esse valor, você precisa para gerenciá-lo no código do aplicativo.

Veja a seguir um exemplo destas APIs em uso. Consultar o exemplo de código completo no final deste artigo para contexto, incluindo a implementação de 'FillTreeNode'.

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Não é obrigatório, mas você pode querer também manipular o evento [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) e remover os nós filho quando o nó pai é fechado. Isso pode ser importante se o modo de exibição de árvore possui muitos de nós, ou se os dados de nó usam muitos recursos. Você deve considerar o impacto no desempenho de preenchimento um nó de cada vez que for aberto versus deixando os filhos em um nó fechado. A melhor opção depende de seu aplicativo.

Aqui, um exemplo de um manipulador para o evento Collapsed.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Invocar um item

Um usuário pode invocar uma ação (tratando o item como um botão) em vez de selecionar o item. Você manipula o evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) para responder a essa interação do usuário.

> [!NOTE]
> Ao contrário de ListView, que tem a propriedade [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), invocando um item está sempre habilitada no modo de exibição de árvore. Você ainda pode escolher se manipular o evento ou não.

**Classe [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Argumentos de evento ItemInvoked dá acesso ao item invocado. A propriedade [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) tem o nó que foi invocado. Você pode convertê-lo para TreeViewNode e obter o item de dados da propriedade TreeViewNode.Content.

Aqui, um exemplo de um manipulador de evento ItemInvoked. O item de dados é uma [IStorageItem](/uwp/api/windows.storage.istorageitem), e este exemplo mostra apenas algumas informações sobre o arquivo e a árvore. Além disso, se o nó for um nó de pasta, ele expande ou collpases o nó ao mesmo tempo. Caso contrário, o nó expande ou recolhe somente quando você clica na divisa.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>Seleção de item

O controle TreeView é compatível com a seleção única e múltipla. Por padrão, seleção de nós estiver desativada, mas você pode definir a propriedade [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) para permitir a seleção de nós. Os valores [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) são **None**, **único**, e **vários**.

Quando a seleção é habilitada, uma caixa de seleção é mostrada ao lado de cada nó de modo de exibição de árvore e os itens selecionados aparecem realçados. Um usuário pode selecionar ou desmarcar um item usando a caixa de seleção; clicando no item ainda faz com que ele seja invocado.

Nós selecionados são adicionados à coleção de exibição de árvore [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes). Você pode chamar o método [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) para selecionar todos os nós em um modo de exibição de árvore.

> [!NOTE]
> Se você chamar **SelectAll**, todos percebemos que nós são selecionados, independentemente do SelectionMode. Para fornecer uma experiência de usuário consistente, você só deve chamar SelectAll se SelectionMode for **vários**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Nós percebemos/não realizado e seleção

Se o seu modo de exibição de árvore tiver nós não realizados, eles não são levados em consideração para seleção. Aqui estão algumas coisas que você precisa ter em mente sobre seleção conosco não realizados.

- Se um usuário seleciona um nó pai, todos os filhos realizados sob esse pai também serão selecionados. Da mesma forma, se todos os nós filho são selecionados, o nó pai também se torna selecionado.
- O método SelectAll apenas adiciona nós realizadas à coleção SelectedNodes.
- Se um nó pai filhos não realizado é selecionado, os filhos serão selecionados conforme eles são obtidos.

## <a name="code-examples"></a>Exemplos de código

### <a name="tree-view-with-selection-enabled"></a>Exibição de árvore com seleção habilitada

Este exemplo mostra como criar uma estrutura de exibição de árvore em XAML. O modo de exibição de árvore mostra os tipos de sorvete e ingredientes que o usuário pode escolher, organizados em categorias. Seleção múltipla está habilitada e, quando o usuário clica em um botão, os SelectedItems são exibidos na interface do usuário principal do aplicativo.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
    {
        if (node.Parent.Content?.ToString() == "Flavors")
        {
            FlavorList.Text += node.Content + "; ";
        }
        else if (node.HasChildren == false)
        {
            ToppingList.Text += node.Content + "; ";
        }
    }
}

private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="pictures-and-music-library-tree-view"></a>Exibir imagens e árvore de biblioteca de músicas

Este exemplo mostra como criar um modo de exibição de árvore que mostra o conteúdo e a estrutura dos usuários imagens e bibliotecas de música. O número de itens não pode ser conhecido antecipadamente, para que cada nó é preenchido quando ele tem expandido e será esvaziada quando ele estiver recolhido.

Um modelo de item personalizado é usado para exibir os itens de dados, que são do tipo [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> O código neste exemplo requer as funcionalidades picturesLibrary e musicLibrary. Para obter mais informações sobre acesso a arquivos, consulte [permissões de acesso a arquivo](../../files/file-access-permissions.md), [enumerar e consultar arquivos e pastas](../../files/quickstart-listing-files-and-folders.md) e [arquivos e pastas nas bibliotecas de música, fotos e vídeos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();
    InitializeTreeView();
}

private void InitializeTreeView()
{
    // A TreeView can have more than 1 root node. The Pictures library
    // and the Music library will each be a root node in the tree.
    // Get Pictures library.
    StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
{
    // Get the contents of the folder represented by the current tree node.
    // Add each item as a new child node of the node that's being expanded.

    // Only process the node if it's a folder and has unrealized children.
    StorageFolder folder = null;
    if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
    {
        folder = node.Content as StorageFolder;
    }
    else
    {
        // The node isn't a folder, or it's already been filled.
        return;
    }

    IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

    if (itemsList.Count == 0)
    {
        // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        // that the chevron appears, but don't try to process children that aren't there.
        return;
    }

    foreach (var item in itemsList)
    {
        var newNode = new TreeViewNode();
        newNode.Content = item;

        if (item is StorageFolder)
        {
            // If the item is a folder, set HasUnrealizedChildren to true. 
            // This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = true;
        }
        else
        {
            // Item is StorageFile. No processing needed for this scenario.
        }
        node.Children.Add(newNode);
    }
    // Children were just added to this node, so set HasUnrealizedChildren to false.
    node.HasUnrealizedChildren = false;
}

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}

private void RefreshButton_Click(object sender, RoutedEventArgs e)
{
    sampleTreeView.RootNodes.Clear();
    InitializeTreeView();
}
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
    ' Get the contents of the folder represented by the current tree node.
    ' Add each item as a new child node of the node that's being expanded.

    ' Only process the node if it's a folder and has unrealized children.
    Dim folder As StorageFolder = Nothing
    If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
        folder = TryCast(node.Content, StorageFolder)
    Else
        ' The node isn't a folder, or it's already been filled.
        Return
    End If

    Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
    If itemsList.Count = 0 Then
        ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        ' that the chevron appears, but don't try to process children that aren't there.
        Return
    End If

    For Each item In itemsList
        Dim newNode As New TreeViewNode With {
            .Content = item
        }
        If TypeOf item Is StorageFolder Then
            ' If the item is a folder, set HasUnrealizedChildren to True.
            ' This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = True
        Else
            ' Item is StorageFile. No processing needed for this scenario.
        End If
        node.Children.Add(newNode)
    Next

    ' Children were just added to this node, so set HasUnrealizedChildren to False.
    node.HasUnrealizedChildren = False
End Sub

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub

Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
    sampleTreeView.RootNodes.Clear()
    InitializeTreeView()
End Sub
```

## <a name="related-articles"></a>Artigos relacionados

- [Classe TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [Classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView e GridView](listview-and-gridview.md)