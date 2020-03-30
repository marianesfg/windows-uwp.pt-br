---
Description: Use o controle de deslizar para atualizar para obter o novo conteúdo em uma lista.
title: Puxar para atualizar
label: Pull-to-refresh
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: faff0857679d50f6995640bbf9bf0222bb0d2e37
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081608"
---
# <a name="pull-to-refresh"></a>Puxar para atualizar

A ação deslizar para atualizar permite a um usuário extrair uma lista de dados com toque para recuperar mais dados. Deslizar para atualizar é amplamente usado em dispositivos com tela touch. Você pode usar a API mostrada aqui para implementar a ação deslizar para atualizar em seu aplicativo.

> **APIs importantes**: [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![gif deslizar para atualizar](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use a ação deslizar para atualizar quando você tiver uma lista ou uma grade de dados que o usuário talvez queira atualizar regularmente e será provável que seu aplicativo seja executado em dispositivos móveis touch-first.

Você também pode usar o [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) para criar uma experiência consistente de atualização que é invocada de outras maneiras, como por um botão Atualizar.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/PullToRefresh">abri-lo e ver PullToRefresh em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="refresh-controls"></a>Controles de atualização

Deslizar para atualizar é habilitado por dois controles.

- **RefreshContainer** – um ContentControl que fornece um wrapper para a experiência de deslizar para atualizar. Ele manipula as interações de toque e gerencia o estado do seu visualizador interno de atualização.
- **RefreshVisualizer** – encapsula a visualização de atualização explicada na próxima seção.

O controle principal é o **RefreshContainer**, que você insere como um wrapper em torno do conteúdo que o usuário desliza para disparar uma atualização. RefreshContainer só funciona com toque, portanto, recomendamos que você também tenha um botão de atualização disponível para os usuários que não têm uma interface de toque. Você pode posicionar o botão Atualizar em uma localização adequada no aplicativo, em uma barra de comandos ou em uma localização próxima à superfície sendo atualizada.

## <a name="refresh-visualization"></a>Visualização de atualização

A visualização de atualização padrão é um controle giratório de progresso circular que é usado para comunicação para quando uma atualização ocorrer e o progresso da atualização depois que ele for iniciado. O visualizador de atualização tem cinco estados.

 A distância que o usuário precisa deslizar para baixo em uma lista para iniciar uma atualização é chamada de _limite_. O [Estado](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) do visualizador é determinado pelo estado da ação de deslizar que diz respeito a esse limite. Os valores possíveis estão contidos na enumeração [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate).

### <a name="idle"></a>Idle

O estado padrão do visualizador é **Ocioso**. O usuário não está interagindo com o RefreshContainer por toque e não há uma atualização em andamento.

Visualmente, não há nenhuma evidência do visualizador de atualização.

### <a name="interacting"></a>Interação

Quando o usuário deslizar a lista na direção especificada pela propriedade PullDirection, antes de o limite ser alcançado, o visualizador estará no estado **Interação**.

- Se o usuário liberar o controle nesse estado, o controle retornará para **Ocioso**.

    ![deslizar para atualizar antes do limite](images/ptr-prethreshold.png)

    Visualmente, o ícone é exibido como desabilitado (60% de opacidade). Além disso, o ícone gira uma rotação completa com a ação de rolagem.

- Se o usuário desliza a lista para depois do limite, o visualizador faz a transição de **Interação** para **Pendente**.

    ![deslizar para atualizar no limite](images/ptr-atthreshold.png)

    Visualmente, o ícone alterna para 100% de opacidade e pulsos no tamanho de até 150% e, depois, retorna para 100% do tamanho durante a transição.

### <a name="pending"></a>Pending (Pendente)

Quando o usuário desliza a lista para além do limite, o visualizador fica no estado **Pendente**.

- Se o usuário move a lista de volta acima do limite sem liberá-lo, ele retorna para o estado **Interação**.
- Se o usuário libera a lista, uma solicitação de atualização é iniciada e faz a transição para o estado **Em atualização**.

![deslizar para atualizar após o limite](images/ptr-postthreshold.png)

Visualmente, o ícone é 100% em tamanho e opacidade. Nesse estado, o ícone continua a se mover para baixo com a ação de rolagem, mas não gira.

### <a name="refreshing"></a>Atualizando

Quando o usuário libera o visualizador além do limite, ele fica no estado **Em atualização**.

Quando esse estado é inserido, o evento **RefreshRequested** é acionado. Este é o sinal para começar a atualização de conteúdo do aplicativo. Argumentos de evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contêm um objeto [Deferral](/uwp/api/windows.foundation.deferral), que você deve executar um identificador para o manipulador de eventos. Em seguida, você deverá marcar o adiamento como concluído quando seu código para realizar a atualização tiver sido concluído.

Quando a atualização for concluída, o visualizador retornará para o estado **Ocioso**.

Visualmente, o ícone volta para a localização do limite e gira para a duração da atualização. Este giro é usado para mostrar o progresso da atualização e é substituído pela animação do conteúdo de entrada.

### <a name="peeking"></a>Espiar

Quando o usuário desliza na direção de atualização de uma posição inicial em que uma atualização não é permitida, o visualizador entra no estado **Espiar**. Isso normalmente acontece quando o ScrollViewer não está na posição 0 quando o usuário começa a deslizar.

- Se o usuário liberar o controle nesse estado, o controle retornará para **Ocioso**.

## <a name="pull-direction"></a>Direção de deslizamento

Por padrão, o usuário desliza uma lista de cima para baixo para iniciar uma atualização. Se você tiver uma lista ou grade com uma orientação diferente, deverá alterar a direção de deslizamento do contêiner de atualização para corresponder.

A propriedade [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) usa um desses valores de [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection): **BottomToTop**, **TopToBottom**, **RightToLeft** ou **LeftToRight**.

Quando você altera a direção de deslizamento, a posição inicial do controle giratório de progresso do visualizador gira automaticamente para que a seta seja iniciada na posição apropriada para a direção de deslizamento. Se necessário, você pode alterar a propriedade [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) para substituir o comportamento automático. Na maioria dos casos, recomendamos deixar o valor padrão como **Automático**.

## <a name="implement-pull-to-refresh"></a>Implementar o padrão puxar para atualizar

Adicionar a funcionalidade deslizar para atualizar a uma lista requer apenas algumas etapas.

1. Encapsule sua lista em um controle **RefreshContainer**.
1. Manipule o evento **RefreshRequested** para atualizar seu conteúdo.
1. Opcionalmente, inicie uma atualização chamando **RequestRefresh** (por exemplo, com o clique de um botão).

> [!NOTE]
> Você pode instanciar um RefreshVisualizer por conta própria. No entanto, recomendamos que você encapsule seu conteúdo em um RefreshContainer e use o RefreshVisualizer fornecido pela propriedade RefreshContainer.Visualizer, até mesmo para cenários não sensíveis ao toque. Neste artigo, assumimos que o visualizador é sempre obtido do contêiner de atualização.

> Além disso, usamos os membros RequestRefresh e RefreshRequested do contêiner de atualização por conveniência. `refreshContainer.RequestRefresh()` é equivalente a `refreshContainer.Visualizer.RequestRefresh()` e nenhum acionará o evento RefreshContainer.RefreshRequested e os eventos RefreshVisualizer.RefreshRequested.

### <a name="request-a-refresh"></a>Solicitar uma atualização

O contêiner de atualização manipula interações de toque para permitir que o usuário atualize conteúdo por toque. Recomendamos que você forneça outras funcionalidades para interfaces não sensíveis ao toque, como um controle de voz ou botão de atualização.

Para iniciar uma atualização, chame o método [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh).

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Quando você chama RequestRefresh, o estado do visualizador vai diretamente de **Ocioso** para **Em atualização**.

### <a name="handle-a-refresh-request"></a>Manipular uma solicitação de atualização

Para obter conteúdo atualizado quando necessário, manipule o evento RefreshRequested. No manipulador de eventos, você precisará de código específico ao seu aplicativo para obter o conteúdo atualizado.

Os argumentos de evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contêm um objeto [Deferral](/uwp/api/windows.foundation.deferral). Obtenha um identificador para o adiamento no manipulador de eventos. Em seguida, marque o adiamento como concluído quando seu código para executar a atualização for concluído.

```csharp
// See the Examples section for the full code.
private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
{
    // Respond to a request by performing a refresh and using the deferral object.
    using (var RefreshCompletionDeferral = args.GetDeferral())
    {
        // Do some async operation to refresh the content

         await FetchAndInsertItemsAsync(3);

        // The 'using' statement ensures the deferral is marked as complete.
        // Otherwise, you'd call
        // RefreshCompletionDeferral.Complete();
        // RefreshCompletionDeferral.Dispose();
    }
}
```

### <a name="respond-to-state-changes"></a>Responder a alterações de estado

É possível responder a alterações no estado do visualizador, se necessário. Por exemplo, para evitar várias solicitações de atualização, você poderá desabilitar um botão de atualização enquanto o visualizador estiver sendo atualizado.

```csharp
// See the Examples section for the full code.
private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
{
    // Respond to visualizer state changes.
    // Disable the refresh button if the visualizer is refreshing.
    if (args.NewState == RefreshVisualizerState.Refreshing)
    {
        RefreshButton.IsEnabled = false;
    }
    else
    {
        RefreshButton.IsEnabled = true;
    }
}
```

## <a name="examples"></a>Exemplos

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>Como usar um ScrollViewer em um RefreshContainer
> [!NOTE]
> O conteúdo de um RefreshContainer precisa ser um controle rolável, como ScrollViewer, GridView, ListView etc. A definição do conteúdo como um controle como Grid resultará em um comportamento indefinido.

Este exemplo mostra como usar a ação deslizar para atualizar com um visualizador de rolagem.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Como adicionar a ação deslizar para atualizar a um ListView

Este exemplo mostra como usar a ação deslizar para atualizar com uma exibição de lista.

```xaml
<StackPanel Margin="0,40" Width="280">
    <CommandBar OverflowButtonVisibility="Collapsed">
        <AppBarButton x:Name="RefreshButton" Click="RefreshButtonClick"
                      Icon="Refresh" Label="Refresh"/>
        <CommandBar.Content>
            <TextBlock Text="List of items" 
                       Style="{StaticResource TitleTextBlockStyle}"
                       Margin="12,8"/>
        </CommandBar.Content>
    </CommandBar>

    <RefreshContainer x:Name="RefreshContainer">
        <ListView x:Name="ListView1" Height="400">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <Grid Height="80">
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="*" />
                        </Grid.RowDefinitions>
                        <TextBlock Text="{x:Bind Path=Header}"
                                   Style="{StaticResource SubtitleTextBlockStyle}"
                                   Grid.Row="0"/>
                        <TextBlock Text="{x:Bind Path=Date}"
                                   Style="{StaticResource CaptionTextBlockStyle}"
                                   Grid.Row="1"/>
                        <TextBlock Text="{x:Bind Path=Body}"
                                   Style="{StaticResource BodyTextBlockStyle}"
                                   Grid.Row="2"
                                   Margin="0,4,0,0" />
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RefreshContainer>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<ListItemData> Items { get; set; } 
        = new ObservableCollection<ListItemData>();

    public MainPage()
    {
        this.InitializeComponent();

        Loaded += MainPage_Loaded;
        ListView1.ItemsSource = Items;
    }

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        Loaded -= MainPage_Loaded;
        RefreshContainer.RefreshRequested += RefreshContainer_RefreshRequested;
        RefreshContainer.Visualizer.RefreshStateChanged += Visualizer_RefreshStateChanged;

        // Add some initial content to the list.
        await FetchAndInsertItemsAsync(2);
    }

    private void RefreshButtonClick(object sender, RoutedEventArgs e)
    {
        RefreshContainer.RequestRefresh();
    }

    private async void RefreshContainer_RefreshRequested(RefreshContainer sender, RefreshRequestedEventArgs args)
    {
        // Respond to a request by performing a refresh and using the deferral object.
        using (var RefreshCompletionDeferral = args.GetDeferral())
        {
            // Do some async operation to refresh the content

            await FetchAndInsertItemsAsync(3);

            // The 'using' statement ensures the deferral is marked as complete.
            // Otherwise, you'd call
            // RefreshCompletionDeferral.Complete();
            // RefreshCompletionDeferral.Dispose();
        }
    }

    private void Visualizer_RefreshStateChanged(RefreshVisualizer sender, RefreshStateChangedEventArgs args)
    {
        // Respond to visualizer state changes.
        // Disable the refresh button if the visualizer is refreshing.
        if (args.NewState == RefreshVisualizerState.Refreshing)
        {
            RefreshButton.IsEnabled = false;
        }
        else
        {
            RefreshButton.IsEnabled = true;
        }
    }

    // App specific code to get fresh data.
    private async Task FetchAndInsertItemsAsync(int updateCount)
    {
        for (int i = 0; i < updateCount; ++i)
        {
            // Simulate delay while we go fetch new items.
            await Task.Delay(1000);
            Items.Insert(0, GetNextItem());
        }
    }

    private ListItemData GetNextItem()
    {
        return new ListItemData()
        {
            Header = "Header " + DateTime.Now.Second.ToString(),
            Date = DateTime.Now.ToLongDateString(),
            Body = DateTime.Now.ToLongTimeString()
        };
    }
}

public class ListItemData
{
    public string Header { get; set; }
    public string Date { get; set; }
    public string Body { get; set; }
}
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [Interações por toque](../input/touch-interactions.md)
- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Animações de expressão](../../composition/composition-animation.md)
