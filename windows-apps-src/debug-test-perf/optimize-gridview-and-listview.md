---
author: mcleblanc
ms.assetid: 26DF15E8-2C05-4174-A714-7DF2E8273D32
title: Otimização da interface do usuário em ListView e GridView
description: Melhore o desempenho e o tempo de inicialização em ListView e GridView por meio de virtualização da interface do usuário, redução de elementos e atualização progressiva de itens.
---
# Otimização da interface do usuário em ListView e GridView

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Observação**  
Para obter mais detalhes, consulte a sessão //build/ em [Dramatically Increase Performance when Users Interact with Large Amounts of Data in GridView and ListView](https://channel9.msdn.com/events/build/2013/3-158).

Melhore o desempenho e o tempo de inicialização em [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) por meio de virtualização da interface do usuário, redução de elementos e atualização progressiva de itens. Para técnicas de virtualização de dados, consulte [Virtualização de dados de ListView e GridView](listview-and-gridview-data-optimization.md).

## Dois fatores importantes no desempenho de coleções

A manipulação de coleções é um cenário comum. Um visualizador de fotos tem coleções de fotos, um leitor tem coleções de artigos/livros/histórias, e um aplicativo de compras tem coleções de produtos. Este tópico mostra o que você pode fazer para tornar seu aplicativo eficiente na manipulação de coleções.

Há dois fatores importantes de desempenho quando se trata de coleções: um é o tempo gasto pelo thread de interface do usuário na criação de itens; o outro é a memória usada pelo conjunto de dados brutos e os elementos de interface do usuário usados para renderizar esses dados.

Para o movimento panorâmico/rolagem suave, é essencial que o thread de interface do usuário faça um trabalho eficiente e inteligente de instanciação, associação de dados e definição do layout de itens.

## Virtualização de interface do usuário

