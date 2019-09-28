---
Description: Uma caixa de entrada de texto que fornece sugestões à medida que o usuário digita.
title: Caixa de combinação (lista suspensa)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 8f053be1aeb88454b94d7c04ba2627818ea43736
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339408"
---
# <a name="combo-box"></a>Caixa de combinação

Use uma caixa de combinação (também conhecida como lista suspensa) para apresentar uma lista de itens que o usuário pode selecionar. Uma caixa de combinação começa em um estado compacto e se expande para mostrar uma lista de itens selecionáveis.

Quando a caixa de combinação é fechada, ela exibe a seleção atual ou fica vazia se não há nenhum item selecionado. Quando o usuário expande a caixa de combinação, ela exibe a lista de itens selecionáveis.

> **APIs importantes**: [classe ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [propriedade IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propriedade Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [evento TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Uma caixa de combinação no estado compacto com um cabeçalho.

![Exemplo de uma lista suspensa no estado compacto](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

- Use um controle de lista suspensa para permitir aos usuários selecionar um ou mais valores de um conjunto de itens que podem ser representados adequadamente com linhas de texto únicas.
- Use um modo de exibição de lista ou de grade em vez de uma caixa de combinação para exibir itens que contenham várias linhas de texto ou imagens.
- Quando houver menos de cinco itens, considere a possibilidade de usar [botões de opção](radio-button.md) (se somente um item puder ser selecionado) [ou caixas de seleção](checkbox.md) (se vários itens puderem ser selecionados).
- Use a caixa de combinação quando os itens de seleção forem de importância secundária no fluxo do seu aplicativo. Se a opção padrão for recomendada para a maioria dos usuários em grande parte das situações, mostrar todos os itens usando uma exibição de lista pode chamar mais atenção para as opções do que o necessário. Você pode economizar espaço e minimizar a distração usando uma caixa de combinação.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ComboBox">abrir o aplicativo e conferir a ComboBox em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Uma caixa de combinação no estado compacto pode mostrar um cabeçalho.

![Exemplo de uma lista suspensa no estado compacto](images/combo_box_collapsed.png)

Embora caixas de combinação se expandam para dar suporte a tamanhos maiores de cadeia de caracteres, evite cadeias de caracteres excessivamente longas que são difíceis de ler.

![Exemplo de uma lista suspensa com a cadeia de caracteres de texto longo](images/combo_box_listitemstate.png)

Se a coleção em uma caixa de combinação for grande o suficiente, será exibida uma barra de rolagem para acomodá-la. Agrupe itens logicamente na lista.

![Exemplo de uma barra de rolagem em uma lista suspensa](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>Criar uma caixa de combinação

Popule a caixa de combinação adicionando objetos diretamente à coleção [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou associando a propriedade [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) a uma fonte de dados. Itens adicionados a ComboBox são encapsulados em contêineres [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem).

Esta é uma caixa de combinação simples com itens adicionados em XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

O exemplo a seguir demonstra a associação de uma caixa de combinação a uma coleção de objetos FontFamily.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>Seleção de item

Assim como ListView e GridView, ComboBox deriva de [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector), portanto, você pode obter a seleção do usuário da mesma maneira padrão.

Você pode obter ou definir o item selecionado de uma caixa de combinação usando a propriedade [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) e pode obter ou definir o índice do item selecionado usando a propriedade [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex).

Para obter o valor de uma propriedade específica no item de dados selecionado, você pode usar a propriedade [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue). Nesse caso, defina o [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) para especificar de qual propriedade do item selecionado obter o valor.

> [!TIP]
> Se você definir SelectedItem ou SelectedIndex para indicar a seleção padrão, ocorrerá uma exceção se a propriedade for definida antes que a coleção de Itens da caixa de combinação seja populada. A menos que você defina seus Itens em XAML, é melhor manipular o evento Loaded da caixa de combinação e definir SelectedItem ou SelectedIndex no manipulador de eventos Loaded.

Você pode associar a essas propriedades em XAML ou manipular o evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) para responder a alterações de seleção.

No código do manipulador de eventos, você pode o item selecionado na propriedade [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Você poderá obter o item selecionado anteriormente (se houver) na propriedade [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). As coleções AddedItems e RemovedItems contêm, cada uma, apenas um item porque a caixa de combinação não dá suporte à seleção múltipla.

Este exemplo mostra como manipular o evento SelectionChanged e como associar ao item selecionado.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged e navegação com teclado

Por padrão, o evento SelectionChanged ocorre quando um usuário clica, toca ou pressiona Enter em um item na lista para confirmar sua seleção e a caixa de combinação é fechada. A seleção não muda quando o usuário navega na lista da caixa de combinação aberta com as teclas de direção do teclado.

Para fazer uma caixa de combinação com “atualizações ao vivo” enquanto o usuário está navegando na lista aberta usando as teclas de seta (como um menu suspenso de seleção de fonte), defina [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) como [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Isso faz com que o evento SelectionChanged ocorra quando o foco mudar para outro item na lista aberta.

#### <a name="selected-item-behavior-change"></a>Alteração de comportamento do item selecionado

No Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, o comportamento dos itens selecionados é atualizado para dar suporte a caixas de combinação editáveis.

Antes do SDK 17763, o valor da propriedade SelectedItem (e, portanto, SelectedValue e SelectedIndex) precisava estar na coleção de Itens da caixa de combinação. Usando o exemplo anterior, definir `colorComboBox.SelectedItem = "Pink"` resulta em:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

No SDK 17763 e posteriores, o valor da propriedade SelectedItem (e, portanto, SelectedValue e SelectedIndex) não precisa estar na coleção de Itens da caixa de combinação. Usando o exemplo anterior, definir `colorComboBox.SelectedItem = "Pink"` resulta em:

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>Pesquisa de texto

As caixas de combinação suportam automaticamente pesquisas dentro de suas coleções. Como os usuários digitam caracteres em um teclado físico enquanto enfocam uma caixa de combinação aberta ou fechada, os candidatos que correspondem à cadeia do usuário são inseridos na exibição. Essa funcionalidade é especialmente útil quando estiver navegando uma longa lista. Por exemplo, quando interage com uma lista suspensa contendo uma lista de estados, os usuários podem pressionar a tecla "w" para trazer "Washington" até a exibição para que haja uma seleção rápida. A pesquisa de texto não diferencia maiúsculas de minúsculas.

Você pode definir a propriedade [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) como **false** para desabilitar essa funcionalidade.

## <a name="make-a-combo-box-editable"></a>Tornar uma caixa de combinação editável

> [!IMPORTANT]
> Este recurso requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior.

Por padrão, uma caixa de combinação permite que o usuário selecione em uma lista predefinida de opções. No entanto, há casos em que a lista contém apenas um subconjunto de valores válidos e o usuário deve ser capaz de inserir outros valores que não estão listados. Para dar suporte a isso, você pode tornar a caixa de combinação editável.

Para tornar uma caixa de combinação editável, defina a propriedade [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) como **true**. Em seguida, manipule o evento [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para trabalhar com o valor inserido pelo usuário.

Por padrão, o valor de SelectedItem é atualizado quando o usuário confirma texto personalizado. Você pode substituir esse comportamento definindo **Handled** como **true** nos argumentos do evento TextSubmitted. Quando o evento for marcado como manipulado, a caixa de combinação não executará mais nenhuma ação após o evento e permanecerá no estado de edição. SelectedItem não será atualizado.

Este exemplo mostra uma caixa de combinação editável simples. A lista contém cadeias de caracteres simples e qualquer valor inserido pelo usuário é usado como digitado.

Um seletor de "nomes usados recentemente" permite que o usuário insira cadeias de caracteres personalizadas. A lista 'RecentlyUsedNames' contém alguns valores que o usuário pode escolher, mas também é possível adicionar um novo valor personalizado. A propriedade 'CurrentName' representa o nome digitado no momento.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texto enviado

Você pode manipular o evento [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para trabalhar com o valor inserido pelo usuário. No caso de manipulador de eventos, normalmente você validará que o valor inserido pelo usuário é válido e, em seguida, usará o valor em seu aplicativo. Dependendo da situação, você também pode adicionar o valor à lista de opções da caixa de combinação para uso futuro.

O evento TextSubmitted ocorre quando essas condições são atendidas:

- A propriedade IsEditable é **true**
- O usuário insere texto que não corresponde a uma entrada existente na lista da caixa de combinação
- O usuário pressiona Enter ou tira o foco da caixa de combinação.

O evento TextSubmitted não ocorrerá se o usuário inserir o texto e, em seguida, navegar para cima ou para baixo na lista.

### <a name="sample---validate-input-and-use-locally"></a>Exemplo – Validar a entrada e usar localmente

Neste exemplo, um seletor de tamanho da fonte contém um conjunto de valores correspondentes ao crescimento do tamanho da fonte, mas o usuário pode inserir tamanhos de fonte que não estão na lista.

Quando o usuário adiciona um valor que não está na lista, o tamanho da fonte é atualizado, mas o valor não é adicionado à lista de tamanhos de fonte.

Se o valor inserido recentemente não for válido, use SelectedValue para reverter a propriedade Text para o último valor válido conhecido.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Exemplo – Validar a entrada e adicionar à lista

Aqui, um "seletor de cor favorita" contém as cores favoritas mais comuns (vermelho, azul, verde e laranja), mas o usuário pode inserir uma cor favorita que não está na lista. Quando o usuário adiciona uma cor válida (como rosa), a cor recém-inserida é adicionada à lista e definida como "cor favorita" ativa.

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Limite o conteúdo de texto dos itens da caixa de combinação a uma única linha.
- Classifique os itens em uma caixa de combinação na ordem mais lógica. Agrupe opções relacionadas e coloque as opções mais comuns na parte superior. Classifique os nomes em ordem alfabética, os números em ordem numérica e as datas em ordem cronológica.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.
- [Exemplo de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artigos relacionados

- [Controles de texto](text-controls.md)
- [Verificação ortográfica](text-controls.md)
- [Pesquisa](search.md)
- [Classe TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Classe Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propriedade String.Length](https://docs.microsoft.com/dotnet/api/system.string.length)
