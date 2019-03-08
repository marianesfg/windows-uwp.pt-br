---
title: Aplicativos UWP (plataforma) dos comandos no Windows Universal
description: Como usar as classes XamlUICommand e StandardUICommand (juntamente com a interface ICommand) para compartilhar e gerenciar os comandos em vários tipos de controle, independentemente do dispositivo e o tipo de entrada que está sendo usado.
author: Karl-Bridge-Microsoft
ms.service: ''
ms.topic: overview
ms.date: 11/01/2018
ms.author: kbridge
ms.openlocfilehash: 32d5005f9965b14d5080344832eb185f0e711689
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646521"
---
# <a name="commanding-in-universal-windows-platform-uwp-apps-using-standarduicommand-xamluicommand-and-icommand"></a>Comandos em aplicativos da plataforma Universal do Windows (UWP) usando StandardUICommand, XamlUICommand e ICommand

Neste tópico, descreveremos o comando nos aplicativos da plataforma Universal do Windows (UWP). Especificamente, discutiremos como você pode usar o [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) e [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) para compartilhar e gerenciar os comandos em vários tipos de controle, independentemente de classes (juntamente com a interface ICommand) o dispositivo e o tipo de entrada que está sendo usado.

![Um diagrama que representa um uso comum para um comando compartilhado: interface do usuário várias superfícies com um comando 'favorito'](images/commanding/generic-commanding.png)

*Comandos de compartilhamento entre vários controles, independentemente do dispositivo e o tipo de entrada*

## <a name="important-apis"></a>APIs Importantes

- [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) e [System](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand)

## <a name="overview"></a>Visão geral

<!-- See https://blogs.msdn.microsoft.com/jebarson/2017/07/26/writing-an-asynchronous-relaycommand-implementing-icommand/ -->

<!-- A command describes an action but not its implementation (in other words, the what, but not the how). For example, the "Paste" command indicates that the user wants to copy something from the clipboard to a target control, but does not specify how. -->

Comandos podem ser chamados diretamente por meio de interações de interface do usuário, como clicar em um botão ou selecionar um item de um menu de contexto. Eles também podem ser invocados indiretamente por meio de um dispositivo de entrada, como um acelerador de teclado, gesto, reconhecimento de fala ou uma ferramenta de automação/acessibilidade. Quando invocado, o comando pode ser processado por um controle (navegação de texto em um controle de edição), uma janela (navegação) ou o aplicativo (Sair).

Comandos podem operar em um contexto específico dentro de seu aplicativo, como excluir texto ou desfazer uma ação, ou podem ser sem contexto, como a desativação do áudio ou ajustando o brilho.

A imagem a seguir mostra duas interfaces de comando (um [CommandBar](app-bars.md) e flutuante contextuais [CommandBarFlyout](command-bar-flyout.md)) que compartilham muitos dos mesmos comandos.

![Exemplo de interface de comando](images/commanding/command-interface-example.png)

## <a name="command-interactions"></a>Interações de comando

Devido à variedade de dispositivos, os tipos de entrada e as superfícies de interface do usuário que podem afetar como um comando é invocado, é recomendável expor seus comandos por meio de superfícies de comandos tantos quanto possível. Eles podem incluir uma combinação de [passe o dedo](swipe.md), [barra de menus](menus.md), [CommandBar](app-bars.md), [CommandBarFlyout](command-bar-flyout.md)e tradicional [ menu de contexto](menus.md).

**Para comandos críticos, use aceleradores de entrada específica.** Aceleradores de entrada permitem que um usuário executar ações mais rapidamente, dependendo do dispositivo de entrada que está sendo usada.

Aqui estão algumas comuns aceleradores de entrada para vários tipos de entrada:

- **Ponteiro** -Mouse a & brir botões em foco
- **Teclado** -atalhos (chaves de acesso e teclas de aceleração)
- **Toque** -passe o dedo
- **Toque** -Deslizar para atualizar dados

Você deve considerar as experiências de usuário e o tipo de entrada para tornar a funcionalidade do aplicativo universalmente acessíveis. Por exemplo, coleções (especialmente aqueles editáveis de usuário) normalmente incluem uma variedade de comandos específicos que são executadas de modo bem diferente, dependendo do dispositivo de entrada.

