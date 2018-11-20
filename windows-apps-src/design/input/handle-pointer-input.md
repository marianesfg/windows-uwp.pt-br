---
author: Karl-Bridge-Microsoft
Description: Receive, process, and manage input data from pointing devices such as touch, mouse, pen/stylus, and touchpad, in your Universal Windows Platform (UWP) applications.
title: Manipular entrada de ponteiro
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: caneta, mouse, touchpad, toque, ponteiro, entrada, interação do usuário
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ba685f30eb0cf94314996587073a82440cf6c951
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7290039"
---
# <a name="handle-pointer-input"></a>Manusear entrada do ponteiro

Receba, processe e gerencie dados de entrada de dispositivos apontadores (como touch, mouse, caneta e touchpad) nos seus aplicativos da Plataforma Universal do Windows (UWP).

> [!Important]
> Crie interações personalizadas somente se houver uma exigência clara e bem-definida e se as interações com suporte dos controles da plataforma não oferecerem suporte ao seu cenário.  
> Se você personalizar as experiências de interação no seu aplicativo do Windows, os usuários esperam que elas sejam consistentes, intuitivas e detectáveis. Por esses motivos, recomendamos que você modele suas interações personalizadas naquelas com suporte dos [controles da plataforma](../controls-and-patterns/controls-by-function.md). Os controles de plataforma oferecem a experiência da interação do usuário da Plataforma Universal do Windows (UWP) completa, inclusive interações padrão, efeitos físicos animados, feedback visual e acessibilidade. 

