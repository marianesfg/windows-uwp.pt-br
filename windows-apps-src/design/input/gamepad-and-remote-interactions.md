---
Description: Otimize seu aplicativo para a entrada do Xbox gamepad e controle remoto.
title: Interações de Gamepad e de controle remoto
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1292051362b9751d41b530f6b47f226d36228252
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592801"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interações de Gamepad e de controle remoto

![imagem de teclado e gamepad](images/keyboard/keyboard-gamepad.jpg)

***Muitas experiências de interação são compartilhadas entre o teclado, gamepad e controle remoto***

Criar experiências de interação em seus aplicativos de plataforma Universal do Windows (UWP) que garantem que seu aplicativo é útil e acessível por meio de ambos os tradicionais entrados tipos de computadores, laptops e tablets (mouse, teclado, toque e assim por diante), bem como os tipos de entrada típico da TV e Xbox *10 pés* experiência, como o controle remoto e gamepad.

Ver [projetar para Xbox e TV](../devices/designing-for-tv.md) para obter diretrizes de design geral nos aplicativos UWP na *10 pés* experiência.

## <a name="overview"></a>Visão geral

Neste tópico, discutiremos o que você deve considerar em seu design de interação (ou o que você não fizer isso, se a plataforma parecer depois dela para você) e fornecer diretrizes, recomendações e sugestões para a criação de aplicativos da UWP que são agradáveis de usar, independentemente de dispositivo, tipo de entrada, ou habilidades de usuário e preferências.

Linha inferior, seu aplicativo deve ser tão intuitiva e fácil de usar na *2 pés* ambiente como ela está no *10 pés* ambiente (e vice-versa). Suporte a dispositivos preferencial do usuário, possibilitam a interface do usuário se concentrar claras e inequívocas, organizar o conteúdo para que navegação seja consistente e previsível e fornecer aos usuários o caminho mais curto para o que eles desejam.

> [!NOTE]
> A maioria dos trechos de código deste tópico está em XAML/C#; entretanto, os princípios e conceitos se aplicam a todos os aplicativos UWP. Se você estiver desenvolvendo um aplicativo UWP HTML/JavaScript para Xbox, confira a excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) no GitHub.


## <a name="optimize-for-both-2-foot-and-10-foot-experiences"></a>Otimizar para experiências de pé 2 e 10 pés

