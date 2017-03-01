---
author: Karl-Bridge-Microsoft
Description: Receba, processe e gerencie dados de entrada de dispositivos apontadores, como toque, mouse, caneta e touchpad, em aplicativos da Plataforma Universal do Windows (UWP).
title: Manusear entrada do ponteiro
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: "caneta, mouse, touchpad, toque, ponteiro, entrada, interação do usuário"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d93583c4d6eeaa8e81bda4672d38386f07e7dcc5
ms.lasthandoff: 02/07/2017

---

# <a name="handle-pointer-input"></a>Manusear entrada do ponteiro
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Receba, processe e gerencie dados de entrada de dispositivos apontadores, como toque, mouse, caneta e touchpad, em aplicativos da Plataforma Universal do Windows (UWP).

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)</li>
<li>[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)</li>
<li>[**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)</li>
</ul>
</div>

**Importante**  
Caso você implemente seu próprio suporte à interação, tenha em mente que os usuários esperam uma experiência intuitiva que envolva a interação direta com os elementos da interface do usuário do aplicativo. Recomendamos que você modele suas interações personalizadas na [Lista de controles](https://msdn.microsoft.com/library/windows/apps/mt185406) para manter tudo consistente e detectável. Os controles de plataforma oferecem a experiência da interação do usuário da Plataforma Universal do Windows (UWP) completa, inclusive interações padrão, efeitos físicos animados, comentários visuais e acessibilidade. Crie interações personalizadas somente se houver uma exigência clara e bem-definida e se as interações básicas não derem suporte a seu cenário.


## <a name="pointers"></a>Ponteiros
Muitas experiências de interação envolvem a identificação do objeto pelo usuário com o qual ele deseja interagir apontando para ele usando dispositivos de entrada, como toque, mouse, caneta e touchpad. Como os dados brutos de dispositivos de interface humana (HID) fornecidos por esses dispositivos de entrada incluem muitas propriedades comuns, as informações são promovidas em uma pilha de entrada unificada e exposta como dados de ponteiro independentes de dispositivo e consolidados. Assim, os aplicativos UWP podem consumir esses dados sem se preocupar com o dispositivo de entrada que está sendo usado.

**Observação**  As informações específicas do dispositivo também são promovidas dos dados brutos de HID, caso o aplicativo exija isso.

 

Cada ponto de entrada (ou contato) na pilha de entrada é representado por um objeto [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) exposto por meio do parâmetro [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) fornecido por vários eventos de ponteiro. No caso de entrada com várias canetas ou multitoque, cada contato é tratado como um ponto de entrada único.

## <a name="pointer-events"></a>Eventos de ponteiro


Os eventos de ponteiro expõem informações básicas, como estado de detecção (em intervalo ou contato) e tipo de dispositivo, além de informações estendidas, como local, pressão e geometria de contato. Além disso, propriedades específicas do dispositivo, como qual botão do mouse um usuário pressionou ou se a ponta da borracha da caneta está sendo usada, também estão disponíveis. Caso o aplicativo precise diferenciar dispositivos de entrada e suas funcionalidades, consulte [Identificar dispositivos de entrada](identify-input-devices.md).

Os aplicativos UWP podem escutar os seguintes eventos de ponteiro:

**Observação**  Chame [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) para restringir a entrada de ponteiro a um elemento da interface do usuário específico. Quando um ponteiro é capturado por um elemento, somente esse objeto recebe os eventos de entrada do ponteiro, mesmo quando o ponteiro é movido para fora da área delimitadora do objeto. Você normalmente captura o ponteiro dentro de um manipulador de eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) como [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) (botão do mouse pressionado, toque ou caneta em contato) deve ser true para que **CapturePointer** seja bem-sucedido.

 

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
<td align="left"><p>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>Ocorre quando um ponteiro é cancelado pela plataforma.</p>
<ul>
<li>Os ponteiros de toque são cancelados quando uma caneta é detectada dentro do intervalo da superfície de entrada.</li>
<li>Um contato ativo não é detectado a mais de 100 ms.</li>
<li>O monitor/tela é alterado (resolução, configurações, configuração multi-mon).</li>
<li>A área de trabalho está bloqueada ou o usuário fez logoff.</li>
<li>O número de contatos simultâneos excedeu o número suportado pelo dispositivo.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>Ocorre quando outro elemento da interface do usuário captura o ponteiro, o ponteiro foi liberado ou outro ponteiro foi capturado programaticamente.</p>
<div class="alert">
<strong>Observação</strong>  Não há evento de captura do ponteiro correspondente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerEntered</strong>](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>Ocorre quando um ponteiro entra na área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo para disparar esse evento, um toque direto no elemento ou uma movimentação para a área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o toque, a caneta dispara esse evento com um toque da caneta no elemento ou uma movimentação para a área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que, quando true, dispara esse evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>Ocorre quando um ponteiro sai da área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo para disparar esse evento quando o ponteiro sai da área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o toque, a caneta dispara esse evento ao sair da área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que dispara esse evento quando o estado muda de true para false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>Ocorre quando um ponteiro muda coordenadas, estado do botão, pressão, inclinação ou geometria de contato (por exemplo, largura e altura) dentro da área delimitadora de um elemento. Isso pode acontecer de maneiras ligeiramente diferentes para entrada de toque, touchpad, mouse e caneta.</p>
<ul>
<li>O toque requer um contato do dedo e só dispara esse evento quando em contato dentro da área delimitadora do elemento.</li>
<li>Mouse e touchpad têm um cursor na tela que está sempre visível e dispara esse evento mesmo caso nenhum botão do mouse ou do touchpad tenha sido pressionado.</li>
<li>Assim como o toque, a caneta dispara esse evento quando em contato dentro da área delimitadora do elemento. No entanto, a caneta também tem um estado de foco ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que, quando true e dentro da área delimitadora do elemento, dispara esse evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>Ocorre quando o ponteiro indica uma ação de pressionar (como um toque para baixo, botão do mouse para baixo, caneta para baixo ou botão de touchpad para baixo) dentro da área delimitadora de um elemento.</p>
<p>[<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918) deve ser chamado no manipulador desse evento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[<strong>PointerReleased</strong>](https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>Ocorre quando o ponteiro indica uma ação de liberação (como um toque para cima, botão do mouse para cima, caneta para cima ou botão de touchpad para cima) dentro da área delimitadora de um elemento ou, caso o ponteiro seja capturado, fora da área delimitadora.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>Ocorre quando a roda do mouse é girada.</p>
<p>A entrada do mouse é associada a um único ponteiro atribuído quando detectada pela primeira vez. Clicar em um botão do mouse (esquerdo, roda ou direito) cria uma associação secundária entre o ponteiro e esse botão por meio do evento [<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970).</p></td>
</tr>
</tbody>
</table>

 

## <a name="example"></a>Exemplo


Aqui estão alguns exemplos de código de um aplicativo de rastreamento básico de ponteiro básico que mostram como escutar e identificar eventos de ponteiro, além de obter várias propriedades de ponteiros ativos.

### <a name="create-the-ui"></a>Criar a interface do usuário

Para este exemplo, usamos um retângulo (`targetContainer`) como o objeto de destino para a entrada de ponteiro. A cor do destino muda quando o status do ponteiro muda.

Os detalhes de cada ponteiro são exibidos em um bloco de texto flutuante que se move com o ponteiro. Os eventos de ponteiro propriamente ditos são exibidos à esquerda do retângulo (para relatar a sequência de eventos).

Este é o Extensible Application Markup Language (XAML) desse exemplo.

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
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
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### <a name="listen-for-pointer-events"></a>Escutar eventos de ponteiro

Na maioria dos casos, recomendamos que você obtenha informações do ponteiro por meio do [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) do manipulador de eventos.

Caso o argumento do evento não exponha os detalhes necessários do evento, você pode acessar informações estendidas de [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) expostas por meio dos métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) e [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

Para este exemplo, usamos um retângulo (`targetContainer`) como o objeto de destino para a entrada de ponteiro. A cor do destino muda quando o status do ponteiro muda.

O código a seguir configura o objeto de destino, declara variáveis globais e identifica os diversos ouvintes de eventos de ponteiro para o destino.

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### <a name="handle-pointer-events"></a>Manipular eventos de ponteiro

Em seguida, usamos comentários da interface do usuário para demonstrar manipuladores de eventos de ponteiro básicos.

-   Esse manipulador gerencia um evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). Adicionamos o evento ao log de eventos, adicionamos o ponteiro à matriz de ponteiros usada para acompanhar os ponteiros de interesse e exibimos os detalhes do ponteiro.

    **Observação**  Os eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) e [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) nem sempre ocorrem em pares. O aplicativo deve escutar e manipular qualquer evento que possa concluir uma ação de ponteiro para baixo (como [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) e [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Esse manipulador gerencia um evento [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968). Adicionamos o evento ao log de eventos, adicionamos o ponteiro à coleção de ponteiros e exibimos os detalhes do ponteiro.

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Esse manipulador gerencia um evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970). Adicionamos o evento ao log de eventos e atualizamos os detalhes do ponteiro.

    **Importante**  A entrada do mouse é associada a um único ponteiro atribuído quando detectada pela primeira vez. Se o usuário clicar em um botão do mouse (esquerdo, roda ou direito), será criada uma associação secundária entre o ponteiro e esse botão por meio do evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). O evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) é disparado somente quando esse mesmo botão do mouse é liberado (nenhum outro botão pode ser associado ao ponteiro até que o evento seja concluído). Em razão dessa associação exclusiva, outros cliques em botões do mouse são roteados por meio do evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970).

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   Esse manipulador gerencia um evento [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973). Adicionamos o evento ao log de eventos, adicionamos o ponteiro à matriz de ponteiros (se necessário) e exibimos os detalhes do ponteiro.

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   Esse manipulador gerencia um evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) em que o contato com o digitalizador é encerrado. Adicionamos o evento ao log de eventos, removemos o ponteiro da coleção de ponteiros e atualizamos os detalhes do ponteiro.

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   Esse manipulador gerencia um evento [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) em que o contato com o digitalizador é mantido. Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Esse manipulador gerencia um evento [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964). Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Esse manipulador gerencia um evento [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965). Adicionamos o evento ao log de eventos, removemos o ponteiro da matriz de ponteiros e atualizamos os detalhes do ponteiro.

    **Observação**  [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) pode ocorrer no lugar de [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972). A captura do ponteiro pode ser perdida por vários motivos.

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### <a name="get-pointer-properties"></a>Obter as propriedades do ponteiro

Conforme mencionado anteriormente, você deve obter as informações do ponteiro mais estendido de um objeto [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) obtidas por meio dos métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) e [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

-   Em primeiro lugar, criamos um novo [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para cada ponteiro.

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   Em seguida, fornecemos uma maneira de atualizar as informações do ponteiro em um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) existente associado a esse ponteiro.

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   Por fim, consultamos várias propriedades de ponteiro.

```    CSharp
         String queryPointer(PointerPoint ptrPt)
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

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### <a name="complete-example"></a>Exemplo completo

O código C\# para este exemplo é mostrado a seguir. Para obter links para amostras mais complexas, consulte os artigos relacionados no final desta página.

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
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

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## <a name="related-articles"></a>Artigos relacionados


**Exemplos**
* [Amostra de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Exemplo do modo de interação do usuário](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo-morto**
* [Entrada: amostra de eventos de entrada do usuário XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: exemplo de funcionalidades do dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de manipulações e gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Entrada: amostra de teste de hit de toque](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: amostra de tinta simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
 

 





