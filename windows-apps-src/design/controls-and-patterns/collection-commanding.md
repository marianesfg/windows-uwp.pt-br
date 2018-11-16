---
author: mijacobs
description: Como usar comandos contextuais para implementar esses tipos de ações de um modo que forneça a melhor experiência possível para todos os tipos de entrada.
title: Comandos contextuais
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.author: mijacobs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f06d7015fcb208b55fe0cb57b96eaecbc99317cc
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "7111843"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>Comandos contextuais para coleções e listas



Muitos aplicativos contêm coleções de conteúdo na forma de listas, grades e árvores que os usuários podem manipular. Por exemplo, os usuários podem excluir, renomear, sinalizar ou atualizar os itens. Este artigo mostra como usar os comandos contextuais para implementar esses tipos de ações de maneira que fornece a melhor experiência possível para todos os tipos de entrada.  

> **APIs importantes**: [interface ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand), [propriedade UIElement.ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), [interface INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![Usar uma variedade de entradas para executar o comando de favorito](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>Criar comandos para todos os tipos de entrada

Como os usuários podem interagir com um aplicativo UWP usando [uma ampla gama de dispositivos e entradas](../devices/index.md), seu aplicativo deve expor comandos por menus de contexto independentes do tipo de entrada e aceleradores de entrada específicos. A inclusão de ambos permite que o usuário invoque rapidamente comandos no conteúdo, independentemente do tipo de entrada ou dispositivo.

Esta tabela mostra alguns comandos típicos de coleção e os modos de expor esses comandos. 

| Comando          | Independentes do tipo de entrada | Acelerador de mouse | Acelerador de teclado | Acelerador de toque |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Excluir item      | Menu de contexto   | Focalizar botão      | Tecla DEL              | Passar o dedo para excluir   |
| Sinalizar item        | Menu de contexto   | Focalizar botão      | Ctrl+Shift+G         | Passar o dedo para sinalizar     |
| Atualizar dados     | Menu de contexto   | N/D               | Tecla F5               | Puxar para atualizar   |
| Um item favorito | Menu de contexto   | Focalizar botão      | F, Ctrl+S            | Passar o dedo para incluir nos Favoritos |


* **Em geral, você deve disponibilizar todos os comandos de um item no [menu de contexto](menus.md) do item.** Os menus de contexto são acessíveis aos usuários, independentemente do tipo de entrada e devem conter todos os comandos contextuais que o usuário pode executar.

* **Para comandos acessados com frequência, considere o uso de aceleradores de entrada.** Os aceleradores de entrada permitem que o usuário execute ações rapidamente com base em seus dispositivos de entrada. Os aceleradores de entrada incluem:
    - Ação de passar o dedo (acelerador de toque)
    - Puxe para atualizar dados (acelerador de toque)
    - Atalhos de teclado (acelerador de teclado)
    - Teclas de acesso (acelerador de teclado)
    - Botões de foco de mouse e caneta (acelerador de ponteiro)

> [!NOTE]
> Os usuários devem ser capazes de acessar todos os comandos de qualquer tipo de dispositivo. Por exemplo, se os comandos do seu aplicativo são expostos somente por meio de aceleradores de ponteiro do botão de foco, os usuários de toque não poderão acessá-los. No mínimo, use um menu de contexto para fornecer acesso a todos os comandos.  

## <a name="example-the-podcastobject-data-model"></a>Exemplo: o modelo de dados PodcastObject

Para demonstrar nossas recomendações de comandos, este artigo cria uma lista de podcasts para um aplicativo de podcast. O exemplo de código demonstra como permitir que o usuário salve como "favorito" um podcast específico de uma lista.

Esta é a definição do objeto de podcast com a qual trabalharemos: 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

Observe que PodcastObject implementa [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) para responder às alterações de propriedade quando o usuário alterna a propriedade IsFavorite.

## <a name="defining-commands-with-the-icommand-interface"></a>Definição de comandos com a interface ICommand

A [interface ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) ajuda você a definir um comando que está disponível para vários tipos de entrada. Por exemplo, em vez de escrever o mesmo código para um comando de exclusão nos dois manipuladores de eventos diferentes, um para quando o usuário pressiona a tecla Delete e outro para quando o usuário clica com o botão direito do mouse em "Excluir" em um menu de contexto, você pode implementar a lógica de excluir uma vez como um [ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)e, em seguida, torná-lo disponível para diferentes tipos de entrada.

É necessário definir o ICommand que representa a ação "Favoritos". Vamos usar o método [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) do comando para incluir o podcast nos favoritos. O podcast específico será fornecido para o método execute por meio do parâmetro do comando, que pode ser vinculado usando a propriedade CommandParameter.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

Para usar o mesmo comando com vários elementos e coleções, você pode armazenar o comando como um recurso na página ou no aplicativo.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

Para executar o comando, é necessário chamar o método [Execute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute).

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>Criação de um UserControl para responder a diversas entradas

Quando você tiver uma lista de itens e cada um deve responder a várias entradas, é possível simplificar o código ao definir uma [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl) para o item e usá-lo para definir manipuladores de eventos e o menu de contexto dos itens. 

Para criar um UserControl no Visual Studio:
1. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto. Um menu de contexto aparece.
2. Selecione **Adicionar > Novo Item...** <br />A caixa de diálogo **Adicionar Novo Item** aparece. 
3. Selecione UserControl na lista de itens. Dê o nome que você deseja e clique em **Adicionar**. O Visual Studio gera um stub UserControl para você. 

No exemplo de podcast, cada um será exibido em uma lista, o que expõe diversas maneiras de incluir um podcast nos "Favoritos". O usuário será capaz de executar as seguintes ações para incluir o podcast nos "Favoritos":
- Invocar um menu de contexto
- Executar atalhos de teclado
- Mostrar um botão de foco
- Executar um gesto de passar o dedo

Para encapsular esses comportamentos e usar o FavoriteCommand, vamos criar um novo [UserControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl) chamado "PodcastUserControl" para representar um podcast na lista.

O PodcastUserControl exibe os campos do PodcastObject como TextBlocks e responde a diversas interações do usuário. Abordaremos a referência e explicaremos melhor o PodcastUserControl neste artigo.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

Observe que o PodcastUserControl mantém uma referência ao PodcastObject como uma DependencyProperty. Isso nos permite associar PodcastObjects ao PodcastUserControl.

Depois de gerar alguns PodcastObjects, você pode criar uma lista de podcasts ao vincular PodcastObjects a um ListView. Os objetos PodcastUserControl descrevem a visualização do PodcastObjects e, portanto, são definidos usando o ItemTemplate do ListView.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>Criar menus de contexto

Os menus de contexto exibem uma lista de comandos ou opções quando o usuário as solicita. Os menus de contexto fornecem comandos contextuais relacionados ao elemento anexado e, em geral, são reservados para ações secundárias específicas desse item.

![Mostrar um menu de contexto no item](images/ContextualCommand_RightClick.png)

O usuário pode invocar os menus de contexto usando essas "ações de contexto":

| Entrada    | Ação de contexto                          |
| -------- | --------------------------------------- |
| Mouse    | Clique com o botão direito do mouse                             |
| Teclado | Shift+F10, botão de Menu                  |
| Toque    | Pressionamento prolongado em item                      |
| Caneta      | Pressionamento de botão do cilindro da caneta, um pressionamento prolongado em item |
| Gamepad  | Botão Menu                             |

**Como o usuário pode abrir um menu de contexto independentemente do tipo de entrada, o menu de contexto deve conter todos os comandos contextuais disponíveis para o item de lista.**

### <a name="contextflyout"></a>ContextFlyout

A [propriedade ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), definida pela classe UIElement, facilita a criação de um menu de contexto que funciona com todos os tipos de entrada. Você fornece um submenu que representa o menu de contexto usando MenuFlyout e, quando o usuário executa uma "ação de contexto" conforme definido acima, o MenuFlyout correspondente ao item será exibido.

Adicionaremos um ContextFlyout para o PodcastUserControl. O MenuFlyout especificado como ContextFlyout contém um único item para incluir um podcast nos favoritos. Observe que este MenuFlyoutItem usa o favoriteCommand definido acima, com o CommandParamter associado a PodcastObject.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

Observe que você também pode usar o [evento ContextRequested](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested) para responder às ações de contexto. O evento ContextRequested não será acionado se um ContextFlyout foi especificado.

## <a name="creating-input-accelerators"></a>Criação de aceleradores de entrada

Embora cada item da coleção deva ter um menu de contexto com todos os comandos contextuais, convém permitir que os usuários realizem rapidamente um conjunto menor de comandos realizados com frequência. Por exemplo, um aplicativo de endereçamento pode ter comandos secundários como Responder, Arquivar, Mover para pasta, Definir sinalizador e Excluir que aparecem em um menu de contexto, mas os comandos mais comuns são Excluir e Sinalizar. Depois de identificar os comandos mais comuns, você pode usar aceleradores com base na entrada para facilitar a execução desses comandos por um usuário.

No aplicativo de podcast, o comando executado com frequência é o comando "Favorite".

### <a name="keyboard-accelerators"></a>Aceleradores de teclado

#### <a name="shortcuts-and-direct-key-handling"></a>Atalhos e manipulação direta de tecla

![Pressione Ctrl e F para executar uma ação](images/ContextualCommand_Keyboard.png)

Dependendo do tipo de conteúdo, você pode identificar determinadas combinações de teclas que devem realizar uma ação. Por exemplo, em um aplicativo de email, a tecla DEL pode ser usada para excluir o email selecionado. Em um aplicativo de podcast, as teclas Ctrl + S ou F poderiam incluir um podcast como favorito para uso posterior. Embora alguns comandos tenham atalhos de teclado comuns e conhecidas, como DEL para excluir, outros comandos têm atalhos de aplicativo ou domínio específico. Use atalhos conhecidos se possível ou considere o fornecer um texto de lembrete em uma dica de ferramenta para ensinar o usuário sobre o comando de atalho.

O aplicativo pode responder quando o usuário pressionar uma tecla usando o evento [KeyDown](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent). Em geral, os usuários esperam que o aplicativo responda ao pressionarem a tecla em vez de aguardar até que a soltem.

Este exemplo explica como adicionar o manipulador de eventos KeyDown a PodcastUserControl para incluir um podcast como favorito quando o usuário pressiona Ctrl + S ou F. Ele usa o mesmo comando de antes.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>Aceleradores de mouse

![Passe o mouse sobre um item para relevar um botão](images/ContextualCommand_HovertoReveal.png)

Os usuários estão familiarizados com os menus de contexto ao clicar com o botão direito do mouse, mas você pode desejar capacitar os usuários para executar comandos comuns com apenas um único clique do mouse. Para habilitar essa experiência, você pode incluir botões dedicados em canvas do item de coleção. Para permitir que os usuários atuem rapidamente usando o mouse e reduzir a poluição visual, você pode optar por exibir somente esses botões quando o usuário está com o ponteiro dentro de um item de lista específico.

Neste exemplo, o comando Favorite é representado por um botão definido diretamente no PodcastUserControl. Observe que o botão neste exemplo usa o mesmo comando, FavoriteCommand, como antes. Para alternar a visibilidade desse botão, você pode usar VisualStateManager para alternar entre os estados visuais quando o ponteiro entra e sai do controle.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

Os botões de foco devem aparecer e desaparecer quando o mouse entra e sai do item. Para responder a eventos de mouse, você pode usar os eventos [PointerEntered](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) e [PointerExited](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) em PodcastUserControl.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

Os botões exibidos no estado de foco podem ser acessados somente por meio do tipo de entrada de ponteiro. Como esses botões são limitados à entrada de ponteiro, você pode optar por minimizar ou remover o preenchimento ao redor do ícone do botão para otimizar a entrada de ponteiro. Se você optar por fazer isso, certifique-se de que o volume do botão seja de pelo menos 20 x 20 px para ser utilizável com caneta e mouse.

### <a name="touch-accelerators"></a>Aceleradores de toque

#### <a name="swipe"></a>Deslizar o dedo

![Deslizar o dedo sobre um item para revelar o comando](images/ContextualCommand_Swipe.png)

Os comandos efetuados ao deslizar o dedo são aceleradores de toque que permitem aos usuários em dispositivos sensíveis ao toque executar ações comuns secundárias. Ao deslizar o dedo, os usuários podem interagir com conteúdo de maneira rápida e natural, usando as ações comuns como passar o dedo para excluir ou passar o dedo para invocar. Consulte o artigo sobre [comandos de deslizar o dedo](swipe.md) para saber mais.

Para integrar o passar o dedo na sua coleção, você precisa de dois componentes: SwipeItems, que guarda os comandos; e um SwipeControl, que envolve o item e permite a interação do passar o dedo.

Os SwipeItems podem ser definidos como um Recurso no PodcastUserControl. Neste exemplo, o SwipeItems contém um comando para tornar um item Favorito.

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

O SwipeControl envolve o item e permite que o usuário interaja com ele usando o gesto de passar o dedo. Observe que o SwipeControl contém uma referência aos SwipeItems como seus RightItems. O item Favoritos será exibido quando o usuário passar o dedo da direita para a esquerda.

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

Quando o usuário desliza o dedo para invocar o comando Favorite, o método Invoked é chamado.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>Puxar para atualizar

A ação de Puxar para atualizar permite a um usuário extrair uma coleção de dados com toque para recuperar mais dados. Consulte o artigo [puxar para atualizar](pull-to-refresh.md) a fim de saber mais.

### <a name="pen-accelerators"></a>Aceleradores de caneta

O tipo de entrada de caneta fornece a precisão de entrada de ponteiro. Os usuários podem realizar ações comuns, como abrir os menus de contexto com aceleradores baseados em caneta. Para abrir um menu de contexto, os usuários podem tocar na tela com o botão do cilindro da caneta pressionado ou manter o conteúdo pressionado. Os usuários também podem usar a caneta para focalizar o conteúdo a fim de obter uma compreensão mais profunda da interface do usuário, como exibir dicas de ferramentas ou para revelar ações de foco secundário, semelhante ao mouse.

Para otimizar seu aplicativo para entrada de caneta, consulte o artigo [interação com caneta](../input/pen-and-stylus-interactions.md).


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

* Certifique-se de que os usuários podem acessar todos os comandos de todos os tipos de dispositivos UWP.
* Inclua um menu de contexto que fornece acesso a todos os comandos disponíveis para um item da coleção. 
* Forneça aceleradores de entradas para comandos usados com frequência. 
* Use a [interface ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) para implementar comandos. 

## <a name="related-topics"></a>Tópicos relacionados
* [Interface ICommand](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [Menus e menus de contexto](menus.md)
* [Passar o dedo](swipe.md)
* [Puxar para atualizar](pull-to-refresh.md)
* [Interação com caneta](../input/pen-and-stylus-interactions.md)
* [Adaptar seu aplicativo para gamepad e Xbox](../devices/designing-for-tv.md)
