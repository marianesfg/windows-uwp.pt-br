---
title: Comando em aplicativos da UWP (Plataforma Universal do Windows)
description: Como usar as classes XamlUICommand e StandardUICommand (juntamente com a interface ICommand) para compartilhar e gerenciar os comandos entre vários tipos de controles, independentemente do tipo de entrada e do dispositivo que está sendo usado.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 09/13/2019
ms.openlocfilehash: cd34c9444c68b8e959f449124fe4ae294ec14fc4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257716"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>Aplique comandos em aplicativos da UWP (Plataforma Universal do Windows) usando StandardUICommand, XamlUICommand e ICommand

Neste tópico, descreveremos o comando nos aplicativos da UWP (Plataforma Universal do Windows). Especificamente, discutiremos como usar as classes [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) e [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) (juntamente com a interface ICommand) para compartilhar e gerenciar os comandos entre vários tipos de controles, independentemente do tipo de entrada e do dispositivo que está sendo usado.

![Um diagrama que representa um uso comum para um comando compartilhado: várias superfícies de IU com um comando 'favorito'](images/commanding/generic-commanding.png)

*Compartilhe comandos entre vários controles, independentemente do tipo de entrada e do dispositivo*

## <a name="important-apis"></a>APIs Importantes

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) e [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Visão geral

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Os comandos podem ser invocados diretamente por meio de interações de interface do usuário, como clicar em um botão ou selecionar um item de um menu de contexto. Eles também podem ser invocados indiretamente por meio de um dispositivo de entrada, como um acelerador de teclado, um gesto, reconhecimento de fala ou uma ferramenta de automação/acessibilidade. Quando invocado, o comando pode ser processado por um controle (navegação de texto em um controle de edição), uma janela (navegação de retorno) ou pelo aplicativo (sair).

Os comandos podem ser operados em um contexto específico no aplicativo, como ao excluir texto ou desfazer uma ação, ou podem ter contexto livre, como desativar o áudio ou ajustar o brilho.

A imagem a seguir mostra duas interfaces de comando (um [CommandBar](app-bars.md) e um [CommandBarFlyout](command-bar-flyout.md) contextual flutuante) que compartilham alguns dos mesmos comandos.

![Barra de comandos no Fotos do Windows](images/control-examples/command-bar-photos.png)<br>*Barra de comandos no Fotos do Windows*

![Menu de contexto na galeria do Fotos do Windows](images/ContextMenu_example.png)<br>*Menu de contexto na galeria do Fotos do Windows*

## <a name="command-interactions"></a>Interações de comando

Devido à variedade de dispositivos, tipos de entrada e superfícies de IU que podem afetar como um comando é invocado, é recomendável expor seus comandos pelo máximo de superfícies de comandos possível. Eles podem incluir uma combinação de [Passar o dedo](swipe.md), [MenuBar](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md) e o [menu de contexto](menus.md) tradicional.

**Para comandos críticos, use aceleradores de entrada específica.** Aceleradores de entrada permitem que um usuário execute ações mais rapidamente dependendo do dispositivo de entrada usado.

Aqui estão alguns aceleradores de entrada comuns para vários tipos de entrada:

- **Ponteiro** – botões de foco de mouse e caneta
- **Teclado** – atalhos (teclas de acesso e de aceleração)
- **Toque** – passar o dedo
- **Toque** – deslizar para atualizar os dados

Você deve considerar as experiências de usuário e o tipo de entrada para tornar a funcionalidade do aplicativo universalmente acessível. Por exemplo, coleções (especialmente aquelas editáveis pelo usuário) normalmente incluem uma variedade de comandos específicos que são executados de modo bem diferente dependendo do dispositivo de entrada.

A tabela a seguir mostra alguns comandos típicos de coleção e os modos de expor esses comandos.

| Comando          | Independentes do tipo de entrada | Acelerador de mouse | Acelerador de teclado | Acelerador de toque |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Excluir item      | Menu de contexto   | Botão Focalizar      | Tecla DEL              | Deslizar o dedo para excluir   |
| Sinalizar item        | Menu de contexto   | Botão Focalizar      | Ctrl+Shift+G         | Deslizar o dedo para sinalizar     |
| Atualizar dados     | Menu de contexto   | N/D               | Tecla F5               | Puxar para atualizar   |
| Adicionar item a Favoritos | Menu de contexto   | Botão Focalizar      | F, Ctrl+S            | Deslizar o dedo para adicionar a Favoritos |

**Sempre fornecer um menu de contexto** Recomendamos incluir todos os comandos contextuais relevantes em um menu de contexto tradicional ou CommandBarFlyout, pois ambos são compatíveis com todos os tipos de entrada. Por exemplo, se um comando é exposto somente durante um evento de focalização do ponteiro, ele não pode ser usado em um dispositivo somente de toque.

## <a name="commands-in-uwp-applications"></a>Comandos em aplicativos da UWP

Há algumas maneiras de compartilhar e gerenciar experiências de comandos em um aplicativo UWP. É possível definir manipuladores de eventos para interações padrão, como Click, em code-behind (isso pode ser ineficiente dependendo da complexidade de sua interface do usuário). É possível associar o ouvinte de eventos para interações padrão a um manipulador compartilhado ou você pode associar a propriedade Command do controle a uma implementação de ICommand que descreve a lógica de comando.

Para fornecer experiências de usuário abrangentes e avançadas em superfícies de comando com eficiência e duplicação de código mínima, é recomendável usar os recursos de associação de comando descritos neste tópico (para manipulação de eventos padrão, confira os tópicos de eventos individuais).

Para associar um controle a um recurso de comando compartilhado, você mesmo pode implementar as interfaces ICommand ou pode criar seu comando usando uma classe base [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) ou um dos comandos de plataforma definidos pela classe derivada [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand).

- A interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) permite criar comandos reutilizáveis e totalmente personalizados em seu aplicativo.
- O [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) também fornece essa funcionalidade, mas simplifica o desenvolvimento expondo um conjunto de propriedades de comando internas, como comportamento do comando, atalhos de teclado (tecla de acesso e tecla de aceleração), ícone, rótulo e descrição.
- O [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) simplifica ainda mais as coisas, permitindo que você escolha de um conjunto de comandos de plataforma padrão com as propriedades predefinidas.

