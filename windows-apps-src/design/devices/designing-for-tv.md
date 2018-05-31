---
author: eliotcowley
Description: Design your app so that it looks good and functions well on your television.
title: Projetando para Xbox e TV
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, experiência de 3 metros, gamepad, controle remoto, interação de entrada
ms.author: elcowle
ms.date: 12/5/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c9751ef316dbec7334fc12242d71dd58ae2cb262
ms.sourcegitcommit: cceaf2206ec53a3e9155f97f44e4795a7b6a1d78
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2018
ms.locfileid: "1700962"
---
# <a name="designing-for-xbox-and-tv"></a>Projetar para TV e Xbox

Projete seu aplicativo UWP (Plataforma Universal do Windows) para que ele tenha uma boa aparência e funcione bem no Xbox One e em telas de televisão.

## <a name="overview"></a>Visão geral

A Plataforma Universal do Windows permite que você crie experiências agradáveis em diversos dispositivos com o Windows 10.
A maior parte da funcionalidade fornecida pela estrutura UWP permite que os aplicativos usem a mesma interface de usuário (UI) entre esses dispositivos, sem nenhum trabalho adicional.
No entanto, adaptar e otimizar seu aplicativo para funcionar perfeitamente em telas de TV e no Xbox One requer considerações especiais.

A experiência de sentar em seu sofá na sala, usando um gamepad ou um controle remoto para interagir com sua TV, é chamada de **experiência de 3 metros**.
Ela é chamada assim porque o usuário em geral está sentado a aproximadamente 3 metros de distância da tela.
Isso proporciona desafios únicos que não estão presentes na, digamos, experiência de *meio metro* ou na interação com um computador.
Se você estiver desenvolvendo um aplicativo para Xbox One ou qualquer outro dispositivo que é mostrado na tela da TV e usa um controlador para entrada, você sempre deve ter isso em mente.

Nem todas as etapas deste artigo são necessárias para fazer com que seu aplicativo funcione bem em experiências de 3 metros, mas compreendê-las e tomar as decisões apropriadas para seu aplicativo resultará em uma experiência de 3 metros melhor adaptada às necessidades específicas do seu aplicativo.
À medida que você dá vida ao seu aplicativo no ambiente de 3 metros, considere os princípios de design seguintes.

### <a name="simple"></a>Simples

Projetar para o ambiente de 3 metros apresenta um conjunto único de desafios. A resolução e a distância de exibição podem tornar difícil para as pessoas processar muitas informações.
Tente manter seu design limpo, reduzido aos componentes mais simples possíveis. A quantidade de informações exibidas em uma TV deve ser semelhante ao que você veria em um celular, e não em uma área de trabalho.

![Tela inicial do Xbox One](images/designing-for-tv/xbox-home-screen.png)

### <a name="coherent"></a>Coerente

Os aplicativos UWP no ambiente de 3 metros devem ser intuitivos e fácil de usar. Torne o foco claro e inconfundível.
Organize o conteúdo para que o movimento no espaço seja consistente e previsível. Ofereça às pessoas o caminho mais curto para o que elas desejam fazer.

![Aplicativo de filmes do Xbox One](images/designing-for-tv/xbox-movies-app.png)

_**Todos os filmes mostrados na captura de tela estão disponíveis no Microsoft Movies e TV.**_  

### <a name="captivating"></a>Atraentes

As experiências cinematográficas mais imersivas ocorrem na tela grande. Cenário de ponta a ponta, movimento elegante e uso vibrante de cor e tipografia levam seus aplicativos para o próximo nível. Seja ousado e belo.

![Aplicativo de Avatar do Xbox One](images/designing-for-tv/xbox-avatar-app.png)

### <a name="optimizations-for-the-10-foot-experience"></a>Otimizações para a experiência de 3 metros

Agora que você conhece os princípios de design de aplicativo UWP adequados para a experiência de 3 metros, leia a seguinte visão geral sobre maneiras específicas em que você pode otimizar seu aplicativo e proporcionar uma excelente experiência do usuário.

