---
Description: Responda às ações de pressionamento de tecla em teclados de hardware ou de software nos aplicativos usando o teclado e manipuladores de eventos de classe.
title: Eventos de teclado
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: teclado, gamepad, remoto, acessibilidade, navegação, foco, texto, entrada, interações de usuário, liberar tecla, pressionar tecla
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2800db96177f77648d2d2a98f5cd87c930f6840a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258332"
---
# <a name="keyboard-events"></a>Eventos de teclado

## <a name="keyboard-events-and-focus"></a>Foco e eventos do teclado

Os seguintes eventos podem ocorrer para teclados físicos e virtuais.

| Evento                                      | Descrição                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) | Ocorre quando uma tecla é pressionada.  |
| [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)     | Ocorre quando uma tecla é solta. |

> [!IMPORTANT]
> Alguns controles do Windows Runtime manipulam eventos de entrada internamente. Nestes casos, pode parecer que não ocorre um evento de entrada, porque o ouvinte de evento não chama o manipulador associado. Geralmente, este subconjunto de teclas é processado pelo manipulador de classe para proporcionar suporte interno de acessibilidade de teclado básico. Por exemplo, a classe [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) sobrepõe os eventos [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown) para a tecla Enter ou a barra de espaço (bem como [**OnPointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onpointerpressed)) e os encaminha para o evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) do controle. Quando uma tecla é manipulada pela classe de controle, os eventos [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) não são acionados.  
> Isso fornece um teclado integrado equivalente para invocar o botão, semelhante a tocar com um dedo ou clicar com o mouse. Outras teclas, além da tecla Enter e da barra de espaço, ainda acionam os eventos [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Para obter mais informações sobre como a manipulação baseada em classes de eventos funciona (especificamente, a seção "Manipuladores de evento de entrada em controles), consulte [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).


Os controles em sua interface do usuário geram eventos de teclado apenas quando possuem foco de entrada. Um controle individual ganha foco quando o usuário clica ou toca diretamente nesse controle no layout, ou utiliza a tecla Tab para avançar na sequência de tabulação dentro da área de conteúdo.

Você também pode chamar um método [**Focus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focus) do controle para forçar o foco. Isso é necessário quando você implementa teclas de atalho porque o foco do teclado não está definido por padrão quando a interface do usuário é carregada. Para obter mais informações, consulte **Exemplos de tecla de atalho** posteriormente neste tópico.

Para um controle receber o foco de entrada, ele deve estar habilitado, visível e apresentar valores de propriedade [**IsTabStop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) e [**HitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible) como **true**. Esse é o estado padrão da maioria dos controles. Quando um controle tem um foco de entrada, ele pode acionar e responder a eventos de entrada de teclado conforme descrito posteriormente neste tópico. Você também pode responder a um controle que recebe ou perde foco manipulando os eventos [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) e [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus).

Por padrão, a sequência de tabulação de controles está na ordem que aparece na Extensible Application Markup Language (XAML). No entanto, é possível modificar essa ordem usando a propriedade [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex). Para saber mais, consulte [Implementando a acessibilidade de teclado](https://docs.microsoft.com/previous-versions/windows/apps/hh868161(v=win.10)).

## <a name="keyboard-event-handlers"></a>Manipuladores de eventos do teclado


Um manipulador de eventos de entrada implementa um delegado que fornece as seguintes informações:

-   O remetente do evento. O remetente relata o objeto ao qual o manipulador de eventos está anexado.
-   Dados do evento. Para eventos do teclado, esses dados serão uma instância de [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs). O delegado para manipuladores é [**KeyEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyeventhandler). As propriedades mais relevantes de **KeyRoutedEventArgs** para a maioria dos cenários de manipulação são [**Key**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) e possivelmente [**KeyStatus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus).
-   [**Originalização**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource). Como eventos do teclado são eventos roteados, os dados dos eventos fornecem **OriginalSource**. Se você deliberadamente permite que eventos subam a árvore de um objeto, **OriginalSource** é, por vezes, o objeto em questão em vez do remetente. No entanto, isso depende do seu design. Para saber mais sobre como você pode usar **OriginalSource** em vez do remetente, consulte a seção "Eventos roteados do teclado" deste tópico, ou [Visão geral de eventos e eventos roteados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

### <a name="attaching-a-keyboard-event-handler"></a>Anexando um manipulador de eventos do teclado

Você pode anexar funções de manipulação de eventos do teclado a qualquer objeto que inclua o evento como um membro. Isso inclui qualquer classe derivada de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement). O exemplo de XAML a seguir mostra como anexar manipuladores ao evento [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) de um [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

Você também pode anexar um manipulador de eventos em código. Para saber mais, consulte [Events and routed events overview](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

### <a name="defining-a-keyboard-event-handler"></a>Definindo um manipulador de eventos do teclado

O exemplo a seguir mostra a definição do manipulador de eventos incompleta para o manipulador de eventos [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) que foi anexado no exemplo anterior.

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>Usando KeyRoutedEventArgs

Todos os eventos do teclado usam [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) para dados de eventos, e **KeyRoutedEventArg** contém as seguintes propriedades:

-   [**Chaves**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key)
-   [**KeyStatus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)
-   [**Cargo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)
-   [**Original**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) (Herdado de [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs))

### <a name="key"></a>Chave

O evento [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) será acionado se a tecla for pressionada. Da mesma forma, [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) será acionado se a tecla for liberada. Normalmente, você escuta eventos para processar um valor de tecla específico. Para determinar qual tecla é pressionada ou liberada, verifique o valor de [**Key**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) nos dados do evento. **Key** retorna um valor [**VirtualKey**](https://docs.microsoft.com/uwp/api/Windows.System.VirtualKey). A enumeração **VirtualKey** inclui todas as teclas com suporte.

### <a name="modifier-keys"></a>Teclas modificadoras

As teclas modificadoras são teclas como Ctrl ou Shift que os usuários normalmente pressionam em combinação com outras teclas. Seu aplicativo pode usar essas combinações como atalhos de teclado para chamar comandos do aplicativo.

É possível detectar combinações de teclas de atalho usando os manipuladores de evento [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Você pode controlar o estado pressionado das teclas modificadoras em que tem interesse. Quando ocorre um evento de teclado para uma tecla não modificadora, é possível verificar se uma tecla modificadora está no estado pressionado ao mesmo tempo.

> [!NOTE]
> A tecla Alt é representada pelo valor **VirtualKey.Menu**.

 

### <a name="shortcut-keys-example"></a>Amostra de teclas de atalho


O exemplo a seguir mostra como implementar as teclas de atalho. Neste exemplo, os usuários podem controlar a reprodução de mídia usando os botões Play, Pause e Stop, ou os atalhos de teclado Ctrl+P, Ctrl+A e Ctrl+S. O botão XAML mostra os atalhos usando dicas de ferramentas e propriedades [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) nos rótulos dos botões. Esta autodocumentação é importante para aumentar a usabilidade e acessibilidade de seu aplicativo. Para obter mais informações, consulte [Acessibilidade do teclado](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility).

Observe também que a página define o foco de entrada para si própria quando é carregada. Sem essa etapa, nenhum controle terá foco de entrada inicial e o aplicativo não acionará eventos de entrada até que o usuário defina o foco de entrada manualmente (por exemplo, pressionando TAB ou clicando em um controle).

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) 
{
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) 
    {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> A configuração de [**AutomationProperties.AcceleratorKey**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.acceleratorkey) ou [**AutomationProperties.AccessKey**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey) no XAML oferece informações sobre cadeia de caracteres, que documentam a tecla de atalho para chamar a ação em particular. As .informações são capturadas por clientes de Automação da Interface do Usuário da Microsoft como o Narrador e são tipicamente fornecidas diretamente ao usuário.
>
> A configuração de **AutomationProperties.AcceleratorKey** ou de **AutomationProperties.AccessKey** não tem qualquer ação por conta própria. Você ainda precisa anexar manipuladores para eventos [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) ou [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) para realmente implementar o comportamento de atalho de teclado em seu aplicativo. Além disso, a decoração de texto sublinhado para uma tecla de acesso não é fornecida automaticamente. Você deve sublinhar explicitamente o texto para a tecla específica em seu mnemônico como formatação [**Underline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Underline) embutida se desejar mostrar texto sublinhado na interface do usuário.

 

## <a name="keyboard-routed-events"></a>Eventos de teclado roteados


Alguns eventos são eventos roteados, inclusive [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup). Eventos roteados usam a estratégia de roteamento por propagação. A estratégia de roteamento por propagação indica que um evento se origina de um objeto filho e então é encaminhado para sucessivos objetos pai na árvore de objetos. Isso apresenta outra oportunidade para manipular o mesmo evento e interagir com os mesmos dados de eventos.

Considere o exemplo em XAML a seguir, que manipula eventos [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) para um objeto [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) e dois objetos [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). Neste caso, se você soltar uma tecla enquanto o foco é mantido por cada objeto **Button**, ele aciona o evento **KeyUp**. O evento é, então, propagado para o **Canvas** pai.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

O exemplo a seguir mostra como implementar o manipulador de eventos [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) no conteúdo XAML correspondente do exemplo anterior.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

Observe o uso da propriedade [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) no manipulador anterior. Aqui, **OriginalSource** relata o objeto que acionou o evento. O objeto não poderia ser o [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel), pois o **StackPanel** não é um controle e não pode ter foco. Apenas um dos dois botões dentro de **StackPanel** poderiam ter possivelmente acionado o evento, mas qual deles? Você usa **OriginalSource** para distinguir o objeto de origem do evento real se estiver manipulando o evento em um objeto pai.

### <a name="the-handled-property-in-event-data"></a>A propriedade Handled nos dados do evento

Dependendo da sua estratégia de manipulação de eventos, você pode querer que apenas um manipulador de eventos reaja a um evento propagado. Por exemplo, se você tiver um manipulador [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) específico anexado a um dos controles [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), ele terá a mesma oportunidade de manipular o evento. Nesse caso, talvez você não queira que o painel pai manipule também o evento. Nesse cenário, use a propriedade [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) nos dados do evento.

O objetivo da propriedade [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) em uma classe de dados de eventos roteados é informar que outro manipulador registrado por você anteriormente na rota do evento já atuou. Isso influencia o comportamento do sistema de eventos roteados. Quando você define **Handled** como **true** em um manipulador de eventos, o roteamento desse evento para, e ele não é enviado para os elementos pais sucessivos.

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler e eventos do teclado já manipulados

Você pode usar uma técnica especial para anexar manipuladores que podem atuar em eventos que já estão marcados como manipulados. Essa técnica usa o método [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) para registrar um manipulador, em vez de usar atributos XAML ou sintaxe específica de linguagem para adicionar manipuladores, como + = em C\#.

Uma limitação geral dessa técnica é que a API **AddHandler** obtém um parâmetro do tipo [**RoutedEvent**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEvent), que identifica o evento roteado em questão. Nem todos os eventos roteados fornecem um identificador **RoutedEvent**, e essa consideração afeta os eventos roteados que podem ainda ser manipulados no caso [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled). Os eventos [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) possuem identificadores de eventos roteados ([**KeyDownEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydownevent) e [**KeyUpEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyupevent)) em [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement). Contudo, outros eventos, como [**TextBox.TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged), não possuem identificadores de eventos roteados e, portanto, não podem ser usados com a técnica **AddHandler**.

### <a name="overriding-keyboard-events-and-behavior"></a>Substituindo eventos do teclado e o comportamento

Você pode substituir eventos-chave para controles específicos (como [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)) para fornecer navegação de foco consistente para vários dispositivos de entrada, incluindo teclado e gamepad.

No exemplo a seguir, criamos a subclasse do controle e substituo o comportamento de KeyDown para mover o foco para o conteúdo GridView quando qualquer tecla de seta é pressionada.

```csharp
  public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> Se usar um GridView apenas para layout, considere usar outros controles, como [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) com [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid).

## <a name="commanding"></a>Execução de comandos

Um pequeno número de elementos da interface do usuário fornece suporte interno para comandos. Comandos usam eventos roteados relacionados à entrada na sua implementação subjacente. Eles permitem o processamento de entrada relacionada da interface do usuário, como uma determinada ação do ponteiro ou uma tecla de aceleração específica, invocando um único manipulador de comandos.

Se comandos estiverem disponíveis para um elemento da interface do usuário, considere usar as respectivas APIs de comando em vez de qualquer evento de entrada à parte. Para obter mais informações, consulte [**ButtonBase.Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

Também é possível implementar [**ICommand**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.ICommand) para encapsular a funcionalidade do comando que você invoca de manipuladores de eventos comuns. Isso permite usar comandos mesmo quando não há nenhuma propriedade **Command** disponível.

## <a name="text-input-and-controls"></a>Entrada de texto e controles

Certos controles reagem a eventos do teclado com a sua própria manipulação. Por exemplo, [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) é um controle projetado para capturar e depois representar visualmente o texto que foi inserido com o uso de um teclado. Ele usa [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) e [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) na sua própria lógica, para capturar toques de tecla, depois ativa também seu próprio evento [**TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) caso o texto seja realmente alterado.

Geralmente, você ainda pode adicionar manipuladores para [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) e [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) a uma [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), ou a qualquer controle relacionado feito para processar entrada de texto. Contudo, de acordo com o seu design, um controle pode não responder a todos os valores de teclas a ele direcionados por eventos de teclas. Cada controle tem um comportamento específico.

Como exemplo, [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) (a classe base para [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)) processa [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) para que possa verificar a barra de espaço ou a tecla Enter. **ButtonBase** considera **KeyUp** equivalente a um botão esquerdo do mouse apertado para emitir um evento [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Esse processamento do evento é realizado quando **ButtonBase** substitui o método virtual [**OnKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeyup). Em sua implementação, ele define [**Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) como **true**. O resultado é que qualquer pai de um botão que estiver ouvindo um evento de tecla, no caso de uma barra de espaço, não receberá o evento já manipulado pelos próprios manipuladores.

Outro exemplo é [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Algumas teclas, como as teclas de SETA, não são consideradas texto por **TextBox** e são, em vez disso, consideradas específicas do comportamento da interface do usuário do controle. **TextBox** marca esses casos de evento como manipulados.

Controles personalizados podem implementar o próprio comportamento de substituição para eventos de teclas substituindo [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown) / [**OnKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeyup). Se o seu controle personalizado processa teclas aceleradoras específicas ou tem um comportamento de controle ou foco semelhante ao cenário descrito para [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), você deve colocar essa lógica nas próprias substituições de **OnKeyDown** / **OnKeyUp**.

## <a name="the-touch-keyboard"></a>O teclado virtual

Os controles de entrada de texto oferecem suporte automático para o teclado virtual. Quando o usuário define o foco de entrada como um controle de texto usando a entrada por toque, o teclado virtual aparece automaticamente. Quando o foco de entrada não está em um controle de texto, o teclado virtual é ocultado.

Quando o teclado virtual aparece, ele reposiciona automaticamente sua interface de usuário para garantir que elemento focado permaneça visível. Isso pode fazer com que outras áreas importantes de sua interface de usuário se movam para fora da tela. No entanto, você pode desabilitar o comportamento padrão e fazer seus próprios ajustes de interface de usuário quando o teclado virtual aparece. Para saber mais, veja a [Amostra de teclado virtual](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Se você criar um controle personalizado que exige entrada de texto, mas não deriva de um controle de entrada de texto padrão, será possível adicionar suporte de teclado virtual implementando os padrões corretos de controle de Automação da Interface do Usuário. Para saber mais, veja a [Amostra de teclado virtual](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

O pressionamento das teclas no teclado virtual aciona os eventos [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) e [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) assim como o pressionamento das teclas no teclado físico. No entanto, o teclado virtual não acionará eventos de entrada para Ctrl+A, Ctrl+Z, Ctrl+X, Ctrl+C e Ctrl+V, os quais são reservados para a manipulação de texto no controle de entrada.

Você pode tornar a entrada de dados muito mais rápida e fácil para os usuários em seu aplicativo definindo o escopo de entrada do controle de texto para corresponder ao tipo de dados que o usuário deve inserir. O escopo de entrada oferece uma dica sobre o tipo de entrada de texto esperado pelo controle, para que o sistema possa fornecer um layout de teclado virtual especializado para o tipo de entrada. Por exemplo, se uma caixa de texto for usada somente para a inserção de um PIN de 4 dígitos, defina a propriedade [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) como [**Number**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Isso informa ao sistema para mostrar o layout do teclado numérico, facilitando a inserção do PIN. Para obter mais detalhes, consulte [Usar o escopo de entrada para alterar o teclado virtual](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard).

## <a name="related-articles"></a>Artigos relacionados

**Desenvolvedores**
* [Interações de teclado](keyboard-interactions.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)
* [Responder à presença do teclado de toque](respond-to-the-presence-of-the-touch-keyboard.md)

**Designers**
* [Diretrizes de design do teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)

**Exemplos**
* [Exemplo de teclado de toque](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Exemplo de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Amostra de elementos visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Exemplos de arquivo-morto**
* [Exemplo de entrada](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: exemplo de recursos do dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: exemplo de teclado de toque](https://code.msdn.microsoft.com/windowsapps/Touch-keyboard-sample-43532fda)
* [Respondendo à aparência do exemplo de teclado na tela](https://code.msdn.microsoft.com/windowsapps/keyboard-events-sample-866ba41c)
* [Exemplo de edição de texto XAML](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
 

 