> [!Important]
> Em aplicativos da UWP, os comandos são implementações da interface [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (C++) ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#), dependendo da estrutura de linguagem escolhida.

## <a name="command-experiences-using-the-standarduicommand-class"></a>Experiências de comando usando a classe StandardUICommand

Derivada de [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (derivada de [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) para C++ ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) para C#), a classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) expõe um conjunto de comandos de plataforma padrão com propriedades predefinidas, como ícone, acelerador de teclado e descrição.

Um [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) fornece uma maneira rápida e consistente de definir comandos comuns, como `Save` ou `Delete`. Tudo o que você precisa fazer é fornecer as funções execute e canExecute.

### <a name="example"></a>Exemplo

![Amostra de StandardUICommand](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

| Baixe o código desse exemplo |
| -------------------- |
| [Amostra de comando da UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip) |

Neste exemplo, vamos mostrar como aprimorar um [ListView](listview-and-gridview.md) básico com um comando de exclusão de item implementado pela classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) enquanto otimizamos a experiência do usuário para vários tipos de entrada usando um [MenuBar](menus.md), o controle [Passar o dedo](swipe.md), os botões de focalizar e o [menu de contexto](menus.md).

> [!NOTE]
> Esta amostra requer o pacote NuGet Microsoft.UI.Xaml.Controls, parte da [Biblioteca de interfaces do usuário do Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

A interface do usuário de amostra inclui um [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinco itens. O [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) Delete está associado a um [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), um [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), um [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) e ao [Menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

``` xaml
<Page
    x:Class="StandardUICommandSample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:StandardUICommandSample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="60"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                StandardUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the StandardUICommand class to 
                share a platform command and consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a standard delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1" Padding="10">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True" 
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered" 
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer" >
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem" 
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left" 
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. Primeiro, definimos uma classe `ListItemData`, que contém uma cadeia de caracteres de texto e o ICommand para cada ListViewItem em nosso ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Na classe MainPage, definimos uma coleção de objetos `ListItemData` para [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) do [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Podemos, em seguida, populá-lo com uma coleção inicial de cinco itens (com o Delete do [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) associado e com texto).

```csharp
/// <summary>
/// ListView item collection.
/// </summary>
ObservableCollection<ListItemData> collection = 
    new ObservableCollection<ListItemData>();

/// <summary>
/// Handler for the layout Grid control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    // Create the standard Delete command.
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(
            new ListItemData {
                Text = "List item " + i.ToString(),
                Command = deleteCommand });
    }
}

/// <summary>
/// Handler for the ListView control load event.
/// </summary>
/// <param name="sender">Source of the control loaded event</param>
/// <param name="e">Event args for the loaded event</param>
private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    // Populate the ListView with the item collection.
    listView.ItemsSource = collection;
}
```

3. Em seguida, definimos o manipulador ExecuteRequested do ICommand no qual implementamos o comando de exclusão de item.

``` csharp
/// <summary>
/// Handler for the Delete command.
/// </summary>
/// <param name="sender">Source of the command event</param>
/// <param name="e">Event args for the command event</param>
private void DeleteCommand_ExecuteRequested(
    XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    // If possible, remove specfied item from collection.
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Por fim, definimos manipuladores para vários eventos do ListView, incluindo os eventos [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) e [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Os manipuladores de eventos de ponteiro são usados para mostrar ou ocultar o botão Excluir para cada item.

```csharp
/// <summary>
/// Handler for the ListView selection changed event.
/// </summary>
/// <param name="sender">Source of the selection changed event</param>
/// <param name="e">Event args for the selection changed event</param>
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

/// <summary>
/// Handler for the pointer entered event.
/// Displays the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer entered event</param>
/// <param name="e">Event args for the pointer entered event</param>
private void ListViewSwipeContainer_PointerEntered(
    object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(
            sender as Control, "HoverButtonsShown", true);
    }
}

/// <summary>
/// Handler for the pointer exited event.
/// Hides the delete item "hover" buttons.
/// </summary>
/// <param name="sender">Source of the pointer exited event</param>
/// <param name="e">Event args for the pointer exited event</param>

private void ListViewSwipeContainer_PointerExited(
    object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(
        sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>Experiências de comando usando a classe XamlUICommand

Se precisar criar um comando não definido pela classe [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) ou se desejar obter mais controle sobre a aparência do comando, a classe [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) será derivada da interface [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand), adicionando várias propriedades de interface do usuário (como ícone, rótulo, descrição e atalhos de teclado), métodos e eventos para definir rapidamente a interface do usuário e o comportamento de um comando personalizado.

O [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) permite especificar a interface do usuário por meio da associação de controle, como ícone, rótulo, descrição e atalhos de teclado (uma tecla de acesso e um acelerador de teclado), sem definir as propriedades individuais.

### <a name="example"></a>Exemplo

![Amostra de XamlUICommand](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

| Baixe o código desse exemplo |
| -------------------- |
| [Amostra de comando da UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip) |

Este exemplo compartilha a funcionalidade Delete do exemplo [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) anterior, mas mostra como a classe [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) permite definir um comando de exclusão personalizado com seu próprio ícone de fonte, rótulo, acelerador de teclado e descrição. Como no exemplo de [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand), aprimoramos um [ListView](listview-and-gridview.md) básico com um comando de exclusão de item implementado pela classe [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) enquanto otimizamos a experiência do usuário para vários tipos de entrada usando um [MenuBar](menus.md), o controle [Passar o dedo](swipe.md), os botões de focalizar e o [menu de contexto](menus.md).

Muitos controles de plataforma usam as propriedades XamlUICommand nos bastidores, assim como nosso exemplo de StandardUICommand na seção anterior. 

> [!NOTE]
> Esta amostra requer o pacote NuGet Microsoft.UI.Xaml.Controls, parte da [Biblioteca de interfaces do usuário do Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

A interface do usuário de amostra inclui um [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinco itens. O Delete do [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) personalizado está associado a um [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), um [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), um [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) e ao [Menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

``` xaml
<Page
    x:Class="XamlUICommand_Sample.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:XamlUICommand_Sample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxcontrols="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <XamlUICommand x:Name="CustomXamlUICommand" 
                       ExecuteRequested="DeleteCommand_ExecuteRequested"
                       Description="Custom XamlUICommand" 
                       Label="Custom XamlUICommand">
            <XamlUICommand.IconSource>
                <FontIconSource FontFamily="Wingdings" Glyph="&#x4D;"/>
            </XamlUICommand.IconSource>
            <XamlUICommand.KeyboardAccelerators>
                <KeyboardAccelerator Key="D" Modifiers="Control"/>
            </XamlUICommand.KeyboardAccelerators>
        </XamlUICommand>

        <Style x:Key="HorizontalSwipe" 
               TargetType="ListViewItem" 
               BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="Height" Value="70"/>
            <Setter Property="Padding" Value="0"/>
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
            <Setter Property="BorderThickness" Value="0"/>
        </Style>
        
    </Page.Resources>

    <Grid Loaded="ControlExample_Loaded" Name="MainGrid">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <StackPanel Grid.Row="0" 
                    Padding="10" 
                    BorderThickness="0,0,0,1" 
                    BorderBrush="LightBlue"
                    Background="AliceBlue">
            <TextBlock Style="{StaticResource HeaderTextBlockStyle}">
                XamlUICommand sample
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,10">
                This sample shows how to use the XamlUICommand class to 
                share a custom command with consistent user experiences 
                across various controls.
            </TextBlock>
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}" Margin="0,0,0,0">
                Specifically, we define a custom delete command and add it 
                to a variety of command surfaces, all of which share a common 
                icon, label, keyboard accelerator, and description.
            </TextBlock>
        </StackPanel>

        <muxcontrols:MenuBar Grid.Row="1">
            <muxcontrols:MenuBarItem Title="File">
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Edit">
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" 
                                Command="{StaticResource CustomXamlUICommand}"/>
            </muxcontrols:MenuBarItem>
            <muxcontrols:MenuBarItem Title="Help">
            </muxcontrols:MenuBarItem>
        </muxcontrols:MenuBar>

        <ListView x:Name="ListViewRight" Grid.Row="2" 
                  Loaded="ListView_Loaded" 
                  IsItemClickEnabled="True"
                  SelectionMode="Single" 
                  SelectionChanged="ListView_SelectionChanged" 
                  ItemContainerStyle="{StaticResource HorizontalSwipe}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:ListItemData">
                    <UserControl PointerEntered="ListViewSwipeContainer_PointerEntered"
                                 PointerExited="ListViewSwipeContainer_PointerExited">
                        <UserControl.ContextFlyout>
                            <MenuFlyout>
                                <MenuFlyoutItem 
                                    Command="{x:Bind Command}" 
                                    CommandParameter="{x:Bind Text}" />
                            </MenuFlyout>
                        </UserControl.ContextFlyout>
                        <Grid AutomationProperties.Name="{x:Bind Text}">
                            <VisualStateManager.VisualStateGroups>
                                <VisualStateGroup x:Name="HoveringStates">
                                    <VisualState x:Name="HoverButtonsHidden" />
                                    <VisualState x:Name="HoverButtonsShown">
                                        <VisualState.Setters>
                                            <Setter Target="HoverButton.Visibility" 
                                                    Value="Visible" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateManager.VisualStateGroups>
                            <SwipeControl x:Name="ListViewSwipeContainer">
                                <SwipeControl.RightItems>
                                    <SwipeItems Mode="Execute">
                                        <SwipeItem x:Name="DeleteSwipeItem"
                                                   Background="Red" 
                                                   Command="{x:Bind Command}" 
                                                   CommandParameter="{x:Bind Text}"/>
                                    </SwipeItems>
                                </SwipeControl.RightItems>
                                <Grid VerticalAlignment="Center">
                                    <TextBlock Text="{x:Bind Text}" 
                                               Margin="10" 
                                               FontSize="18" 
                                               HorizontalAlignment="Left"       
                                               VerticalAlignment="Center"/>
                                    <AppBarButton x:Name="HoverButton" 
                                                  IsTabStop="False" 
                                                  HorizontalAlignment="Right" 
                                                  Visibility="Collapsed" 
                                                  Command="{x:Bind Command}" 
                                                  CommandParameter="{x:Bind Text}"/>
                                </Grid>
                            </SwipeControl>
                        </Grid>
                    </UserControl>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Code-behind**

1. Primeiro, definimos uma classe `ListItemData`, que contém uma cadeia de caracteres de texto e o ICommand para cada ListViewItem em nosso ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Na classe MainPage, definimos uma coleção de objetos `ListItemData` para [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) do [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Podemos, em seguida, populá-lo com uma coleção inicial de cinco itens (com o [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) associado e com texto).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(
           new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Em seguida, definimos o manipulador ExecuteRequested do ICommand no qual implementamos o comando de exclusão de item.

``` csharp
private void DeleteCommand_ExecuteRequested(
   XamlUICommand sender, ExecuteRequestedEventArgs args)
{
    if (args.Parameter != null)
    {
        foreach (var i in collection)
        {
            if (i.Text == (args.Parameter as string))
            {
                collection.Remove(i);
                return;
            }
        }
    }
    if (ListViewRight.SelectedIndex != -1)
    {
        collection.RemoveAt(ListViewRight.SelectedIndex);
    }
}
```

4. Por fim, definimos manipuladores para vários eventos do ListView, incluindo os eventos [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited) e [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged). Os manipuladores de eventos de ponteiro são usados para mostrar ou ocultar o botão Excluir para cada item.

```csharp
private void ListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ListViewRight.SelectedIndex != -1)
    {
        var item = collection[ListViewRight.SelectedIndex];
    }
}

private void ListViewSwipeContainer_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == 
        Windows.Devices.Input.PointerDeviceType.Mouse || 
        e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-icommand-interface"></a>Experiências de comando usando a interface ICommand

Os controles padrão da UWP (botão, lista, seleção, calendário, texto preditivo) fornecem a base para muitas experiências de comando comuns. Para obter uma lista completa dos tipos de controle, confira [Controles e padrões para aplicativos UWP](index.md).

A maneira mais simples de dar suporte a uma experiência de comando estruturada é definir uma implementação da interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) para C++ ou [System.Windows.Input.ICommand](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) para C#).  Esta instância ICommand, em seguida, poderá ser associada a controles como botões.

> [!NOTE]
> Em alguns casos, pode eficiente associar um método ao evento Click e uma propriedade à propriedade IsEnabled.

#### <a name="example"></a>Exemplo

![Exemplo de interface de comando](images/commanding/icommand.gif)

*Exemplo de ICommand*

| Baixe o código desse exemplo |
| -------------------- |
| [Amostra de comando da UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip) |

Neste exemplo básico, demonstraremos como um único comando pode ser invocado com o clique de um botão, com um acelerador de teclado e ao girar o botão de rolagem do mouse.

Usamos dois [ListViews](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), um populado com cinco itens e outro vazio, bem como dois botões, um para mover os itens do ListView à esquerda para o ListView à direita, outro para mover os itens da direita para esquerda. Cada botão é associado a um comando correspondente (ViewModel.MoveRightCommand e ViewModel.MoveLeftCommand, respectivamente) e são habilitados e desabilitados automaticamente com base no número de itens no ListView associado.

**O código XAML a seguir define a interface do usuário para o nosso exemplo.**

```xaml
<Page
    x:Class="UICommand1.View.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:vm="using:UICommand1.ViewModel"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Page.Resources>
        <vm:OpacityConverter x:Key="opaque" />
    </Page.Resources>

    <Grid Name="ItemGrid"
          Background="AliceBlue"
          PointerWheelChanged="Page_PointerWheelChanged">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <ListView Grid.Column="0" VerticalAlignment="Center"
                  x:Name="CommandListView" 
                  ItemsSource="{x:Bind Path=ViewModel.ListItemLeft}" 
                  SelectionMode="None" IsItemClickEnabled="False" 
                  HorizontalAlignment="Right">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
        <Grid Grid.Column="1" Margin="0,0,0,0"
              HorizontalAlignment="Center" 
              VerticalAlignment="Center">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel Grid.Row="1">
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE893;" 
                          Opacity="{x:Bind Path=ViewModel.ListItemLeft.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Add" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Next"/>
                        <TextBlock>Move item right</TextBlock>
                    </StackPanel>
                </Button>
                <Button Name="MoveItemLeftButton" 
                            Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand}">
                    <Button.KeyboardAccelerators>
                        <KeyboardAccelerator 
                            Modifiers="Control" 
                            Key="Subtract" />
                    </Button.KeyboardAccelerators>
                    <StackPanel>
                        <SymbolIcon Symbol="Previous"/>
                        <TextBlock>Move item left</TextBlock>
                    </StackPanel>
                </Button>
                <FontIcon FontFamily="{StaticResource SymbolThemeFontFamily}" 
                          FontSize="40" Glyph="&#xE892;"
                          Opacity="{x:Bind Path=ViewModel.ListItemRight.Count, 
                                        Mode=OneWay, Converter={StaticResource opaque}}"/>
            </StackPanel>
        </Grid>
        <ListView Grid.Column="2" 
                  x:Name="CommandListViewRight" 
                  VerticalAlignment="Center" 
                  IsItemClickEnabled="False" 
                  SelectionMode="None"
                  ItemsSource="{x:Bind Path=ViewModel.ListItemRight}" 
                  HorizontalAlignment="Left">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="vm:ListItemData">
                    <Grid VerticalAlignment="Center">
                        <AppBarButton Label="{x:Bind ListItemText}">
                            <AppBarButton.Icon>
                                <SymbolIcon Symbol="{x:Bind ListItemIcon}"/>
                            </AppBarButton.Icon>
                        </AppBarButton>
                    </Grid>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

**Aqui está o code-behind para a interface do usuário anterior.**

No code-behind, nos conectamos ao nosso modelo de exibição que contém nosso código de comando. Além disso, definimos um manipulador para a entrada do botão de rolagem do mouse, que também conecta nosso código de comando.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Controls;
using UICommand1.ViewModel;
using Windows.System;
using Windows.UI.Core;

namespace UICommand1.View
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Reference to our view model.
        public UICommand1ViewModel ViewModel { get; set; }

        // Initialize our view and view model.
        public MainPage()
        {
            this.InitializeComponent();
            ViewModel = new UICommand1ViewModel();
        }

        /// <summary>
        /// Handle mouse wheel input and assign our
        /// commands to appropriate direction of rotation.
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void Page_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            var props = e.GetCurrentPoint(sender as UIElement).Properties;

            // Require CTRL key and accept only vertical mouse wheel movement 
            // to eliminate accidental wheel input.
            if ((Window.Current.CoreWindow.GetKeyState(VirtualKey.Control) != 
                CoreVirtualKeyStates.None) && !props.IsHorizontalMouseWheel)
            {
                bool delta = props.MouseWheelDelta < 0 ? true : false;

                switch (delta)
                {
                    case true:
                        ViewModel.MoveRight();
                        break;
                    case false:
                        ViewModel.MoveLeft();
                        break;
                    default:
                        break;
                }
            }
        }
    }
}
```

**Aqui está o código de nosso modelo de exibição**

Nosso modelo de exibição é onde definimos os detalhes de execução para os dois comandos em nosso aplicativo, populamos um ListView e fornecemos um conversor de valor de opacidade para ocultar ou exibir algumas interfaces do usuário adicionais com base na contagem de itens de cada ListView.

```csharp
using System;
using System.Collections.ObjectModel;
using System.ComponentModel;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Data;