A tabela a seguir mostra alguns comandos de coleção típica e maneiras de expor esses comandos.

| Comando          | Independentes do tipo de entrada | Acelerador de mouse | Acelerador de teclado | Acelerador de toque |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Excluir item      | Menu de contexto   | Focalizar botão      | Tecla DEL              | Passar o dedo para excluir   |
| Sinalizar item        | Menu de contexto   | Focalizar botão      | Ctrl+Shift+G         | Passar o dedo para sinalizar     |
| Atualizar dados     | Menu de contexto   | N/D               | Tecla F5               | Puxar para atualizar   |
| Um item favorito | Menu de contexto   | Focalizar botão      | F, Ctrl+S            | Passar o dedo para incluir nos Favoritos |

**Sempre fornecer um menu de contexto** recomendamos incluir todos os comandos contextuais relevantes em um menu de contexto tradicional ou CommandBarFlyout, pois ambos têm suporte para todos os tipos de entrada. Por exemplo, se um comando é exposto somente durante um evento de passagem do ponteiro, ele não pode ser usado em um dispositivo somente de toque.

## <a name="commands-in-uwp-applications"></a>Comandos em aplicativos UWP

Há algumas maneiras que você possa compartilhar e gerenciar experiências de comandos em um aplicativo UWP. Você pode definir manipuladores de eventos para interações padrão, como clique, no code-behind (Isso pode ser ineficiente, dependendo da complexidade da sua interface do usuário), você pode vincular o ouvinte de eventos para interações padrão a um manipulador compartilhado ou você pode associar o controle Propriedade Command para uma implementação de ICommand que descreve a lógica de comando.

Para fornecer experiências de usuário abrangente e rico em superfícies de comando com eficiência e com eliminação de duplicação de código mínimo, é recomendável usar o comando recursos descritos neste tópico de associação (padrão para manipulação de eventos, consulte os tópicos do evento individual).

Para associar um controle a um recurso compartilhado do comando, você pode implementar as interfaces ICommand, ou você pode compilar seu comando de qualquer um de [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe base ou um dos comandos de plataforma definidos pelo [ StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe derivada.

- A interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) ou [System](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)) permite que você crie totalmente personalizado, reutilizável comandos em seu aplicativo.
- [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) também fornece essa funcionalidade, mas simplifica o desenvolvimento, expondo um conjunto de propriedades de comando internas, como o comportamento do comando, os atalhos de teclado (tecla de acesso e a tecla de aceleração), ícone, rótulo e a descrição.
- [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) simplifica ainda mais as coisas, permitindo que você escolha um conjunto de comandos de plataforma padrão com as propriedades predefinidas.

