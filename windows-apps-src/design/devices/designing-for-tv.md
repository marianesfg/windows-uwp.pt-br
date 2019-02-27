---
Description: Design your app so that it looks good and functions well on your television.
title: Projetando para Xbox e TV
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
keywords: Xbox, TV, experiência de 3 metros, gamepad, controle remoto, interação de entrada
ms.date: 11/13/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 788f47c1b29766cae1f437992aee8414580f3935
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116488"
---
# <a name="designing-for-xbox-and-tv"></a>Projetar para TV e Xbox

Projete seu aplicativo UWP (Plataforma Universal do Windows) para que ele tenha uma boa aparência e funcione bem no Xbox One e em telas de televisão.

Consulte [interações de Gamepad e controle remoto](../input/gamepad-and-remote-interactions.md) para obter orientações sobre as experiências de interação em aplicativos UWP na experiência de *10 pés* .

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
| [Dimensionamento de elemento de interface do usuário](#ui-element-sizing)  | A Plataforma Universal do Windows usa [dimensionamento e pixels eficazes](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para dimensionar a interface do usuário de acordo com a distância de exibição. Compreender o dimensionamento e aplicá-lo em sua interface do usuário ajudará você a otimizar seu aplicativo para o ambiente de 3 metros.  |
|  [Área segura para a TV](#tv-safe-area) | A UWP evitará automaticamente exibir qualquer interface do usuário em áreas não seguras para TV (áreas perto das bordas da tela) por padrão. No entanto, isso cria um efeito de "box-in" em que a interface do usuário parece estar em letterbox. Para que seu aplicativo seja realmente imersivo na TV, você deve modificá-lo para que ele se estenda para as bordas da tela em TVs compatíveis com esse recurso. |
| [Cores](#colors)  |  A UWP é compatível com temas de cores e um aplicativo que respeita o tema do sistema será padonizado como **escuro** no Xbox One. Se o seu aplicativo tiver um tema de cor específico, você deve considerar que algumas cores não funcionam bem na TV e devem ser evitadas. |
| [Som](../style/sound.md)    | Sons desempenham um papel fundamental na experiência de 3 metros, ajudando a imergir e enviar seus comentários para o usuário. A UWP fornece funcionalidade que ativa sons para controles comuns automaticamente quando o aplicativo é executado no Xbox One. Saiba mais sobre o suporte a som incorporado à UWP e saiba como tirar proveito dele.    |
| [Diretrizes de controles da interface do usuário](#guidelines-for-ui-controls)  |  Há vários controles de interface do usuário que funcionam bem em vários dispositivos, mas existem certas considerações quando usados na TV. Leia sobre algumas práticas recomendadas para usar esses controles ao projetar para a experiência de 3 metros. |
| [Gatilho de estado visual personalizado para Xbox](#custom-visual-state-trigger-for-xbox) | Para adaptar seu aplicativo UWP para a experiência de 3 metros, recomendamos que você use um *gatilho de estado visual* personalizado para fazer alterações de layout quando o app detectar que foi iniciado em um console Xbox. |

Além do anterior considerações de design e layout, há um número de otimizações [gamepad e a interação de controle remoto](../input/gamepad-and-remote-interactions.md) , que você deve considerar ao criar seu aplicativo.

| Recurso        | Descrição           |
| -------------------------------------------------------------- |--------------------------------|
| [Interação e navegação de foco do plano XY](../input/gamepad-and-remote-interactions.md#xy-focus-navigation-and-interaction) | **Navegação de foco do plano XY** permite que o usuário navegue na interface do usuário do seu aplicativo. No entanto, isso limita o usuário a navegar para cima, para baixo, para a esquerda e direita. Recomendações para lidar com isso e outras considerações são descritas nesta seção. |
| [Modo de mouse](../input/gamepad-and-remote-interactions.md#mouse-mode)|Navegação de foco do plano XY não é prático ou possível, para alguns tipos de aplicativos, como mapas ou desenho e aplicativos de pintura. Nesses casos, o **modo de mouse** permite aos usuários navegar livremente com um gamepad ou controle remoto, assim como um mouse em um computador.|
| [Foco visual](../input/gamepad-and-remote-interactions.md#focus-visual)  | O foco visual é uma borda que destaca o elemento focalizado no momento da interface do usuário. Isso ajuda o usuário a identificar rapidamente a interface do usuário são navegar pela ou interagir com.  |
| [Envolvimento de foco](../input/gamepad-and-remote-interactions.md#focus-engagement) | Envolvimento de foco exige que o usuário pressionar o botão **um/Select** em um gamepad ou controle remoto quando um elemento de interface do usuário tem o foco para interagir com ele. |
| [Botões de hardware](../input/gamepad-and-remote-interactions.md#hardware-buttons) | O gamepad e controle remoto fornecem configurações e botões muito diferentes. |

> [!NOTE]
> A maioria dos trechos de código deste tópico está em XAML/C#; entretanto, os princípios e conceitos se aplicam a todos os aplicativos UWP. Se você estiver desenvolvendo um aplicativo UWP HTML/JavaScript para Xbox, confira a excelente biblioteca [TVHelpers](https://github.com/Microsoft/TVHelpers/wiki) no GitHub.

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

Recomendamos que você use determinados elementos da interface do usuário para estender para as bordas da tela a fim de fornecer mais imersão para o usuário. Isso inclui [ScrollViewers](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [painéis de navegação](../controls-and-patterns/navigationview.md), e [CommandBars](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar).

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

Com essa linha de código, a janela do aplicativo se estenderá para as bordas da tela, portanto, você precisará mover todos os elementos interativos e essenciais da interface do usuário para a área de segurança para TV descrita anteriormente. A interface de usuário transitória, como menus de contexto e [ComboBoxes](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) abertas, permanecerá automaticamente dentro da área de segurança para TV.

![Limites da janela principal](images/designing-for-tv/core-window-bounds.png)

#### <a name="pane-backgrounds"></a>Telas de fundo do painel

Os painéis de navegação normalmente são desenhados perto da borda da tela, para que a tela de fundo se estenda na área insegura para TV, de forma a não inserir espaços estranhos. Para fazer isso, basta mudar a cor da tela de fundo do painel de navegação para a cor da tela de fundo do aplicativo.

O uso dos limites da janela principal conforme descrito anteriormente permitirá que você desenhe sua interface de usuário até as bordas da tela, mas, em seguida, você deve usar margens positivas no conteúdo da [SplitView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SplitView) para mantê-lo dentro da área de segurança para TV.

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

[CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) é outro exemplo de um painel que é normalmente posicionado perto de uma ou mais bordas do aplicativo e, assim como na TV, sua tela de fundo deve se estender até as bordas da tela. Ela geralmente contém um botão **Mais**, representado por "..." no lado direito, que deve permanecer na área de segurança para TV. A seguir estão algumas estratégias diferentes para obter as interações e os efeitos visuais desejados.

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

A UWP tem uma funcionalidade que mantém o foco visual dentro do [VisibleBounds](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview.visiblebounds), mas você precisa adicionar preenchimento para garantir que os itens de lista/grade possam rolar para a exibição da área de segurança. Especificamente, você adiciona uma margem positiva ao [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) ou [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) do [ItemsPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsPresenter), como no trecho de código a seguir:

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
> Este trecho de código é especificamente para `ListView`s; para um estilo de `GridView`, defina o atributo [TargetType](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate.targettype) como [ControlTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) e [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) como `GridView`.

Para um controle mais refinado sobre como itens são exibidos, se seu aplicativo for destinado ao versão 1803 ou posterior, você pode usar o [evento UIElement.BringIntoViewRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested). Você pode colocá-la no [ItemsPanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) de **ListView**/**GridView** para capturá-lo antes que o **ScrollViewer** interno, como os trechos de código a seguir:

```xaml
<GridView x:Name="gridView">
    <GridView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsWrapGrid Orientation="Horizontal"
                           BringIntoViewRequested="ItemsWrapGrid_BringIntoViewRequested"/>
        </ItemsPanelTemplate>
    </GridView.ItemsPanel>
</GridView>
```

```cs
// The BringIntoViewRequested event is raised by the framework when items receive keyboard (or Narrator) focus or 
// someone triggers it with a call to UIElement.StartBringIntoView.
private void ItemsWrapGrid_BringIntoViewRequested(UIElement sender, BringIntoViewRequestedEventArgs args)
{
    if (args.VerticalAlignmentRatio != 0.5)  // Guard against our own request
    {
        args.Handled = true;
        // Swallow this request and restart it with a request to center the item.  We could instead have chosen
        // to adjust the TargetRect’s Y and Height values to add a specific amount of padding as it bubbles up, 
        // but if we just want to center it then this is easier.

        // (Optional) Account for sticky headers if they exist
        var headerOffset = 0.0;
        var itemsWrapGrid = sender as ItemsWrapGrid;
        if (gridView.IsGrouping && itemsWrapGrid.AreStickyGroupHeadersEnabled)
        {
            var header = gridView.GroupHeaderContainerFromItemContainer(args.TargetElement as GridViewItem);
            if (header != null)
            {
                headerOffset = ((FrameworkElement)header).ActualHeight;
            }
        }

        // Issue a new request
        args.TargetElement.StartBringIntoView(new BringIntoViewOptions()
        {
            AnimationDesired = true,
            VerticalAlignmentRatio = 0.5, // a normalized alignment position (0 for the top, 1 for the bottom)
            VerticalOffset = headerOffset, // applied after meeting the alignment ratio request
        });
    }
}
```

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

Desde que seu aplicativo use um recurso de pincel, como **SystemControlForegroundAccentBrush**, ou um recurso de cor (**SystemAccentColor**), ou em vez disso, chame as cores de destaque diretamente por meio da API [UIColorType.Accent*](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType), essas cores são substituídas por cores de destaque disponíveis no Xbox One. Cores de pincel de alto contraste também são obtidas do sistema da mesma maneira que em um computador e um telefone.

Para saber mais sobre cores de destaque em geral, veja [Cor de destaque](../style/color.md#accent-color).

### <a name="color-variance-among-tvs"></a>Variação de cores entre TVs

Ao projetar para TV, observe que as cores são exibidas de forma bem diferente, dependendo da TV em que elas são renderizadas. Não pressuponha que as cores ficarão exatamente como aparecem no monitor. Se o seu aplicativo depende de diferenças sutis de cor para diferenciar partes da interface do usuário, as cores poderiam se misturar e os usuários ficariam confusos. Tente usar cores que sejam diferentes o suficiente para que os usuários possam claramente diferenciá-las, independentemente da TV que estiverem usando.

### <a name="tv-safe-colors"></a>Cores seguras para a TV

Os valores RGB de uma cor representam intensidades de vermelho, verde e azul. As TVs não lidam com intensidades extremas muito bem&mdash;elas podem produzir um efeito de faixa estranho ou aparecem desbotadas em determinadas TVs. Além disso, as cores de alta intensidade podem causar floração (pixels próximos começam a desenhar as mesmas cores). Embora existam diferentes escolas de pensamento em relação às cores consideradas seguras para a TV, as cores dentro dos valores RGB de 16-235 (ou 10-EB em hexadecimal) são geralmente seguras para uso em TV.

![Intervalo de cores seguras para TV](images/designing-for-tv/tv-safe-colors-2.png)

Historicamente, aplicativos no Xbox tinham que adaptar suas cores para ficar dentro desse intervalo de cor "seguro para a TV"; no entanto, a partir da atualização dos criadores do segundo semestre, o Xbox One dimensiona automaticamente o conteúdo de intervalo completo no intervalo seguro para a TV. Isso significa que a maioria dos desenvolvedores de aplicativos não precisa mais se preocupar com as cores seguras para a TV.

> [!IMPORTANT]
> O conteúdo de vídeo que já está no intervalo de cores seguro para a TV não tem esse efeito de escala de cor aplicado quando executado usando [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk).

Se você estiver desenvolvendo um aplicativo usando o DirectX 11 ou DirectX 12 e criando sua própria cadeia de troca para renderizar a interface do usuário ou vídeo, você pode especificar o espaço de cores que você usa chamando [IDXGISwapChain3::SetColorSpace1](https://docs.microsoft.com/windows/desktop/api/dxgi1_4/nf-dxgi1_4-idxgiswapchain3-setcolorspace1), que permitirá que o sistema saiba se ele precisa dimensionar as cores ou não.

## <a name="guidelines-for-ui-controls"></a>Diretrizes de controles da interface do usuário

Há vários controles de interface do usuário que funcionam bem em vários dispositivos, mas existem certas considerações quando usados na TV. Leia sobre algumas práticas recomendadas para usar esses controles ao projetar para a experiência de 3 metros.

### <a name="pivot-control"></a>Controle Pivot

Um [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) oferece navegação rápida de modos de exibição dentro de um aplicativo com a seleção de diferentes cabeçalhos ou guias. O controle sublinha qualquer cabeçalho que tenha o foco, tornando mais óbvio qual cabeçalho está atualmente selecionado quando é usado um gamepad/controle remoto.

![Sublinhado dinâmico](images/designing-for-tv/pivot-underline.png)

Você pode definir a propriedade [Pivot.IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.isheaderitemscarouselenabledproperty) como `true` para que os pivôs sempre mantenham a mesma posição, em vez de fazer o cabeçalho dinâmico selecionado sempre se mover para a primeira posição. Essa é uma experiência melhor para exibições em tela grande, como a TV, pois a disposição do cabeçalho pode distrair os usuários. Se nem todos os cabeçalhos dinâmicos couberem na tela ao mesmo tempo, haverá uma barra de rolagem para permitir que os clientes vejam os outros cabeçalhos; entretanto, você deveria garantir que todos eles se encaixassem na tela para proporcionar a melhor experiência. Para saber mais, consulte [Guias e pivôs](/windows/uwp/design/controls-and-patterns/pivot).

### <a name="navigation-pane-a-namenavigation-pane-"></a>Painel de navegação <a name="navigation-pane" />

Um painel de navegação (também conhecido como *menu hambúrguer*) é um controle de navegação frequentemente usado em aplicativos UWP. Normalmente é um painel com várias opções para seleção em um menu de estilo de lista que levará o usuário a páginas diferentes. Em geral, esse painel começa recolhido para economizar espaço, e o usuário pode abri-lo clicando em um botão.

Embora os painéis de navegação sejam muito acessíveis com mouse e toque, o gamepad/controle remoto os torna menos acessíveis, já que o usuário precisa navegar até um botão para abrir o painel. Portanto, uma boa prática é fazer o botão **Exibir** abrir o painel de navegação, além de permitir que o usuário o abra navegando até o lado esquerdo da página. Amostra de código sobre como implementar esse padrão de design pode ser encontrada no documento [Navegação por foco programática](../input/focus-navigation-programmatic.md#split-view-code-sample). Isso oferecerá ao usuário acesso muito fácil ao conteúdo do painel. Para obter mais informações sobre como os painéis de navegação se comportam em tamanhos de tela diferentes, assim como práticas recomendadas para a navegação com gamepad/controle remoto, consulte [Painéis de navegação](../controls-and-patterns/navigationview.md).

### <a name="commandbar-labels"></a>Rótulos de CommandBar

É uma boa ideia ter os rótulos colocados à direita dos ícones em uma [CommandBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar), para que a altura seja minimizada e permaneça consistente. Você pode fazer isso definindo a propriedade [CommandBar.DefaultLabelPosition](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.defaultlabelpositionproperty) como `CommandBarDefaultLabelPosition.Right`.

![CommandBar com rótulos à direita dos ícones](images/designing-for-tv/commandbar.png)

A configuração dessa propriedade também fará com que os rótulos sejam sempre exibidos, o que funciona bem para a experiência de 3 metros porque minimiza o número de cliques do usuário. Esse também é um modelo excelente para outros tipos de dispositivos seguirem.

### <a name="tooltip"></a>Dica de ferramenta

O controle [Tooltip](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip) que foi introduzido como uma maneira de fornecer mais informações na interface do usuário quando o usuário passar o mouse ou tocar e segurar um elemento. Para gamepad e controle remoto, `Tooltip` é exibido após um breve momento quando o elemento recebe o foco, permanece na tela por um período curto de tempo e, em seguida, desaparece. Esse comportamento pode causar distração se muitos `Tooltip`s forem usados. Tente evitar o uso de `Tooltip` ao projetar para TV.

### <a name="button-styles"></a>Estilos de botão

Embora os botões padrão da UWP funcionem bem na TV, alguns estilos visuais de botões chamam mais a atenção para a interface do usuário, o que você pode considerar para todas as plataformas, especialmente na experiência de 3 metros, que se beneficia com a comunicação clara de onde o foco está localizado. Para ler mais sobre esses estilos, veja [Botões](../controls-and-patterns/buttons.md).

### <a name="nested-ui-elements"></a>Elementos de interface do usuário aninhados

A interface do usuário aninhada expõe itens acionáveis aninhados dentro de um elemento de interface do usuário do contêiner onde o item aninhado, bem como o item de contêiner podem focar de forma independente umas nas outras.

A interface do usuário aninhada funciona bem para alguns tipos de entrada, mas nem sempre para gamepad e remoto, que dependem de navegação do plano XY. Certifique-se de seguir as orientações neste tópico para garantir que sua interface do usuário seja otimizada para o ambiente de 3 metros e que o usuário possa acessar facilmente todos os elementos interativos. Uma solução comum é colocar elementos de interface do usuário aninhada em um `ContextFlyout` (veja [CommandBar e ContextFlyout](#commandbar-and-contextflyout)).

Para obter mais informações sobre a interface do usuário aninhada, consulte [Interface do usuário aninhada em itens de lista](../controls-and-patterns/nested-ui.md).

### <a name="mediatransportcontrols"></a>MediaTransportControls

O elemento [MediaTransportControls](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaTransportControls) permite que os usuários interajam com sua mídia, fornecendo uma experiência de reprodução padrão que permite reproduzir, pausar, ativar as legendas ocultas e muito mais. Esse controle é uma propriedade de [MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) e oferece suporte a duas opções de layout: *linha única* e *linha dupla*. No layout de linha única, os botões de reprodução e controle deslizante estão localizados em uma linha, com o botão Reproduzir/Pausar localizado à esquerda do controle deslizante. No layout de duas linhas, o controle deslizante ocupa sua própria linha, com os botões de reprodução em uma linha inferior separada. Ao projetar para a experiência de 3 metros, o layout de duas linhas deve ser usado, pois fornece navegação melhor para gamepad. Para habilitar o layout de duas linhas, defina `IsCompact="False"` no elemento `MediaTransportControls` na propriedade [TransportControls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) do `MediaPlayerElement`.

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

> ![NOTA] `MediaPlayerElement` só está disponível no Windows 10, versão 1607 e posterior. Se você estiver desenvolvendo um aplicativo para uma versão anterior do Windows 10, precisará usar [MediaElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). As recomendações acima se aplicam a `MediaElement` e a propriedade `TransportControls` é acessada da mesma forma.

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