## <a name="important-apis"></a>APIs importantes
- [Windows.Devices.Input](https://msdn.microsoft.com/library/windows/apps/br225648)
- [Windows.UI.Input](https://msdn.microsoft.com/library/windows/apps/br208383)
- [Windows.UI.Xaml.Input](https://msdn.microsoft.com/library/windows/apps/br242084)

## <a name="pointers"></a>Ponteiros
A maioria das experiências de interação normalmente envolve a identificação do objeto pelo usuário com o qual ele deseja interagir apontando para ele por meio de dispositivos de entrada, como touch, mouse, caneta e touchpad. Como os dados brutos de dispositivos de interface humana (HID) fornecidos por esses dispositivos de entrada incluem muitas propriedades comuns, os dados são promovidos e consolidados em uma pilha de entrada unificada e expostos como dados de ponteiro independentes de dispositivo. Assim, os aplicativos UWP podem consumir esses dados sem se preocupar com o dispositivo de entrada que está sendo usado.

> [!NOTE]
> As informações específicas do dispositivo também são promovida dos dados brutos de HID, caso o aplicativo as exija.
 

Cada ponto de entrada (ou contato) na pilha de entrada é representado por um objeto [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) exposto por meio do parâmetro [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) nos vários manipuladores de eventos de ponteiro. No caso de entrada com várias canetas ou de multi-touch, cada contato é tratado como um ponteiro de entrada único.

## <a name="pointer-events"></a>Eventos de ponteiro


Os eventos de ponteiro expõem informações básicas, como o tipo de dispositivo de entrada e o estado de detecção (em intervalo ou contato), além de informações estendidas, como localização, pressão e geometria de contato. Além disso, propriedades específicas do dispositivo, como qual botão do mouse um usuário pressionou ou se a ponta da borracha da caneta está sendo usada, também estão disponíveis. Caso o aplicativo precise diferenciar dispositivos de entrada e suas funcionalidades, consulte [Identificar dispositivos de entrada](identify-input-devices.md).

Os aplicativos UWP podem escutar os seguintes eventos de ponteiro:

> [!NOTE]
> Restrinja a entrada de ponteiro a um elemento específico da interface do usuário chamando [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) nesse elemento dentro de um manipulador de eventos de ponteiro. Quando um ponteiro é capturado por um elemento, somente esse objeto recebe os eventos de entrada do ponteiro, mesmo quando o ponteiro é movido para fora da área delimitadora do objeto. O [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) (botão do mouse pressionado, toque ou caneta em contato) deve ser true para que **CapturePointer** seja bem-sucedido.
 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Evento</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208964"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Ocorre quando um ponteiro é cancelado pela plataforma. Isso pode ocorrer nas seguintes circunstâncias:</p>
<ul>
<li>Os ponteiros de toque são cancelados quando uma caneta é detectada dentro do intervalo da superfície de entrada.</li>
<li>Um contato ativo não é detectado a mais de 100 ms.</li>
<li>O monitor/tela é alterado (resolução, configurações, configuração multi-mon).</li>
<li>A área de trabalho está bloqueada ou o usuário fez logoff.</li>
<li>O número de contatos simultâneos excedeu o número suportado pelo dispositivo.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208965"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Ocorre quando outro elemento da interface do usuário captura o ponteiro, o ponteiro é liberado ou outro ponteiro é capturado programaticamente.</p>
<div class="alert">
<strong>Observação</strong>não há nenhum evento de captura do ponteiro correspondente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208968"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Ocorre quando um ponteiro entra na área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo para disparar esse evento, um toque direto no elemento ou uma movimentação para a área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o toque, a caneta dispara esse evento com um toque da caneta no elemento ou uma movimentação para a área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que, quando true, aciona esse evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208969"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Ocorre quando um ponteiro sai da área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo para disparar esse evento quando o ponteiro sai da área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o touch, a caneta aciona esse evento ao sair da área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que aciona esse evento quando o estado muda de true para false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208970"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Ocorre quando um ponteiro muda coordenadas, estado do botão, pressão, inclinação ou geometria de contato (por exemplo, largura e altura) dentro da área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo e só dispara esse evento quando em contato dentro da área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o touch, a caneta aciona esse evento quando em contato dentro da área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que, quando true e dentro da área delimitadora do elemento, aciona esse evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208971"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Ocorre quando o ponteiro indica uma ação de pressionar (como um toque para baixo, botão do mouse para baixo, caneta para baixo ou botão de touchpad para baixo) dentro da área delimitadora de um elemento.</p>
<p>[CapturePointer](https://msdn.microsoft.com/library/windows/apps/br208918) deve ser chamado no manipulador desse evento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208972"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Ocorre quando o ponteiro indica uma ação de liberação (como um toque para cima, botão do mouse para cima, caneta para cima ou botão de touchpad para cima) dentro da área delimitadora de um elemento ou, caso o ponteiro seja capturado, fora da área delimitadora.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208973"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Ocorre quando o botão de rolagem do mouse é girado.</p>
<p>A entrada do mouse é associada a um único ponteiro atribuído quando detectada pela primeira vez. Se o usuário clicar em um botão do mouse (esquerdo, de rolagem ou direito), será criada uma associação secundária entre o ponteiro e esse botão por meio do evento [PointerMoved](https://msdn.microsoft.com/library/windows/apps/br208970).</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Exemplo de evento de ponteiro

Aqui estão alguns trechos de código de um aplicativo de rastreamento de ponteiro básico que mostram como escutar e manipular eventos para vários ponteiros, além de obter várias propriedades para os ponteiros associados.

![Interface do usuário do aplicativo de ponteiro](images/pointers/pointers1.gif)

**Baixe esse exemplo em [Exemplo de entrada de ponteiro (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Criar a interface do usuário

Para este exemplo, usamos um [Rectangle](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.shapes.rectangle) (`Target`) como o objeto consumindo a entrada de ponteiro. A cor do destino muda quando o status do ponteiro muda.

Os detalhes de cada ponteiro são exibidos em um [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) flutuante que segue o ponteiro conforme ele se move. Os eventos de ponteiro propriamente ditos são relatados no [RichTextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) à direita do retângulo.

Este é o Extensible Application Markup Language (XAML) da interface do usuário nesse exemplo. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>Escutar eventos de ponteiro

Na maioria dos casos, recomendamos que você obtenha informações do ponteiro por meio do [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) do manipulador de eventos.

Caso o argumento do evento não exponha os detalhes necessários do evento, você pode acessar informações estendidas de [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) expostas por meio dos métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) e [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

O código a seguir configura o objeto de dicionário global para rastrear cada ponteiro ativo e identifica os diversos ouvintes de eventos de ponteiro para o objeto de destino.

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>Manipular eventos de ponteiro

Em seguida, usamos comentários da interface do usuário para demonstrar manipuladores de eventos de ponteiro básicos.

-   Esse manipulador gerencia o evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). Adicionamos o evento ao log de eventos, o ponteiro ao dicionário de ponteiros ativos e exibimos os detalhes do ponteiro.

    > [!NOTE]
    > Os eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) e [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) nem sempre ocorrem em pares. O aplicativo deve escutar e manipular qualquer evento que possa concluir um ponteiro para baixo (como [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) e [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).      

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968). Adicionamos o evento ao log de eventos, o ponteiro à coleção de ponteiros e exibimos os detalhes do ponteiro.

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970). Adicionamos o evento ao log de eventos e atualizamos os detalhes do ponteiro.

    > [!Important]
    > A entrada do mouse é associada a um único ponteiro atribuído quando detectada pela primeira vez. Se o usuário clicar em um botão do mouse (esquerdo, roda ou direito), será criada uma associação secundária entre o ponteiro e esse botão por meio do evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). O evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) é disparado somente quando esse mesmo botão do mouse é liberado (nenhum outro botão pode ser associado ao ponteiro até que o evento seja concluído). Devido a essa associação exclusiva, outros cliques em botões do mouse são roteados por meio do evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970).     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973). Adicionamos o evento ao log de eventos, o ponteiro à matriz de ponteiros (se necessário) e exibimos os detalhes do ponteiro.

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) onde o contato com o digitalizador é encerrado. Adicionamos o evento ao log de eventos, removemos o ponteiro da coleção de ponteiros e atualizamos os detalhes do ponteiro.

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   Esse manipulador gerencia o evento [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) (quando o contato com o digitalizador é mantido). Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964). Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   Esse manipulador gerencia o evento [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965). Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

    > [!NOTE]
    > [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) pode ocorrer no lugar de [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972). A captura do ponteiro pode ser perdida por vários motivos, incluindo interação do usuário, captura programática de outro ponteiro, chamando [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972).     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>Obter as propriedades do ponteiro