A virtualização da interface do usuário é o aprimoramento mais importante que você pode fazer. Isso significa que os elementos de interface do usuário que representam os itens são criados por demanda. Para uma associação de controle de itens para uma coleção de 1.000 itens, seria um desperdício de recursos criar a interface do usuário para todos os itens ao mesmo tempo, pois eles não podem ser todos exibidos ao mesmo tempo. [
              **ListView**
            ](https://msdn.microsoft.com/library/windows/apps/BR242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) (e outros controles derivados de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) padrão) executam a virtualização de interface do usuário para você. Quando os itens estão quase sendo rolados para a exibição (a algumas páginas distância), a estrutura gera a interface do usuário para os itens e os armazena em cache. Quando torna-se improvável que os itens sejam mostrados novamente, a estrutura recupera a memória.

Se você oferece um modelo de painel de itens personalizado (consulte [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemspanel.aspx)), certifique-se de usar um painel de virtualização como [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) ou [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795). Se você usar [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227651), [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/BR227717) ou [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635), não obterá a virtualização. Além disso, os seguintes eventos [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) serão gerados somente quando você estiver usando um [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Dn298849) ou um [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/Dn298795): [**ChoosingGroupHeaderContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer), [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) e [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging).

O conceito de um visor é crítico para a virtualização da interface do usuário, pois a estrutura precisa criar os elementos que provavelmente serão exibidos. No geral, o visor de um [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) é a extensão do controle lógico. Por exemplo, o visor de uma [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) é a largura e a altura do elemento **ListView**. Alguns painéis permitem espaço ilimitado para elementos filho, como exemplo há [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) e um [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), com linhas ou colunas dimensionadas automaticamente. Quando um **ItemsControl** virtualizado é colocado em um painel assim, ele ocupa espaço suficiente para exibir todos os itens, o que destrói a virtualização. Restaure a virtualização configurando uma largura e uma altura no **ItemsControl**.

## Redução de elemento por item

Mantenha a quantidade de elementos de interface do usuário usados para renderizar seus itens em um mínimo razoável.

Quando um controle de itens é mostrado pela primeira vez, todos os elementos necessários para renderizar um visor completo de itens são criados. Além disso, conforme os itens se aproximam do visor, a estrutura atualiza os elementos de interface do usuário nos modelos de item em cache com os objetos de dados vinculados. Minimizar a complexidade da marcação dentro de modelos compensa na memória e no tempo gasto no thread da interface do usuário, melhorando a capacidade de resposta principalmente em movimentos panorâmicos/rolagem. Os modelos em questão são o modelo de item (consulte [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)) e o modelo de controle de um [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) ou um [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) (o modelo de controle de item ou [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)). A vantagem de até mesmo uma pequena redução na contagem de elementos é multiplicada pela quantidade de itens exibidos.

Para obter exemplos de redução de elemento, consulte [Otimizar sua marcação XAML](optimize-xaml-loading.md).

Os modelos de controle padrão [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) e [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridviewitem.aspx) contêm um elemento [**ListViewItemPresenter** ](https://msdn.microsoft.com/library/windows/apps/Dn298500). Este apresentador é um único elemento otimizado que exibe elementos visuais complexos para foco, seleção e outros estados visuais. Se você já tem modelos de controle de item personalizados ([**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle)) ou se, no futuro, você editar uma cópia de um modelo de controle de item, recomendamos a utilização de um **ListViewItemPresenter**, pois esse elemento proporcionará um ótimo equilíbrio entre desempenho e personalização na maioria dos casos. Você personaliza o apresentador definindo propriedades nele. Por exemplo, veja a seguir uma marcação que remove a marca de verificação que aparece por padrão quando um item é selecionado e muda a cor de fundo do item escolhido para laranja.

```xml
...
<ListView>
 ...
 <ListView.ItemContainerStyle>
 <Style TargetType="ListViewItem">
 <Setter Property="Template">
 <Setter.Value>
 <ControlTemplate TargetType="ListViewItem">
 <ListViewItemPresenter SelectionCheckMarkVisualEnabled="False" SelectedBackground="Orange"/>
 </ControlTemplate>
 </Setter.Value>
 </Setter>
 </Style>
 </ListView.ItemContainerStyle>
</ListView>
<!-- ... -->
```

Há cerca de 25 propriedades com nomes autodescritivos semelhantes a [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectioncheckmarkvisualenabled.aspx) e [**SelectedBackground**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.selectedbackground.aspx). Caso os tipos de apresentador não sejam personalizáveis o bastante para seu caso de uso, você pode editar uma cópia do modelo de controle `ListViewItemExpanded` ou `GridViewItemExpanded` em vez disso. Eles podem ser encontrados em `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Lembre-se de que usar esses modelos significa trocar parte do desempenho pelo aumento da personalização.

## Atualizar progressivamente os itens em ListView e GridView

Se você está virtualização de dados, então pode manter a capacidade de resposta em [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) elevada configurando o controle para renderizar elementos de interface do usuário temporários para os itens que ainda estão sendo baixados ou carregados. Os elementos temporários são, então, substituídos progressivamente pela interface do usuário real conforme os dados carregam.

Além disso, não importa de onde você está carregando os dados (disco local, rede ou nuvem), um usuário pode aplicar panorâmica/rolar uma [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) ou uma [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) tão rapidamente que não seria possível renderizar cada item com fidelidade total ao mesmo tempo em que é mantida a fluidez do movimento panorâmico/rolagem. Para preservar o movimento panorâmico/rolagem suave, você pode optar por renderizar um item em várias fases além de usar espaços reservados.

Um exemplo dessas técnicas é geralmente visto em aplicativos de exibição de fotos: embora nem todas as imagens tenham sido carregadas e exibidas, o usuário ainda pode fazer o movimento panorâmico/rolagem e interagir com a coleção. Ou, para um item de "filme", você pode mostrar o título na primeira fase, a classificação na segunda fase e uma imagem do pôster na terceira fase. O usuário vê os dados importantes sobre cada item o mais cedo possível, e isso significa que eles podem executar a ação de uma vez. Em seguida, as informações menos importantes são preenchidas conforme o tempo permite. Estes são os recursos de plataforma que você pode usar para implementar essas técnicas.

### Espaços reservados

O recurso de elementos visuais de espaço reservado temporário fica ativado por padrão e é controlado com a propriedade [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders). Durante o rápido movimento panorâmico/rolagem, esse recurso dá ao usuário uma dica visual de que há mais itens que ainda serão exibidos totalmente, ao mesmo tempo em que a fluidez é preservada. Se você usar uma das técnicas abaixo, pode configurar **ShowsScrollingPlaceholders** para falso, caso prefira que o sistema não renderize espaços reservados.

**Atualizações progressivas de modelos de dados usando x:Phase**

Consulte aqui como usar o [atributo x:Phase](https://msdn.microsoft.com/library/windows/apps/Mt204790) com associações [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) para implementar atualizações progressivas de modelos de dados.

1.  Esta é a aparência da fonte de associação (esta é a fonte de dados que será vinculada).

    ```csharp
namespace LotsOfItems
    {
        public class ExampleItem
        {
            public string Title { get; set; }
            public string Subtitle { get; set; }
            public string Description { get; set; }
        }

        public class ExampleItemViewModel
        {
            private ObservableCollection<ExampleItem> exampleItems = new ObservableCollection<ExampleItem>();
            public ObservableCollection<ExampleItem> ExampleItems { get { return this.exampleItems; } }

            public ExampleItemViewModel()
            {
                for (int i = 1; i < 150000; i++)
                {
                    this.exampleItems.Add(new ExampleItem(){
                        Title = "Title: " + i.ToString(),
                        Subtitle = "Sub: " + i.ToString(),
                        Description = "Desc: " + i.ToString()
                    });
                }
            }
        }
    }
    ```
2.  Esta é a marcação contida em `DeferMainPage.xaml`. A visualização em grade contém um modelo de item com elementos vinculados às propriedades **Title**, **Subtitle** e **Description** da classe **MyItem**. O padrão de **x:Phase** é 0. Aqui, os itens serão renderizados inicialmente com apenas o título visível. Em seguida, o elemento de subtítulo será vinculados a dados e visível para todos os itens e assim por diante, até que todas as fases sejam processadas.
    ```xml
    <Page
        x:Class="LotsOfItems.DeferMainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Text="{x:Bind Subtitle}" x:Phase="1"/>
                            <TextBlock Text="{x:Bind Description}" x:Phase="2"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```

3.  Se você executar o aplicativo agora e aplicar panorâmica/rolar rápido o suficiente pela visualização em grade, notará que cada vez que um novo item aparece na tela, ele é renderizado primeiro como um retângulo cinza escuro (graças à propriedade [**ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) no padrão **verdadeiro**), depois aparece o título, seguido pelo subtítulo, seguido pela descrição.

**Atualizações progressivas de modelo de dados usando ContainerContentChanging**

A estratégia geral para o evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) é usar **Opacidade** para ocultar elementos que não precisam ser visíveis imediatamente. Quando os elementos são reciclados, eles mantêm seus valores antigos, então queremos ocultá-los até termos atualizado os valores do novo item de dados. Use a propriedade **Phase** nos argumentos de evento para determinar quais elementos atualizar e exibir. Se forem necessárias fases adicionais, registramos um retorno de chamada.

1.  Usaremos a origem da associação para **x:Phase**.

2.  Aqui está a marcação que `MainPage.xaml` contém. A visualização em grade declara um manipulador para seu evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging) e um modelo de item com elementos usados para exibir as propriedades **Title**, **Subtitle** e **Description** da classe **MyItem**. Para obter os benefícios máximos de desempenho ao usar **ContainerContentChanging**, não usamos associações na marcação, ao invés disso, atribuímos valores programaticamente. A exceção aqui é o elemento que exibe o título, o qual consideramos estar na fase 0.
    ```xml
    <Page
        x:Class="LotsOfItems.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:lotsOfItems="using:LotsOfItems"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <GridView ItemsSource="{x:Bind ViewModel.ExampleItems}" ContainerContentChanging="GridView-ContainerContentChanging">
                <GridView.ItemTemplate>
                    <DataTemplate x:DataType="lotsOfItems:ExampleItem">
                        <StackPanel Height="100" Width="100" Background="OrangeRed">
                            <TextBlock Text="{x:Bind Title}"/>
                            <TextBlock Opacity="0"/>
                            <TextBlock Opacity="0"/>
                        </StackPanel>
                    </DataTemplate>
                </GridView.ItemTemplate>
            </GridView>
        </Grid>
    </Page>
    ```
3.  Finalmente, aqui está a implementação do manipulador de evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.containercontentchanging). Este código também mostra como adicionamos uma propriedade do tipo **RecordingViewModel** para **MainPage**, a fim de exporta a classe de associação de origem da classe que representa nossa página de marcação. Desde que você não tenha associações [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) em seu modelo de dados, marque o objeto de argumentos de evento como manipulado na primeira fase do manipulador para dar a dica para o item que ele não precisa definir um contexto de dados.
    ```csharp
    namespace LotsOfItems
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                this.ViewModel = new ExampleItemViewModel();
            }

            public ExampleItemViewModel ViewModel { get; set; }

            // Display each item incrementally to improve performance.
            private void GridView-ContainerContentChanging(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 0)
                {
                    throw new System.Exception("We should be in phase 0, but we are not.");
                }

                // It's phase 0, so this item's title will already be bound and displayed.

                args.RegisterUpdateCallback(this.ShowSubtitle);

                args.Handled = true;
            }

            private void ShowSubtitle(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 1)
                {
                    throw new System.Exception("We should be in phase 1, but we are not.");
                }

                // It's phase 1, so show this item's subtitle.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[1] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Subtitle;
                textBlock.Opacity = 1;

                args.RegisterUpdateCallback(this.ShowDescription);
            }

            private void ShowDescription(ListViewBase sender, ContainerContentChangingEventArgs args)
            {
                if (args.Phase != 2)
                {
                    throw new System.Exception("We should be in phase 2, but we are not.");
                }

                // It's phase 2, so show this item's description.
                var templateRoot = args.ItemContainer.ContentTemplateRoot as StackPanel;
                var textBlock = templateRoot.Children[2] as TextBlock;
                textBlock.Text = (args.Item as ExampleItem).Description;
                textBlock.Opacity = 1;
            }
        }
    }
    ```

4.  Se você executar o aplicativo agora e aplicar panorâmica/rolar rapidamente pela visualização em grade, verá o mesmo comportamento que para **x:Phase**.

## Reciclagem de contêiner com coleções heterogêneas

Em alguns aplicativos, você precisa ter diferentes interfaces do usuário para diferentes tipos de item dentro de uma coleção. Isso pode gerar uma situação em que fica impossível para os painéis de virtualização reutilizarem/reciclarem os elementos visuais usados para exibir os itens. Recriar os elementos visuais de um item durante o movimento panorâmico elimina muitos dos ganhos de desempenho obtidos com a virtualização. No entanto, um pouco de planejamento pode permitir que os painéis de virtualização reutilizem os elementos. Os desenvolvedores têm duas opções, dependendo da situação: o evento [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) ou um seletor de modelo de item. A abordagem **ChoosingItemContainer** tem um desempenho melhor.

**O evento ChoosingItemContainer**

[
              **ChoosingItemContainer**
            ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer)  é um evento que permite que você forneça um item (**ListViewItem**/**GridViewItem**) to the [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)/[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) sempre que um novo item é necessário durante a inicialização ou a reciclagem. Você pode criar um contêiner com base no tipo de item de dados que o contêiner exibirá (mostrado no exemplo a seguir). **ChoosingItemContainer** é a melhor maneira de usar diferentes modelos de dados para itens diversificados. Contêiner em cache é algo que pode ser alcançado usando **ChoosingItemContainer**. Por exemplo, se você tiver cinco modelos diferentes, com um modelo ocorrendo uma ordem de magnitude com mais frequência do que os outros, então ChoosingItemContainer permite que você não apenas crie itens nas taxas necessárias, mas também mantenha uma quantidade adequada de elementos em cache e disponíveis para reciclagem. [
              **ChoosingGroupHeaderContainer**
            ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosinggroupheadercontainer) oferece as mesmas funcionalidades para cabeçalhos de grupo.

```csharp
// Example shows how to use ChoosingItemContainer to return the correct
// DataTemplate when one is available. This example shows how to return different 
// data templates based on the type of FileItem. Available ListViewItems are kept
// in two separate lists based on the type of DataTemplate needed.
private void lst-ChoosingItemContainer
    (ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    // Determines type of FileItem from the item passed in.
    bool special = args.Item is DifferentFileItem;

    // Uses the Tag property to keep track of whether a particular ListViewItem's 
    // datatemplate should be a simple or a special one.
    string tag = special ? "specialFiles" : "simpleFiles";

    // Based on the type of datatemplate needed return the correct list of 
    // ListViewItems, this could have also been handled with a hash table. These 
    // two lists are being used to keep track of ItemContainers that can be reused.
    List<UIElement> relevantStorage = special ? specialFileItemTrees : simpleFileItemTrees;

    // args.ItemContainer is used to indicate whether the ListView is proposing an 
    // ItemContainer (ListViewItem) to use. If args.Itemcontainer, then there was a 
    // recycled ItemContainer available to be reused.
    if (args.ItemContainer != null)
    {
        // The Tag is being used to determine whether this is a special file or 
        // a simple file.
        if (args.ItemContainer.Tag.Equals(tag))
        {
            // Great: the system suggested a container that is actually going to 
            // work well.
        }
        else
        {
            // the ItemContainer's datatemplate does not match the needed 
            // datatemplate.
            args.ItemContainer = null;
        }
    }

    if (args.ItemContainer == null)
    {
        // see if we can fetch from the correct list.
        if (relevantStorage.Count > 0)
        {
            args.ItemContainer = relevantStorage[0] as SelectorItem;
        }
        else
        {
            // there aren't any (recycled) ItemContainers available. So a new one 
            // needs to be created.
            ListViewItem item = new ListViewItem();
            item.ContentTemplate = this.Resources[tag] as DataTemplate;
            item.Tag = tag;
            args.ItemContainer = item;
        }
    }
}
```

**Seletor de modelo de item**

Um seletor de modelo de item ([**DataTemplateSelector**](https://msdn.microsoft.com/library/windows/apps/BR209469)) permite que um aplicativo retorne um modelo de item diferente em tempo de execução com no tipo do item de dados que será exibido. Isso torna o desenvolvimento mais produtivo, mas faz a virtualização da interface do usuário mais difícil, porque nem todo modelo de item pode ser reutilizado para todo item de dados.

Quando um item é reciclado (**ListViewItem**/**GridViewItem**), a estrutura deve decidir se os itens que estão disponíveis para uso na fila de reciclagem (a fila de reciclagem é um cache de itens que não estão sendo usados no momento para exibir dados) têm um modelo de item que corresponderá ao desejado pelo item de dados atual. Se não houver itens na fila de reciclagem com o modelo de item apropriado, um novo item é criado, e o modelo de item apropriado é instanciado para ele. Se, por outro lado, a fila de reciclagem contiver um item com o modelo de item apropriado, esse item será removido da fila de reciclagem e será usado para o item de dados atual. Um seletor de modelo de item funciona em situações em que somente um pequeno número de modelos de item é usado e há uma distribuição uniforme em toda a coleção de itens que usam modelos de item diferentes.

Quando há uma distribuição desigual de itens que usam modelos de item diferentes, novos modelos de item provavelmente precisarão ser criados durante o movimento panorâmico e isso elimina muitos dos ganhos obtidos com a virtualização. Além disso, um seletor de modelo de item considera apenas cinco possíveis candidatos ao avaliar se um determinado contêiner pode ser reutilizado para o item de dados atual. Então, você deve considerar criteriosamente se seus dados são apropriados para uso com um seletor de modelo de item antes de usar um em seu aplicativo. Se a maior parte de coleção for homogênea, o seletor está retornando o mesmo tipo na maioria (possivelmente todas) das vezes. Apenas tenha cuidado com o preço que está pagando pelas raras exceções a essa homogeneidade e pondere se usar [**ChoosingItemContainer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.choosingitemcontainer) (ou dois controles de item) não é preferível.

 



<!--HONumber=May16_HO2-->


