---
author: Jwmsft
Description: Use the pull-to-refresh control to get new content into a list.
title: Puxar para atualizar
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8b1dd6bd1bc165a79ba123c94e63e1dcfa58ec21
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100843"
---
# <a name="pull-to-refresh"></a>Puxar para atualizar

A ação de Puxar para atualizar permite a um usuário extrair uma lista de dados com toque para recuperar mais dados. Puxar para atualizar é amplamente usado em dispositivos com tela touch. Você pode a API exibida aqui implementar o padrão puxar para atualizar em seu aplicativo.

> **APIs importantes**: [RefreshContainer](/uwp/api/windows.ui.xaml.controls.refreshcontainer), [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer)

![gif puxar para atualizar](images/Pull-To-Refresh.gif)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use o padrão puxar para atualizar quando você tiver uma lista ou uma grade de dados que o usuário talvez queira atualizar regularmente, e é provável que seu aplicativo seja executado em dispositivos móveis touch-first.

Você também pode usar o [RefreshVisualizer](/uwp/api/windows.ui.xaml.controls.refreshvisualizer) para criar uma experiência consistente de atualização que é chamada de outras maneiras, como por um botão Atualizar.

## <a name="refresh-controls"></a>Controles de atualização

Atualização de recepção é habilitada por 2 controles.

- **RefreshContainer** -um ContentControl que fornece um wrapper para a experiência de atualização de recepção. Ele manipula as interações de toque e gerencia o estado do seu visualizador interno de atualização.
- **RefreshVisualizer** -encapsula a visualização de atualização explicada na próxima seção.

O controle principal é o **RefreshContainer**, que você insere como um wrapper em torno do conteúdo que o usuário agrupa para disparar uma atualização. RefreshContainer só funciona com toque, portanto, recomendamos que você também tem um botão de atualização disponível para os usuários que não têm uma interface de toque. Você pode posicionar o botão Atualizar em um local adequado no aplicativo, em uma barra de comandos ou em um local próximo a superfície sendo atualizado.

## <a name="refresh-visualization"></a>Visualização de atualização

A visualização de atualização padrão é um controle giratório progresso circular que é usado para se comunicar quando uma atualização ocorrerá e o progresso da atualização depois que ele é iniciado. O Visualizador de atualização tem 5 estados.

 A distância que o usuário precisa puxe para baixo em uma lista para iniciar uma atualização é chamada a _limite_. O visualizador [estado](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.State) é determinado pelo estado pull que diz respeito a esse limite. Os valores possíveis são contidos na enumeração [RefreshVisualizerState](/uwp/api/windows.ui.xaml.controls.refreshvisualizerstate).

### <a name="idle"></a>Ocioso

O estado padrão do Visualizador é **Idle**. O usuário não está interagindo com o RefreshContainer através do toque e não há uma atualização em andamento.

Visualmente, não há nenhuma evidência de Visualizador de atualização.

### <a name="interacting"></a>Interação

Quando o usuário agrupa a lista na direção especificada pela propriedade PullDirection e antes do limite é alcançado, o visualizador consta o estado **interagindo**.

- Se o usuário libera o controle nesse estado, o controle retorna ao **Idle**.

    ![Atualização de recepção Pre-limite](images/ptr-prethreshold.png)

    Visualmente, o ícone é exibido como desativado (60% de opacidade). Além disso, o ícone gira uma rotação completa com a ação de rolagem.

- Se o usuário agrupa a lista ultrapassou o limite, o Visualizador de transição da **interagindo** para **pendente**.

    ![puxar para atualizar no limite](images/ptr-atthreshold.png)

    Visualmente, o ícone alterna para 100% de opacidade e pulsos no tamanho até 150% e depois retornar para 100% do tamanho durante a transição.

### <a name="pending"></a>Pendente

Quando o usuário ter recebido a lista ultrapassou o limite, o visualizador consta o estado **pendente**.

- Se o usuário move a lista volta acima do limite sem liberá-lo, ele retorna para o estado **interagindo**.
- Se o usuário libera a lista, uma solicitação de atualização será iniciada e faz a transição para o estado **Refreshing**.

![puxar para atualizar após o limite](images/ptr-postthreshold.png)

Visualmente, o ícone é 100% em tamanho e opacidade. Nesse estado, o ícone continua a se mover para baixo com a ação de rolagem, mas não gira.

### <a name="refreshing"></a>Atualizando

Quando o usuário libera o visualiser ultrapassou o limite, cabe ao estado **Refreshing**.

Quando esse estado é inserido, o evento **RefreshRequested** é acionado. Este é o sinal para começar a atualização de conteúdo do aplicativo. Argumentos de evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contêm um objeto [adiamento](/uwp/api/windows.foundation.deferral), o que você deve executar um identificador para o manipulador de eventos. Em seguida, você deve marcar o adiamento como concluído quando seu código para realizar a atualização foi concluída.

Quando a atualização for concluída, o visualizador retorna para o estado **Idle**.

Visualmente, o ícone paga para o local do limite e gira para a duração da atualização. Este giratório é usado para mostrar o progresso da atualização e é substituído pela animação do conteúdo de entrada.

### <a name="peeking"></a>Inspecionar

