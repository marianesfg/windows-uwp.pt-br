---
Description: Filtre os itens em sua coleção por meio de entrada do usuário.
title: Coleções de filtragem
label: Filtering collections
template: detail.hbs
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
ms.openlocfilehash: 24669b81c244339509e30a43a0da8a2b27e67eeb
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302650"
---
# <a name="filtering-collections-and-lists-through-user-input"></a>Como filtrar coleções e listas por meio de entrada do usuário
Se sua coleção exibir muitos itens ou estiver fortemente ligada à interação do usuário, a filtragem será um recurso útil de implementar. A filtragem usando o método descrito neste artigo pode ser implementada na maioria dos controles de coleção, incluindo [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) e [ItemsRepeater](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.itemsrepeater?view=winui-2.2). Muitos tipos de entrada do usuário podem ser usados para filtrar uma coleção, como caixas de seleção, botões de opção e controles deslizantes, mas este artigo se concentrará na criação de entrada de usuário baseada em texto e no seu uso para atualizar uma ListView em tempo real, de acordo com a pesquisa do usuário. 

> [!NOTE]
> Este artigo se concentrará na filtragem com uma ListView. O método de filtragem também pode ser aplicado a outros controles de coleções, como GridView, ItemsRepeater ou TreeView.

## <a name="setting-up-the-ui-and-xaml-for-filtering"></a>Como configurar a interface do usuário e o XAML para filtragem
Para implementar a filtragem, seu aplicativo deve ter uma ListView exibida junto com uma TextBox ou outro controle que permita a entrada do usuário. O texto que o usuário digita na TextBox será usado como filtro, ou seja, somente os resultados que contiverem sua consulta de entrada/pesquisa de texto serão exibidos. À medida que o usuário digita na TextBox, a ListView é constantemente atualizada com resultados filtrados – especificamente, sempre que o texto na TextBox muda, mesmo que em uma única letra, a ListView passará por seus itens e filtrará com esse termo.

O código a seguir mostra uma interface do usuário com uma ListView simples e seu DataTemplate, juntamente com uma TextBox que acompanha. Neste exemplo, a ListView exibe uma coleção de objetos Person. Person é uma classe definida no code-behind (não mostrado no exemplo de código abaixo) e cada objeto Person tem as seguintes propriedades: FirstName, LastName e Company.

Usando a TextBox, os usuários podem digitar um termo de pesquisa/filtragem para filtrar a lista de objetos Person por sobrenome. Observe que a TextBox está associada a um nome específico (`FilterByLName`) e tem seu próprio evento TextChanged (`FilteredLV_LNameChanged`). O nome de ligação nos permite acessar o conteúdo/texto da TextBox no code-behind e o evento TextChanged será acionado sempre que o usuário digitar na caixa de texto, permitindo executar uma operação de filtragem ao receber a entrada do usuário. 

Para a filtragem funcionar, ListView deve ter uma fonte de dados que possa ser manipulada no code-behind, como um `ObservableCollection<>`. Nesse caso, a propriedade ItemsSource de ListView é atribuída a um `ObservableCollection<Person>` no code-behind. 

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="1*"></ColumnDefinition>
        <ColumnDefinition Width="1*"></ColumnDefinition>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
            <RowDefinition Height="400"></RowDefinition>
            <RowDefinition Height="400"></RowDefinition>
    </Grid.RowDefinitions>

    <ListView x:Name="FilteredListView"
                Grid.Column="0"
                Margin="0,0,20,0">

        <ListView.ItemTemplate>
            <DataTemplate x:DataType="local:Person">
                <StackPanel>
                    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" Margin="0,5,0,5">
                        <Run Text="{x:Bind FirstName}"></Run>
                        <Run Text="{x:Bind LastName}"></Run>
                    </TextBlock>
                    <TextBlock Style="{ThemeResource BodyTextBlockStyle}" Margin="0,5,0,5" Text="{x:Bind Company}"/>
                </StackPanel>
            </DataTemplate>
        </ListView.ItemTemplate>

    </ListView>

    <TextBox x:Name="FilterByLName" Grid.Column="1" Header="Last Name" Width="200"
             HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0,0,0,20"
             TextChanged="FilteredLV_LNameChanged"/>
