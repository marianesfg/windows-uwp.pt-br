---
Description: ItemsRepeater é um controle leve para gerar e apresentar uma coleção de itens.
title: ItemsRepeater
label: ItemsRepeater
template: detail.hbs
ms.date: 02/01/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b7eb2aa8f753c3e8b956ed722d1f807362bc204
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081716"
---
# <a name="itemsrepeater"></a>ItemsRepeater

Use um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para criar experiências de coleção personalizadas usando um sistema de layout flexível, exibições personalizadas e virtualização.

Diferentemente de [ListView](/uwp/api/windows.ui.xaml.controls.listview), [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não proporciona uma experiência do usuário final abrangente: ele não tem interface do usuário padrão nem fornece nenhuma política relacionada a foco, seleção ou interação do usuário. Em vez disso, ele é um bloco de construção que você pode usar para criar suas próprias experiências baseadas em coleção e controles personalizados exclusivos. Embora ele não tenha nenhuma política interna, permite que você anexe a política para criar a experiência de que precisa. Por exemplo, você pode definir o layout a ser usado, a política de teclado, a política de seleção etc.

Você pode pensar em [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) conceitualmente como um painel controlado por dados, em vez de um controle completo, como ListView. Você especifica uma coleção de itens de dados a serem exibidos, um modelo de item que gera um elemento de interface do usuário para cada item de dados e um layout que determina como os elementos devem ser dimensionados e posicionados. Em seguida, ItemsRepeater produz elementos filho com base na fonte de dados e exibe-os conforme especificado pelo layout e pelo modelo de item. Não é necessário que os itens exibidos sejam homogêneos, pois o ItemsRepeater pode carregar o conteúdo para representar os itens de dados com base em critérios especificados por você em um seletor de modelo de dados.

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **ItemsRepeater** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da Biblioteca de interface do usuário do Windows:** [Classe ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
>
> **APIs de plataforma:** [Classe ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para criar exibições personalizadas para coleções de dados. Embora ele possa ser usado para apresentar um conjunto básico de itens, muitas vezes você pode usá-lo como o elemento de exibição no modelo de um controle personalizado.

Se você precisar de um controle pronto para uso para exibir dados em uma lista ou grade com personalização mínima, considere usar uma [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou uma [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

O ItemsRepeater não tem uma coleção de Itens internos. Se você precisa fornecer uma coleção de itens diretamente, em vez de associar a uma fonte de dados separado, é provável que precise de uma experiência com mais política e deve usar [ListView](/uwp/api/windows.ui.xaml.controls.listview) ou [GridView](/uwp/api/windows.ui.xaml.controls.gridview).

[ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) e ItemsRepeater permitem experiências de coleção personalizável, mas ItemsRepeater dá suporte a layouts de virtualização da interface do usuário, enquanto o ItemsControl, não. É recomendável usar ItemsRepeater, em vez de ItemsControl, seja apenas para apresentar alguns itens de dados ou para criar um controle de coleção personalizado.

> [!NOTE]
> Se você tiver uma situação em que sinta que ItemsControl atende às suas necessidades e ItemsRepeater não atende, deixe comentários sobre o [projeto do GitHub de Biblioteca de Interface do Usuário do Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues) e informe-nos.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abri-lo e ver o <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> em funcionamento.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="scrolling-with-itemsrepeater"></a>Rolagem com ItemsRepeater

[**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não é derivado de [**Controle**](/uwp/api/windows.ui.xaml.controls.control), portanto, não tem um modelo de controle. Portanto, não contém nenhuma rolagem interna, como uma ListView ou outros controles de coleção contêm.

Quando você usa um **ItemsRepeater**, deve fornecer funcionalidade de rolagem encapsulando-o em um controle [**ScrollViewer**](/uwp/api/windows.ui.xaml.controls.scrollviewer).

> [!NOTE]
> Se seu aplicativo for ser executado em versões anteriores do Windows – aquelas lançadas *antes* do Windows 10, versão 1809 –, você também precisará hospedar o **ScrollViewer** dentro de [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost). 
> ```xaml
> <muxc:ItemsRepeaterScrollHost>
>     <ScrollViewer>
>         <muxc:ItemsRepeater ... />
>     </ScrollViewer>
> </muxc:ItemsRepeaterScrollHost>
> ```
> Se seu aplicativo será executado apenas em versões recentes do Windows 10, versão 1809 e posteriores, assim, não é necessário usar o [**ItemsRepeaterScrollHost**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeaterscrollhost).
>
> Antes do Windows 10, versão 1809, o **ScrollViewer** não implementava a interface [**IScrollAnchorProvider**](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) de que o **ItemsRepeater** precisa.  O **ItemsRepeaterScrollHost** permite que o **ItemsRepeater** coordene-se com **ScrollViewer** em versões anteriores para preservar corretamente o local visível dos itens que o usuário está exibindo.  Caso contrário, os itens podem parecer se mover ou desaparecer de repente quando os itens na lista são alterados ou o aplicativo é redimensionado.

## <a name="create-an-itemsrepeater"></a>Criar um ItemsRepeater

Para usar um [**ItemsRepeater**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), você precisa dar a ele os dados a serem exibidos definindo a propriedade **ItemsSource**. Em seguida, informe-o de como exibir os itens definindo a propriedade [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate).

### <a name="itemssource"></a>ItemsSource

Para preencher a exibição, defina a propriedade [**ItemsSource**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) como uma coleção de itens de dados. Aqui, **ItemsSource** é definido em código diretamente como uma instância de uma coleção.

```csharp
ObservableCollection<string> Items = new ObservableCollection<string>();

ItemsRepeater itemsRepeater1 = new ItemsRepeater();
itemsRepeater1.ItemsSource = Items;
```

Você também pode associar a propriedade **ItemsSource** a uma coleção em XAML. Para saber mais sobre vinculação de dados, consulte [Visão geral de vinculação de dados](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart).


```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}"/>
```

### <a name="itemtemplate"></a>ItemTemplate
Para especificar como um item de dados é visualizado, defina a propriedade [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) como um [**DataTemplate**](/uwp/api/windows.ui.xaml.datatemplate) ou [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) você definiu. O modelo de dados de um item define como os dados são visualizados. Por padrão, o item é exibido no modo de exibição com um **TextBlock** que usa a representação de cadeia de caracteres do objeto de dados.

No entanto, você geralmente deseja mostrar uma apresentação mais avançada de seus dados usando um modelo que define o layout e a aparência de um ou mais controles que você usará para exibir um item individual. Os controles usados no modelo podem ser associado às propriedades do objeto de dados ou ter conteúdo estático definido de modo embutido.

#### <a name="datatemplate"></a>DataTemplate
Neste exemplo, o item de dados é uma cadeia de caracteres simples. O **DataTemplate** inclui uma imagem à esquerda do texto e define o estilo de **TextBlock** para exibir a cadeia de caracteres em uma cor azul-petróleo.

> [!NOTE]
> Ao usar a [extensão de marcação x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) em um **DataTemplate**, você precisa especificar o DataType (`x:DataType`) no DataTemplate.

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

É assim que os itens apareceriam quando exibidos com este **DataTemplate**.

![Itens exibidos com um modelo de dados](images/listview-itemstemplate.png)

O número de elementos usados no **DataTemplate** para um item poderá ter um impacto significativo no desempenho se o modo de exibição exibir um grande número de itens. Para obter mais informações e exemplos de como usar **DataTemplates** para definir a aparência dos itens em sua lista, veja [Contêineres e modelos de item](item-containers-templates.md).

> [!TIP]
> Para conveniência, quando você quiser declarar o modelo embutido, em vez de referenciá-lo como um recurso estático, poderá especificar o **DataTemplate** ou o **DataTemplateSelector** como o filho direto de **ItemsRepeater**.  Ele será atribuído como o valor da propriedade **ItemTemplate**. Por exemplo, isto é válido:
> ```xaml
> <ItemsRepeater ItemsSource="{x:Bind Items}">
>     <DataTemplate>
>         <!-- ... -->
>     </DataTemplate>
> </ItemsRepeater>
> ```

> [!TIP]
> Diferentemente de **ListView** e outros controles de coleção, o **ItemsRepeater** não encapsula os elementos de um **DataTemplate** com um contêiner de itens adicionais que inclui política padrão, como margens, preenchimento, visuais de seleção ou um ponteiro sobre o estado visual. Em vez disso, **ItemsRepeater** apresenta apenas o que é definido no **DataTemplate**. Se você quiser que os itens tenham a mesma aparência que um item de exibição de lista, poderá incluir explicitamente um contêiner, como **ListViewItem**, em seu modelo de dados. **ItemsRepeater** mostrará os visuais **ListViewItem**, mas não usará automaticamente outras funcionalidades, como seleção ou mostrar a caixa de seleção múltipla.
>
> Da mesma forma, se a coleta de dados for uma coleção de controles reais, como **botão** (`List<Button>`), você poderá colocar um **ContentPresenter** no seu **DataTemplate** para exibir o controle.

#### <a name="datatemplateselector"></a>DataTemplateSelector

Os itens exibidos no modo de exibição não precisam ser do mesmo tipo. Você pode fornecer a propriedade [**ItemTemplate**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) com um [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector) para selecionar diferentes **DataTemplate**s com base em critérios especificados por você.

Este exemplo pressupõe que foi definido um **DataTemplateSelector** que decide entre dois diferentes **DataTemplate**s para representar um item Pequeno e Grande.

```xaml
<ItemsRepeater ...>
    <ItemsRepeater.ItemTemplate>
        <local:VariableSizeTemplateSelector Large="{StaticResource LargeItemTemplate}" 
                                            Small="{StaticResource SmallItemTemplate}"/>
    </ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Ao definir um **DataTemplateSelector** para usar com **ItemsRepeater**, você só precisa implementar uma substituição para o método [**SelectTemplateCore(Object)** ](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore#Windows_UI_Xaml_Controls_DataTemplateSelector_SelectTemplateCore_System_Object_). Para obter mais informações e exemplos, veja [**DataTemplateSelector**](/uwp/api/windows.ui.xaml.controls.datatemplateselector).

> [!NOTE]
> Uma alternativa a **DataTemplate**s para gerenciar como os elementos são criados em cenários mais avançados é implementar seu próprio [**Windows.UI.Xaml.Controls.IElementFactory**](/uwp/api/windows.ui.xaml.controls.ielementfactory) a ser usado como o **ItemTemplate**.  Ele será responsável por gerar o conteúdo quando solicitado.

## <a name="configure-the-data-source"></a>Configurar a fonte de dados

Use a propriedade [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemssource) para especificar a coleção a ser usada para gerar o conteúdo dos itens. Você pode definir o ItemsSource como qualquer tipo que implemente **IEnumerable**. Interfaces de coleção adicionais implementadas por sua fonte de dados determinam qual funcionalidade está disponível para o ItemsRepeater interagir com seus dados.

Esta lista mostra as interfaces disponíveis e quando considerar o uso de cada uma delas.

- [IEnumerable](/dotnet/api/system.collections.generic.ienumerable-1)(.NET) / [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)

  - Pode ser usada para conjuntos de dados pequenos e estáticos.

    No mínimo, a fonte de dados deve implementar a interface IEnumerable / IIterable. Se isso for tudo o que é compatível, o controle iterará por tudo uma vez para criar uma cópia que pode ser usada para acessar itens por meio de um valor de índice.

- [IReadonlyList](/dotnet/api/system.collections.generic.ireadonlylist-1)(.NET) / [IVectorView](/uwp/api/windows.foundation.collections.ivectorview_t_)

  - Pode ser usado para conjuntos de dados estáticos somente leitura.

    Permite que o controle acesse itens por índice e evita a cópia interna redundante.

- [IList](/dotnet/api/system.collections.generic.ilist-1)(.NET) / [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

  - Pode ser usado para conjuntos de dados estáticos.

    Permite que o controle acesse itens por índice e evita a cópia interna redundante.

    **Aviso**: Alterações a lista/vetor sem implementar [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged) não serão refletidas na interface do usuário.

- [INotifyCollectionChanged](/dotnet/api/system.collections.specialized.inotifycollectionchanged)(.NET)

  - Recomendado para dar suporte à notificação de alteração.

    Permite que o controle observe e reaja a alterações na fonte de dados e reflita essas alterações na interface do usuário.

- [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)

  - Dá suporte para notificação de alteração

    Como a interface **INotifyCollectionChanged**, permite que o controle observe e reaja a alterações na fonte de dados.

    **Aviso**: O Windows.Foundation.IObservableVector\<T> não dá suporte a uma ação de "Mover". Isso pode fazer a interface do usuário de um item perder seu estado visual.  Por exemplo, um item selecionado no momento e/ou com foco em que a movimentação é obtida por uma ação de "Remover" seguida por "Adicionar" perderá o foco e não será mais selecionado.

    O Platform.Collections.Vector\<T> usa IObservableVector\<T> e tem essa mesma limitação. Se o suporte para uma ação de 'Mover' for necessário, use a interface **INotifyCollectionChanged**.  A classe do .NET ObservableCollection\<T> usa **INotifyCollectionChanged**.

- [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping)

  - Quando um identificador exclusivo pode ser associado a cada item.  Recomendado ao usar 'Redefinir' como a ação de alteração da coleção.

    Permite que o controle recupere com muita eficiência a interface do usuário existente depois de receber uma ação de 'Redefinir' rígida como parte de um evento **INotifyCollectionChanged** ou **IObservableVector**. Depois de receber uma redefinição, o controle usará a ID exclusiva fornecida para associar os dados atuais aos elementos que já foram criados. Sem a chave para o mapeamento de índice, o controle teria que precisaria supor que precisa começar do zero na criação da interface do usuário para os dados.

As interfaces listadas acima, a não ser IKeyIndexMapping, fornecem o mesmo comportamento no ItemsRepeater que em ListView e GridView.


As interfaces a seguir em um ItemsSource habilitam uma funcionalidade especial nos controles ListView e GridView, mas atualmente não têm efeito sobre um ItemsRepeater:

- [ISupportIncrementalLoading](/uwp/api/windows.ui.xaml.data.isupportincrementalloading)
- [IItemsRangeInfo](/uwp/api/windows.ui.xaml.data.iitemsrangeinfo)
- [ISelectionInfo](/uwp/api/windows.ui.xaml.data.iselectioninfo)

> [!TIP]
> Seus comentários são muito importantes para nós! Queremos saber sua opinião sobre o [projeto do GitHub de Biblioteca de Interface do Usuário do Windows](https://github.com/Microsoft/microsoft-ui-xaml/issues). Considere adicionar suas ideias sobre propostas existentes, como [#374](https://github.com/Microsoft/microsoft-ui-xaml/issues/374): Adicione suporte a carregamento incremental para ItemsRepeater.

Uma abordagem alternativa a carregar incrementalmente os dados conforme o usuário rola para cima ou para baixo é observar a posição do visor do ScrollViewer e carregar mais dados conforme o visor se aproxima da extensão.

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

Os itens mostrados pelo [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) são organizados por um [objeto Layout](/uwp/api/microsoft.ui.xaml.controls.layout) que gerencia o dimensionamento e o posicionamento de seus elementos filho. Quando usado com um ItemsRepeater, o objeto de Layout permite a virtualização da interface do usuário. Os layouts fornecidos são [StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) e [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout). Por padrão, o ItemsRepeater usa um StackLayout com orientação vertical.

### <a name="stacklayout"></a>StackLayout

[StackLayout](/uwp/api/microsoft.ui.xaml.controls.stacklayout) organiza os elementos em uma única linha que você pode orientar horizontal ou verticalmente.

Você pode definir a propriedade [Espaçamento](/uwp/api/microsoft.ui.xaml.controls.stacklayout.spacing) para ajustar a quantidade de espaço entre os itens. O espaçamento é aplicado na direção da [Orientação](/uwp/api/microsoft.ui.xaml.controls.stacklayout.orientation) do layout.

![Espaçamento do layout de pilha](images/stack-layout.png)

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

O [UniformGridLayout](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout) posiciona os elementos sequencialmente em um layout de disposição. Os itens são dispostos na ordem da esquerda para a direita quando a [Orientação](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.orientation) é **Horizontal** e dispostos de cima para baixo quando a Orientação é **Vertical**. Cada item é dimensionado igualmente.

![Espaçamento de layout de grade uniforme](images/uniform-grid-layout.png)

O número de itens em cada linha de um layout horizontal é influenciado pela largura mínima do item. O número de itens em cada coluna de um layout vertical é influenciado pela altura mínima do item.

- Você pode fornecer explicitamente um tamanho mínimo para usar definindo as propriedades [MinItemHeight](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemheight) e [MinItemWidth](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minitemwidth).
- Se você não especificar um tamanho mínimo, o tamanho medido do primeiro item será considerado o tamanho mínimo por item.

Você também pode definir o espaçamento mínimo para o layout a ser incluído entre linhas e colunas definindo as propriedades [MinColumnSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.mincolumnspacing) e [MinRowSpacing](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.minrowspacing).

![Espaçamento e dimensionamento uniformes de grade](images/uniform-grid-sizing-spacing.png)

Após o número, se itens em uma linha ou coluna tiverem sido determinados com base no espaçamento e o tamanho do item mínima, poderá haver espaço não utilizado deixado após o último item na linha ou coluna (conforme ilustrado na imagem anterior). Você pode especificar se qualquer espaço extra é ignorado, usado para aumentar o tamanho de cada item ou usado para criar o espaço extra entre os itens. Isso é controlado pelas propriedades [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) e [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification).

Você pode definir a propriedade [ItemsStretch](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsstretch) para especificar como o tamanho do item é aumentado para preencher o espaço não utilizado.

Essa lista mostra os valores disponíveis. As definições presumem o padrão **Orientação** de **Horizontal**.

- **Nenhum**: O espaço extra é deixado sem uso no final da linha. Este é o padrão.
- **Fill**: os itens recebem largura extra para usar o espaço disponível (altura, se vertical).
- **Uniform**: os itens recebem largura extra para usar o espaço disponível e recebem altura extra para manter a taxa de proporção (altura e largura são alternadas, se vertical).

Esta imagem mostra o efeito dos valores **ItemsStretch** em um layout horizontal.

![Ampliação do item de grade uniforme](images/uniform-grid-item-stretch.png)

Quando **ItemsStretch** é **None**, você pode definir a propriedade [ItemsJustification](/uwp/api/microsoft.ui.xaml.controls.uniformgridlayout.itemsjustification) para especificar como espaço extra é usado para alinhar os itens.

Essa lista mostra os valores disponíveis. As definições presumem o padrão **Orientação** de **Horizontal**.

- **Start**: Os itens são alinhados com o início da linha. O espaço extra é deixado sem uso no final da linha. Este é o padrão.
- **Center**: Os itens são alinhados no centro da linha. O espaço extra é dividido igualmente no início e no fim da linha.
- **End**: Os itens são alinhados com o fim da linha. O espaço extra é deixado sem uso no início da linha.
- **SpaceAround**: Os itens são distribuídos uniformemente. Uma quantidade igual de espaço é adicionada antes e depois de cada item.
- **SpaceBetween**: Os itens são distribuídos uniformemente. Uma quantidade igual de espaço é adicionada antes e entre cada item. Nenhum espaço é adicionado no início e no fim da linha.
- **SpaceEvenly**: Os itens são distribuídos uniformemente com uma quantidade igual de espaço entre cada item e no início e no fim da linha.

Esta imagem mostra o efeito dos valores **ItemsStretch** em um layout vertical (aplicado a colunas, em vez de linhas).

![Justificação de item de grade uniforme](images/uniform-grid-item-justification.png)

> [!TIP]
> A propriedade **ItemsStretch** afeta a passagem de _medir_ do layout. A propriedade **ItemsJustification** afeta a passagem de _organizar_ do layout.

Este exemplo mostra como definir a propriedade **ItemsRepeater.Layout** para um **UniformGridLayout**.

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

Quando você hospeda itens em um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), pode ser necessário executar alguma ação quando um item é mostrado ou deixa de ser mostrado, como iniciar um download assíncrono de algum conteúdo, associar o elemento a um mecanismo para controlar a seleção ou interromper uma tarefa em segundo plano.

Em um controle de virtualização, você não pode contar com eventos Carregados/Descarregados, pois o elemento não pode ser removido da árvore visual em tempo real quando ele é reciclado. Em vez disso, outros eventos são fornecidos para gerenciar o ciclo de vida de elementos. Este diagrama mostra o ciclo de vida de um elemento em um ItemsRepeater e quando os eventos relacionados são gerados.

![Diagrama de ciclo de vida de eventos](images/items-repeater-lifecycle.png)

- [**ElementPrepared**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementprepared) ocorre sempre que um elemento fica pronto para uso. Isso ocorre para um elemento criado recentemente, bem como para um elemento que já existe e está sendo usado novamente da fila de reciclagem.
- [**ElementClearing**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementclearing) ocorre imediatamente sempre que um elemento é enviado para a fila de reciclagem, como quando ele fica fora do intervalo de itens realizados.
- [**ElementIndexChanged**](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.elementindexchanged
) ocorre para cada realizadas UIElement realizado em que o índice do item que ele representa mudou. Por exemplo, quando outro item é adicionado ou removido da fonte de dados, o índice para itens que vêm depois na ordenação recebe este evento.

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

## <a name="sorting-filtering-and-resetting-the-data"></a>Classificar, filtrar e redefinir dados

Quando você executa ações como filtrar ou classificar seu conjunto de dados, tradicionalmente podia comparar o conjunto de dados anterior com o novo, então emitir notificações de alteração granular por meio de [INotifyCollectionChanged](/uwp/api/windows.ui.xaml.interop.inotifycollectionchanged). No entanto, muitas vezes é mais fácil substituir completamente os dados antigos pelos novos dados e disparar uma notificação de alteração da coleção usando a ação [Redefinir](/uwp/api/windows.ui.xaml.interop.notifycollectionchangedaction), em vez disso.

Normalmente, uma redefinição faz com que um controle lance os elementos filho existentes e recomece, criando a interface do usuário desde o início na posição de rolagem 0, já que ela não tem conhecimento de exatamente como os dados foram alterados durante a redefinição.

No entanto, se a coleção atribuída como ItemsSource for compatível com identificadores exclusivos implementando a interface [IKeyIndexMapping](/uwp/api/microsoft.ui.xaml.controls.ikeyindexmapping), o ItemsRepeater poderá identificar rapidamente:

- UIElements reutilizáveis para dados que existiam antes e depois da redefinição
- itens visíveis anteriormente que foram removidos
- novos itens adicionados que serão visíveis

Isso permite que o ItemsRepeater evite recomeçar da posição de rolagem 0. Também permite que ele restaure rapidamente UIElements para dados que não foram alterados em uma redefinição, resultando em um melhor desempenho.

Este exemplo mostra como exibir uma lista de itens em uma pilha vertical em que _MyItemsSource_ é uma fonte de dados personalizada que encapsula uma lista subjacente de itens. Expõe uma propriedade _Data_ que pode ser usada para reatribuir uma nova lista a ser usada como a fonte de itens, que, em seguida, dispara uma redefinição.

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

## <a name="create-a-custom-collection-control"></a>Criar um controle de coleção personalizado

Você pode usar o [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) para criar um controle de coleção personalizado com seu próprio tipo de controle para apresentar a cada item.

> [!NOTE]
> Isso é semelhante a usar **ItemsControl**, mas, em vez de derivar de **ItemsControl** e colocar um **ItemsPresenter** no modelo de controle, você deriva **Control** e insere um **ItemsRepeater** no modelo de controle. O controle de coleção personalizado "tem um" **ItemsRepeater** versus "é um" **ItemsControl**. Isso significa que você deve escolher explicitamente quais propriedades expor, em vez de quais propriedades herdadas não são compatíveis.

Este exemplo mostra como colocar um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no modelo de um controle personalizado chamado _MediaCollectionView_ e expor suas propriedades.

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

Você pode aninhar um [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) no [ItemTemplate](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.itemtemplate) de outro ItemsRepeater para criar layouts de virtualização aninhados. A estrutura usará de modo eficiente os recursos minimizando a realização desnecessária de elementos que não estão visíveis ou estão perto do visor atual.

Este exemplo mostra como você pode exibir uma lista de itens agrupados em uma pilha vertical. O ItemsRepeater externo gera cada grupo. No modelo para cada grupo, outro ItemsRepeater gera os itens.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->

<Page.Resources>
    <muxc:StackLayout x:Key="MyGroupLayout"/>
    <muxc:StackLayout x:Key="MyItemLayout" Orientation="Horizontal"/>
</Page.Resources>

<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind AppNotifications}"
                      Layout="{StaticResource MyGroupLayout}">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="ExampleApp:AppNotifications">
        <!-- Group -->
        <StackPanel>
          <!-- Header -->
          <TextBlock Text="{x:Bind AppTitle}"/>
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
A imagem abaixo mostra o layout básico que é criado usando o exemplo acima como uma diretriz.

![Layout aninhado com repetidor de itens](images/items-repeater-nested-layout.png)

Este exemplo mostra um layout para um aplicativo que tem várias categorias que podem ser alteradas com a preferência do usuário e são apresentadas como rolagem horizontal de listas. O layout desse exemplo também é representado pela imagem acima.

```xaml
<!-- xmlns:muxc="using:Microsoft.UI.Xaml.Controls" -->
<!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
<ScrollViewer>
  <muxc:ItemsRepeater ItemsSource="{x:Bind Categories}"
                      Background="LightGreen">
    <muxc:ItemsRepeater.ItemTemplate>
      <DataTemplate x:DataType="local:Category">
        <StackPanel Margin="12,0">
          <TextBlock Text="{x:Bind Name}" Style="{ThemeResource TitleTextBlockStyle}"/>
          <!-- Include the <muxc:ItemsRepeaterScrollHost> if targeting Windows 10 versions earlier than 1809. -->
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

## <a name="bringing-an-element-into-view"></a>Colocar um elemento na exibição

A estrutura XAML já processa colocar um FrameworkElement em exibição quando 1) recebe o foco do teclado ou 2) recebe o foco do Narrador. Pode haver outros casos em que você precise explicitamente colocar um elemento em exibição. Por exemplo, em resposta a uma ação do usuário ou para restaurar o estado da interface do usuário depois de uma navegação de página.

Colocar um elemento virtualizado em exibição envolve o seguinte:
1. Realizar um UIElement para um item
2. Executar o layout para garantir que o elemento tenha uma posição válida
3. Iniciar uma solicitação para colocar o elemento realizado na exibição

O exemplo abaixo demonstra estas etapas como parte de restaurar a posição de rolagem de um item em uma lista simples vertical depois de uma navegação de página. No caso de dados hierárquicos usando ItemsRepeaters aninhados, a abordagem é essencialmente a mesma, mas deve ser feita em cada nível da hierarquia.

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

[ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater) não fornece uma experiência de acessibilidade padrão. A documentação sobre [Facilidade de uso para aplicativos UWP](/windows/uwp/design/usability) fornece uma grande quantidade de informações para ajudá-lo a garantir que seu aplicativo proporcione uma experiência de usuário inclusiva. Se você estiver usando o ItemsRepeater para criar um controle personalizado, veja a documentação sobre [Pares de automação personalizados](/windows/uwp/design/accessibility/custom-automation-peers).

### <a name="keyboarding"></a>Teclado
O suporte para teclado mínimo para a movimentação de foco que o ItemsRepeater fornece baseia-se na [Navegação direcional 2D para teclado](/windows/uwp/design/input/focus-navigation#2d-directional-navigation-for-keyboard) de XAML.

![Navegação de direção](/windows/uwp/design/input/images/keyboard/directional-navigation.png)

O [modo XYFocusKeyboardNavigation](/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) do ItemsRepeater está _Habilitado_ por padrão. Dependendo da experiência pretendida, considere adicionar suporte para [interações de teclado](/windows/uwp/design/input/keyboard-interactions) comuns, como Home, End, PageUp e PageDown.

O ItemsRepeater automaticamente garante que a ordem de tabulação padrão para seus itens (seja virtualizado ou não) siga a mesma ordem em que os itens são fornecidos nos dados. Por padrão, o ItemsRepeater tem sua propriedade [TabFocusNavigation](/uwp/api/windows.ui.xaml.uielement.tabfocusnavigation) definida como [Uma vez](/uwp/api/windows.ui.xaml.input.keyboardnavigationmode), e não como o padrão comum de _Local_.

> [!NOTE]
> O ItemsRepeater não se lembra automaticamente do último item focalizado.  Isso significa que, quando um usuário está usando Shift+Tab, pode ser levado para o último item realizado.

### <a name="announcing-item-_x_-of-_y_-in-screen-readers"></a>Anunciando o "Item _X_ de _Y_" em Leitores de Tela

Você precisa gerenciar a definição das propriedades de automação apropriadas, como valores para **PositionInSet** e **SizeOfSet**, além de mantê-las atualizadas quando os itens forem adicionados, movidos, removidos etc.

Em alguns layouts personalizados, pode não haver uma sequência óbvia para a ordem visual.  No mínimo, os usuários esperam que os valores das propriedades PositionInSet e SizeOfSet usadas pelos leitores de tela correspondam à ordem em que os itens aparecem nos dados (compensado em 1 para coincidir com a contagem natural, em vez de serem baseados em 0).

A melhor maneira de conseguir isso é fazendo com que o par de automação para o controle de item implemente os métodos [GetPositionInSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpositioninsetcore) e [GetSizeOfSetCore](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getsizeofsetcore) e relate a posição do item no conjunto de dados representado pelo controle. O valor é computado somente em tempo de execução quando acessado por uma tecnologia assistencial e mantê-lo atualizado deixa de ser um problema. O valor corresponde à ordem dos dados.

Este exemplo mostra como você poderia fazer isso ao apresentar um controle personalizado chamado _CardControl_.

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