Quando o usuário recebe a direção de atualização de uma posição inicial em que uma atualização não é permitida, o visualizador insere o estado **inspeção**. Isso normalmente acontece quando o ScrollViewer não está na posição 0 quando o usuário começa a puxar.

- Se o usuário libera o controle nesse estado, o controle retorna ao **Idle**.

## <a name="pull-direction"></a>Trajeto para puxar

Por padrão, o usuário recebe uma lista de cima para baixo para iniciar uma atualização. Se você tiver uma lista ou grade com uma orientação diferente, você deve alterar a direção de pull do contêiner de atualização para corresponder.

O [PullDirection](/uwp/api/windows.ui.xaml.controls.refreshcontainer.PullDirection) propriedade usa um destes valores [RefreshPullDirection](/uwp/api/windows.ui.xaml.controls.refreshpulldirection): **BottomToTop**, **TopToBottom**, **RightToLeft**, ou **LeftToRight**.

Quando você altera o dircetion pull, a posição inicial do controle giratório de progresso do visualizador gira automaticamente para a seta é iniciado na posição apropriada para a direção de recepção. Se necessário, você pode alterar a propriedade [RefreshVisualizer.Orientation](/uwp/api/windows.ui.xaml.controls.refreshvisualizer.Orientation) para substituir o comportamento automático. Na maioria dos casos, recomendamos deixar o valor padrão de **automática**.

## <a name="implement-pull-to-refresh"></a>Implementar o padrão puxar para atualizar

Para adicionar a funcionalidade de recepção para atualizar para uma lista requer apenas algumas etapas.

1. Empacotar sua lista em um controle **RefreshContainer**.
1. Manipular o evento **RefreshRequested** para atualizar seu conteúdo.
1. Opcionalmente, iniciar uma atualização chamando **RequestRefresh** (por exemplo, de um botão clique).

> [!NOTE]
> Você pode instanciar um RefreshVisualizer por conta própria. No entanto, recomendamos que você encapsula seu conteúdo em um RefreshContainer e use o RefreshVisualizer fornecido pela propriedade RefreshContainer.Visualizer, até mesmo para cenários não sensíveis ao toque. Neste artigo, presumimos que o visualizador sempre é obtido do contêiner de atualização.

> Além disso, use RequestRefresh e RefreshRequested membros de atualização do contêiner por conveniência. `refreshContainer.RequestRefresh()` é equivalente a `refreshContainer.Visualizer.RequestRefresh()`, e nenhuma irá disparar eventos RefreshContainer.RefreshRequested e os eventos RefreshVisualizer.RefreshRequested.

### <a name="request-a-refresh"></a>Solicitar uma atualização

O contêiner de atualização manipula interações de toque para permitir que o usuário atualizar conteúdo através do toque. Recomendamos que você fornecer outras funcionalidades para interfaces não sensíveis ao toque, como um controle de voz ou botão de atualização.

Para iniciar uma atualização, chame o método [RequestRefresh](/uwp/api/windows.ui.xaml.controls.refreshcontainer.RequestRefresh).

```csharp
// See the Examples section for the full code.
private void RefreshButtonClick(object sender, RoutedEventArgs e)
{
    RefreshContainer.RequestRefresh();
}
```

Quando você chamar RequestRefresh, o estado do visualizador vai diretamente da **Idle** para **Refreshing**.

### <a name="handle-a-refresh-request"></a>Tratar uma solicitação de atualização

Para obter conteúdo atualizado quando necessário, manipule o evento RefreshRequested. No caso de manipulador, você precisará código específico ao seu aplicativo para obter o conteúdo atualizado.

Argumentos de evento ([RefreshRequestedEventArgs](/uwp/api/windows.ui.xaml.controls.refreshrequestedeventargs)) contêm um objeto [adiamento](/uwp/api/windows.foundation.deferral). Obtenha um identificador para o adiamento no manipulador de eventos. Em seguida, você deve marcar o adiamento como concluído quando seu código para realizar a atualização foi concluída.

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

Você pode responder a alterações no estado do visualizador, se necessário. Por exemplo, para evitar várias solicitações de atualização, você pode desabilitar um botão Atualizar enquanto o visualizador estão sendo atualizado.

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

### <a name="using-a-scrollviewer-in-a-refreshcontainer"></a>Usando um ScrollViewer em um RefreshContainer

Este exemplo mostra como usar a atualização de recepção com um visualizador de rolagem.

```xaml
<RefreshContainer>
    <ScrollViewer VerticalScrollMode="Enabled"
                  VerticalScrollBarVisibility="Auto"
                  HorizontalScrollBarVisibility="Auto">
 
        <!-- Scrollviewer content -->

    </ScrollViewer>
</RefreshContainer>
```

### <a name="adding-pull-to-refresh-to-a-listview"></a>Adicionando pull para atualizar a um ListView

Este exemplo mostra como usar a atualização de recepção com um visualizador de lista.

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

## <a name="related-articles"></a>Artigos relacionados

- [Interações por toque](../input/touch-interactions.md)
- [Exibição de lista e exibição de grade](listview-and-gridview.md)
- [Contêineres e modelos de itens](item-containers-templates.md)
- [Animações de expressão](../../composition/composition-animation.md)