namespace UICommand1.ViewModel
{
    /// <summary>
    /// UI properties for our list items.
    /// </summary>
    public class ListItemData
    {
        /// <summary>
        /// Gets and sets the list item content string.
        /// </summary>
        public string ListItemText { get; set; }
        /// <summary>
        /// Gets and sets the list item icon.
        /// </summary>
        public Symbol ListItemIcon { get; set; }
    }

    /// <summary>
    /// View Model that sets up a command to handle invoking the move item buttons.
    /// </summary>
    public class UICommand1ViewModel
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand MoveLeftCommand { get; private set; }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand MoveRightCommand { get; private set; }

        // Item collections
        public ObservableCollection<ListItemData> ListItemLeft { get; } = 
           new ObservableCollection<ListItemData>();
        public ObservableCollection<ListItemData> ListItemRight { get; } = 
           new ObservableCollection<ListItemData>();

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            MoveLeftCommand = 
               new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            MoveRightCommand = 
               new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            LoadItems();
        }

        /// <summary>
        ///  Populate our list of items.
        /// </summary>
        public void LoadItems()
        {
            for (var x = 0; x <= 4; x++)
            {
                listItem = new ListItemData();
                listItem.ListItemText = "Item " + (ListItemLeft.Count + 1).ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.Add(listItem);
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return ListItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return ListItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (ListItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                ListItemRight.Add(listItem);
                listItem.ListItemText = "Item " + ListItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemLeft.RemoveAt(ListItemLeft.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (ListItemRight.Count > 0)
            {
                listItem = new ListItemData();
                ListItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + ListItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                ListItemRight.RemoveAt(ListItemRight.Count - 1);
                MoveRightCommand.RaiseCanExecuteChanged();
                MoveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// Views subscribe to this event to get notified of property updates.
        /// </summary>
        public event PropertyChangedEventHandler PropertyChanged;

        /// <summary>
        /// Notify subscribers of updates to the named property
        /// </summary>
        /// <param name="propertyName">The full, case-sensitive, name of a property.</param>
        protected void NotifyPropertyChanged(string propertyName)
        {
            PropertyChangedEventHandler handler = this.PropertyChanged;
            if (handler != null)
            {
                PropertyChangedEventArgs args = new PropertyChangedEventArgs(propertyName);
                handler(this, args);
            }
        }
    }

    /// <summary>
    /// Convert a collection count to an opacity value of 0.0 or 1.0.
    /// </summary>
    public class OpacityConverter : IValueConverter
    {
        /// <summary>
        /// Converts a collection count to an opacity value of 0.0 or 1.0.
        /// </summary>
        /// <param name="value">The count passed in</param>
        /// <param name="targetType">Ignored.</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns>1.0 if count > 0, otherwise returns 0.0</returns>
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            return ((int)value > 0 ? 1.0 : 0.0);
        }

        /// <summary>
        /// Not used, converter is not intended for two-way binding. 
        /// </summary>
        /// <param name="value">Ignored</param>
        /// <param name="targetType">Ignored</param>
        /// <param name="parameter">Ignored</param>
        /// <param name="language">Ignored</param>
        /// <returns></returns>
        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

**Por fim, aqui está nossa implementação da interface ICommand**

Aqui, definimos um comando que implementa a interface [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) e simplesmente retransmite sua funcionalidade a outros objetos.

```csharp
using System;
using System.Windows.Input;

namespace UICommand1
{
    /// <summary>
    /// A command whose sole purpose is to relay its functionality 
    /// to other objects by invoking delegates. 
    /// The default return value for the CanExecute method is 'true'.
    /// <see cref="RaiseCanExecuteChanged"/> needs to be called whenever
    /// <see cref="CanExecute"/> is expected to return a different value.
    /// </summary>
    public class RelayCommand : ICommand
    {
        private readonly Action _execute;
        private readonly Func<bool> _canExecute;

        /// <summary>
        /// Raised when RaiseCanExecuteChanged is called.
        /// </summary>
        public event EventHandler CanExecuteChanged;

        /// <summary>
        /// Creates a new command that can always execute.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        public RelayCommand(Action execute)
            : this(execute, null)
        {
        }

        /// <summary>
        /// Creates a new command.
        /// </summary>
        /// <param name="execute">The execution logic.</param>
        /// <param name="canExecute">The execution status logic.</param>
        public RelayCommand(Action execute, Func<bool> canExecute)
        {
            if (execute == null)
                throw new ArgumentNullException("execute");
            _execute = execute;
            _canExecute = canExecute;
        }

        /// <summary>
        /// Determines whether this <see cref="RelayCommand"/> can execute in its current state.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        /// <returns>true if this command can be executed; otherwise, false.</returns>
        public bool CanExecute(object parameter)
        {
            return _canExecute == null ? true : _canExecute();
        }

        /// <summary>
        /// Executes the <see cref="RelayCommand"/> on the current command target.
        /// </summary>
        /// <param name="parameter">
        /// Data used by the command. If the command does not require 
        /// data to be passed, this object can be set to null.
        /// </param>
        public void Execute(object parameter)
        {
            _execute();
        }

        /// <summary>
        /// Method used to raise the <see cref="CanExecuteChanged"/> event
        /// to indicate that the return value of the <see cref="CanExecute"/>
        /// method has changed.
        /// </summary>
        public void RaiseCanExecuteChanged()
        {
            var handler = CanExecuteChanged;
            if (handler != null)
            {
                handler(this, EventArgs.Empty);
            }
        }
    }
}
```

## <a name="summary"></a>Resumo

A Plataforma Universal do Windows fornece um sistema de comandos robusto e flexível que permite que você crie aplicativos que compartilham e gerenciam os comandos nos tipos de controle, dispositivos e tipos de entrada.

Ao criar comandos para seus aplicativos UWP, use as seguintes abordagens:

- Ouça e manipule eventos no XAML/code-behind
- Associe a um método de manipulação de eventos, como Click
- Defina sua própria implementação do ICommand
- Crie objetos de XamlUICommand com seus próprios valores para um conjunto predefinido de propriedades
- Crie objetos de StandardUICommand com um conjunto de valores e propriedades de plataforma predefinido

## <a name="next-steps"></a>Próximas etapas

Para obter um exemplo completo que demonstra uma implementação de [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) e [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand), confira a amostra da [Galeria de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics).

## <a name="see-also"></a>Consulte também

[Controles e padrões para aplicativos UWP](index.md)

### <a name="samples"></a>Exemplos

#### <a name="topic-samples"></a>Amostras de tópico

- [Amostra de comando da UWP (StandardUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-standarduicommand.zip)
- [Amostra de comando da UWP (XamlUICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-xamluicommand.zip)
- [Amostra de comando da UWP (ICommand)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-commanding-icommand.zip)

#### <a name="other-samples"></a>Outras amostras

- [Amostras da Plataforma Universal do Windows (C# e C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)
- [Galeria de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery)