| Recurso        | Descrição           |
| -------------------------------------------------------------- |--------------------------------|
| [Gamepad e controle remoto](#gamepad-and-remote-control)      | Certificar-se de que seu aplicativo funciona bem com gamepad e controle remoto é a etapa mais importante na otimização para experiências de 3 metros. Existem várias melhorias específicas no gamepad e no controle remoto que você pode fazer para otimizar a experiência de interação do usuário em um dispositivo em que suas ações são relativamente limitadas. |
| [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction) | A UWP oferece **navegação de foco do plano XY** que permite que o usuário navegue pela interface do usuário do seu aplicativo. No entanto, isso limita o usuário a navegar para cima, para baixo, para a esquerda e direita. Recomendações para lidar com isso e outras considerações são descritas nesta seção. |
| [Modo de mouse](#mouse-mode)|Em algumas interfaces do usuário, como mapas e superfícies de desenho, não é possível ou prático usar navegação de foco do plano XY. Para essas interfaces, a UWP fornece o **modo de mouse** para permitir que o gamepad/controle remoto navegue livremente, como um mouse em um computador desktop.|
| [Foco visual](#focus-visual)  | O foco visual é a borda em torno do elemento de interface do usuário que tem o foco no momento. Isso ajuda a orientar o usuário para que ele possa navegar facilmente em sua interface do usuário sem se perder. Se o foco não estiver claramente visível, o usuário pode se perder em sua interface do usuário e não ter uma boa experiência.  |
| [Envolvimento de foco](#focus-engagement) | Definir o envolvimento do foco em um elemento de interface do usuário exige que o usuário pressione o botão **A/Selecionar** para interagir com ele. Isso pode ajudar a criar uma melhor experiência para o usuário ao navegar na interface do usuário do seu aplicativo.
| [Dimensionamento de elemento de interface do usuário](#ui-element-sizing)  | A Plataforma Universal do Windows usa [dimensionamento e pixels eficazes](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para dimensionar a interface do usuário de acordo com a distância de exibição. Compreender o dimensionamento e aplicá-lo em sua interface do usuário ajudará você a otimizar seu aplicativo para o ambiente de 3 metros.  |
|  [Área segura para a TV](#tv-safe-area) | A UWP evitará automaticamente exibir qualquer interface do usuário em áreas não seguras para TV (áreas perto das bordas da tela) por padrão. No entanto, isso cria um efeito de "box-in" em que a interface do usuário parece estar em letterbox. Para que seu aplicativo seja realmente imersivo na TV, você deve modificá-lo para que ele se estenda para as bordas da tela em TVs compatíveis com esse recurso. |
| [Cores](#colors)  |  A UWP é compatível com temas de cores e um aplicativo que respeita o tema do sistema será padonizado como **escuro** no Xbox One. Se o seu aplicativo tiver um tema de cor específico, você deve considerar que algumas cores não funcionam bem na TV e devem ser evitadas. |
| [Som](../style/sound.md)    | Sons desempenham um papel fundamental na experiência de 3 metros, ajudando a imergir e enviar seus comentários para o usuário. A UWP fornece funcionalidade que ativa sons para controles comuns automaticamente quando o aplicativo é executado no Xbox One. Saiba mais sobre o suporte a som incorporado à UWP e saiba como tirar proveito dele.    |
| [Diretrizes de controles da interface do usuário](#guidelines-for-ui-controls)  |  Há vários controles de interface do usuário que funcionam bem em vários dispositivos, mas existem certas considerações quando usados na TV. Leia sobre algumas práticas recomendadas para usar esses controles ao projetar para a experiência de 3 metros. |
| [Gatilho de estado visual personalizado para Xbox](#custom-visual-state-trigger-for-xbox) | Para adaptar seu aplicativo UWP para a experiência de 3 metros, recomendamos que você use um *gatilho de estado visual* personalizado para fazer alterações de layout quando o app detectar que foi iniciado em um console Xbox.

> [!NOTE]
> A maioria dos trechos de código deste tópico está em XAML/C#; entretanto, os princípios e conceitos se aplicam a todos os aplicativos UWP. Se você estiver desenvolvendo um aplicativo UWP HTML/JavaScript para Xbox, confira a excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) no GitHub.

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
| Tecla Enter                 | Botão A/Selecionar                       |
| Escape                | Botão B/Voltar*                        |

\*Quando nem os eventos [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941.aspx) nem [KeyUp](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) do botão B forem manipulados pelo aplicativo, o evento [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) será acionado, o que deve resultar na navegação regressiva no aplicativo. No entanto, você precisa implementar isso por conta própria, como no seguinte trecho de código:

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
| Page up/Page down  | Page up/Page down | Gatilhos esquerdo/direito | [CalendarView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.calendarview.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [ComboBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Exibições que dão suporte à rolagem vertical
| Página esquerda/direita | Nenhum(a) | Botões superiores esquerdo/direito | [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx), [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx), [ListViewBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.aspx), [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), `ScrollViewer`, [Selector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.selector.aspx), [LoopingSelector](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.loopingselector.aspx), [FlipView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flipview.aspx) | Exibições que dão suporte à rolagem horizontal
| Ampliar/reduzir        | Ctrl +/- | Gatilhos esquerdo/direito | Nenhum(a) | `ScrollViewer`, exibições que dão suporte a ampliação e redução |
| Abrir/fechar painel de navegação | Nenhum(a) | Exibir | Nenhum(a) | Painéis de navegação |
| [Pesquisar](#search-experience) | Nenhum(a) | Botão Y | Nenhum(a) | Atalho para a função de pesquisa principal no aplicativo |
| [Abrir menu de contexto](#commandbar-and-contextflyout) | Clicar com o botão direito do mouse | Botão Menu | [ContextFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout) | Menus de contexto |

## <a name="xy-focus-navigation-and-interaction"></a>Interação e navegação de foco do plano XY

Se o seu aplicativo dá suporte à navegação de foco adequada para teclado, isso será convertido corretamente para o gamepad e o controle remoto.
A navegação com as teclas de direção é mapeada para o **D-pad** (bem como o **joystick esquerdo** no gamepad), e a interação com os elementos de interface do usuário é mapeada para a tecla **Enter/Select** (consulte [Gamepad e controle remoto](#gamepad-and-remote-control)).

Muitos eventos e propriedades são usados pelo teclado e pelo gamepad &mdash; ambos disparam os eventos `KeyDown` e `KeyUp`, e ambos navegarão apenas para controles que tenham as propriedades `IsTabStop="True"` e `Visibility="Visible"`. Para obter diretrizes de design de teclado, consulte [Interações de teclado](../input/keyboard-interactions.md).

Se o suporte ao teclado for implementado corretamente, seu aplicativo funcionará razoavelmente bem; no entanto, pode haver algum trabalho extra necessário para dar suporte a cada cenário. Pense em necessidades específicas do seu aplicativo para oferecer a melhor experiência possível ao usuário.

> [!IMPORTANT]
> O modo de mouse é habilitado por padrão para aplicativos UWP executados no Xbox One. Para desabilitar o modo de mouse e habilitar a navegação de foco do plano XY, defina `Application.RequiresPointerMode=WhenRequested`.

### <a name="debugging-focus-issues"></a>Depurando problemas de foco

O método [FocusManager.GetFocusedElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.focusmanager.getfocusedelement.aspx) informará qual elemento tem o foco no momento. Isso é útil para situações em que a localização do foco visual talvez não seja óbvia. Você pode registrar essa informação na janela de saída do Visual Studio da seguinte forma:

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

* A propriedade [IsTabStop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.istabstop.aspx) ou [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) está definida incorretamente.
* O controle que obtém o foco é na verdade maior que você imagina &mdash; a navegação do plano XY examina o tamanho total do controle ([ActualWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualwidth.aspx) e [ActualHeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.actualheight.aspx)), não apenas a parte do controle que renderiza algo interessante.
* Um controle focalizável está por cima de outro &mdash; a navegação do plano XY não dá suporte a controles que estão sobrepostos.

Se a navegação do plano XY ainda não estiver funcionando da maneira esperada após a correção desses problemas, você poderá apontar manualmente para o elemento que deve receber o foco usando o método descrito em [Substituindo a navegação padrão](#overriding-the-default-navigation).

Se a navegação do plano XY estiver funcionando conforme o esperado, mas nenhum foco visual for exibido, a causa poderá ser uma das seguintes:

* Você remodelou o controle e não incluiu um foco visual. Defina `UseSystemFocusVisuals="True"` ou adicione um foco visual manualmente.
* Você moveu o foco chamando `Focus(FocusState.Pointer)`. O parâmetro [FocusState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.focusstate.aspx) controla o que acontece com o foco visual. Em geral, isso deve ser definido como `FocusState.Programmatic`, que mantém o foco visual visível se ele estava visível antes, e o oculta se ele estava oculto antes.

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

Tente permitir que o usuário execute as tarefas mais comuns com o menor número de cliques. No exemplo a seguir, o [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) é colocado entre o botão **Reproduzir** (que inicialmente recebe o foco) e um elemento comumente usado, sendo assim, um elemento desnecessário seja colocado entre as tarefas prioritárias.

![As práticas recomendadas de navegação fornecem caminho com menos cliques](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

No exemplo a seguir, o [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) é colocado acima do botão **Reproduzir** em vez disso.
Simplesmente reorganizar a interface do usuário para que elementos desnecessários não sejam colocados entre tarefas prioritárias melhorará consideravelmente a usabilidade do seu aplicativo.

![TextBlock movido para cima do botão Reproduzir para que não fique mais entre tarefas prioritárias](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### <a name="commandbar-and-contextflyout"></a>CommandBar e ContextFlyout

Ao usar um [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), tenha em mente o problema de rolagem em uma lista, como mencionado em [Problema: elementos de interface do usuário localizados após uma lista/grade de rolagem longa](#problem-ui-elements-located-after-long-scrolling-list-grid). A imagem a seguir mostra um layout de interface do usuário com o `CommandBar` na parte inferior de uma lista/grade. O usuário precisa rolar até o final da lista/grade para acessar o elemento `CommandBar`.

![CommandBar na parte inferior da lista/grade](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

E se você colocar o elemento `CommandBar` *acima* da lista/grade? Embora o usuário que rolou a lista/grade para baixo tenha que rolar de volta para cima para acessar o `CommandBar`, haverá um pouco menos de navegação do que na configuração anterior. Observe que isso pressupõe que o foco inicial do seu aplicativo seja colocado ao lado ou acima do `CommandBar`; Essa abordagem não funcionará bem se o foco inicial está abaixo da lista/grade. Se esses itens de `CommandBar` forem itens de ação global que não precisem ser acessados com frequência (como um botão **Sincronizar**), talvez seja aceitável tê-los acima da lista/grade.

Embora você não possa empilhar itens de uma `CommandBar`verticalmente, colocá-los contra a direção de rolagem (por exemplo, para a esquerda ou direita de uma lista de rolagem vertical, ou para cima ou para baixo de uma lista de rolagem horizontal) é outra opção que você poderá considerar se ela funcionar bem no layout de sua interface do usuário.

Se o seu aplicativo tiver uma `CommandBar` cujos itens precisam ser prontamente acessados pelos usuários, considere colocar esses itens dentro de uma propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) e remova-os da `CommandBar`. `ContextFlyout` é uma propriedade de [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx) e é o [menu de contexto](../controls-and-patterns/dialogs.md) associado a esse elemento. No computador, quando você clica em um elemento com `ContextFlyout`, esse menu de contexto é aberto. No Xbox One, isso acontecerá quando você pressionar o botão **Menu** enquanto o foco estiver em um elemento.

### <a name="ui-layout-challenges"></a>Desafios de layout de interface do usuário

Alguns layouts de interface do usuário são mais desafiadores devido à natureza da navegação de foco do plano XY e devem ser avaliados caso a caso. Embora não haja uma única maneira "correta", e a solução que você escolher depende das necessidades específicas do seu aplicativo, existem algumas técnicas que você pode utilizar para criar uma ótima experiência de TV.

Para entender isso melhor, vamos examinar um aplicativo imaginário que ilustra alguns desses problemas, e as técnicas para superá-los.

> [!NOTE]
> Esse aplicativo fictício serve para ilustrar problemas de interface do usuário e as possíveis soluções para eles, e não se destina a mostrar a melhor experiência de usuário para seu aplicativo específico.

A seguir há um aplicativo de imóveis imaginário que mostra uma lista de imóveis disponíveis para venda, um mapa, uma descrição de uma propriedade e outras informações. Esse aplicativo apresenta três desafios que você pode solucionar usando as seguintes técnicas:

- [Reorganização da interface de usuário](#ui-rearrange)
- [Envolvimento de foco](#engagement)
- [Modo de mouse](#mouse-mode)

![Aplicativo de imóveis fictício](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### Problema: elementos de interface do usuário localizados após uma lista/grade de rolagem longa <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

O [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) de propriedades mostradas na imagem a seguir é uma lista de rolagem muito longa. Se o [envolvimento](#focus-engagement) *não* for necessária no `ListView`, quando o usuário navegar para a lista, o foco será colocado no primeiro item da lista. Para o usuário acessar o botão **Anterior** ou **Próximo**, ele deve passar por todos os itens da lista. Em casos assim, nos quais exigir que o usuário percorra a lista inteira é trabalhoso, ou seja, quando a lista não é curta o suficiente para essa experiência ser aceitável, talvez você queira considerar outras opções.

![Aplicativo de imóveis: a lista de 50 itens leva 51 cliques para alcançar os botões abaixo](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### <a name="solutions"></a>Soluções

**Reorganização da interface de usuário <a name="ui-rearrange"></a>**

A menos que o foco inicial seja colocado na parte inferior da página, os elementos de interface do usuário colocados acima de uma lista de rolagem longa são normalmente mais acessíveis que se forem colocados abaixo.
Se esse novo layout funciona para outros dispositivos, alterar o layout para todas as famílias de dispositivos em vez de fazer alterações especiais na interface do usuário apenas para o Xbox One pode ser uma abordagem mais barata.
Além disso, colocar os elementos de interface do usuário contra a direção de rolagem (ou seja, na horizontal em uma lista de rolagem vertical, ou na vertical em uma lista de rolagem horizontal) tornará a acessibilidade ainda melhor.

![Aplicativo de imóveis: colocar botões acima da lista de rolagem longa](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

**Envolvimento de foco <a name="engagement"></a>**

Quando o envolvimento é *necessário*, todo o `ListView` se torna um destino de foco único. O usuário poderá ignorar o conteúdo da lista para acessar o próximo elemento focalizável. Leia mais sobre quais controles oferecem suporte a envolvimento e como usá-los em [Envolvimento de foco](#focus-engagement).

![Aplicativo de imóveis: definir o envolvimento como necessário para que leve apenas 1 clique para acessar os botões Anterior/Próximo](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### <a name="problem-scrollviewer-without-any-focusable-elements"></a>Problema: ScrollViewer sem elementos focalizáveis

Como a navegação de foco do plano XY depende de navegar para um elemento de interface do usuário focalizável ao mesmo tempo, um [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) que não contém elementos focalizáveis (tal como um elemento apenas com texto, como neste exemplo) pode criar um cenário em que o usuário não consegue exibir todo o conteúdo no `ScrollViewer`.
Para obter soluções para esse e outros cenários relacionados, veja [Envolvimento de foco](#focus-engagement).

![Aplicativo de imóveis: ScrollViewer apenas com texto](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### <a name="problem-free-scrolling-ui"></a>Problema: interface de usuário de rolagem livre

Quando seu aplicativo requer uma interface de usuário de rolagem livre, como uma superfície de desenho ou, neste exemplo, um mapa, a navegação de foco do plano XY simplesmente não funciona.
Nesses casos, você pode ativar o [modo de mouse](#mouse-mode) para permitir que o usuário navegue livremente dentro de um elemento de interface do usuário.

![Mapear elemento de interface do usuário usando o modo de mouse](images/designing-for-tv/map-mouse-mode.png)

## <a name="mouse-mode"></a>Modo de mouse

Conforme descrito em [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction), o foco no Xbox One é movido por meio de um sistema de navegação do plano XY, permitindo que o usuário mude o foco de controle para controle movendo-se para cima, para baixo, para a esquerda e para a direita.
No entanto, alguns controles, como [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) e [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx), exigem uma interação de mouse na qual os usuários podem mover livremente o ponteiro dentro dos limites do controle.
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
> O valor `WhenFocused` tem suporte apenas em objetos [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx). Se você tentar definir esse valor em um controle, será gerada uma exceção.

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

Você pode até recusar os elementos visuais de foco fornecidos pelo sistema desenhando seus próprios estados visuais de uso. Para saber mais, consulte [VisualState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstate.Aspx).

### <a name="light-dismiss-overlay"></a>Sobreposição light dismiss

Para chamar a atenção do usuário para os elementos de interface do usuário que ele está manipulando no momento com o controle de jogo ou o controle remoto, a UWP adiciona automaticamente uma camada de "fumaça" que abrange as áreas externas à interface do usuário pop-up quando o aplicativo é executado no Xbox One. Isso não exige nenhum trabalho extra, mas é algo que você deve ter em mente ao projetar sua interface do usuário. Você pode definir a propriedade `LightDismissOverlayMode` em qualquer `FlyoutBase` para habilitar ou desabilitar a camada de fumaça; o padrão passa a ser `Auto`, o que significa que ela está habilitada no Xbox e desabilitada em outros lugares. Para obter mais informações, consulte [Modal vs. light dismiss](../controls-and-patterns/menus.md).

## <a name="focus-engagement"></a>Envolvimento de foco

O envolvimento de foco destina-se a facilitar o uso de um gamepad ou controle remoto para interagir com um aplicativo.

> [!NOTE]
> Definir o envolvimento do foco não afeta o teclado ou outros dispositivos de entrada.

Quando a propriedade `IsFocusEngagementEnabled` em um objeto [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) é definida como `True`, ela marca o controle para exigir o envolvimento de foco. Isso significa que o usuário deve pressionar o botão **A/Selecionar** para "envolver" o controle e interagir com ele. Ao terminar, ele poderá pressionar o botão **B/Voltar** para desvincular o controle e navegar fora dele.

> [!NOTE]
> `IsFocusEngagementEnabled` é uma API nova e ainda não foi documentada.

### <a name="focus-trapping"></a>Trapping de foco

O trapping de foco é o que acontece quando um usuário tenta navegar na interface do usuário de em aplicativo mas fica "preso" dentro de um controle, o que torna difícil ou impossível mover-se para fora do controle.

O exemplo a seguir mostra a interface do usuário que cria o ajuste de registro de foco.

![Botões esquerdo e direito de um controle deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping.png)

Se o usuário deseja navegar do botão esquerdo para o botão direito, seria lógico pressupor que basta pressionar direita duas vezes D-pad/no joystick esquerdo.
No entanto, se o [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx) não exigisse envolvimento, o comportamento a seguir ocorreria: quando o usuário pressionasse direta pela primeira vez, o foco poderia alternar para o `Slider`, e quando ele pressionasse direita novamente, o `Slider`do identificador se moveria para a direita. O usuário poderia continuar movendo o identificador para a direita e não seria capaz de acessar o botão.

Há várias abordagens para contornar esse problema. Uma é criar um layout diferente, semelhante ao exemplo de aplicativo de imóveis em [Interação e navegação de foco do plano XY](#xy-focus-navigation-and-interaction), onde podemos realocar os botões **Anterior** e **Próximo** acima de `ListView`. Empilhar os controles verticalmente em vez de horizontalmente como na imagem a seguir pode resolver o problema.

![Botões acima e abaixo de um controle deslizante horizontal](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

Agora, o usuário pode navegar para cada um dos controles, pressionando a tecla para cima e para baixo no D-pad/joystick esquerdo e quando o `Slider` tem o foco, ele poderá pressionar a tecla para esquerda e para direita a fim de mover o identificador do `Slider`, conforme o esperado.

Outra abordagem para solucionar esse problema é exigir envolvimento no `Slider`. Se você definir `IsFocusEngagementEnabled="True"`, isso resultará no comportamento a seguir.

![Exigir o envolvimento do foco no controle deslizante para que o usuário possa navegar para o botão à direita](images/designing-for-tv/focus-engagement-slider.png)

Quando o `Slider` exige envolvimento de foco, o usuário pode acessar o botão à direita pressionando diretamente no D-pad/joystick esquerdo duas vezes. Essa solução é excelente porque não requer nenhum ajuste de interface do usuário e produz o comportamento esperado.

### <a name="items-controls"></a>Controles de itens

Além do controle [Slider](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.slider.aspx), há outros controles para os quais você talvez possa exigir envolvimento, tais como:

- [ListBox](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

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

O controle [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) é um pouco diferente desses controles, pois tem seus próprios quirks a serem considerados. Se você tiver um `ScrollViewer` com conteúdo focalizável, por padrão, navegar para o `ScrollViewer` permitirá que você se mova através de seus elementos focalizáveis. Como em um `ListView`, você deve rolar por cada item para navegar fora do `ScrollViewer`.

Se o `ScrollViewer` *não* tiver conteúdo focalizável&mdash;por exemplo, se ele contiver apenas texto;&mdash;você poderá definir `IsFocusEngagementEnabled="True"` para que o usuário possa vincular o `ScrollViewer` usando o botão **A/Selecionar**. Depois de vinculados, os usuários podem rolar pelo texto usando o **D-pad/joystick esquerdo** e, em seguida, pressionar o botão **B/Voltar** para desvincular quando terminarem.

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
| Slider                | Ativado                        |

Todos os outros controles da UWP não resultarão em mudanças comportamentais ou visuais `IsFocusEngagementEnabled="True"`.

## <a name="ui-element-sizing"></a>Dimensionamento de elemento de interface do usuário

Como o usuário de um aplicativo no ambiente de 3 metros está usando um controle remoto ou gamepad e está sentado a vários metros da tela, há algumas considerações de interface do usuário que precisam ser fatoradas em seu design.
Certifique-se de que a interface do usuário tenha uma densidade de conteúdo adequada e não está desorganizada demais para que o usuário possa navegar e selecionar elementos facilmente. Lembre-se: simplicidade é a chave.

### <a name="scale-factor-and-adaptive-layout"></a>Fator de escala e layout adaptável

**Fator de escala**: ajuda a garantir que os elementos de interface do usuário sejam exibidos com o dimensionamento correto para o dispositivo no qual o aplicativo é executado.
Na área de trabalho, essa configuração pode ser encontrada em **Configurações > Sistema > Exibição** como um valor de deslizamento.
Essa mesma configuração existe no telefone se o dispositivo for compatível com ela.

![Alterar o tamanho do texto, aplicativos e outros itens](images/designing-for-tv/ui-scaling.png)

No Xbox One, não há nenhuma configuração do sistema assim; entretanto, para que os elementos de interface do usuário UWP sejam dimensionados adequadamente para TV, eles são dimensionados em um padrão de **200%** para apps XAML e **150%** para apps HTML.
Desde que os elementos de interface do usuário sejam dimensionados adequadamente para outros dispositivos, ele serão dimensionados adequadamente para TV.
O Xbox One renderiza seu aplicativo em 1080p (1920 x 1080 pixels). Portanto, ao trazer um app de outros dispositivos, como um computador, certifique-se de que a interface do usuário tenha uma boa aparência em 960 x 540 px em escala de 100% (ou 1.280 x 720 px em escala de 100% para apps HTML) utilizando [técnicas adaptáveis](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

O design para Xbox é um pouco diferente do design para computador, pois você só precisa se preocupar com uma resolução, 1.920 x 1.080.
Não importa se o usuário tem uma TV com melhor resolução; os aplicativos UWP sempre serão dimensionados para 1080p.

Tamanhos de ativos corretos do conjunto de 200% (ou 150% para HTML) também serão trazidos para seu app quando ele for executado no Xbox One, independentemente da resolução da TV.

### <a name="content-density"></a>Densidade de conteúdo

Ao projetar seu aplicativo, lembre-se de que o usuário estará visualizando a interface do usuário de uma distância e interagindo com ela usando um controlador de jogo ou controle remoto, o que leva mais tempo para navegar do que usando o mouse ou entrada por toque.

#### <a name="sizes-of-ui-controls"></a>Tamanhos dos controles de interface do usuário

Elementos de interface do usuário interativos devem ser dimensionados em uma altura mínima de 32 epx (pixels efetivos). Este é o padrão para controles UWP comuns e, quando usado em escala de 200%, garante que os elementos de interface do usuário ficam visíveis a uma distância e ajuda a reduzir a densidade do conteúdo.

![Botão UWP em escala de 100% e 200%](images/designing-for-tv/button-100-200.png)

#### <a name="number-of-clicks"></a>Número de cliques

Quando o usuário está navegando de uma borda da tela da TV para a outra, isso deve levar não mais do que **seis cliques** para simplificar a sua interface do usuário. Novamente, o princípio de **simplicidade** aplica-se aqui. Para obter mais detalhes, veja [Caminho de menos cliques](#path-of-least-clicks).

![6 ícones em](images/designing-for-tv/six-clicks.png)

### <a name="text-sizes"></a>Tamanhos do texto

Para tornar sua interface do usuário visível à distância, use as seguintes regras gerais:

* Texto principal e conteúdo de leitura: 15 epx no mínimo
* Texto não crítico e conteúdo complementar: 12 epx no mínimo

Ao usar texto maior na sua interface do usuário, escolha um tamanho que não limita demais o estado real da tela, ocupando espaço que outros tipos de conteúdo poderiam potencialmente preencher.

### <a name="opting-out-of-scale-factor"></a>Recusando o fator de escala

Recomendamos que seu aplicativo tire proveito do suporte ao fator de escala, o que o ajudará a ser executado adequadamente em todos os dispositivos, dimensionando para cada tipo de dispositivo.
No entanto, é possível recusar esse comportamento e projetar toda a sua interface do usuário em escala de 100%. Observe que você não pode alterar o fator de escala para algo diferente de 100%.

Para apps XAML, você pode recusar o fator de escala usando o seguinte trecho de código:

```csharp
bool result =
    Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` informará se você obteve êxito na recusa.

Para obter mais informações, incluindo o código de exemplo para HTML/JavaScript, consulte [Como desativar a colocação em escala](../../xbox-apps/disable-scaling.md).

Certifique-se de calcular os tamanhos dos elementos de interface do usuário apropriados, dobrando os valores de pixel *efetivo* mencionados neste tópico para valores de pixel *real* (ou multiplicando por 1,5 para apps HTML).

## <a name="tv-safe-area"></a>Área segura para a TV

Nem todas as TVs exibem conteúdo até as bordas da tela por motivos históricos e tecnológicos. Por padrão, a UWP evitará exibir qualquer conteúdo de interface do usuário em áreas inseguras para TV e, em vez disso, apenas desenhar o plano de fundo da página.

A área insegura para TV é representada pela área azul na imagem a seguir.

![Área insegura para TV](images/designing-for-tv/tv-unsafe-area.png)

Você pode definir o plano de fundo para uma cor estática ou temática, ou uma imagem, como os trechos de código a seguir demonstram.

### <a name="theme-color"></a>Cor do tema

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### <a name="image"></a>Imagem

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

Esta é a aparência de seu aplicativo sem nenhum trabalho adicional.

![Área segura para a TV](images/designing-for-tv/tv-safe-area.png)

Isso não é ideal porque dá ao aplicativo um efeito "box-in", com partes da interface do usuário, como o painel de navegação e a grade, aparentemente cortadas. No entanto, você pode fazer otimizações para estender partes da interface do usuário para as bordas da tela a fim de dar ao aplicativo um efeito mais cinematográfico.

### <a name="drawing-ui-to-the-edge"></a>Desenhando a interface do usuário para a borda

Recomendamos que você use determinados elementos da interface do usuário para estender para as bordas da tela a fim de fornecer mais imersão para o usuário. Isso inclui [ScrollViewers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx), [painéis de navegação](../controls-and-patterns/navigationview.md), e [CommandBars](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx).

Por outro lado, também é importante que o texto e os elementos interativos sempre evitem as bordas da tela para garantir que eles não sejam cortados em algumas TVs. Recomendamos que você desenhe apenas efeitos visuais não essenciais dentro de 5% das bordas da tela. Como mencionado em [dimensionamento de elemento de interface do usuário](#ui-element-sizing), um aplicativo UWP seguindo o fator de escala padrão do console do Xbox One de 200% utilizará uma área de 960 x 540 epx, portanto, na interface do usuário do seu aplicativo, você deve evitar colocar elementos essenciais da interface do usuário nas seguintes áreas:

- 27 epx da parte superior e inferior
- 48 epx dos lados esquerdo e direito

As seções a seguir descrevem como fazer sua interface do usuário se estender até as bordas da tela.

#### <a name="core-window-bounds"></a>Limites da janela principal

Para aplicativos UWP destinados apenas à experiência de 3 metros, usar os limites da janela principal é uma opção mais simples.

No método `OnLaunched` de `App.xaml.cs`, adicione o seguinte código:

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

Com essa linha de código, a janela do aplicativo se estenderá para as bordas da tela, portanto, você precisará mover todos os elementos interativos e essenciais da interface do usuário para a área de segurança para TV descrita anteriormente. A interface de usuário transitória, como menus de contexto e [ComboBoxes](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) abertas, permanecerá automaticamente dentro da área de segurança para TV.

![Limites da janela principal](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Telas de fundo do painel

Os painéis de navegação normalmente são desenhados perto da borda da tela, para que a tela de fundo se estenda na área insegura para TV, de forma a não inserir espaços estranhos. Para fazer isso, basta mudar a cor da tela de fundo do painel de navegação para a cor da tela de fundo do aplicativo.

O uso dos limites da janela principal conforme descrito anteriormente permitirá que você desenhe sua interface de usuário até as bordas da tela, mas, em seguida, você deve usar margens positivas no conteúdo da [SplitView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) para mantê-lo dentro da área de segurança para TV.

![Painel de navegação estendido até as bordas da tela](images/designing-for-tv/tv-safe-areas-2.png)

Aqui, o plano de fundo do painel de navegação foi estendido para as bordas da tela, enquanto os itens de navegação são mantidos na área de segurança para TV.
O conteúdo da `SplitView` (neste caso, uma grade de itens) foi estendido até a parte inferior da tela para parecer que ele continua e não é cortado, enquanto a parte superior da grade ainda está dentro da área de segurança para TV. (Saiba mais sobre como fazer isso em [Rolando fins de listas e grades](#scrolling-ends-of-lists-and-grids)).

O trecho de código a seguir tem este efeito:

```xml
<SplitView x:Name="RootSplitView"
           Margin="48,0,48,0">
    <SplitView.Pane>
        <ListView x:Name="NavMenuList"
                  ContainerContentChanging="NavMenuItemContainerContentChanging"
                  ItemContainerStyle="{StaticResource NavMenuItemContainerStyle}"
                  ItemTemplate="{StaticResource NavMenuItemTemplate}"
                  ItemInvoked="NavMenuList_ItemInvoked"
                  ItemsSource="{Binding NavMenuListItems}"/>
    </SplitView.Pane>
    <Frame x:Name="frame"
           Navigating="OnNavigatingToPage"
           Navigated="OnNavigatedToPage"/>
</SplitView>
```

[CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) é outro exemplo de um painel que é normalmente posicionado perto de uma ou mais bordas do aplicativo e, assim como na TV, sua tela de fundo deve se estender até as bordas da tela. Ela geralmente contém um botão **Mais**, representado por "..." no lado direito, que deve permanecer na área de segurança para TV. A seguir estão algumas estratégias diferentes para obter as interações e os efeitos visuais desejados.

**Opção 1**: altere a cor do plano de fundo de `CommandBar` para transparente ou a mesma cor do plano de fundo da página:

```xml
<CommandBar x:Name="topbar"
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

Fazer isso fará com que o painel `CommandBar` pareça estar sobre o mesmo plano de fundo que o resto da página, portanto, o plano de fundo flui perfeitamente para a borda da tela.

**Opção 2**: adicione um retângulo em segundo plano cujo preenchimento tenha a mesma cor que a tela de fundo de `CommandBar`, e ainda fique abaixo da `CommandBar` e ocupe o restante da página:

```xml
<Rectangle VerticalAlignment="Top"
            HorizontalAlignment="Stretch"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar"
            VerticalAlignment="Top"
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> [!NOTE]
> Se você estiver usando essa abordagem, esteja ciente de que o botão **Mais** modifica a altura da `CommandBar` aberta, se necessário, para mostrar os rótulos dos `AppBarButton`s abaixo dos respectivos ícones. Recomendamos que você mova os rótulos para a *direita* de seus ícones para evitar esse redimensionamento. Para saber mais, consulte [Rótulos de CommandBar](#commandbar-labels).

Essas duas abordagens também se aplicam aos outros tipos de controles listados nesta seção.

#### <a name="scrolling-ends-of-lists-and-grids"></a>Rolando fins de listas e grades

É comum que listas e grades contenham mais itens dos que podem caber na tela ao mesmo tempo. Se esse for o caso, recomendamos que você estenda a lista ou a grade até a borda da tela. As listas e grades de rolagem horizontal devem se estender até a borda direita e as de rolagem vertical devem se estender até a parte inferior.

![Recorte da grade da área de segurança para TV](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

Embora uma lista ou grade seja estendida dessa forma, é importante manter o foco visual e seu item associado dentro da área de segurança para TV.

![O foco da grade de rolagem deve ser mantido na área de segurança para TV](images/designing-for-tv/scrolling-grid-focus.png)

A UWP tem uma funcionalidade que mantém o foco visual dentro do [VisibleBounds](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx), mas você precisa adicionar preenchimento para garantir que os itens de lista/grade possam rolar para a exibição da área de segurança. Especificamente, você adiciona uma margem positiva ao [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) ou [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) do [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx), como no trecho de código a seguir:

```xml
<Style x:Key="TitleSafeListViewStyle"
       TargetType="ListView">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListView">
                <Border BorderBrush="{TemplateBinding BorderBrush}"
                        Background="{TemplateBinding Background}"
                        BorderThickness="{TemplateBinding BorderThickness}">
                    <ScrollViewer x:Name="ScrollViewer"
                                  TabNavigation="{TemplateBinding TabNavigation}"
                                  HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                  HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                  IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                  VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                  VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                  IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                  IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                  IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                  ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                  IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                  BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                  AutomationProperties.AccessibilityView="Raw">
                        <ItemsPresenter Header="{TemplateBinding Header}"
                                        HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                        HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                        Footer="{TemplateBinding Footer}"
                                        FooterTemplate="{TemplateBinding FooterTemplate}"
                                        FooterTransitions="{TemplateBinding FooterTransitions}"
                                        Padding="{TemplateBinding Padding}"
                                        Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

Você poderia colocar o trecho de código anterior nos recursos da página ou do aplicativo e, em seguida, acessá-lo da seguinte maneira:

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> [!NOTE]
> Este trecho de código é especificamente para `ListView`s; para um estilo de `GridView`, defina o atributo [TargetType](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) como [ControlTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) e [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.style.aspx) como `GridView`.

## <a name="colors"></a>Cores

Por padrão, a Plataforma Universal do Windows dimensiona as cores do aplicativo para o intervalo seguro para a TV (consulte [cores seguras para a TV](#tv-safe-colors) para obter mais informações), para que seu aplicativo tenha uma boa aparência em qualquer aparelho de TV. Além disso, há melhorias que você pode fazer no conjunto de cores que seu aplicativo usa para melhorar a experiência visual na TV.

### <a name="application-theme"></a>Tema de aplicativo

Você pode escolher um **Tema de aplicativo** (escuro ou claro) de acordo com o que é adequado para o seu aplicativo ou você pode recusar o tema. Leia mais sobre recomendações gerais para temas em [Temas de cores](../style/color.md).

A UWP também permite que os aplicativos definam o tema de forma dinâmica com base nas configurações do sistema fornecidas pelos dispositivos nos quais eles são executados.
Embora a UWP sempre respeite as configurações de tema especificadas pelo usuário, cada dispositivo também fornece um tema padrão adequado.
Devido à natureza do Xbox One, que tem mais experiências de *mídia* que de *produtividade*, o tema padrão do sistema é escuro.
Se o tema do seu aplicativo é baseado nas configurações do sistema, espere que ele seja escuro por padrão no Xbox One.

### <a name="accent-color"></a>Cor de destaque

A UWP oferece uma maneira conveniente de expor a **cor de destaque** que o usuário selecionou nas configurações de sistema.

No Xbox One, o usuário é capaz de selecionar uma cor de usuário, assim como ele pode selecionar uma cor de destaque em um computador.
Desde que o aplicativo chame essas cores de destaque por meio de pincéis ou recursos de cor, a cor que o usuário selecionou nas configurações do sistema será usada. Observe que as cores de destaque no Xbox One são por usuário, não por sistema.

Observe também que o conjunto de cores de usuário no Xbox One não é o mesmo em computadores, telefones e outros dispositivos.

Desde que seu aplicativo use um recurso de pincel, como **SystemControlForegroundAccentBrush**, ou um recurso de cor (**SystemAccentColor**), ou em vez disso, chame as cores de destaque diretamente por meio da API [UIColorType.Accent*](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx), essas cores são substituídas por cores de destaque disponíveis no Xbox One. Cores de pincel de alto contraste também são obtidas do sistema da mesma maneira que em um computador e um telefone.

Para saber mais sobre cores de destaque em geral, veja [Cor de destaque](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variação de cores entre TVs

Ao projetar para TV, observe que as cores são exibidas de forma bem diferente, dependendo da TV em que elas são renderizadas. Não pressuponha que as cores ficarão exatamente como aparecem no monitor. Se o seu aplicativo depende de diferenças sutis de cor para diferenciar partes da interface do usuário, as cores poderiam se misturar e os usuários ficariam confusos. Tente usar cores que sejam diferentes o suficiente para que os usuários possam claramente diferenciá-las, independentemente da TV que estiverem usando.

### <a name="tv-safe-colors"></a>Cores seguras para a TV

Os valores RGB de uma cor representam intensidades de vermelho, verde e azul. As TVs não lidam com intensidades extremas muito bem&mdash;elas podem produzir um efeito de faixa estranho ou aparecem desbotadas em determinadas TVs. Além disso, as cores de alta intensidade podem causar floração (pixels próximos começam a desenhar as mesmas cores). Embora existam diferentes escolas de pensamento em relação às cores consideradas seguras para a TV, as cores dentro dos valores RGB de 16-235 (ou 10-EB em hexadecimal) são geralmente seguras para uso em TV.

![Intervalo de cores seguras para TV](images/designing-for-tv/tv-safe-colors-2.png)

Historicamente, aplicativos no Xbox tinham que adaptar suas cores para ficar dentro desse intervalo de cor "seguro para a TV"; no entanto, a partir da atualização dos criadores do segundo semestre, o Xbox One dimensiona automaticamente o conteúdo de intervalo completo no intervalo seguro para a TV. Isso significa que a maioria dos desenvolvedores de aplicativos não precisa mais se preocupar com as cores seguras para a TV.

> [!IMPORTANT]
> O conteúdo de vídeo que já está no intervalo de cores seguro para a TV não tem esse efeito de escala de cor aplicado quando executado usando [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

Se você estiver desenvolvendo um aplicativo usando o DirectX 11 ou DirectX 12 e criando sua própria cadeia de troca para renderizar a interface do usuário ou vídeo, você pode especificar o espaço de cores que você usa chamando [IDXGISwapChain3::SetColorSpace1](https://msdn.microsoft.com/library/windows/desktop/dn903676), que permitirá que o sistema saiba se ele precisa dimensionar as cores ou não.

## <a name="guidelines-for-ui-controls"></a>Diretrizes de controles da interface do usuário

Há vários controles de interface do usuário que funcionam bem em vários dispositivos, mas existem certas considerações quando usados na TV. Leia sobre algumas práticas recomendadas para usar esses controles ao projetar para a experiência de 3 metros.

### <a name="pivot-control"></a>Controle Pivot

Um [Pivot](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) oferece navegação rápida de modos de exibição dentro de um aplicativo com a seleção de diferentes cabeçalhos ou guias. O controle sublinha qualquer cabeçalho que tenha o foco, tornando mais óbvio qual cabeçalho está atualmente selecionado quando é usado um gamepad/controle remoto.

![Sublinhado dinâmico](images/designing-for-tv/pivot-underline.png)

Você pode definir a propriedade [Pivot.IsHeaderItemsCarouselEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabled.aspx) como `true` para que os pivôs sempre mantenham a mesma posição, em vez de fazer o cabeçalho dinâmico selecionado sempre se mover para a primeira posição. Essa é uma experiência melhor para exibições em tela grande, como a TV, pois a disposição do cabeçalho pode distrair os usuários. Se nem todos os cabeçalhos dinâmicos couberem na tela ao mesmo tempo, haverá uma barra de rolagem para permitir que os clientes vejam os outros cabeçalhos; entretanto, você deveria garantir que todos eles se encaixassem na tela para proporcionar a melhor experiência. Para saber mais, consulte [Guias e pivôs](../controls-and-patterns/tabs-pivot.md).

### <a name="navigation-pane-a-namenavigation-pane"></a>Painel de navegação <a name="navigation-pane">

Um painel de navegação (também conhecido como *menu hambúrguer*) é um controle de navegação frequentemente usado em aplicativos UWP. Normalmente é um painel com várias opções para seleção em um menu de estilo de lista que levará o usuário a páginas diferentes. Em geral, esse painel começa recolhido para economizar espaço, e o usuário pode abri-lo clicando em um botão.

Embora os painéis de navegação sejam muito acessíveis com mouse e toque, o gamepad/controle remoto os torna menos acessíveis, já que o usuário precisa navegar até um botão para abrir o painel. Portanto, uma boa prática é fazer o botão **Exibir** abrir o painel de navegação, além de permitir que o usuário o abra navegando até o lado esquerdo da página. Amostra de código sobre como implementar esse padrão de design pode ser encontrada no documento [Navegação por foco programática](../input/focus-navigation-programmatic.md#split-view-code-sample). Isso oferecerá ao usuário acesso muito fácil ao conteúdo do painel. Para obter mais informações sobre como os painéis de navegação se comportam em tamanhos de tela diferentes, assim como práticas recomendadas para a navegação com gamepad/controle remoto, consulte [Painéis de navegação](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Rótulos de CommandBar

É uma boa ideia ter os rótulos colocados à direita dos ícones em uma [CommandBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx), para que a altura seja minimizada e permaneça consistente. Você pode fazer isso definindo a propriedade [CommandBar.DefaultLabelPosition](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.commandbar.defaultlabelposition.aspx) como `CommandBarDefaultLabelPosition.Right`.

![CommandBar com rótulos à direita dos ícones](images/designing-for-tv/commandbar.png)

A configuração dessa propriedade também fará com que os rótulos sejam sempre exibidos, o que funciona bem para a experiência de 3 metros porque minimiza o número de cliques do usuário. Esse também é um modelo excelente para outros tipos de dispositivos seguirem.

### <a name="tooltip"></a>Dica de ferramenta

O controle [Tooltip](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) que foi introduzido como uma maneira de fornecer mais informações na interface do usuário quando o usuário passar o mouse ou tocar e segurar um elemento. Para gamepad e controle remoto, `Tooltip` é exibido após um breve momento quando o elemento recebe o foco, permanece na tela por um período curto de tempo e, em seguida, desaparece. Esse comportamento pode causar distração se muitos `Tooltip`s forem usados. Tente evitar o uso de `Tooltip` ao projetar para TV.

### <a name="button-styles"></a>Estilos de botão

Embora os botões padrão da UWP funcionem bem na TV, alguns estilos visuais de botões chamam mais a atenção para a interface do usuário, o que você pode considerar para todas as plataformas, especialmente na experiência de 3 metros, que se beneficia com a comunicação clara de onde o foco está localizado. Para ler mais sobre esses estilos, veja [Botões](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Elementos de interface do usuário aninhados

A interface do usuário aninhada expõe itens acionáveis aninhados dentro de um elemento de interface do usuário do contêiner onde o item aninhado, bem como o item de contêiner podem focar de forma independente umas nas outras.

A interface do usuário aninhada funciona bem para alguns tipos de entrada, mas nem sempre para gamepad e remoto, que dependem de navegação do plano XY. Certifique-se de seguir as orientações neste tópico para garantir que sua interface do usuário seja otimizada para o ambiente de 3 metros e que o usuário possa acessar facilmente todos os elementos interativos. Uma solução comum é colocar elementos de interface do usuário aninhada em um `ContextFlyout` (veja [CommandBar e ContextFlyout](#commandbar-and-contextflyout)).

Para obter mais informações sobre a interface do usuário aninhada, consulte [Interface do usuário aninhada em itens de lista](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

O elemento [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols.aspx) permite que os usuários interajam com sua mídia, fornecendo uma experiência de reprodução padrão que permite reproduzir, pausar, ativar as legendas ocultas e muito mais. Esse controle é uma propriedade de [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.aspx) e oferece suporte a duas opções de layout: *linha única* e *linha dupla*. No layout de linha única, os botões de reprodução e controle deslizante estão localizados em uma linha, com o botão Reproduzir/Pausar localizado à esquerda do controle deslizante. No layout de duas linhas, o controle deslizante ocupa sua própria linha, com os botões de reprodução em uma linha inferior separada. Ao projetar para a experiência de 3 metros, o layout de duas linhas deve ser usado, pois fornece navegação melhor para gamepad. Para habilitar o layout de duas linhas, defina `IsCompact="False"` no elemento `MediaTransportControls` na propriedade [TransportControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.transportcontrols.aspx) do `MediaPlayerElement`.

```xml
<MediaPlayerElement x:Name="mediaPlayerElement1"  
                    Source="Assets/video.mp4"
                    AreTransportControlsEnabled="True">
    <MediaPlayerElement.TransportControls>
        <MediaTransportControls IsCompact="False"/>
    </MediaPlayerElement.TransportControls>
</MediaPlayerElement>
```  

Visite [Reprodução de mídia](../controls-and-patterns/media-playback.md) para saber mais sobre adição de mídia ao seu aplicativo.

> ![NOTA] `MediaPlayerElement` só está disponível no Windows 10, versão 1607 e posterior. Se você estiver desenvolvendo um aplicativo para uma versão anterior do Windows 10, precisará usar [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926). As recomendações acima se aplicam a `MediaElement` e a propriedade `TransportControls` é acessada da mesma forma.

### <a name="search-experience"></a>Experiência de pesquisa

Procurar conteúdo é uma das funções mais comumente realizadas na experiência de 3 m. Se seu aplicativo fornece uma experiência de pesquisa, é útil para o usuário ter acesso rápido a ela usando o botão **Y** no gamepad como um acelerador.

A maioria dos clientes já deve estar familiarizada com esse acelerador, mas se você quiser, pode adicionar um glifo visual **Y** na interface do usuário para indicar que o cliente pode usar o botão para acessar a funcionalidade de pesquisa. Se você adicionar essa indicação, use o símbolo da fonte **Segoe Xbox MDL2 Symbol** (`&#xE3CC;` para apps XAML, `\E426` para apps HTML) para oferecer consistência com o shell do Xbox e outros apps.

> [!NOTE]
> Como a fonte **Segoe Xbox MDL2 Symbol** está disponível apenas no Xbox, o símbolo não será exibido corretamente em seu computador. No entanto, ele será exibido na TV após a implantação no Xbox.

Como o botão **Y** só está disponível no gamepad, forneça outros métodos de acesso para pesquisar, como botões na interface do usuário. Caso contrário, alguns clientes podem não ser capazes de acessar a funcionalidade.

Na experiência de 3 m, geralmente é mais fácil para os clientes usar uma experiência de pesquisa em tela inteira porque há espaço limitado na tela. Seja pesquisa em tela inteira, tela parcial ou "in-loco", recomendamos que, quando o usuário abrir a experiência de pesquisa, o teclado virtual apareça já aberto, pronto para o cliente inserir termos de pesquisa.

## <a name="custom-visual-state-trigger-for-xbox"></a>Gatilho de estado visual personalizado para Xbox

Para adaptar seu aplicativo UWP para a experiência de 3 metros, recomendamos que você faça alterações de layout quando o aplicativo detectar que foi iniciado em um console do Xbox. Uma maneira de fazer isso é usando um *gatilho de estado visual* personalizado. Os gatilhos de estado visual são mais úteis quando você deseja editar no **Blend for Visual Studio**. O trecho de código a seguir mostra como criar um gatilho de estado visual para Xbox:

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.OpenPaneLength"
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength"
                        Value="96"/>
                <Setter Target="NavMenuList.Margin"
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin"
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle"
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Para criar o gatilho, adicione a classe a seguir ao seu aplicativo. Esta é a classe que é referenciada pelo código XAML acima:

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }

        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

Depois que você tiver adicionando seu gatilho personalizado, seu aplicativo fará automaticamente as modificações de layout especificadas no código XAML sempre que ele detectar que está sendo executado em um console do Xbox One.

Uma outra maneira de você verificar se o seu aplicativo está em execução no Xbox e depois fazer os ajustes adequados é por meio de código. Você pode usar a seguinte variável simples para verificar se o seu aplicativo está em execução no Xbox:

```csharp
bool IsTenFoot = (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily ==
                    "Windows.Xbox");
```

Em seguida, você pode fazer os ajustes adequados à sua interface de usuário no bloco de código após essa verificação. Um exemplo disso é mostrado em [Amostra de cor UWP](#uwp-color-sample).

## <a name="summary"></a>Resumo

O design para a experiência de 3 metros tem algumas considerações especiais a serem levadas em conta que o diferenciam do design para qualquer outra plataforma. Embora você certamente faça uma portabilidade direta de seu aplicativo UWP para o Xbox One e ele funcionará, ele não necessariamente será otimizado para a experiência de 3 metros e isso pode causar frustração no usuário. Seguir as diretrizes deste artigo garantirá que o seu aplicativo seja tão bom quanto possa ser na TV.

## <a name="related-articles"></a>Artigos relacionados

- [Cartilha de dispositivos para aplicativos UWP (Plataforma Universal do Windows)](index.md)
- [Interações de Gamepad e de controle remoto](../input/gamepad-and-remote-interactions.md)
- [Som em aplicativos UWP](../style/sound.md)