No mínimo, recomendamos que você teste seus aplicativos para garantir que eles funcionem bem em cenários de pé 2 e 10 pés, e que toda a funcionalidade é detectável e acessível para o Xbox [gamepad e controle remoto](#gamepad-and-remote-control).

Aqui estão algumas outras maneiras de otimizar seu aplicativo para uso em experiências de pé 2 e 10 pés e com entrada de todos os dispositivos (cada links para a seção apropriada neste tópico).

> [!NOTE]
> Como gamepads Xbox e controles remotos do suporte a vários comportamentos de teclado do UWP e experiências, essas recomendações são apropriadas para ambos os tipos de entrada. Ver [interações de teclado](keyboard-interactions.md) para obter mais informações sobre o teclado.

| Recurso        | Descrição           |
| -------------------------------------------------------------- |--------------------------------|
| [Interação e navegação de foco XY](#xy-focus-navigation-and-interaction) | **Navegação de foco XY** permite que o usuário navegar em torno da interface do usuário do seu aplicativo. No entanto, isso limita o usuário a navegar para cima, para baixo, para a esquerda e direita. Recomendações para lidar com isso e outras considerações são descritas nesta seção. |
| [Modo do mouse](#mouse-mode)|Navegação de foco XY não é prático, ou até mesmo possível, para alguns tipos de aplicativos, como mapas ou de desenho e pintura de aplicativos. Nesses casos, **modo do mouse** permite que os usuários navegar livremente com um gamepad ou o controle remoto, assim como um mouse em um PC.|
| [Visual de foco](#focus-visual)  | O visual de foco é uma borda que realça o elemento focalizado no momento da interface do usuário. Isso ajuda o usuário a identificar rapidamente a interface do usuário são navegar pelos ou interagir com.  |
| [Contrato de foco](#focus-engagement) | Compromisso com o foco exige que o usuário pressionar o **um/selecionar** botão em um gamepad ou o controle remoto quando um elemento de interface do usuário tem o foco para interagir com ele. |
| [Botões de hardware](#hardware-buttons) | O controle remoto e gamepad fornecem configurações e botões muito diferentes. |

## <a name="gamepad-and-remote-control"></a>Gamepad e controle remoto

Assim como o teclado e o mouse estão para o computador e o touch está para o telefone e o tablet, o gamepad e o controle remoto são os principais dispositivos de entrada para a experiência de 3 metros.
Esta seção apresenta os botões de hardware e o que eles fazem.
Em [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction) e [Modo de mouse](#mouse-mode), você aprenderá como otimizar seu aplicativo ao usar esses dispositivos de entrada.

A qualidade do comportamento do gamepad e do controle remoto que você obtém de imediato depende do nível de suporte ao teclado em seu aplicativo. Uma boa maneira de garantir que seu aplicativo funcione bem com gamepad/controle remoto é se certificar de que ele funcione bem com o teclado no computador e, seguida, testá-lo com gamepad/controle remoto para encontrar pontos fracos na sua interface do usuário.

### <a name="hardware-buttons"></a>Botões de hardware

Neste documento, os botões serão chamada pelos nomes fornecidos no diagrama a seguir.

![Diagrama de botões do controle remoto e do gamepad](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

Como você pode ver no diagrama, há alguns botões que são compatíveis com o gamepad mas não com o controle remoto, e vice-versa. Embora você possa usar botões compatíveis apenas com um dispositivo de entrada para tornar a navegação da interface do usuário mais rápida, lembre-se de que usá-los para interações críticas pode criar uma situação em que o usuário não consegue interagir com determinadas partes da interface do usuário.

A tabela a seguir lista todos os botões de hardware compatíveis com aplicativos UWP e qual dispositivo de entrada que é compatível com eles.

| Botão                    | Gamepad   | Controle remoto    |
|---------------------------|-----------|-------------------|
| Botão A/Selecionar           | Sim       | Sim               |
| Botão B/Voltar             | Sim       | Sim               |
| Direcional (D-pad)   | Sim       | Sim               |
| Botão Menu               | Sim       | Sim               |
| Botão Exibir               | Sim       | Sim               |
| Botões X e Y           | Sim       | Não                |
| Joystick esquerdo                | Sim       | Não                |
| Joystick direito               | Sim       | Não                |
| Gatilhos para esquerda e direita   | Sim       | Não                |
| Complementos para esquerda e direita    | Sim       | Não                |
| Botão OneGuide           | Não        | Sim               |
| Botão de volume             | Não        | Sim               |
| Botão Canal            | Não        | Sim               |
| Botões de controle de mídia     | Não        | Sim               |
| Botão Mudo               | Não        | Sim               |

### <a name="built-in-button-support"></a>Suporte interno para botões

A UWP mapeia automaticamente o comportamento existente de entrada do teclado para a entrada do gamepad e do controle remoto. A tabela a seguir lista esses mapeamentos internos.

| Teclado              | Gamepad/controle remoto                        |
|-----------------------|---------------------------------------|
| Teclas de direção            | D-pad (também joystick esquerdo no gamepad)    |
| Barra de espaço              | Botão A/Selecionar                       |
| Enter                 | Botão A/Selecionar                       |
| Escape                | Botão B/Voltar*                        |

\*Quando nenhuma a [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) nem [KeyUp](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) eventos para o botão B são manipulados pelo aplicativo, o [SystemNavigationManager.BackRequested](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) evento será disparado, que deve resulta em volta navegação dentro do aplicativo. No entanto, você precisa implementar isso por conta própria, como no seguinte trecho de código:

```csharp
// This code goes in the MainPage class

public MainPage()
{
    this.InitializeComponent();

    // Handling Page Back navigation behaviors
    SystemNavigationManager.GetForCurrentView().BackRequested +=
        SystemNavigationManager_BackRequested;
}

private void SystemNavigationManager_BackRequested(
    object sender,
    BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = this.BackRequested();
    }
}

public Frame AppFrame { get { return this.Frame; } }

private bool BackRequested()
{
    // Get a hold of the current frame so that we can inspect the app back stack
    if (this.AppFrame == null)
        return false;

    // Check to see if this is the top-most page on the app back stack
    if (this.AppFrame.CanGoBack)
    {
        // If not, set the event to handled and go back to the previous page in the
        // app.
        this.AppFrame.GoBack();
        return true;
    }
    return false;
}
```

> [!NOTE]
> Se o botão B é usado para voltar, então não exiba um botão Voltar na interface do usuário. Se você estiver usando um [Modo de exibição de navegação](../controls-and-patterns/navigationview.md), o botão Voltar ficará oculto automaticamente. Para obter mais informações sobre navegação regressiva, consulte [Histórico de navegação e navegação regressiva para aplicativos UWP](../basics/navigation-history-and-backwards-navigation.md).

Os aplicativos UWP no Xbox One também oferecem suporte ao pressionamento do botão **Menu** para abrir menus de contexto. Para obter mais informações, consulte [CommandBar e ContextFlyout](#commandbar-and-contextflyout).

### <a name="accelerator-support"></a>Suporte a acelerador

Botões aceleradores são aqueles que podem ser usados para acelerar a navegação por meio de uma interface do usuário. No entanto, esses botões podem ser exclusivos para um determinado dispositivo de entrada, sendo assim, tenha em mente que nem todos os usuários poderão usar essas funções. Na verdade, o gamepad é atualmente o único dispositivo de entrada que dá suporte a funções de acelerador para aplicativos UWP no Xbox One.

A tabela a seguir lista o suporte a acelerador incorporado à UWP, bem como o que você pode implementar por conta própria. Utilize esses comportamentos em sua interface do usuário personalizada para oferecer uma experiência de usuário consistente e amigável.

| Interação   | Teclado/Mouse   | Gamepad      | Incorporado em:  | Recomendado para: |
|---------------|------------|--------------|----------------|------------------|
| Page up/Page down  | Page up/Page down | Gatilhos esquerdo/direito | [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Exibições que dão suporte à rolagem vertical
| Página esquerda/direita | Nenhuma | Botões superiores esquerdo/direito | [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox), [ListViewBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListViewBase), [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), `ScrollViewer`, [Selector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Selector), [LoopingSelector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.LoopingSelector), [FlipView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView) | Exibições que dão suporte à rolagem horizontal
| Ampliar/reduzir        | Ctrl +/- | Gatilhos esquerdo/direito | Nenhuma | `ScrollViewer`, modos de exibição que dão suporte e menos zoom |
| Abrir/fechar painel de navegação | Nenhuma | Exibir | Nenhuma | Painéis de navegação |
| [Pesquisa](#search-experience) | Nenhuma | Botão Y | Nenhuma | Atalho para a função de pesquisa principal no aplicativo |
| [Abrir menu de contexto](#commandbar-and-contextflyout) | Clique com o botão direito do mouse | Botão Menu | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menus de contexto |

## <a name="xy-focus-navigation-and-interaction"></a>Interação e navegação de foco do plano XY

Se o seu aplicativo dá suporte à navegação de foco adequada para teclado, isso será convertido corretamente para o gamepad e o controle remoto.
A navegação com as teclas de direção é mapeada para o **D-pad** (bem como o **joystick esquerdo** no gamepad), e a interação com os elementos de interface do usuário é mapeada para a tecla **Enter/Select** (consulte [Gamepad e controle remoto](#gamepad-and-remote-control)).

Muitos eventos e propriedades são usados pelo teclado e pelo gamepad &mdash; ambos disparam os eventos `KeyDown` e `KeyUp`, e ambos navegarão apenas para controles que tenham as propriedades `IsTabStop="True"` e `Visibility="Visible"`. Para obter diretrizes de design de teclado, consulte [Interações de teclado](../input/keyboard-interactions.md).

Se o suporte ao teclado for implementado corretamente, seu aplicativo funcionará razoavelmente bem; no entanto, pode haver algum trabalho extra necessário para dar suporte a cada cenário. Pense em necessidades específicas do seu aplicativo para oferecer a melhor experiência possível ao usuário.

> [!IMPORTANT]
> O modo de mouse é habilitado por padrão para aplicativos UWP executados no Xbox One. Para desabilitar o modo de mouse e habilitar a navegação de foco do plano XY, defina `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Depurando problemas de foco

O método [FocusManager.GetFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) informará qual elemento tem o foco no momento. Isso é útil para situações em que a localização do foco visual talvez não seja óbvia. Você pode registrar essa informação na janela de saída do Visual Studio da seguinte forma:

```csharp
page.GotFocus += (object sender, RoutedEventArgs e) =>
{
    FrameworkElement focus = FocusManager.GetFocusedElement() as FrameworkElement;
    if (focus != null)
    {
        Debug.WriteLine("got focus: " + focus.Name + " (" +
            focus.GetType().ToString() + ")");
    }
};
```

Existem três motivos comuns para a navegação do plano XY não funcionar da maneira esperada:

* A propriedade [IsTabStop](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) ou [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) está definida incorretamente.
* O controle que obtém o foco é na verdade maior que você imagina &mdash; a navegação do plano XY examina o tamanho total do controle ([ActualWidth](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) e [ActualHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)), não apenas a parte do controle que renderiza algo interessante.
* Um controle focalizável está por cima de outro &mdash; a navegação do plano XY não dá suporte a controles que estão sobrepostos.

Se a navegação do plano XY ainda não estiver funcionando da maneira esperada após a correção desses problemas, você poderá apontar manualmente para o elemento que deve receber o foco usando o método descrito em [Substituindo a navegação padrão](#overriding-the-default-navigation).

Se a navegação do plano XY estiver funcionando conforme o esperado, mas nenhum foco visual for exibido, a causa poderá ser uma das seguintes:

* Você remodelou o controle e não incluiu um foco visual. Defina `UseSystemFocusVisuals="True"` ou adicione um foco visual manualmente.
* Você moveu o foco chamando `Focus(FocusState.Pointer)`. O parâmetro [FocusState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FocusState) controla o que acontece com o foco visual. Em geral, isso deve ser definido como `FocusState.Programmatic`, que mantém o foco visual visível se ele estava visível antes, e o oculta se ele estava oculto antes.

O restante desta seção entra em detalhes sobre os desafios de design comuns ao usar a navegação do plano XY e oferece várias maneiras de resolvê-los.

### <a name="inaccessible-ui"></a>Interface do usuário inacessível

Como a navegação de foco do plano XY limita o usuário a se mover para cima, para baixo, para a esquerda e para a direita, você pode se deparar com cenários onde partes da interface do usuário são inacessíveis.
O diagrama a seguir ilustra um exemplo do tipo de layout de interface do usuário que não dá suporte à navegação de foco do plano XY.
Observe que o elemento do meio não é acessível com o uso do gamepad/controle remoto porque a navegação vertical e horizontal será priorizada e o elemento do meio nunca terá prioridade alta o suficiente para receber o foco.

![Elementos nos quatro cantos com elemento inacessível no meio](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

Se por algum motivo a reorganização da interface do usuário não for possível, use uma das técnicas discutidas na próxima seção para substituir o comportamento de foco padrão.

### <a name="overriding-the-default-navigation"></a>Substituindo a navegação padrão

Embora a Plataforma Universal do Windows tente garantir que a navegação com D-pad/joystick esquerdo faça sentido para o usuário, ela não pode garantir que o comportamento seja otimizado para a finalidade do seu aplicativo.
A melhor maneira de garantir que a navegação seja otimizada para o seu aplicativo é testá-lo com um gamepad e confirmar se cada elemento da interface do usuário pode ser acessado pelo usuário de forma que faça sentido nos cenários do seu aplicativo. Caso os cenários do seu aplicativo necessitem de um comportamento não conseguido através da navegação de foco do plano XY fornecida, considere seguir as recomendações das seções a seguir e/ou substituir o comportamento para colocar o foco em um item lógico.

O trecho de código a seguir mostra como você pode substituir o comportamento de navegação de foco do plano XY:

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft"
            Content="Search" />
    <Button x:Name="MyBtnRight"
            Content="Delete"/>
    <Button x:Name="MyBtnTop"
            Content="Update" />
    <Button x:Name="MyBtnDown"
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}"
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

Neste caso, quando o foco estiver no botão `Home` e o usuário navegar para a esquerda, o foco se moverá para o botão `MyBtnLeft`; se o usuário navegar para a direita, o foco se moverá para o botão `MyBtnRight` botão; e assim por diante.

Para impedir que o foco se mova de um controle em uma determinada direção, use a propriedade `XYFocus*` para apontá-lo para o mesmo controle:

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

Usando essas propriedades `XYFocus`, um controle pai também pode forçar a navegação de seus filhos quando o próximo candidato de foco estiver fora de sua árvore visual, a menos que o filho que tem o foco use a mesma propriedade `XYFocus`.

```xml
<StackPanel Orientation="Horizontal" Margin="300,300">
    <UserControl XYFocusRight="{x:Bind ButtonThree}">
        <StackPanel>
            <Button Content="One"/>
            <Button Content="Two"/>
        </StackPanel>
    </UserControl>
    <StackPanel>
        <Button x:Name="ButtonThree" Content="Three"/>
        <Button Content="Four"/>
    </StackPanel>
</StackPanel>
```

No exemplo acima, se o foco estiver no `Button` dois e o usuário navegar para a direita, o melhor candidato de foco será `Button` quatro. No entanto, o foco é movido para `Button` três porque o `UserControl` pai força a navegar até lá quando está fora da sua árvore visual.

### <a name="path-of-least-clicks"></a>Caminho de menos cliques

Tente permitir que o usuário execute as tarefas mais comuns com o menor número de cliques. No exemplo a seguir, o [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) é colocado entre o botão **Reproduzir** (que inicialmente recebe o foco) e um elemento comumente usado, sendo assim, um elemento desnecessário seja colocado entre as tarefas prioritárias.

![As práticas recomendadas de navegação fornecem caminho com menos cliques](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

No exemplo a seguir, o [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) é colocado acima do botão **Reproduzir** em vez disso.
Simplesmente reorganizar a interface do usuário para que elementos desnecessários não sejam colocados entre tarefas prioritárias melhorará consideravelmente a usabilidade do seu aplicativo.

![TextBlock movido para cima do botão Reproduzir para que não fique mais entre tarefas prioritárias](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar e ContextFlyout

Ao usar um [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar), lembre-se o problema de percorrer uma lista, conforme mencionado na [problema: Elementos de interface do usuário localizada após rolagem longa lista/grade](#problem-ui-elements-located-after-long-scrolling-list-grid). A imagem a seguir mostra um layout de interface do usuário com o `CommandBar` na parte inferior de uma lista/grade. O usuário precisa rolar até o final da lista/grade para acessar o elemento `CommandBar`.

![CommandBar na parte inferior da lista/grade](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

E se você colocar o `CommandBar` *acima* lista/grade? Embora o usuário que rolou a lista/grade para baixo tenha que rolar de volta para cima para acessar o `CommandBar`, haverá um pouco menos de navegação do que na configuração anterior. Observe que isso pressupõe que o foco inicial do seu aplicativo seja colocado ao lado ou acima do `CommandBar`; Essa abordagem não funcionará bem se o foco inicial está abaixo da lista/grade. Se esses itens de `CommandBar` forem itens de ação global que não precisem ser acessados com frequência (como um botão **Sincronizar**), talvez seja aceitável tê-los acima da lista/grade.

Embora você não possa empilhar itens de uma `CommandBar`verticalmente, colocá-los contra a direção de rolagem (por exemplo, para a esquerda ou direita de uma lista de rolagem vertical, ou para cima ou para baixo de uma lista de rolagem horizontal) é outra opção que você poderá considerar se ela funcionar bem no layout de sua interface do usuário.

Se o seu aplicativo tiver uma `CommandBar` cujos itens precisam ser prontamente acessados pelos usuários, considere colocar esses itens dentro de uma propriedade [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) e remova-os da `CommandBar`. `ContextFlyout` é uma propriedade de [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) e é a [menu de contexto](../controls-and-patterns/dialogs-and-flyouts/index.md) associado a esse elemento. No computador, quando você clica em um elemento com `ContextFlyout`, esse menu de contexto é aberto. No Xbox One, isso acontecerá quando você pressionar o botão **Menu** enquanto o foco estiver em um elemento.

### <a name="ui-layout-challenges"></a>Desafios de layout de interface do usuário

Alguns layouts de interface do usuário são mais desafiadores devido à natureza da navegação de foco do plano XY e devem ser avaliados caso a caso. Embora não haja uma única maneira "correta", e a solução que você escolher depende das necessidades específicas do seu aplicativo, existem algumas técnicas que você pode utilizar para criar uma ótima experiência de TV.

Para entender isso melhor, vamos examinar um aplicativo imaginário que ilustra alguns desses problemas, e as técnicas para superá-los.

> [!NOTE]
> Esse aplicativo fictício serve para ilustrar problemas de interface do usuário e as possíveis soluções para eles, e não se destina a mostrar a melhor experiência de usuário para seu aplicativo específico.

A seguir há um aplicativo de imóveis imaginário que mostra uma lista de imóveis disponíveis para venda, um mapa, uma descrição de uma propriedade e outras informações. Esse aplicativo apresenta três desafios que você pode solucionar usando as seguintes técnicas:

- [Reorganizar de interface do usuário](#ui-rearrange)
- [Contrato de foco](#engagement)
- [Modo do mouse](#mouse-mode)

![Aplicativo de imóveis fictício](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problema: Elementos de interface do usuário localizados após rolagem longa lista/grade <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

O [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) de propriedades mostradas na imagem a seguir é uma lista de rolagem muito longa. Se o [envolvimento](#focus-engagement)*não* for necessária no `ListView`, quando o usuário navegar para a lista, o foco será colocado no primeiro item da lista. Para o usuário acessar o botão **Anterior** ou **Próximo**, ele deve passar por todos os itens da lista. Em casos assim, nos quais exigir que o usuário percorra a lista inteira é trabalhoso, ou seja, quando a lista não é curta o suficiente para essa experiência ser aceitável, talvez você queira considerar outras opções.

![Aplicativo de imóveis: a lista de 50 itens leva 51 cliques para alcançar os botões abaixo](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Soluções

**Reorganizar de interface do usuário <a name="ui-rearrange"></a>**

A menos que o foco inicial seja colocado na parte inferior da página, os elementos de interface do usuário colocados acima de uma lista de rolagem longa são normalmente mais acessíveis que se forem colocados abaixo.
Se esse novo layout funciona para outros dispositivos, alterar o layout para todas as famílias de dispositivos em vez de fazer alterações especiais na interface do usuário apenas para o Xbox One pode ser uma abordagem mais barata.
Além disso, colocar os elementos de interface do usuário contra a direção de rolagem (ou seja, na horizontal em uma lista de rolagem vertical, ou na vertical em uma lista de rolagem horizontal) tornará a acessibilidade ainda melhor.

![Aplicativo de imóveis: colocar botões acima da lista de rolagem longa](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Contrato de foco <a name="engagement"></a>**

Quando o envolvimento é *necessário*, todo o `ListView` se torna um destino de foco único. O usuário poderá ignorar o conteúdo da lista para acessar o próximo elemento focalizável. Leia mais sobre quais controles oferecem suporte a envolvimento e como usá-los em [Envolvimento de foco](#focus-engagement).

![Aplicativo de imóveis: definir o envolvimento como necessário para que leve apenas 1 clique para acessar os botões Anterior/Próximo](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problema: ScrollViewer sem nenhum elemento focalizável

Como a navegação de foco do plano XY depende de navegar para um elemento de interface do usuário focalizável ao mesmo tempo, um [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) que não contém elementos focalizáveis (tal como um elemento apenas com texto, como neste exemplo) pode criar um cenário em que o usuário não consegue exibir todo o conteúdo no `ScrollViewer`.
Para obter soluções para esse e outros cenários relacionados, veja [Envolvimento de foco](#focus-engagement).

![Aplicativo de imóveis: ScrollViewer com apenas texto](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problema: Interface do usuário livre de rolagem

Quando seu aplicativo requer uma interface de usuário de rolagem livre, como uma superfície de desenho ou, neste exemplo, um mapa, a navegação de foco do plano XY simplesmente não funciona.
Nesses casos, você pode ativar o [modo de mouse](#mouse-mode) para permitir que o usuário navegue livremente dentro de um elemento de interface do usuário.

![Mapear elemento de interface do usuário usando o modo de mouse](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Modo de mouse

Conforme descrito em [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction), o foco no Xbox One é movido por meio de um sistema de navegação do plano XY, permitindo que o usuário mude o foco de controle para controle movendo-se para cima, para baixo, para a esquerda e para a direita.
No entanto, alguns controles, como [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) e [MapControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), exigem uma interação de mouse na qual os usuários podem mover livremente o ponteiro dentro dos limites do controle.
Há também alguns aplicativos nos quais faz sentido para o usuário poder mover o ponteiro em toda a página, ter uma experiência com gamepad/controle remoto semelhante ao que os usuários podem encontrar em um computador com o mouse.

Para esses cenários, você deve solicitar um ponteiro (modo de mouse) para a página inteira ou em um controle dentro de uma página.
Por exemplo, seu aplicativo pode ter uma página que tem um controle `WebView` que usa o modo de mouse somente enquanto está dentro do controle, e navegação de foco do plano XY em todos os outros locais.
Para solicitar um ponteiro, você pode especificar que o deseja **quando uma página ou controle está envolvido** ou **quando uma página tem o foco**.

> [!NOTE]
> Não há suporte para a solicitação de um ponteiro quando um controle recebe o foco.

Para aplicativos Web hospedados e XAML executados no Xbox One, o modo de mouse está ativado por padrão para o aplicativo inteiro. É altamente recomendável que você desative essa opção e otimize seu aplicativo para navegação do plano XY. Para fazer isso, defina a propriedade `Application.RequiresPointerMode` como `WhenRequested` para habilitar o modo de mouse somente quando um controle ou uma página o solicitar.

Para fazer isso em um app XAML, use o código a seguir em sua classe `App`:

```csharp
public App()
{
    this.InitializeComponent();
    this.RequiresPointerMode =
        Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Para obter mais informações, incluindo o código de exemplo para HTML/JavaScript, consulte [Como desabilitar o modo de mouse](../../xbox-apps/how-to-disable-mouse-mode.md).

O diagrama a seguir mostra os mapeamentos de botões para gamepad/controle remoto no modo de mouse.

![Mapeamentos de botões para gamepad/controle remoto no modo de mouse](images/designing-for-tv/10ft_infographics_mouse-mode.png)

> [!NOTE]
> O modo de mouse só tem suporte no Xbox One com gamepad/controle remoto. Em outras famílias de dispositivos e tipos de entrada ele é silenciosamente ignorado.

Use a propriedade [RequiresPointer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.requirespointer) em um controle ou uma página para ativar o modo de mouse no item. Essa propriedade tem três valores possíveis: `Never` (o valor padrão), `WhenEngaged` e `WhenFocused`.

### <a name="activating-mouse-mode-on-a-control"></a>Ativando o modo de mouse em um controle

Quando o usuário vincula um controle a `RequiresPointer="WhenEngaged"`, o modo de mouse é ativado no controle até o usuário desvinculá-lo. O trecho de código a seguir demonstra um `MapControl` simples que ativa o modo de mouse quando ativado:

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true"
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page>
```

> [!NOTE]
> Se um controle ativar o modo de mouse quando ativado, ele também deverá exigir a interação com `IsEngagementRequired="true"`; caso contrário, o modo de mouse nunca será ativado.

Quando um controle está no modo de mouse, seus controles aninhados estarão no modo de mouse também. O modo solicitado de seus filhos será ignorado; é impossível que um pai esteja no modo de mouse, mas um filho não esteja.

Além disso, o modo solicitado de um controle é inspecionado apenas quando ele recebe o foco, para que o modo não mude dinamicamente enquanto ele tem foco.

### <a name="activating-mouse-mode-on-a-page"></a>Ativando o modo de mouse em uma página

Quando uma página tem a propriedade `RequiresPointer="WhenFocused"`, o modo de mouse será ativado para a página inteira quando ela receber foco. O trecho de código a seguir demonstra como fornecer essa propriedade a uma página:

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page>
```

> [!NOTE]
> O valor `WhenFocused` tem suporte apenas em objetos [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page). Se você tentar definir esse valor em um controle, será gerada uma exceção.

### <a name="disabling-mouse-mode-for-full-screen-content"></a>Desativando o modo de mouse para conteúdo de tela inteira

Geralmente, ao exibir vídeos ou outros tipos de conteúdo em tela inteira, você deseja ocultar o cursor porque ele pode distrair o usuário. Esse cenário ocorre quando o resto do aplicativo usa o modo de mouse, mas você deseja desativá-lo ao mostrar conteúdo de tela inteira. Para fazer isso, coloque o conteúdo de tela inteira em seu próprio `Page` e siga as etapas abaixo.

1. No objeto `App`, defina `RequiresPointerMode="WhenRequested"`.
2. Em cada objeto `Page`, *exceto* para a tela inteira `Page`, defina `RequiresPointer="WhenFocused"`.
3. Para a `Page` em tela inteira, defina `RequiresPointer="Never"`.

Dessa forma, o cursor nunca aparecerá ao mostrar conteúdo de tela inteira.

## <a name="focus-visual"></a>Foco visual

O foco visual é a borda em torno do elemento de interface do usuário que tem o foco no momento. Isso ajuda a orientar o usuário para que ele possa navegar facilmente em sua interface do usuário sem se perder.

Com uma atualização visual e várias opções de personalização adicionadas ao foco visual, os desenvolvedores podem confiar que um único foco visual funcionará bem em computadores e no Xbox One, bem como em quaisquer outros dispositivos Windows 10 que dão suporte a teclado e/ou gamepad/controle remoto.

Embora o mesmo foco visual possa ser usado em diferentes plataformas, o contexto em que o usuário se encontra é ligeiramente diferente para a experiência de 3 metros. Você deve presumir que o usuário não está prestando atenção total à toda a tela da TV e, portanto, é importante que o elemento focalizado no momento esteja claramente visível para o usuário em todos os momentos para evitar a frustração de procurar o foco visual.

Também é importante ter em mente que o foco visual é exibido por padrão, ao usar um gamepad ou controle remoto, mas *não* um teclado. Portanto, mesmo se você não implementá-lo, ele será exibido quando você executar seu aplicativo no Xbox One.

### <a name="initial-focus-visual-placement"></a>Posicionamento visual do foco inicial

Ao iniciar um aplicativo ou navegar para uma página, coloque o foco em um elemento de interface do usuário que faça sentido como o primeiro elemento no qual o usuário pode executar uma ação. Por exemplo, um aplicativo de fotos pode colocar o foco no primeiro item da galeria e um aplicativo de música navegado até um modo de exibição detalhado de uma música pode colocar o foco no botão Reproduzir para facilitar a reprodução da música.

Tente colocar o foco inicial na região superior esquerda do seu aplicativo (ou o canto superior direito para um fluxo da direita para a esquerda). A maioria dos usuários tende a se concentrar no que canto primeiro porque é onde o fluxo de conteúdo do aplicativo geralmente começa.

### <a name="making-focus-clearly-visible"></a>Tornando o foco claramente visível

Um foco visual sempre deve ficar visível na tela para que o usuário possa recomeçar de onde parou sem procurar o foco. Da mesma forma, deve haver um item focalizável na tela o tempo todo; por exemplo, não use pop-ups apenas com texto e sem elementos focalizáveis.

Uma exceção a essa regra seria para experiências de tela inteira, como assistir a vídeos ou exibir imagens, em cujos casos não seria adequado mostrar o foco visual.

### <a name="reveal-focus"></a>Foco do Revelação

O foco do Revelação é um efeito de iluminação que anima a borda de elementos focalizáveis, como um botão, quando o usuário move o foco do gamepad ou do teclado até eles. Ao animar o brilho ao redor da borda dos elementos em foco, Revelar foco proporciona aos usuários uma melhor compreensão de onde o foco está e para onde ele está indo.

O foco do Revelação está desativado por padrão. Para experiências de 10 pés, você deve optar por revelar o foco definindo a [propriedade Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind) no construtor do aplicativo.

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Para obter mais informações, consulte as diretrizes de [Foco do Revelação](/windows/uwp/design/style/reveal-focus).

### <a name="customizing-the-focus-visual"></a>Personalizando o foco visual

Se você quiser personalizar o foco visual, poderá fazer isso modificando as propriedades relacionadas ao foco visual de cada controle. Há várias propriedades desse tipo que você pode utilizar para personalizar seu aplicativo.

Você pode até recusar os elementos visuais de foco fornecidos pelo sistema desenhando seus próprios estados visuais de uso. Para saber mais, consulte [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState).

### <a name="light-dismiss-overlay"></a>Sobreposição light dismiss

Para chamar a atenção do usuário para os elementos de interface do usuário que ele está manipulando no momento com o controle de jogo ou o controle remoto, a UWP adiciona automaticamente uma camada de "fumaça" que abrange as áreas externas à interface do usuário pop-up quando o aplicativo é executado no Xbox One. Isso não exige nenhum trabalho extra, mas é algo que você deve ter em mente ao projetar sua interface do usuário. Você pode definir a propriedade `LightDismissOverlayMode` em qualquer `FlyoutBase` para habilitar ou desabilitar a camada de fumaça; o padrão passa a ser `Auto`, o que significa que ela está habilitada no Xbox e desabilitada em outros lugares. Para obter mais informações, consulte [Modal vs. light dismiss](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Envolvimento de foco

O envolvimento de foco destina-se a facilitar o uso de um gamepad ou controle remoto para interagir com um aplicativo.

> [!NOTE]
> Definir o envolvimento do foco não afeta o teclado ou outros dispositivos de entrada.

Quando a propriedade `IsFocusEngagementEnabled` em um objeto [FrameworkElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) é definida como `True`, ela marca o controle para exigir o envolvimento de foco. Isso significa que o usuário deve pressionar o botão **A/Selecionar** para "envolver" o controle e interagir com ele. Ao terminar, ele poderá pressionar o botão **B/Voltar** para desvincular o controle e navegar fora dele.

> [!NOTE]
> `IsFocusEngagementEnabled` é uma API nova e ainda não foi documentada.

### <a name="focus-trapping"></a>Trapping de foco

O trapping de foco é o que acontece quando um usuário tenta navegar na interface do usuário de em aplicativo mas fica "preso" dentro de um controle, o que torna difícil ou impossível mover-se para fora do controle.

O exemplo a seguir mostra a interface do usuário que cria o ajuste de registro de foco.

![Botões esquerdo e direito de um controle deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Se o usuário deseja navegar do botão esquerdo para o botão direito, seria lógico pressupor que basta pressionar direita duas vezes D-pad/no joystick esquerdo.
No entanto, se o [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) não exigisse envolvimento, o comportamento a seguir ocorreria: quando o usuário pressionasse direta pela primeira vez, o foco poderia alternar para o `Slider`, e quando ele pressionasse direita novamente, o `Slider`do identificador se moveria para a direita. O usuário poderia continuar movendo o identificador para a direita e não seria capaz de acessar o botão.

Há várias abordagens para contornar esse problema. Uma é criar um layout diferente, semelhante ao exemplo de aplicativo de imóveis em [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction), onde podemos realocar os botões **Anterior** e **Próximo** acima de `ListView`. Empilhar os controles verticalmente em vez de horizontalmente como na imagem a seguir pode resolver o problema.

![Botões acima e abaixo de um controle deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Agora, o usuário pode navegar para cada um dos controles, pressionando a tecla para cima e para baixo no D-pad/joystick esquerdo e quando o `Slider` tem o foco, ele poderá pressionar a tecla para esquerda e para direita a fim de mover o identificador do `Slider`, conforme o esperado.

Outra abordagem para solucionar esse problema é exigir envolvimento no `Slider`. Se você definir `IsFocusEngagementEnabled="True"`, isso resultará no comportamento a seguir.

![Exigir o envolvimento do foco no controle deslizante para que o usuário possa navegar para o botão à direita](images/designing-for-tv/focus-engagement-slider.png)

Quando o `Slider` exige envolvimento de foco, o usuário pode acessar o botão à direita pressionando diretamente no D-pad/joystick esquerdo duas vezes. Essa solução é excelente porque não requer nenhum ajuste de interface do usuário e produz o comportamento esperado.

### <a name="items-controls"></a>Controles de itens

Além do controle [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider), há outros controles para os quais você talvez possa exigir envolvimento, tais como:

- [ListBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox)
- [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)
- [FlipView]https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView)

Ao contrário do controle `Slider`, esses controles não aprisionam o foco dentro deles mesmos; no entanto, eles podem causar problemas de usabilidade quando contêm grandes quantidades de dados. A seguir está um exemplo de um `ListView` que contém uma grande quantidade de dados.

![ListView com grande quantidade de dados e botões acima e abaixo](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

Semelhante ao exemplo do `Slider`, vamos tentar navegar do botão na parte superior para o botão na parte inferior com um gamepad/controle remoto.
Começando com foco no botão superior, pressionar para baixo no D-pad/joystick colocará o foco no primeiro item no `ListView` ("Item 1").
Quando o usuário pressiona para baixo novamente, o próximo item na lista obtém o foco, não o botão na parte inferior.
Para acessar o botão, o usuário deve navegar por cada item no `ListView` primeiro.
Se o `ListView` contiver uma grande quantidade de dados, isso pode ser inconveniente e não uma experiência de usuário ideal.

Para solucionar esse problema, defina a propriedade `IsFocusEngagementEnabled="True"` no `ListView` para exigir envolvimento.
Isso permitirá que o usuário ignore rapidamente o `ListView` simplesmente pressionando para baixo. Entretanto, eles não poderão rolar a lista ou escolher um item nela a menos que a vinculem, pressionando o botão **A/Selecionar** quando ela tem o foco e, em seguida, pressionando o botão **B/Voltar** para desvincular.

![ListView com envolvimento necessário](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### <a name="scrollviewer"></a>ScrollViewer

O controle [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) é um pouco diferente desses controles, pois tem seus próprios quirks a serem considerados. Se você tiver um `ScrollViewer` com conteúdo focalizável, por padrão, navegar para o `ScrollViewer` permitirá que você se mova através de seus elementos focalizáveis. Como em um `ListView`, você deve rolar por cada item para navegar fora do `ScrollViewer`.

Se o `ScrollViewer`*não* tiver conteúdo focalizável&mdash;por exemplo, se ele contiver apenas texto;&mdash;você poderá definir `IsFocusEngagementEnabled="True"` para que o usuário possa vincular o `ScrollViewer` usando o botão **A/Selecionar**. Depois de vinculados, os usuários podem rolar pelo texto usando o **D-pad/joystick esquerdo** e, em seguida, pressionar o botão **B/Voltar** para desvincular quando terminarem.

Outra abordagem seria definir `IsTabStop="True"` no `ScrollViewer` para que os usuários não tenham que envolver o controle&mdash;eles podem simplesmente colocar o foco nele e, em seguida, rolar a tela usando o **D-pad/joystick esquerdo** quando não houver elementos focalizáveis no `ScrollViewer`.

### <a name="focus-engagement-defaults"></a>Padrões de envolvimento de foco

Alguns controles tornam o ajuste de registro de foco comum o suficiente para garantir que as configurações padrão exijam o envolvimento de foco, enquanto outros têm o envolvimento de foco desativado por padrão, mas podem se beneficiar ao ativá-lo. A tabela a seguir lista esses controles e seus comportamentos de envolvimento de foco padrão.

| Controle               | Padrão de envolvimento de foco  |
|-----------------------|---------------------------|
| CalendarDatePicker    | Ativado                        |
| FlipView              | Desativado                       |
| GridView              | Desativado                       |
| ListBox               | Desativado                       |
| ListView              | Desativado                       |
| ScrollViewer          | Desativado                       |
| SemanticZoom          | Desativado                       |
| Controle deslizante                | Ativado                        |

Todos os outros controles da UWP não resultarão em mudanças comportamentais ou visuais `IsFocusEngagementEnabled="True"`.

## <a name="summary"></a>Resumo

Você pode criar aplicativos de UWP que são otimizados para uma experiência ou dispositivo específico, mas a plataforma Universal do Windows também permite que você crie aplicativos que podem ser usados com êxito em todos os dispositivos, nas experiências de pé 2 e 10 pés e independentemente da entrada capacidade de dispositivo ou usuário. Usando as recomendações neste artigo pode garantir que seu aplicativo é tão bom quanto possível sobre a TV e um PC.

## <a name="related-articles"></a>Artigos relacionados

- [Projetando para TV e Xbox](../devices/designing-for-tv.md)
- [Instruções elementares do dispositivo para aplicativos da plataforma Universal do Windows (UWP)](index.md)
