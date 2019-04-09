---
Description: ItemsRepeater é um controle leve para gerar e apresentar uma coleção de itens.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3344230bc52013825d94cfbe3668acfa0d7a2e13
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913996"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Use uma [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) Criar coleção personalizada experiências usando um sistema de layout flexível, modos de exibição personalizados e virtualização.

Diferentemente [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não fornece uma experiência do usuário final abrangente – ele não tem nenhum padrão da interface do usuário e não fornece nenhuma política de interação de foco, a seleção ou o usuário. Em vez disso, ele é um bloco de construção que você pode usar para criar suas próprias experiências exclusivas baseada em coleção e controles personalizados. Embora ele não tem nenhuma política interna, ele permite que você anexar política para criar a experiência que você precisa. Por exemplo, você pode definir o layout a ser usado, a política keyboarding, a política de seleção, etc.

Você pode pensar [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) conceitualmente como um painel controlada por dados, em vez de um controle completo, como ListView. Você especifica uma coleção de itens de dados a serem exibidos, um modelo de item que gera um elemento de interface do usuário para cada item de dados e um layout que determina como os elementos devem ser dimensionados e posicionados. Em seguida, ItemsRepeater produz elementos filho com base na fonte de dados e exibe-os conforme especificado pelo modelo de item e layout. Não é necessário que os itens exibidos sejam homogêneos, porque ItemsRepeater pode carregar o conteúdo para representar os itens de dados com base em critérios especificados por você em um seletor de modelo de dados.

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Esse controle é incluído como parte da biblioteca de interface do usuário do Windows, um pacote do NuGet que contém os novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte o [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs importantes**: [Classe ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), [classe ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use uma [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para criar exibições personalizadas para coleções de dados. Enquanto ele pode ser usado para apresentar um conjunto básico de itens, muitas vezes você pode usá-lo como o elemento de exibição no modelo de um controle personalizado.

Se você precisar de um controle de caixa para exibir dados em uma lista ou grade com personalização mínima, considere o uso de um [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

ItemsRepeater não tem uma coleção de itens internos. Se você precisa fornecer uma coleção de itens diretamente, em vez de associar a uma fonte de dados separado, em seguida, é provável que precisam de uma experiência mais alta-policy e deve usar [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) e ItemsRepeater os dois permitem experiências de coleção personalizável, mas ItemsRepeater dá suporte a layouts de virtualização da interface do usuário, enquanto o ItemsControl não. É recomendável usar ItemsRepeater em vez de ItemsControl, se seu para apresentar alguns itens de dados apenas ou criação de um controle de coleção personalizada.

> [!NOTE]
> Se você tiver uma situação em que você se sentir ItemsControl atende às suas necessidades e não ItemsRepeater, deixar comentários sobre o [projeto GitHub de biblioteca de interface do usuário do Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues) e informe-nos.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para abrir o aplicativo e ver o <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Rolagem com ItemsRepeater

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não é derivado de [controle](/uwp/api/windows.ui.xaml.controls.control), portanto, ele não tem um modelo de controle. Portanto, ele não contém qualquer interno de rolagem, como um ListView ou fazer outros controles de coleção.

Quando você usa um ItemsRepeater, você deve fornecer funcionalidade de rolagem encapsulando-o em um [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer) controle.

## <a name="create-an-itemsrepeater"></a>Criar um ItemsRepeater

Para usar um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), você precisa dar-lhe os dados serem exibidos, definindo a propriedade ItemsSource. Em seguida, informar a ele como exibir os itens, definindo o [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propriedade.

### <a name="itemssource"></a>ItemsSource

Para preencher o modo de exibição, defina as [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propriedade a uma coleção de itens de dados. Aqui, o ItemsSource está definida no código diretamente a uma instância de uma coleção.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Você também pode associar a propriedade ItemsSource a uma coleção em XAML. Para saber mais sobre vinculação de dados, consulte [Visão geral de vinculação de dados](https://msdn.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="data-template"></a>Modelo de dados

Um modelo de dados de um item define como os dados são visualizados. Por padrão, o item é exibido no modo de exibição como a representação de cadeia de caracteres do objeto de dados à qual está associada usando um TextBlock. No entanto, você geralmente quer mostrar uma apresentação mais sofisticada de seus dados. Para especificar exatamente como os itens são exibidos, você define uma [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate). O XAML no DataTemplate define o layout e a aparência dos controles usados para exibir cada item. Os controles no layout podem ser associados a propriedades de um objeto de dados ou ter conteúdo estático definido embutido. Atribuir o DataTemplate para a [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) propriedade do ItemsRepeater.

Neste exemplo, o objeto de dados é uma cadeia de caracteres simple. Você usa um DataTemplate para adicionar uma imagem à esquerda do texto e definir o estilo de TextBlock para exibir a cadeia de caracteres na turquesa.

> [!NOTE]
> Ao usar a [extensão de marcação x:Bind](https://msdn.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) em um DataTemplate, você precisa especificar o DataType (`x:DataType`) no DataTemplate.

```xaml
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
```

Aqui está como os itens apareceria quando exibidos com esse modelo de dados.

![Itens exibidos com um modelo de dados](images/listview-itemstemplate.png)

O número de elementos usados no modelo de dados para um item pode ter um impacto significativo no desempenho se o modo de exibição exibe um grande número de itens. Para obter mais informações e exemplos de como usar modelos de dados para definir a aparência de itens na lista, consulte [contêineres e modelos de Item](item-containers-templates.md).

> [!TIP]
> ItemsRepeater não encapsular o conteúdo do DataTemplate em um contêiner de itens, como ListView e outros controles de coleção. Em vez disso, ItemsRepeater apresenta somente o que é definido no DataTemplate. Se você desejar que os itens para ter a mesma aparência, como um item de exibição de lista, você pode usar um contêiner, como ListViewItem, em seu modelo de dados. ItemsRepeater mostrará os visuais de ListViewItem, mas não faz uso de outras funcionalidades, como seleção ou mostrando a caixa de seleção múltipla.
>
> Da mesma forma, se a coleta de dados é uma coleção de controles reais, como botão (`List<Button>`), você pode colocar um ContentPresenter o DataTemplate para exibir o controle.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Os itens exibidos no modo de exibição não precisará ser do mesmo tipo. [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) pode usar um **DataTemplateSelector** para carregar um modelo de dados para representar os itens de dados com base em critérios especificados por você. Para obter mais informações e exemplos, consulte [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Uma alternativa ao uso de DataTemplate ou DataTemplateSelector é implementar sua própria classe derivada de [Microsoft.UI.Xaml.Controls.ElementFactory](/uwp/api/microsoft.ui.xaml.controls.elementfactory) que é responsável por gerar o conteúdo quando solicitado.

## <a name="configure-the-data-source"></a>Configurar a fonte de dados

Use o [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) propriedade para especificar a coleção a ser usada para gerar o conteúdo dos itens. Você pode definir o ItemsSource a qualquer tipo que implementa **IEnumerable**. Interfaces de coleção adicionais implementadas por sua fonte de dados determinam qual funcionalidade está disponível para o ItemsRepeater para interagir com seus dados.

Esta lista mostra as interfaces disponíveis e quando considerar o uso de cada um deles.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Pode ser usado para conjuntos de dados pequenos e estáticos.

    No mínimo, a fonte de dados deve implementar IEnumerable / interface IIterable. Se isso for tudo o que há suporte para o controle irá iterar por meio de tudo o que uma vez para criar uma cópia que pode ser usada para acessar itens por meio de um valor de índice.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Pode ser usado para conjuntos de dados estáticos, somente leitura.

    Permite que o controle para acessar itens por índice e evita a cópia interna redundante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Pode ser usado para conjuntos de dados estáticos.

    Permite que o controle para acessar itens por índice e evita a cópia interna redundante.

    **Aviso**: Altera para o vetor de lista/sem implementar [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) não serão refletidas na interface do usuário.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Recomendado para dar suporte à notificação de alteração.

    Permite que o controle observar e reagir a alterações na fonte de dados e refletir essas alterações na interface do usuário.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Dá suporte à notificação de alteração

    Como o **INotifyCollectionChanged** interface, isso permite que o controle observar e reagir a alterações na fonte de dados.

    **Aviso**: O Windows.Foundation.IObservableVector\<T > não dá suporte a uma ação de 'Movimentação'. Isso pode causar a interface do usuário para um item perder seu estado visual.  Por exemplo, um item que está selecionado no momento e/ou tem o foco em que a mudança é obtida por uma 'Remover' seguida de 'Adicionar' perderá o foco e não mais ser selecionado.

    O Platform.Collections.Vector\<T > usa IObservableVector\<T > e tem essa limitação mesma. Se o suporte para uma ação de 'Movimentação' é necessária, em seguida, use o **INotifyCollectionChanged** interface.  O .NET ObservableCollection\<T > classe usa **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Quando um identificador exclusivo pode ser associado a cada item.  Recomendado ao usar 'Redefinir' como a ação de alteração da coleção.

    Permite que o controle recuperar com muita eficiência a interface do usuário existente depois de receber uma ação de 'Redefinir' rígida como parte de um **INotifyCollectionChanged** ou **IObservableVector** eventos. Depois de receber uma redefinição, o controle usará a ID exclusiva fornecida para associar os dados atuais com os elementos que já tinha criado. Sem a chave para o mapeamento de índice, o controle teria que supor que ele precisa começar do zero na criação da interface do usuário para os dados.

As interfaces listadas acima, que não seja IKeyIndexMapping, fornecem o mesmo comportamento no ItemsRepeater como faziam no ListView e GridView.


As interfaces a seguir em um ItemsSource habilitar uma funcionalidade especial dos controles ListView e GridView, mas atualmente não têm efeito sobre um ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Seus comentários são muito importantes para nós! Queremos saber sua opinião sobre o [projeto GitHub de biblioteca de interface do usuário do Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). Considere adicionar suas ideias em propostas existentes, como [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Adicione suporte a carregamento incremental ItemsRepeater.

É uma abordagem alternativa para carregar incrementalmente os dados conforme o usuário rola para cima ou para baixo observar a posição do visor do ScrollViewer e carregar mais dados, como o visor se aproximar a extensão.

```xaml
<ScrollViewer ViewChanged="ScrollViewer_ViewChanged">
    <ItemsRepeater ItemsSource="{x:Bind MyItemsSource}" .../>
</ScrollViewer>
```

```csharp
private async void ScrollViewer_ViewChanged(object sender, ScrollViewerViewChangedEventArgs e)
{
    if (!e.IsIntermediate)
    {
        var scroller = (ScrollViewer)sender;
        var distanceToEnd = scroller.ExtentHeight - (scroller.VerticalOffset + scroller.ViewportHeight);

        // trigger if within 2 viewports of the end
        if (distanceToEnd <= 2.0 * scroller.ViewportHeight
                && MyItemsSource.HasMore && !itemsSource.Busy)
        {
            // show an indeterminate progress UI
            myLoadingIndicator.Visibility = Visibility.Visible;

            await MyItemsSource.LoadMoreItemsAsync(/*DataFetchSize*/);

            loadingIndicator.Visibility = Visibility.Collapsed;
        }
    }
}
```

## <a name="change-the-layout-of-items"></a>Alterar o layout dos itens

Itens mostrados pela [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) são organizadas por um [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) objeto que gerencia o dimensionamento e posicionamento de seus elementos filho. Quando usado com um ItemsRepeater, o objeto de Layout permite a virtualização de interface do usuário. Os layouts fornecidos estão [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) e [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Por padrão, o ItemsRepeater usa um StackLayout com orientação vertical.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) organiza os elementos em uma única linha que você pode orientar horizontal ou verticalmente.

Você pode definir as [espaçamento](/en-us/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) propriedade para ajustar a quantidade de espaço entre itens. Espaçamento é aplicado na direção do layout da [orientação](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation).

![Espaçamento de layout de pilha](images/stack-layout.png)

Este exemplo mostra como definir a propriedade ItemsRepeater.Layout para um StackLayout com orientação horizontal e espaçamento de 8 pixels.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}" ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:StackLayout Orientation="Horizontal" Spacing="8"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

### <a name="uniformgridlayout"></a>UniformGridLayout

O [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) posiciona os elementos em um layout de disposição em sequência. Itens estão dispostos na ordem da esquerda para a direita quando o [orientação](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) é **Horizontal**e disposto de cima para baixo quando a orientação é **Vertical**. Cada item é dimensionado igualmente.

![Espaçamento de layout de grade uniforme](images/uniform-grid-layout.png)

O número de itens em cada linha de um layout horizontal é influenciado pela largura mínima do item. O número de itens em cada coluna de um layout vertical é influenciado pela altura mínima do item.

- Você pode fornecer explicitamente um tamanho mínimo para usar definindo o [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) e [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth) propriedades.
- Se você não especificar um tamanho mínimo, o tamanho medido do primeiro item é considerado o tamanho mínimo por item.

Você também pode definir o espaçamento mínimo para o layout incluir entre linhas e colunas definindo a [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) e [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing) propriedades.

![Espaçamento e o dimensionamento uniforme de grade](images/uniform-grid-sizing-spacing.png)

Depois que o número se itens em uma linha ou coluna tiver sido determinado com base no espaçamento e o tamanho do item mínima, pode haver espaço não utilizado deixado após o último item na linha ou coluna (conforme ilustrado na imagem anterior). Você pode especificar se qualquer espaço extra é ignorado, usado para aumentar o tamanho de cada item ou usado para criar o espaço extra entre os itens. Isso é controlado pelo [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) e [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propriedades.

Você pode definir as [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) propriedade para especificar como o tamanho do item é aumentado para preencher o espaço não utilizado.

Esta lista mostra os valores disponíveis. As definições de supor que o padrão **orientação** dos **Horizontal**.

- **Nenhum**: O espaço extra estiver em uso no final da linha. Este é o padrão.
- **Preencher**: Os itens são fornecidos a largura extra para usar o espaço disponível (altura se vertical).
- **Uniforme**: Os itens são dado largura extra para usar o espaço disponível e receber altura extra para manter a taxa de proporção (altura e largura são alternados se vertical).

Esta imagem mostra o efeito do **ItemsStretch** valores em um layout horizontal.

![Ampliação do item de grade uniforme](images/uniform-grid-item-stretch.png)

Quando **ItemsStretch** é **None**, você pode definir o [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) propriedade para especificar como extra espaço é usado para alinhar os itens.

Esta lista mostra os valores disponíveis. As definições de supor que o padrão **orientação** dos **Horizontal**.

- **Iniciar**: Itens são alinhados com o início da linha. O espaço extra estiver em uso no final da linha. Este é o padrão.
- **Centro de**: Itens são alinhados no centro da linha. O espaço extra é dividido igualmente no início e no final da linha.
- **End**: Itens são alinhados com o final da linha. O espaço extra é deixado não utilizado no início da linha.
- **SpaceAround**: Itens são distribuídos uniformemente. Uma quantidade igual de espaço é adicionada antes e depois de cada item.
- **SpaceBetween**: Itens são distribuídos uniformemente. Uma quantidade igual de espaço é adicionada entre cada item. Nenhum espaço é adicionado no início e no final da linha.
- **SpaceEvenly**: Itens são distribuídos uniformemente com uma quantidade igual de espaço entre cada item e no início e no final da linha.

Esta imagem mostra o efeito do **ItemsStretch** valores em um layout vertical (aplicadas a colunas em vez de linhas).

![Justificação de item de grade uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> O **ItemsStretch** propriedade afeta a _medida_ passar de layout. O **ItemsJustification** propriedade afeta a _organizar_ passar de layout.

Este exemplo mostra como definir a **ItemsRepeater.Layout** propriedade para um **UniformGridLayout**.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                    ItemTemplate="{StaticResource MyTemplate}">
    <muxc:ItemsRepeater.Layout>
        <muxc:UniformGridLayout MinItemWidth="200"
                                MinColumnSpacing="28"
                                ItemsJustification="SpaceAround"/>
    </muxc:ItemsRepeater.Layout>
</muxc:ItemsRepeater>
```

## <a name="lifecycle-events"></a>Eventos de ciclo de vida

Quando você hospeda itens em uma [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), talvez seja necessário executar alguma ação quando um item é mostrado ou interrompe o que está sendo mostrado, tais como iniciar um download assíncrono das algum conteúdo, associar o elemento com um mecanismo para controlar a seleção, ou Interrompa uma tarefa em segundo plano.

Em um controle de virtualização, você não pode contar eventos carregado/descarregado porque o elemento não pode ser removido da árvore visual em tempo real quando ele for reciclado. Em vez disso, os outros eventos são fornecidos para gerenciar o ciclo de vida de elementos. Este diagrama mostra o ciclo de vida de um elemento em um ItemsRepeater e quando os eventos relacionados são gerados.

![Diagrama de ciclo de vida de eventos](images/items-repeater-lifecycle.png)

- [**ElementPrepared** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) ocorre sempre que um elemento fica pronto para uso. Isso ocorre para um elemento criado recentemente, bem como um elemento que já existe e está sendo usado novamente da fila de reciclagem.
- [**ElementClearing** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) ocorre imediatamente percebido de cada vez que um elemento foi enviado para a fila de reciclagem, como quando ele ficar fora do intervalo de itens.
- [**ElementIndexChanged** ](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) ocorre para cada realizadas UIElement em que o índice do item que ele representa foi alterada. Por exemplo, quando outro item é adicionado ou removido na fonte de dados, o índice para itens que vêm depois na ordenação receber este evento.

Este exemplo mostra como você poderia usar esses eventos para anexar um serviço de seleção personalizada para acompanhar a seleção de item em um controle personalizado que usa ItemsRepeater para exibir itens.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<UserControl ...>
    ...
    <ScrollViewer>
        <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                            ItemTemplate="{StaticResource MyTemplate}"
                            ElementPrepared="OnElementPrepared"
                            ElementIndexChanged="OnElementIndexChanged"
                            ElementClearing="OnElementClearing">
        </muxc:ItemsRepeater>
    </ScrollViewer>
    ...
</UserControl>
```

```csharp
interface ISelectable
{
    int SelectionIndex { get; set; }
    void UnregisterSelectionModel(SelectionModel selectionModel);
    void RegisterSelectionModel(SelectionModel selectionModel);
}

private void OnElementPrepared(ItemsRepeater sender, ElementPreparedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Wire up this item to recognize a 'select' and listen for programmatic
        // changes to the selection model to know when to update its visual state.
        selectable.SelectionIndex = args.Index;
        selectable.RegisterSelectionModel(this.SelectionModel);
    }
}

private void OnElementIndexChanged(ItemsRepeater sender, ElementIndexChangedEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Sync the ID we use to notify the selection model when the item
        // we represent has changed location in the data source.
        selectable.SelectionIndex = args.NewIndex;
    }
}

private void OnElementClearing(ItemsRepeater sender, ElementClearingEventArgs args)
{
    var selectable = args.Element as ISelectable;
    if (selectable != null)
    {
        // Disconnect handlers to recognize a 'select' and stop
        // listening for programmatic changes to the selection model.
        selectable.UnregisterSelectionModel(this.SelectionModel);
        selectable.SelectionIndex = -1;
    }
}
```

## <a name="sorting-filtering-and-resetting-the-data"></a>Classificação, filtragem e redefinindo os dados

Quando você executar ações como filtragem ou classificação de seu conjunto de dados, você tradicionalmente pode tem em comparação com o conjunto de dados para os novos dados anterior e emitido notificações de alteração granular por meio [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). No entanto, muitas vezes é mais fácil substituir os dados antigos com os novos dados completamente e disparar uma notificação de alteração da coleção usando o [redefinir](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction) ação em vez disso.

Normalmente, uma redefinição faz com que um controle liberar os elementos filho existentes e recomeçar, criando a interface do usuário, desde o início na posição de rolagem 0, pois ela tem exatamente como os dados foram alterados durante a redefinição não reconhece.

No entanto, se a coleção atribuída como ItemsSource dá suporte a identificadores exclusivos com a implementação de [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping) de interface, em seguida, o ItemsRepeater pode identificar rapidamente:

- UIElements reutilizáveis para dados que existiam antes e após a redefinição
- itens visíveis anteriormente foram removidos
- novos itens adicionados que serão visíveis

Isso permite que o ItemsRepeater evitar recomeçar da posição de rolagem 0. Ele também permite que ele restaurar rapidamente a UIElements para dados que não foram alterados em uma redefinição, resultando em um melhor desempenho.

Este exemplo mostra como exibir uma lista de itens em uma pilha vertical onde _MyItemsSource_ é uma fonte de dados personalizados que encapsula uma lista subjacente dos itens. Ela expõe um _dados_ propriedade que pode ser usada para reatribuir uma nova lista a ser usado como a fonte de itens, que, em seguida, dispara uma redefinição.

```xaml
<ScrollViewer x:Name="sv">
    <ItemsRepeater x:Name="repeater"
                ItemsSource="{x:Bind MyItemsSource}"
                ItemTemplate="{StaticResource MyTemplate}">
       <ItemsRepeater.Layout>
           <StackLayout ItemSpacing="8"/>
       </ItemsRepeater.Layout>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Similar to an ItemsControl, a developer sets the ItemsRepeater's ItemsSource.
    // Here we provide our custom source that supports unique IDs which enables
    // ItemsRepeater to be smart about handling resets from the data.
    // Unique IDs also make it easy to do things apply sorting/filtering
    // without impacting any state (i.e. selection).
    MyItemsSource myItemsSource = new MyItemsSource(data);

    repeater.ItemsSource = myItemsSource;

    // ...

    // We can sort/filter the data using whatever mechanism makes the
    // most sense (LINQ, database query, etc.) and then reassign
    // it, which in our implementation triggers a reset.
    myItemsSource.Data = someNewData;
}

// ...


public class MyItemsSource : IReadOnlyList<ItemBase>, IKeyIndexMapping, INotifyCollectionChanged
{
    private IList<ItemBase> _data;

    public MyItemsSource(IEnumerable<ItemBase> data)
    {
        if (data == null) throw new ArgumentNullException();

        this._data = data.ToList();
    }

    public IList<ItemBase> Data
    {
        get { return _data; }
        set
        {
            _data = value;

            // Instead of tossing out existing elements and re-creating them,
            // ItemsRepeater will reuse the existing elements and match them up
            // with the data again.
            this.CollectionChanged?.Invoke(
                this,
                new NotifyCollectionChangedEventArgs(NotifyCollectionChangedAction.Reset));
        }
    }

    #region IReadOnlyList<T>

    public ItemBase this[int index] => this.Data != null
        ? this.Data[index]
        : throw new IndexOutOfRangeException();

    public int Count => this.Data != null ? this.Data.Count : 0;
    public IEnumerator<ItemBase> GetEnumerator() => this.Data.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => this.GetEnumerator();

    #endregion

    #region INotifyCollectionChanged

    public event NotifyCollectionChangedEventHandler CollectionChanged;

    #endregion

    #region IKeyIndexMapping

    private int lastRequestedIndex = IndexNotFound;
    private const int IndexNotFound = -1;

    // When UniqueIDs are supported, the ItemsRepeater caches the unique ID for each item
    // with the matching UIElement that represents the item.  When a reset occurs the
    // ItemsRepeater pairs up the already generated UIElements with items in the data
    // source.
    // ItemsRepeater uses IndexForUniqueId after a reset to probe the data and identify
    // the new index of an item to use as the anchor.  If that item no
    // longer exists in the data source it may try using another cached unique ID until
    // either a match is found or it determines that all the previously visible items
    // no longer exist.
    public int IndexForUniqueId(string uniqueId)
    {
        // We'll try to increase our odds of finding a match sooner by starting from the
        // position that we know was last requested and search forward.
        var start = lastRequestedIndex;
        for (int i = start; i < this.Count; i++)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        // Then try searching backward.
        start = Math.Min(this.Count - 1, lastRequestedIndex);
        for (int i = start; i >= 0; i--)
        {
            if (this[i].PrimaryKey.Equals(uniqueId))
                return i;
        }

        return IndexNotFound;
    }

    public string UniqueIdForIndex(int index)
    {
        var key = this[index].PrimaryKey;
        lastRequestedIndex = index;
        return key;
    }

    #endregion
}

```

## <a name="create-a-custom-collection-control"></a>Criar um controle de coleção personalizada

Você pode usar o [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para criar um controle de coleta personalizado com seu próprio tipo de controle para apresentar a cada item.

> [!NOTE]
> Isso é semelhante a usar **ItemsControl**, mas em vez de derivar de **ItemsControl** e colocando um **ItemsPresenter** no modelo de controle, você deriva  **Controle** e insira um **ItemsRepeater** no modelo de controle. O controle de coleta personalizado "tem um" **ItemsRepeater** versus "é um" **ItemsControl**. Isso significa que você deve escolher explicitamente quais propriedades para expor, em vez de que herdado propriedades não oferecer suporte.

Este exemplo mostra como colocar uma [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no modelo de um controle personalizado chamado _MediaCollectionView_ e expor suas propriedades.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<Style TargetType="local:MediaCollectionView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="local:MediaCollectionView">
                <Border
                    Background="{TemplateBinding Background}"
                    BorderBrush="{TemplateBinding BorderBrush}"
                    BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer">
                        <muxc:ItemsRepeater x:Name="ItemsRepeater"
                                            ItemsSource="{TemplateBinding ItemsSource}"
                                            ItemTemplate="{TemplateBinding ItemTemplate}"
                                            Layout="{TemplateBinding Layout}"
                                            TabFocusNavigation="{TemplateBinding TabFocusNavigation}"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

```csharp
public sealed class MediaCollectionView : Control
{
    public object ItemsSource
    {
        get { return (object)GetValue(ItemsSourceProperty); }
        set { SetValue(ItemsSourceProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemsSource.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemsSourceProperty =
        DependencyProperty.Register(nameof(ItemsSource), typeof(object), typeof(MediaCollectionView), new PropertyMetadata(0));

    public DataTemplate ItemTemplate
    {
        get { return (DataTemplate)GetValue(ItemTemplateProperty); }
        set { SetValue(ItemTemplateProperty, value); }
    }

    // Using a DependencyProperty as the backing store for ItemTemplate.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty ItemTemplateProperty =
        DependencyProperty.Register(nameof(ItemTemplate), typeof(DataTemplate), typeof(MediaCollectionView), new PropertyMetadata(0));

    public Layout Layout
    {
        get { return (Layout)GetValue(LayoutProperty); }
        set { SetValue(LayoutProperty, value); }
    }

    // Using a DependencyProperty as the backing store for Layout.  This enables animation, styling, binding, etc...
    public static readonly DependencyProperty LayoutProperty =
        DependencyProperty.Register(nameof(Layout), typeof(Layout), typeof(MediaCollectionView), new PropertyMetadata(0));

    public MediaCollectionView()
    {
        this.DefaultStyleKey = typeof(MediaCollectionView);
    }
}
```

## <a name="display-grouped-items"></a>Exibir os itens agrupados

Você pode aninhar uma [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) na [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) de outro ItemsRepeater criar aninhada virtualizando layouts. O framework fará uso eficiente de recursos por meio da minimização desnecessária realização dos elementos que não estão visíveis ou próximo do visor atual.

Este exemplo mostra como você pode exibir uma lista de itens agrupados em uma pilha vertical. O ItemsRepeater externa gera cada grupo. No modelo para cada grupo, outro ItemsRepeater gera os itens.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          TextBlock Text="{x:Bind AppTitle}"/>
          <!-- Items -->
          <muxc:ItemsRepeater ItemsSource="{x:Bind Notifications}"
                              Layout="{StaticResource MyItemLayout}"
                              ItemTemplate="{StaticResource MyTemplate}"/>
          <!-- Footer -->
          <Button Content="{x:Bind FooterText}"/>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

Este exemplo mostra um layout para um aplicativo que tem várias categorias, que podem ser alterado com a preferência do usuário e são apresentadas como rolagem horizontal de listas, conforme mostrado aqui.

![Layout aninhado com itens repeater](images/items-repeater-nested-layout.png)

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <ScrollViewer HorizontalScrollMode="Enabled"
                                          VerticalScrollMode="Disabled"
                                          HorizontalScrollBarVisibility="Auto" >
            <muxc:ItemsRepeater ItemsSource="{x:Bind Items}"
                                Background="Orange">
              <muxc:ItemsRepeater.ItemTemplate>
                <DataTemplate x:DataType="local:CategoryItem">
                  <Grid Margin="10"
                        Height="60" Width="120"
                        Background="LightBlue">
                    <TextBlock Text="{x:Bind Name}"
                               Style="{StaticResource SubtitleTextBlockStyle}"
                               Margin="4"/>
                  </Grid>
                </DataTemplate>
              </muxc:ItemsRepeater.ItemTemplate>
              <muxc:ItemsRepeater.Layout>
                <muxc:StackLayout Orientation="Horizontal"/>
              </muxc:ItemsRepeater.Layout>
            </muxc:ItemsRepeater>
          </ScrollViewer>
        </StackPanel>
      </DataTemplate>
    </muxc:ItemsRepeater.ItemTemplate>
  </muxc:ItemsRepeater>
</ScrollViewer>
```

## <a name="bringing-an-element-into-view"></a>Colocar um elemento em modo de exibição

Estrutura do XAML já manipula a colocar um FrameworkElement em modo de exibição quando 1) recebe o foco do teclado ou 2) recebe o foco do Narrador. Pode haver outros casos em que você precise explicitamente colocar um elemento em modo de exibição. Por exemplo, em resposta a uma ação do usuário ou para restaurar o estado da interface do usuário depois de uma navegação de página.

Colocar um elemento virtualizado em modo de exibição envolve o seguinte:
1. Perceber um UIElement para um item
2. Executar o layout para garantir que o elemento tem uma posição válida
3. Iniciar uma solicitação para colocar o elemento realizado no modo de exibição

O exemplo a seguir demonstra estas etapas como parte da restauração a posição de rolagem de um item em uma lista simples, vertical, depois de uma navegação de página. No caso de dados hierárquicos usando ItemsRepeaters aninhada a abordagem é essencialmente o mesmo, mas deve ser feita em cada nível da hierarquia.

```xaml
<ScrollViewer x:Name="scrollviewer">
  <ItemsRepeater x:Name="repeater" .../>
</ScrollViewer>
```

```csharp
public class MyPage : Page
{
    // ...

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

            // retrieve saved offset + index(es) of the tracked element and then bring it into view.
            // ... 

            var element = repeater.GetOrCreateElement(index);

            // ensure the item is given a valid position
            element.UpdateLayout();

            element.StartBringIntoView(new BringIntoViewOptions()
            {
                VerticalOffset = relativeVerticalOffset
            });
    }

    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        base.OnNavigatingFrom(e);

        // retrieve and save the relative offset and index(es) of the scrollviewer's current anchor element ...
        var anchor = this.scrollviewer.CurrentAnchor;
        var index = this.repeater.GetElementIndex(anchor);
        var anchorBounds = anchor.TransformToVisual(this.scrollviewer).TransformBounds(new Rect(0, 0, anchor.ActualWidth, anchor.ActualHeight));
        relativeVerticalOffset = this.sv.VerticalOffset – anchorBounds.Top;
    }
}

```

## <a name="enable-accessibility"></a>Habilitar acessibilidade

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não fornece uma experiência de acessibilidade de padrão. A documentação sobre [facilidade de uso para aplicativos UWP](/windows/uwp/design/usability) fornece uma grande quantidade de informações para ajudá-lo a garantir que seu aplicativo fornece uma experiência de usuário inclusivo. Se você estiver usando o ItemsRepeater para criar um controle personalizado, em seguida, certifique-se ver a documentação em [pares de automação personalizado](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Opções de teclado
Suporte keyboarding mínimo para a movimentação de foco fornece ItemsRepeater baseia-se do XAML [2D navegação direcional para Keyboarding](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard).

![Direção de navegação](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

O ItemsRepeater [modo XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) é _habilitado_ por padrão. Dependendo da experiência pretendida, considere adicionar suporte para comuns [interações de teclado](/windows/uwp/design/input/keyboard-interactions) como Home, End, PageUp e PageDown.

O ItemsRepeater automaticamente garante que a ordem de tabulação padrão para seus itens (se virtualizado ou não) segue a mesma ordem em que os itens são fornecidos nos dados. Por padrão, o ItemsRepeater tem seu [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) propriedade definida como [uma vez](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) em vez do padrão comum de _Local_.

> [!NOTE]
> O ItemsRepeater automaticamente se lembra o último item focalizado.  Isso significa que, quando um usuário está usando Shift + Tab pode ser levados para a última percebido item.

### <a name="announcing-item-x-of-y-in-screen-readers"></a>Anunciando o "Item _X_ dos _Y_" em leitores de tela

Você precisa gerenciar definindo as propriedades de automação apropriada, como valores para **PositionInSet** e **SizeOfSet**e certifique-se de que elas permaneçam atualizadas quando itens são adicionados, movidos, removido, etc.

Em alguns layouts personalizados pode não haver uma sequência óbvia para a ordem visual.  No mínimo, os usuários esperam a que os valores das propriedades PositionInSet e SizeOfSet usadas pelos leitores de tela corresponderá a ordem que dos itens aparecem nos dados (deslocamento por 1 para coincidir com natural de contagem em vez de serem baseado em 0).

A melhor maneira de conseguir isso é fazendo com que o par de automação para a implementação de controle de item de [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) e [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) métodos e a posição do item no conjunto de dados de relatório representado pelo controle. O valor é computado somente em tempo de execução quando acessados por uma tecnologia assistencial e mantê-lo atualizado se torna um não problema. O valor corresponde a ordem dos dados.

Este exemplo mostra como você poderia fazer isso quando a apresentação de um controle personalizado chamado _CardControl_.

```xaml
<ScrollViewer >
    <ItemsRepeater x:Name="repeater" ItemsSource="{x:Bind MyItemsSource}">
       <ItemsRepeater.ItemTemplate>
           <DataTemplate x:DataType="local:CardViewModel">
               <local:CardControl Item="{x:Bind}"/>
           </DataTemplate>
       </ItemsRepeater.ItemTemplate>
   </ItemsRepeater>
</ScrollViewer>
```

```csharp
internal sealed class CardControl : CardControlBase
{
    protected override AutomationPeer OnCreateAutomationPeer() => new CardControlAutomationPeer(this);

    private sealed class CardControlAutomationPeer : FrameworkElementAutomationPeer
    {
        private readonly CardControl owner;

        public CardControlAutomationPeer(CardControl owner) : base(owner) => this.owner = owner;

        protected override int GetPositionInSetCore()
          => ((ItemsRepeater)owner.Parent)?.GetElementIndex(this.owner) + 1 ?? base.GetPositionInSetCore();

        protected override int GetSizeOfSetCore()
          => ((ItemsRepeater)owner.Parent)?.ItemsSourceView?.Count ?? base.GetSizeOfSetCore();
    }
}
```

## <a name="related-articles"></a>Artigos relacionados

- [Listas](lists.md)
- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
