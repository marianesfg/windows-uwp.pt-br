---
description: Você pode criar um modo de exibição de árvore expansível associando o ItemsSource a uma fonte de dados hierárquica ou pode criar e gerenciar objetos de TreeViewNode por conta própria.
title: Modo de exibição em árvore
label: Tree view
template: detail.hbs
ms.date: 06/14/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 4c29e8f2f88469dfbf260268682cf18e0399e327
ms.sourcegitcommit: 81cb0b597bedfed8a54ac8b7e84089ef057fa9e3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68514126"
---
# <a name="treeview"></a>TreeView

O controle XAML [TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) habilita uma lista hierárquica com nós em expansão e em colapso que contêm itens aninhados. Ela pode ser usada para ilustrar uma estrutura de pastas ou relacionamentos aninhados em sua interface do usuário.

As APIs **TreeView** dão suporte aos seguintes recursos:

- Aninhamento de nível N
- Seleção de um ou de vários nós
- Associação de dados para a propriedade **ItemsSource** em **TreeView** e [TreeViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem)
- **TreeViewItem** como raiz do modelo de item **TreeView**
- Tipos arbitrários de conteúdo em um **TreeViewItem**
- Arrastar e soltar entre modos de exibição de árvore

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Este controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **APIs da plataforma** | **APIs da biblioteca de interface do usuário do Windows** |
| - | - |
| [Classe TreeView](/uwp/api/windows.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode), [propriedade TreeView.ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [Classe TreeView](/uwp/api/microsoft.ui.xaml.controls.treeview), [classe TreeViewNode](/uwp/api/microsoft.ui.xaml.controls.treeviewnode), [propriedade TreeView.ItemsSource](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

Neste documento, usaremos o alias **muxc** em XAML para representar a APIs da Biblioteca de interface do usuário do Windows que incluímos em nosso projeto. Adicionamos isso ao nosso elemento [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page):

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

No code-behind, também usaremos o alias **muxc** em C# para representar a APIs da Biblioteca de interface do usuário do Windows que incluímos em nosso projeto. Adicionamos essa instrução **using** na parte superior do arquivo:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

- Use um **TreeView** quando os itens tiverem itens de lista aninhados e se for importante ilustrar a relação hierárquica dos itens para seus colegas e nós.

- Evite usar **TreeView** se realçar o relacionamento aninhado de um item não for prioridade. Para a maioria dos cenários detalhados, um modo de exibição de lista normal é apropriado.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/TreeView">abrir o aplicativo e ver o TreeView em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>Interface do usuário de TreeView

O modo de exibição de árvore usa uma combinação de recuo e ícones para representar a relação aninhada entre nós pai e nós filho. Nós recolhidos usam uma divisa que aponta para a direita e nós expandidos usam uma divisa que aponta para baixo.

![Ícone de divisa em TreeView](images/treeview-simple.png)

Você pode incluir um ícone no modelo de dados do item de modo de exibição de árvore para representar nós. Por exemplo, se você mostrar uma hierarquia de sistemas de arquivos, poderá usar ícones de pasta para os nós pai e ícones de arquivo para os nós folha.

![Os ícones de divisa e pasta juntos em um TreeView](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>Criar um modo de exibição de árvore

Você pode criar um modo de exibição de árvore associando o [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) a uma fonte de dados hierárquica ou pode criar e gerenciar objetos de **TreeViewNode** por conta própria.

Para criar um modo de exibição de árvore, use um controle [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) e uma hierarquia de objetos [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode). Crie a hierarquia de nós adicionando um ou mais nós raiz à coleção **RootNodes** do controle [TreeView](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes). Cada **TreeViewNode** pode, por sua vez, ter mais nós adicionados à sua coleção [Children](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewnode.children). Você pode aninhar nós do modo de exibição de árvore a qualquer profundidade que precisar.

Você pode associar uma fonte de dados hierárquica à propriedade [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) para fornecer o conteúdo do modo de exibição de árvore, assim como faria com [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de **ListView**. De forma semelhante, use [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) (e o [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) opcional) para fornecer um [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) que renderiza o item.

> [!IMPORTANT]
> **ItemsSource** e suas APIs relacionadas exigem o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior ou a [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).
>
> **ItemsSource** é um mecanismo alternativo a **TreeView.RootNodes** para colocar conteúdo no controle **TreeView**. Não é possível definir **ItemsSource** e **RootNodes** ao mesmo tempo. Quando você usa **ItemsSource**, são criados nós que você pode acessar da propriedade **TreeView.RootNodes**.

Veja um exemplo de modo de exibição de árvore simples declarada em XAML. Normalmente, você adiciona os nós no código, mas mostraremos a hierarquia XAML aqui porque ela pode ser útil para visualizar como a hierarquia de nós é criada.

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

Na maioria dos casos, seu modo de exibição de árvore exibe dados de uma fonte de dados, portanto, você normalmente declara o controle **TreeView** raiz em XAML, mas adiciona objetos **TreeViewNode** em código usando associação de dados.

### <a name="bind-to-a-hierarchical-data-source"></a>Associar a uma fonte de dados hierárquica

Para criar um modo de exibição de árvore usando vinculação de dados, defina uma coleção hierárquica para a propriedade **TreeView.ItemsSource**. Em seguida, em **ItemTemplate**, defina a coleção de itens filho como a propriedade **TreeViewItem.ItemsSource**.

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

Confira [Modo de exibição de árvore usando vinculação de dados](#tree-view-using-data-binding) para ver o código completo.

#### <a name="items-and-item-containers"></a>Itens e contêineres de itens

Se você usar **TreeView.ItemsSource**, essas APIs estarão disponíveis para obter o item de dados ou o nó do contêiner e vice-versa.

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | Obtém o item de dados para o contêiner de **TreeViewItem** especificado. |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | Obtém o contêiner de **TreeViewItem** para o item de dados especificado. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | Obtém o **TreeViewNode** para o contêiner de **TreeViewItem** especificado. |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | Obtém o contêiner de **TreeViewItem** para o **TreeViewNode** especificado. |

### <a name="manage-tree-view-nodes"></a>Gerenciar nós do modo de exibição de árvore

Este modo de exibição de árvore é o mesmo que foi criado anteriormente em XAML, mas os nós são criados no código.

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

Essas APIs estão disponíveis para gerenciar a hierarquia de dados de seu modo de exibição de árvore.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Um modo de exibição de árvore pode ter um ou mais nós raiz. Adicione um objeto **TreeViewNode** à coleção **RootNodes** para criar um nó raiz. O **Pai** de um nó raiz sempre é **null**. A **Profundidade** de um nó raiz é 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Filhos](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Adicione objetos **TreeViewNode** à coleção **Children** de um nó pai para criar sua hierarquia de nós. Um nó é o **Pai** de todos os nós na sua coleção de **Filhos**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | **true** se o nó tem filhos realizados. **false** indica uma pasta vazia ou um item. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Use essa propriedade se estiver preenchendo nós conforme eles são expandidos. Confira [Preencher um nó quando ele está se expandindo](#fill-a-node-when-its-expanding) posteriormente neste artigo. |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Indica a que distância do nó raiz um nó filho está. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Obtém o **TreeViewNode** que tem a coleção **Children** de que esse nó faz parte. |

O modo de exibição de árvore usa as propriedades **HasChildren** e **HasUnrealizedChildren** para determinar se o ícone de expandir/recolher é exibido. Se qualquer propriedade for **true**, o ícone será exibido; caso contrário, não será.

## <a name="tree-view-node-content"></a>Conteúdo do nó do modo de exibição de árvore

Você pode armazenar o item de dados que um nó de modo de exibição de árvore representa em sua propriedade [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Nos exemplos anteriores, o conteúdo era um valor de cadeia de caracteres simples. Aqui, um nó do modo de exibição de árvore representa a pasta de **Fotos** do usuário, então a biblioteca de imagens [StorageFolder](/uwp/api/windows.storage.storagefolder) é atribuída à propriedade **Content** do nó.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> Para obter acesso à pasta **Fotos**, você precisa especificar a funcionalidade de **Biblioteca de imagens** no manifesto do aplicativo. Veja [Declarações de funcionalidade de aplicativo](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para saber mais.

Você pode fornecer um [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) para especificar como o item de dados é exibido no modo de exibição de árvore.

> [!NOTE]
> No Windows 10, versão 1803, você precisará recriar o controle **TreeView** e especificar um **ItemTemplate** personalizado se o conteúdo não for uma cadeia de caracteres. Em versões posteriores, defina a propriedade **ItemTemplate**. Para saber mais, confira [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate).

### <a name="item-container-style"></a>Estilo do contêiner do item

Se você usar **ItemsSource** ou **RootNodes**, os elementos reais usados para exibir cada nó – chamado "contêiner" – será um objeto [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem). É possível modificar as propriedades **TreeViewItem** para estilizar o contêiner usando as propriedades [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) ou [ItemContainerStyleSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) de **TreeView**.

Este exemplo mostra como alterar os glifos expandidos/recolhidos para os sinais +/- laranja. No modelo **TreeViewItem** padrão, os glifos são definidos para usar a fonte `Segoe MDL2 Assets`. É possível definir a propriedade **Setter.Value** fornecendo o valor de caractere Unicode no formato usando por XAML, como este: `Value="&#xE948;"`.

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>Seletores de modelo de item

Por padrão, o [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) mostra a representação de cadeia de caracteres do item de dados para cada nó. É possível definir a propriedade [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) para alterar o que é exibido para todos os nós. Ou é possível usar um [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector) para escolher um [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) diferente para os itens do modo de exibição de árvore com base no tipo de item ou em alguns outros critérios especificados.

Por exemplo, em um aplicativo explorador de arquivos, você pode usar um modelo de dados para pastas e outro para arquivos.

![Arquivos e pastas usando modelos de dados diferentes](images/treeview-icons.png)

Veja um exemplo de como criar e usar um seletor de modelo de item.  Para saber mais, confira a classe [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Esse código faz parte de um exemplo maior e não funcionará sozinho. Para ver o exemplo completo, incluindo o código que define `ExplorerItem`, confira o [repositório Xaml-Controls-Gallery](https://github.com/microsoft/Xaml-Controls-Gallery) no GitHub. [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) e [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) contêm o código relevante.

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

O tipo de objeto passado para o método [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) depende de você criar o modo de exibição de árvore definindo a propriedade **ItemsSource** ou criando e gerenciando objetos **TreeViewNode** por conta própria.

- Se **ItemsSource** estiver definido, o objeto será de qualquer tipo que o item de dados seja. No exemplo anterior, o objeto era um `ExplorerItem`, portanto, ele poderia ser usado após uma simples conversão em `ExplorerItem`: `var explorerItem = (ExplorerItem)item;`.
- Se **ItemsSource** não estiver definido e você gerenciar os nós do modo de exibição de árvore por conta própria, o objeto passado para **SelectTemplateCore** será um **TreeViewNode**. Nesse caso, é possível obter o item de dados da propriedade [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content).

Veja um seletor de modelo de dados do exemplo [Modo de exibição de árvore da biblioteca de Fotos e Música](#pictures-and-music-library-tree-view) mostrado posteriormente. O método **SelectTemplateCore** recebe um **TreeViewNode**, que pode ter um [StorageFolder](/uwp/api/windows.storage.storagefolder) ou um [StorageFile](/uwp/api/windows.storage.storagefile) como seu conteúdo. Com base no conteúdo, é possível retornar um modelo padrão ou um modelo específico para a pasta de músicas, a pasta de imagens, os arquivos de música ou os arquivos de imagem.

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>Interagindo com um modo de exibição de árvore

Você pode configurar um modo de exibição de árvore para permitir que o usuário interaja com ele de várias maneiras diferentes:

- expandir ou recolher nós
- seleção de um ou de vários itens
- clicar para invocar um item

### <a name="expandcollapse"></a>Expandir/recolher

Qualquer nó do modo de exibição de árvore que tem filhos sempre pode ser expandido ou recolhido clicando no glifo expandir/recolher. Você também pode expandir ou recolher um nó de forma programática e responder quando um nó alterar o estado.

#### <a name="expandcollapse-a-node-programmatically"></a>Expandir/recolher um nó de forma programática

Há duas maneiras de expandir ou recolher um nó no modo de exibição de árvore em seu código.

- A classe [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) tem os métodos [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) e [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand). Quando chama esses métodos, você passa o **TreeViewNode** que deseja expandir ou recolher.

- Cada [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) tem a propriedade [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Você pode usar essa propriedade para verificar o estado de um nó ou configurá-la para alterar o estado. Você também pode definir essa propriedade no XAML para definir o estado inicial de um nó.

### <a name="fill-a-node-when-its-expanding"></a>Preencher um nó quando ele está se expandindo

Talvez seja necessário mostrar um grande número de nós em seu modo de exibição de árvore ou talvez você não saiba antecipadamente quantos nós terá. O controle **TreeView** não é virtualizado, portanto, você pode gerenciar recursos preenchendo cada nó conforme ele é expandido e removendo os nós filho quando ele é recolhido.

Manipule o evento [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) e use a propriedade [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) para adicionar filhos a um nó quando ele está sendo expandido. A propriedade **HasUnrealizedChildren** indica se o nó precisa ser preenchido ou se a coleção **Children** dele já foi preenchida. É importante lembrar que o **TreeViewNode** não define esse valor, você precisa gerenciá-lo no código do aplicativo.

Veja a seguir um exemplo dessas APIs em uso. Confira o exemplo de código completo no final deste artigo para ver o contexto, incluindo a implementação de **FillTreeNode**.

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

Não é obrigatório, mas você também pode querer manipular o evento [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) e remover os nós filho quando o nó pai for fechado. Isso pode ser importante se o modo de exibição de árvore tem muitos nós ou se os dados do nó usam muitos recursos. Você deve considerar o impacto no desempenho de preencher um nó a cada vez que for aberto versus deixar os filhos em um nó fechado. A melhor opção depende de seu aplicativo.

Aqui, um exemplo de um manipulador para o evento **Collapsed**.

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>Invocando um item

Um usuário pode invocar uma ação (tratando o item como um botão) em vez de selecionar o item. Você manipula o evento [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) para responder a essa interação do usuário.

> [!NOTE]
> Diferente de **ListView**, que tem a propriedade [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled), invocar um item está sempre habilitado no modo de exibição de árvore. Você ainda pode escolher se deseja manipular o evento ou não.

**Classe [TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)**

Os argumentos do evento **ItemInvoked** dão acesso ao item invocado. A propriedade [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) tem o nó que foi invocado. Você pode transmiti-lo para **TreeViewNode** e obter o item de dados da propriedade **TreeViewNode.Content**.

Veja um exemplo de um manipulador de eventos **ItemInvoked**. O item de dados é um [IStorageItem](/uwp/api/windows.storage.istorageitem) e este exemplo mostra apenas algumas informações sobre o arquivo e a árvore. Além disso, se o nó for um nó de pasta, ele expandirá ou recolherá o nó ao mesmo tempo. Caso contrário, o nó expandirá ou recolherá somente quando você clicar na divisa.

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
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
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
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

O controle **TreeView** é compatível com seleção única e múltipla. Por padrão, a seleção de nós fica desativada, mas você pode configurar a propriedade [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) para permitir a seleção de nós. Os valores de [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) são **None**, **Single** e **Multiple**.

#### <a name="multiple-selection"></a>Seleção múltipla

Quando a seleção múltipla está habilitada, uma caixa de seleção é mostrada ao lado de cada nó do modo de exibição de árvore e os itens selecionados aparecem realçados. Um usuário pode selecionar ou desmarcar um item usando a caixa de seleção; clicar no item ainda faz com que ele seja invocado.

Selecionar ou desmarcar um nó pai selecionará ou desmarcará todos os filhos sob aquele nó. Se alguns, mas não todos os filhos sob um nó pai forem selecionados, a caixa de seleção para o nó pai será mostrada como indeterminada (preenchida com uma caixa preta).

![Seleção múltipla em um modo de exibição de árvore](images/treeview-selection.png)

Nós selecionados são adicionados à coleção [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) do modo de exibição de árvore. Você pode chamar o método [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) para selecionar todos os nós em um modo de exibição de árvore.

> [!NOTE]
> Se você chamar **SelectAll**, todos os nós realizados serão selecionados, independentemente do **SelectionMode**. Para fornecer uma experiência de usuário uniforme, você só deverá chamar **SelectAll** se **SelectionMode** for **Multiple**.

#### <a name="selection-and-realizedunrealized-nodes"></a>Seleção de nós realizados/não realizados

Se o modo de exibição de árvore tiver nós não realizados, eles não serão levados em consideração para seleção. Aqui estão algumas coisas que você precisa ter em mente sobre a seleção de nós não realizados.

- Se um usuário selecionar um nó pai, todos os filhos realizados sob esse pai também serão selecionados. Da mesma forma, se todos os nós filho forem selecionados, o nó pai também será selecionado.
- O método **SelectAll** apenas adiciona nós realizados à coleção **SelectedNodes**.
- Se um nó pai com filhos não realizados for selecionado, os filhos serão selecionados conforme forem realizados.

## <a name="code-examples"></a>Exemplos de código

Os exemplos de código a seguir demonstram vários recursos do controle de exibição de árvore.

### <a name="tree-view-using-xaml"></a>Modo de exibição de árvore usando XAML

Este exemplo mostra como criar uma estrutura de modo de exibição de árvore simples em XAML. O modo de exibição de árvore mostra os sabores de sorvete e coberturas que o usuário pode escolher, organizados em categorias. A seleção múltipla está habilitada e, quando o usuário clica em um botão, os itens selecionados são exibidos na interface do usuário principal do aplicativo.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
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
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
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
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>Modo de exibição de árvore usando vinculação de dados

Este exemplo mostra como criar o mesmo modo de exibição de árvore do exemplo anterior. No entanto, em vez de criar a hierarquia de dados em XAML, os dados são criados no código e vinculados à propriedade **ItemsSource** do modo de exibição de árvore. (Os manipuladores de eventos do botão mostrados no exemplo anterior também se aplicam a este exemplo.)

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
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
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>Modo de exibição de árvore de biblioteca de Músicas e Fotos

Este exemplo mostra como criar um modo de exibição de árvore que mostra o conteúdo e a estrutura das bibliotecas de **Fotos** e **Músicas** do usuário. O número de itens não pode ser conhecido antecipadamente, de modo que cada nó é preenchido quando é expandido e esvaziado quando recolhido.

Um modelo de item personalizado é usado para exibir os itens de dados, que são do tipo [IStorageItem](/uwp/api/windows.storage.istorageitem).

> [!IMPORTANT]
> O código neste exemplo requer as funcionalidades **picturesLibrary** e **musicLibrary**. Para obter mais informações sobre acesso a arquivos, confira [Permissões de acesso a arquivo](../../files/file-access-permissions.md), [Enumerar e consultar arquivos e pastas](../../files/quickstart-listing-files-and-folders.md) e [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
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
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
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
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
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
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
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
                var newNode = new muxc.TreeViewNode();
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

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

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
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
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
            Dim newNode As New muxc.TreeViewNode With {
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

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
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

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>Arrastar e soltar itens entre modos de exibição de árvore

O exemplo a seguir demonstra como criar duas exibições de árvore cujos itens podem ser arrastados e soltos entre si. Quando um item é arrastado para o modo de exibição de árvore, ele é adicionado ao final da lista. No entanto, os itens podem ser reordenados dentro de um modo de exibição de árvore. Este exemplo também só leva em conta modos de exibição de conta com um nó raiz.

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>Artigos relacionados

- [Classe TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [Classe ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView e GridView](listview-and-gridview.md)