Conforme mencionado anteriormente, você deve obter as informações do ponteiro mais estendido de um objeto [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) obtidas por meio dos métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) e [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076). Os trechos de código a seguir mostram como.

-   Em primeiro lugar, criamos um novo [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para cada ponteiro.

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   Em seguida, fornecemos uma maneira de atualizar as informações do ponteiro em um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) existente associado a esse ponteiro.

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   Por fim, consultamos várias propriedades de ponteiro.

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>Ponteiro principal
Alguns dispositivos de entrada, como um digitalizador de toque ou touchpad, oferecem suporte a mais do que o típico ponteiro único de um mouse ou uma caneta (na maioria dos casos como o Surface Hub oferece suporte a duas entradas à caneta). 

Use a propriedade somente leitura **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** da classe **[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)** para identificar e diferenciar um único ponteiro principal (o ponteiro principal é sempre o primeiro a ser detectado durante uma sequência de entrada). 

Identificando o ponteiro principal, você pode usá-lo para emular o mouse ou a entrada à caneta, personalizar as interações ou fornecer alguma outra funcionalidade específica ou interface do usuário.

> [!NOTE]
> Se o ponteiro principal for liberado, cancelado ou perdido durante uma sequência de entrada, um ponteiro de entrada principal não será criado até que uma nova sequência de entrada seja iniciada (uma sequência de entrada termina quando todos os ponteiros tiverem sido liberados, cancelados ou perdidos).

## <a name="primary-pointer-animation-example"></a>Exemplo de animação do ponteiro principal

Esses trechos de código mostram como você pode fornecer feedback visual especial para ajudar um usuário a diferenciar entre entradas de ponteiro no seu aplicativo.

Este aplicativo específico usa cor e animação para realçar o ponteiro principal.

![Aplicativo de ponteiro com feedback visual animado](images/pointers/pointers-usercontrol-animation.gif)

**Baixar esse exemplo em [Exemplo de entrada de ponteiro (UserControl com animação)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Feedback visual

Definimos um **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)**, com base em um objeto XAML **[Ellipse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)**, que destaca onde cada ponteiro está na tela e usa um **[Storyboard](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.animation.storyboard)** para animar a elipse que corresponde ao ponteiro principal.

**Aqui está o XAML:**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

E aqui está o code-behind:
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>Criar a interface do usuário
A interface do usuário neste exemplo limita-se à entrada **[Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)** onde rastreamos qualquer ponteiro e renderizamos os indicadores de ponteiro e a animação do ponteiro principal (se aplicável), juntamente com uma barra de cabeçalho contendo um contador de ponteiro e um identificador de ponteiro principal.

Aqui está o MainPage.xaml:

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>Manipular eventos de ponteiro

Por fim, definimos os manipuladores de eventos de ponteiro básicos no code-behind MainPage.xaml.cs. Não reproduziremos o código aqui, pois os conceitos básicos foram abordados no exemplo anterior, mas você pode baixar o exemplo de trabalho em [Exemplo de entrada de ponteiro (UserControl com animação)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip).

## <a name="related-articles"></a>Artigos relacionados

**Exemplos de tópico**
* [Exemplo de entrada de ponteiro (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
* [Exemplo de entrada de ponteiro (UserControl com animação)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

**Outros exemplos**
* [Exemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de manipulações e gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Entrada: amostra de teste de hit de toque](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)