> [!Important]
> Em aplicativos UWP, os comandos são implementações de qualquer um de [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) (C++) ou o [System](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) (C#) interface, dependendo do escolhido estrutura de linguagem.

## <a name="command-experiences-using-the-standarduicommand-class"></a>Experiências de comando usando a classe StandardUICommand

Derivado [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) (derivado de [Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) para C++ ou [System](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand) para C#), o [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe expõe um conjunto de comandos de plataforma padrão com propriedades predefinidas, como o ícone, o Acelerador de teclado e a descrição.

Um [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) fornece uma maneira rápida e consistente para definir os comandos comuns, como `Save` ou `Delete`. Tudo o que você precisa fazer é fornecer as funções execute e canExecute.

### <a name="example"></a>Exemplo

![Exemplo de StandardUICommand](images/commanding/StandardUICommandSampleOptimized.gif)

*StandardUICommandSample*

Neste exemplo, vamos mostrar como aprimorar um basic [ListView](listview-and-gridview.md) com uma exclusão de comando implementado por meio do item a [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe, ao otimizar a experiência do usuário para uma variedade de tipos de entrada usando um [barra de menus](menus.md), [passar o dedo](swipe.md) controle, botões de focalizar, e [menu de contexto](menus.md).

> [!NOTE]
> Este exemplo requer o pacote do Microsoft.UI.Xaml.Controls NuGet, uma parte dos [biblioteca de interface do usuário do Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

O exemplo de interface do usuário inclui um [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinco itens. A exclusão [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) está associado a um [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), um [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), um [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), e [ Menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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

1. Primeiro, definimos um `ListItemData` classe que contém uma cadeia de caracteres de texto e ICommand para cada ListViewItem em nosso ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Na classe MainPage, definimos uma coleção de `ListItemData` objetos para o [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) da [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Podemos, em seguida, preenchê-lo com uma coleção inicial de cinco itens (com texto e respectivos [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) excluir).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    var deleteCommand = new StandardUICommand(StandardUICommandKind.Delete);
    deleteCommand.ExecuteRequested += DeleteCommand_ExecuteRequested;

    DeleteFlyoutItem.Command = deleteCommand;

    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = deleteCommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Em seguida, definimos o manipulador de ICommand ExecuteRequested no qual podemos implementar o comando de exclusão de item.

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
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

4. Por fim, podemos definir manipuladores para vários eventos do ListView, incluindo [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), e [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) eventos. Os manipuladores de eventos de ponteiro são usados para mostrar ou ocultar o botão Excluir para cada item.

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
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(sender as Control, "HoverButtonsShown", true);
    }
}

private void ListViewSwipeContainer_PointerExited(object sender, PointerRoutedEventArgs e)
{
    VisualStateManager.GoToState(sender as Control, "HoverButtonsHidden", true);
}
```

## <a name="command-experiences-using-the-xamluicommand-class"></a>Experiências de comando usando a classe XamlUICommand

Se você precisa criar um comando que não é definido pelo [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) classe, ou você deseja obter mais controle sobre a aparência de comando, o [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe deriva o [ ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) interface, adicionando várias propriedades (como um ícone, rótulo, descrição e atalhos de teclado) de interface do usuário, métodos e eventos para definir rapidamente a interface do usuário e o comportamento de um comando personalizado.

[XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) permite que você especifique a interface do usuário por meio do controle de associação, como um ícone de rótulo, descrição e (uma chave de acesso e um acelerador de teclado), os atalhos de teclado sem definir as propriedades individuais.

### <a name="example"></a>Exemplo

![Exemplo de XamlUICommand](images/commanding/XamlUICommandSampleOptimized.gif)

*XamlUICommandSample*

Este exemplo compartilha a funcionalidade de exclusão de versões anteriores [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) exemplo, mas mostra como o [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe permite que você defina um comando de exclusão personalizado com seu próprio ícone de fonte, rótulo, Acelerador de teclado e a descrição. Como o [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) exemplo, podemos aprimorar básico [ListView](listview-and-gridview.md) com uma exclusão de comando implementado por meio do item a [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) classe, ao otimizar o experiência do usuário para uma variedade de tipos de entrada usando um [barra de menus](menus.md), [passar o dedo](swipe.md) controle, botões de focalizar, e [menu de contexto](menus.md).

Muitos controles de plataforma usam as propriedades de XamlUICommand nos bastidores, assim como nosso exemplo StandardUICommand na seção anterior. 

> [!NOTE]
> Este exemplo requer o pacote do Microsoft.UI.Xaml.Controls NuGet, uma parte dos [biblioteca de interface do usuário do Microsoft Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

**Xaml:**

O exemplo de interface do usuário inclui um [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) de cinco itens. O custom [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) Delete está associado a um [MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem), um [SwipeItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipeitem), um [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton), e [ Menu ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

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
        <XamlUICommand x:Name="CustomXamlUICommand" ExecuteRequested="DeleteCommand_ExecuteRequested" Description="Custom XamlUICommand" Label="Custom XamlUICommand">
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
                <MenuFlyoutItem x:Name="DeleteFlyoutItem" Command="{StaticResource CustomXamlUICommand}"/>
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

1. Primeiro, definimos um `ListItemData` classe que contém uma cadeia de caracteres de texto e ICommand para cada ListViewItem em nosso ListView.

```csharp
public class ListItemData
{
    public String Text { get; set; }
    public ICommand Command { get; set; }
}
```

2. Na classe MainPage, definimos uma coleção de `ListItemData` objetos para o [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) da [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate). Podemos, em seguida, preenchê-lo com uma coleção inicial de cinco itens (com texto e respectivos [XamlUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand)).

```csharp
ObservableCollection<ListItemData> collection = new ObservableCollection<ListItemData>();

private void ControlExample_Loaded(object sender, RoutedEventArgs e)
{
    for (var i = 0; i < 5; i++)
    {
        collection.Add(new ListItemData { Text = "List item " + i.ToString(), Command = CustomXamlUICommand });
    }
}

private void ListView_Loaded(object sender, RoutedEventArgs e)
{
    var listView = (ListView)sender;
    listView.ItemsSource = collection;
}
```

3. Em seguida, definimos o manipulador de ICommand ExecuteRequested no qual podemos implementar o comando de exclusão de item.

``` csharp
private void DeleteCommand_ExecuteRequested(XamlUICommand sender, ExecuteRequestedEventArgs args)
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

4. Por fim, podemos definir manipuladores para vários eventos do ListView, incluindo [PointerEntered](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered), [PointerExited](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited), e [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) eventos. Os manipuladores de eventos de ponteiro são usados para mostrar ou ocultar o botão Excluir para cada item.

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
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
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

Controles padrão do UWP (botão, lista, seleção, calendário, texto preditivo) fornecem a base para muitas experiências de comando comuns. Para obter uma lista completa dos tipos de controle, consulte [controles e padrões para aplicativos UWP](index.md).

A maneira mais simples para dar suporte a uma experiência estruturada de comando é definir uma implementação da interface ICommand ([Windows.UI.Xaml.Input.ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) para o C++ ou [System](https://docs.microsoft.com/dotnet/api/system.windows.input.icommand)para C#).  Esta instância ICommand, em seguida, poderão estar associada a controles como botões.

> [!NOTE]
> Em alguns casos, pode ser apenas tão eficiente para associar um método para o evento de clique e uma propriedade para a propriedade IsEnabled.

#### <a name="example"></a>Exemplo

![Exemplo de interface de comando](images/commanding/icommand.gif)

*Exemplo de ICommand*
 
Neste exemplo, demonstraremos como um único comando pode ser chamado com um botão clique, um acelerador de teclado e girando a roda do mouse.

Nós usamos dois [ListViews](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview), um preenchida com cinco itens e outro vazio e dois botões, uma para mover os itens do ListView à esquerda para o ListView à direita, e o outro para mover os itens da direita para esquerda. Cada botão é associado a um comando correspondente (ViewModel.MoveRightCommand e ViewModel.MoveLeftCommand, respectivamente) e são habilitados e desabilitados automaticamente com base no número de itens em sua ListView associado.

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
        <Grid Grid.Column="1" Margin="0,0,5,0"
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
                          Opacity="{x:Bind Path=ViewModel.listItemLeft.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
                <Button Name="MoveItemRightButton" ToolTipService.ToolTip="Tooltip"
                        Margin="0,10,0,10" Width="120" HorizontalAlignment="Center"
                        Command="{x:Bind Path=ViewModel.MoveRightCommand, Mode=OneWay}">
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
                            Command="{x:Bind Path=ViewModel.MoveLeftCommand, Mode=OneWay}">
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
                          Opacity="{x:Bind Path=ViewModel.listItemRight.Count, Mode=OneWay, Converter={StaticResource opaque}}"/>
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

No code-behind, nos conectamos ao nosso modelo de exibição que contém nosso código de comando. Além disso, podemos definir um manipulador para a entrada da roda do mouse, que também se conecta nosso código de comando.

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

Nosso modelo de exibição é onde podemos definir os detalhes de execução para os dois comandos em nosso aplicativo, preencher um ListView e fornecer um conversor de valor de opacidade para ocultar ou exibir algumas interfaces do usuário adicionais com base na contagem de item de cada ListView.

```csharp
using System;
using System.Collections.ObjectModel;
using System.Collections.Specialized;
using System.ComponentModel;
using Windows.UI.Xaml;
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
    public class UICommand1ViewModel : INotifyPropertyChanged
    {
        /// <summary>
        /// The command to invoke when the Move item left button is pressed.
        /// </summary>
        public RelayCommand moveLeftCommand;
        public RelayCommand MoveLeftCommand { get => moveLeftCommand; private set { } }

        /// <summary>
        /// The command to invoke when the Move item right button is pressed.
        /// </summary>
        public RelayCommand moveRightCommand;
        public RelayCommand MoveRightCommand { get => moveRightCommand; private set { } }

        // Item collections
        public ObservableCollection<ListItemData> listItemLeft;
        public ObservableCollection<ListItemData> ListItemLeft { get => listItemLeft; private set { } }
        public ObservableCollection<ListItemData> listItemRight;
        public ObservableCollection<ListItemData> ListItemRight { get => listItemRight; private set { } }

        public ListItemData listItem;

        /// <summary>
        /// Sets up a command to handle invoking the move item buttons.
        /// </summary>
        public UICommand1ViewModel()
        {
            moveLeftCommand = new RelayCommand(new Action(MoveLeft), CanExecuteMoveLeftCommand);
            moveRightCommand = new RelayCommand(new Action(MoveRight), CanExecuteMoveRightCommand);

            listItemLeft = new ObservableCollection<ListItemData>();
            listItemRight = new ObservableCollection<ListItemData>();

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
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemLeft.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
            }
        }

        /// <summary>
        /// Move left command valid when items present in the list on right.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveLeftCommand()
        {
            return listItemRight.Count > 0;
        }

        /// <summary>
        /// Move right command valid when items present in the list on left.
        /// </summary>
        /// <returns>True, if count is greater than 0.</returns>
        private bool CanExecuteMoveRightCommand()
        {
            return listItemLeft.Count > 0;
        }

        /// <summary>
        /// The command implementation to execute when the Move item right button is pressed.
        /// </summary>
        public void MoveRight()
        {
            if (listItemLeft.Count > 0)
            {
                listItem = new ListItemData();
                listItemRight.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemLeft.RemoveAt(listItemLeft.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
            }
        }

        /// <summary>
        /// The command implementation to execute when the Move item left button is pressed.
        /// </summary>
        public void MoveLeft()
        {
            if (listItemRight.Count > 0)
            {
                listItem = new ListItemData();
                listItemLeft.Add(listItem);
                listItem.ListItemText = "Item " + listItemRight.Count.ToString();
                listItem.ListItemIcon = Symbol.Emoji;
                listItemRight.RemoveAt(listItemRight.Count - 1);
                moveRightCommand.RaiseCanExecuteChanged();
                moveLeftCommand.RaiseCanExecuteChanged();
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
        /// <param name="value">The bool passed in</param>
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

Aqui, definimos um comando que implementa o [ICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) de interface e simplesmente retransmite sua funcionalidade a outros objetos.

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
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
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
        /// Data used by the command. If the command does not require data to be passed, this object can be set to null.
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

A plataforma Universal do Windows fornece um sistema de comando robusto e flexível que permite que você crie aplicativos que compartilham e gerenciar os comandos nos tipos de controle, dispositivos e tipos de entrada.

Ao criar comandos para seus aplicativos UWP, use as seguintes abordagens:

- Ouça e manipule eventos no XAML/code-behind
- Associar a um método, como o clique de manipulação de eventos
- Definir sua própria implementação do ICommand
- Criar objetos de XamlUICommand com seus próprios valores para um conjunto predefinido de propriedades
- Criar objetos de StandardUICommand com um conjunto de propriedades de plataforma predefinida e valores

## <a name="next-steps"></a>Próximas etapas

Para obter um exemplo completo que demonstra uma [XamlUiCommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xamluicommand) e [StandardUICommand](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.standarduicommand) implementação, consulte a [da Galeria de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) exemplo.

## <a name="see-also"></a>Consulte também

[Controles e padrões para aplicativos UWP](index.md)

<!---Some context for the following links goes here
- [link to next logical step for the customer](global-quickstart-template.md)--->

<!--- Required:
In Overview articles, provide at least one next step and no more than three.
Next steps in overview articles will often link to a quickstart.
Use regular links; do not use a blue box link. What you link to will depend on what is really a next step for the customer.
Do not use a "More info section" or a "Resources section" or a "See also section".
--->