</Grid>
```
## <a name="filtering-the-data"></a>Como filtrar os dados
As consultas [Linq](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) permitem agrupar, ordenar e selecionar determinados itens em uma coleção. Para filtrar uma lista, criaremos uma consulta Linq que seleciona apenas os termos que correspondem ao termo da consulta/filtragem de pesquisa inserido pelo usuário digitado na TextBox `FilterByLName`. O resultado da consulta pode ser atribuído a um objeto da coleção [IEnumerable<T>](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1). Assim que tivermos essa coleção, poderemos usá-la para comparar com a lista original, removendo itens que não correspondem e adicionando itens que correspondem (no caso de um backspace).

> [!NOTE]
> Para que a ListView se anime da maneira mais intuitiva ao adicionar e subtrair itens, é importante remover e adicionar itens à coleção ItemsSource de ListView em si, em vez de criar uma nova coleção de objetos filtrados e atribuí-la à propriedade ItemsSource de ListView.

Para começar, precisaremos inicializar nossa fonte de dados original em uma coleção separada, como uma `List<T>` ou uma `ObservableCollection<T>`. Neste exemplo, temos um `List<Person>` chamado `People` que contém todos os objetos Person mostrados na ListView (o preenchimento/a inicialização dessa Lista não é mostrada no snippet de código abaixo). Também precisaremos de uma lista para manter os dados filtrados, que mudarão constantemente sempre que um filtro for aplicado. Isso será um `ObservableCollection<Person>` chamado `PeopleFiltered` e, na inicialização, terá o mesmo conteúdo que `People`.
 
O código a seguir executa a operação de filtragem por meio das etapas a seguir, mostradas no código abaixo:
 - Defina a propriedade ItemsSource da ListView como `PeopledFiltered`. 
 - Defina o evento TextChanged, `FilteredLV_LNameChanged()`, para a TextBox `FilterByLName`. Dentro dessa função, filtre os dados.
 - Para filtrar os dados, acesse o termo de consulta/filtragem de pesquisa inserido pelo usuário por meio de `FilterByLName.Text`. Use uma consulta LINQ para selecionar os itens em `People` cujo sobrenome contém o termo `FilterByLName.Text` e adicione esses itens correspondentes a uma coleção chamada `TempFiltered`.
 - Compare a coleção de `PeopleFiltered` atual com os itens filtrados recentemente no `TempFiltered`, removendo e adicionando itens de `PeopleFiltered` quando necessário.
 - À medida que os itens são removidos e adicionados de `PeopleFiltered`, a ListView será atualizada e animada de acordo.

 ```csharp
using System.Linq;

public MainPage()
{
    // Define People collection to hold all Person objects. 
    // Populate collection - i.e. add Person objects (not shown)
    IList<Person> People = new List<Person>();

    // Create PeopleFiltered collection and copy data from original People collection
    ObservableCollection<Person> PeopleFiltered = new ObservableCollection<Person>(People);

    // Set the ListView's ItemsSource property to the PeopleFiltered collection
    FilteredListView.ItemsSource = PeopleFiltered;

    // ... 
}

private void FilteredLV_LNameChanged(object sender, TextChangedEventArgs e)
{
    /* Perform a Linq query to find all Person objects (from the original People collection)
    that fit the criteria of the filter, save them in a new List called TempFiltered. */
    List<Person> TempFiltered;
    
    /* Make sure all text is case-insensitive when comparing, and make sure 
    the filtered items are in a List object */
    TempFiltered = people.Where(contact => contact.LastName.Contains(FilterByLName.Text, StringComparison.InvariantCultureIgnoreCase)).ToList();
    
    /* Go through TempFiltered and compare it with the current PeopleFiltered collection,
    adding and subtracting items as necessary: */

    // First, remove any Person objects in PeopleFiltered that are not in TempFiltered
    for (int i = PeopleFiltered.Count - 1; i >= 0; i--)
    {
        var item = PeopleFiltered[i];
        if (!TempFiltered.Contains(item))
        {
            PeopleFiltered.Remove(item);
        }
    }

    /* Next, add back any Person objects that are included in TempFiltered and may 
    not currently be in PeopleFiltered (in case of a backspace) */

    foreach (var item in TempFiltered)
    {
        if (!PeopleFiltered.Contains(item))
        {
            PeopleFiltered.Add(item);
        }
    }
}
 ```

Agora, à medida que o usuário digitar seus termos de filtragem na TextBox `FilterByLName`, a ListView será atualizada imediatamente para mostrar apenas as pessoas cujo sobrenome contém o termo de filtragem.

## <a name="next-steps"></a>Próximas etapas

### <a name="get-the-sample-code"></a>Obter o código de exemplo
- Se você tiver o aplicativo XAML Controles Gallery</strong> instalado, clique [aqui](xamlcontrolsgallery:/item/ListView) para abrir o aplicativo e ver um exemplo mais robusto e detalhado de filtragem de lista na página ListView.
- Obtenha o [aplicativo do XAML Controls Gallery (Microsoft Store)](https://www.microsoft.com/store/productId/9MSVH128X2ZT).

### <a name="related-articles"></a>Artigos relacionados
- [Listas](lists.md)
- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Comando de coleção](collection-commanding.md)