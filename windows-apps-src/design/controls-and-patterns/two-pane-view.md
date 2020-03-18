---
Description: TwoPaneView é um controle de layout que ajuda você a gerenciar a exibição de aplicativos que têm duas áreas distintas de conteúdo.
title: Exibição de dois painéis
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a070a72324408746f67b8814554160a76ee0ce4
ms.sourcegitcommit: e4b48989c91cd77ba73c90d9eb9cd67b88d52f21
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79191625"
---
# <a name="two-pane-view"></a>Exibição de dois painéis

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) é um controle de layout que ajuda você a gerenciar a exibição de aplicativos que têm duas áreas distintas de conteúdo, como exibição mestre/detalhe.

> [!IMPORTANT]
> Este artigo descreve a funcionalidade e as diretrizes que estão em versão prévia pública e podem ser substancialmente modificadas antes de passarem para a disponibilidade geral. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

Embora ele funcione em todos os dispositivos Windows, o controle TwoPaneView foi projetado para ajudar você a aproveitar ao máximo os dispositivos de tela dupla automaticamente, sem a necessidade de codificação especial. Em um dispositivo de tela dupla, a exibição de dois painéis garante que a interface do usuário seja dividida corretamente quando ela abranger o espaço entre as telas, de modo que o conteúdo seja apresentado em ambos os lados do espaço.

> [!NOTE]
> Um _dispositivo de tela dupla_ é um tipo especial de dispositivo com funcionalidades exclusivas. Não é equivalente a um dispositivo de desktop com vários monitores. Para obter mais informações sobre os dispositivos de tela dupla, confira [Introdução aos dispositivos de tela dupla](/dual-screen/introduction). (Confira [Mostrar várias exibições](/windows/uwp/design/layout/show-multiple-views) para obter mais informações sobre as maneiras de otimizar seu aplicativo para vários monitores.)

| Obter a biblioteca de interface do usuário do Windows |
| - |
| Este controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/). |

| APIs da plataforma | APIs da Biblioteca de Interface do Usuário do Windows |
| - | - |
| [Classe TwoPaneView](/uwp/api/windows.ui.xaml.controls.twopaneview) | [Classe TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) |

Neste documento, usaremos o alias **muxc** em XAML para representar a APIs da Biblioteca de interface do usuário do Windows que incluímos em nosso projeto. Adicionamos isso ao nosso elemento [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page):

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

No code-behind, também usaremos o alias **muxc** em C# para representar a APIs da Biblioteca de interface do usuário do Windows que incluímos em nosso projeto. Adicionamos essa instrução **using** na parte superior do arquivo:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use a exibição de dois painéis quando tiver duas áreas distintas de conteúdo e:

- O conteúdo precisar ser reorganizado e redimensionado automaticamente para se ajustar melhor à janela.
- A área secundária de conteúdo precisar ser mostrada/ocultada com base no espaço disponível.
- O conteúdo precisa ser dividido corretamente entre as duas telas de um dispositivo de tela dupla.

## <a name="examples"></a>Exemplos

Estas imagens mostram um aplicativo em execução em tela única e estendidos em telas duplas. A exibição de dois painéis adapta a interface do usuário do aplicativo às diversas configurações de tela.

![Aplicativo de exibição de dois painéis em tela única](images/two-pane-view/tpv-single.png)

> _Aplicativo em uma tela única._

![Aplicativo de exibição de dois painéis em telas duplas no modo horizontal](images/two-pane-view/tpv-dual-wide.png)

> _Aplicativo que abrange um dispositivo de tela dupla no modo horizontal._

![Aplicativo de exibição de dois painéis em telas duplas no modo vertical](images/two-pane-view/tpv-dual-tall.png)

> _Aplicativo que abrange um dispositivo de tela dupla no modo vertical._

## <a name="how-it-works"></a>Como funciona

Na exibição de dois painéis, há dois painéis em que você coloca o conteúdo. Ela ajusta o tamanho e a organização dos painéis, dependendo do espaço disponível para a janela. Os layouts de painel possíveis são definidos pela enumeração [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode):

| Valor&nbsp;da enumeração | Descrição |
| - | - |
| `SinglePane` | Somente um painel é mostrado, conforme especificado pela propriedade [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority). |
| `Wide` | Os painéis são mostrados lado a lado ou só um painel é mostrado, conforme especificado pela propriedade [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration). |
| `Tall` | Os painéis são mostrados de cima para baixo ou só um painel é mostrado, conforme especificado pela propriedade [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration). |

Configure a exibição de dois painéis definindo a [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para especificar qual painel será mostrado quando houver espaço para apenas um painel. Em seguida, especifique se `Pane1` é mostrado na parte superior ou inferior nas janelas verticais ou à esquerda ou à direita nas janelas horizontais.

A exibição de dois painéis lida com o tamanho e a organização dos painéis, mas você ainda precisa fazer com que o conteúdo dentro do painel se adapte às alterações no tamanho e na orientação. Confira [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml) e [Painéis de layout](/windows/uwp/design/layout/layout-panels) para obter mais informações sobre como criar uma interface do usuário adaptável.

O [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) gerencia a exibição dos painéis com base no estado de abrangência do aplicativo.

- Em uma tela única

    Quando o aplicativo estiver em tela única, o `TwoPaneView` ajustará o tamanho e a posição dos próprios painéis com base nas configurações de propriedade que você especificar. Explicaremos essas propriedades mais detalhadamente na próxima seção. A única diferença entre os dispositivos é que alguns dispositivos, como computadores desktop, permitem janelas redimensionáveis, enquanto outros dispositivos não.

- Estendido entre telas duplas

    O `TwoPaneView` foi projetado para facilitar a otimização da interface do usuário para ser estendida nos dispositivos com duas telas. A janela é dimensionada para usar todo o espaço disponível nas telas. Quando o aplicativo abranger ambas as telas de um dispositivo de tela dupla, cada tela exibirá o conteúdo de um dos painéis e abrangerá corretamente o conteúdo ao longo do espaço. O reconhecimento de abrangência é interno quando você usa a exibição de dois painéis. Você só precisa definir a configuração vertical/horizontal para especificar qual painel é mostrado em qual tela. A exibição de dois painéis cuida do resto.

## <a name="how-to-use-the-two-pane-view-control"></a>Como usar o controle de exibição de dois painéis

O [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) não precisa ser o elemento raiz do layout da página. Na verdade, você geralmente o usará dentro de um controle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) que fornece a navegação geral do aplicativo. O `TwoPaneView` se adapta de acordo, independentemente da localização dele na árvore XAML; no entanto, recomendamos que você não aninhe um `TwoPaneView` dentro de outro `TwoPaneView`. (Se você fizer isso, somente o `TwoPaneView` externo reconhecerá a extensão.)

### <a name="add-content-to-the-panes"></a>Adicionar conteúdo aos painéis

Cada painel de uma exibição de dois painéis pode conter um só `UIElement` XAML. Para adicionar conteúdo, você normalmente coloca um painel de layout XAML em cada painel e, em seguida, adiciona outros controles e o conteúdo ao painel. Os painéis podem ter o tamanho alterado e alternar entre os modos horizontal e vertical e, portanto, você precisará garantir que o conteúdo de cada painel possa se adaptar a essas alterações. Confira [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml) e [Painéis de layout](/windows/uwp/design/layout/layout-panels) para obter mais informações sobre como criar uma interface do usuário adaptável.

Este exemplo cria a interface do usuário do aplicativo de informações/imagens simples mostradas anteriormente na seção _Exemplos_. Quando o aplicativo é estendido entre telas duplas, a imagem e as informações são mostradas em telas separadas. Em uma única tela, o conteúdo pode ser mostrado em dois painéis ou combinado em apenas um painel, dependendo da quantidade de espaço disponível. (Quando só houver espaço para um painel, mova o conteúdo de Pane2 para Pane1 e permita que o usuário role a página para ver o conteúdo oculto. Você verá o código disso mais adiante na seção _Como responder às alterações de modo_.)

![Imagem pequena do aplicativo de exemplo distribuída em telas duplas](images/two-pane-view/tpv-left-right.png)

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" MinHeight="40"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <CommandBar DefaultLabelPosition="Right">
        <AppBarButton x:Name="Share" Icon="Share" Label="Share" Click="Share_Click"/>
        <AppBarButton x:Name="Print" Icon="Print" Label="Print" Click="Print_Click"/>
    </CommandBar>

    <muxc:TwoPaneView
        x:Name="MyTwoPaneView"
        Grid.Row="1"
        MinWideModeWidth="959"
        MinTallModeHeight="863"
        ModeChanged="TwoPaneView_ModeChanged">

        <muxc:TwoPaneView.Pane1>
            <Grid x:Name="Pane1Root">
                <ScrollViewer>
                    <StackPanel x:Name="Pane1StackPanel">
                        <Image Source="Assets\LandscapeImage8.jpg"
                               VerticalAlignment="Top" HorizontalAlignment="Center"
                               Margin="16,0"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane1>

        <muxc:TwoPaneView.Pane2>
            <Grid x:Name="Pane2Root">
                <ScrollViewer x:Name="DetailsContent">
                    <StackPanel Padding="16">
                        <TextBlock Text="Mountain.jpg" MaxLines="1"
                                       Style="{ThemeResource HeaderTextBlockStyle}"/>
                        <TextBlock Text="Date Taken:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="8/29/2019 9:55am"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Dimensions:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="800x536"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Resolution:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="96 dpi"
                                       Style="{ThemeResource SubtitleTextBlockStyle}"/>
                        <TextBlock Text="Description:"
                                       Style="{ThemeResource SubheaderTextBlockStyle}"
                                       Margin="0,24,0,0"/>
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Maecenas porttitor congue massa. Fusce posuere, magna sed pulvinar ultricies, purus lectus malesuada libero, sit amet commodo magna eros quis urna."
                                       Style="{ThemeResource SubtitleTextBlockStyle}"
                                       TextWrapping="Wrap"/>
                    </StackPanel>
                </ScrollViewer>
            </Grid>
        </muxc:TwoPaneView.Pane2>
    </muxc:TwoPaneView>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="TwoPaneViewStates">
            <VisualState x:Name="Normal"/>
            <VisualState x:Name="Wide">
                <VisualState.Setters>
                    <Setter Target="MyTwoPaneView.Pane1Length"
                            Value="2*"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="specify-which-pane-to-display"></a>Especificar qual painel será exibido

Quando a exibição de dois painéis puder exibir apenas um painel, ela usará a propriedade [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para determinar qual painel deverá ser exibido. Por padrão, PanePriority é definido como **Pane1**. Veja como você pode definir essa propriedade em XAML ou no código.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" PanePriority="Pane2">
```

```csharp
MyTwoPaneView.PanePriority = Microsoft.UI.Xaml.Controls.TwoPaneViewPriority.Pane2;
```

### <a name="pane-sizing"></a>Dimensionamento do painel

Em um dispositivo de tela única, o tamanho dos painéis é determinado pelas propriedades [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) e [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Elas usam valores de [GridLength](/uwp/api/windows.ui.xaml.gridlength) que dão suporte ao dimensionamento _automático_ e _em estrela_(\*). Confira a seção _Propriedades de layout_ de [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties) para obter uma explicação do dimensionamento automático e em estrela.

Por padrão, `Pane1Length` é definido como `Auto` e é dimensionado para se ajustar ao respectivo conteúdo. `Pane2Length` é definido como `*` e usa todo o espaço restante.

![Exibição de dois painéis com painéis definidos para tamanhos padrão](images/two-pane-view/tpv-size-default.png)

> _Painéis com dimensionamento padrão_

Os valores padrão são úteis para um layout típico de mestre/detalhe, no qual você tem uma lista de itens em `Pane1` e muitos detalhes em `Pane2`. No entanto, dependendo do conteúdo, talvez você prefira dividir o espaço de outra maneira. Aqui, `Pane1Length` é definido como `2*`, de modo que fica com duas vezes mais espaço que `Pane2`.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![Exibição de dois painéis com o painel 1 usando dois terços de tela e o painel 2 usando um terço](images/two-pane-view/tpv-size-2.png)

> _Painéis dimensionados 2* e *_

> [!NOTE]
> Conforme mencionado anteriormente, quando o aplicativo é estendido entre telas duplas, essas propriedades são ignoradas e cada painel preenche uma das telas.

Se você definir um painel para usar o dimensionamento automático, poderá controlar o tamanho definindo a altura e a largura do painel que apresenta o conteúdo do painel. Nesse caso, talvez seja necessário processar o evento ModeChanged e definir as restrições de altura e largura do conteúdo conforme apropriado do modo atual.

### <a name="display-in-wide-or-tall-mode"></a>Exibição no modo horizontal ou vertical

Quando usado em apenas uma tela, o [Modo](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) de exibição da exibição de dois painéis é determinado pelas propriedades [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) e [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Ambas as propriedades têm um valor padrão igual a 641 px, o mesmo que [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Esta tabela mostra como a altura e a largura de TwoPaneView determinam qual modo de exibição é usado.

| Condição de TwoPaneView  | Modo |
|---------|---------|
| `Width` > `MinWideModeWidth` | O modo `Wide` é usado |
| `Width` <= `MinWideModeWidth` e `Height` > `MinTallModeHeight` | O modo `Tall` é usado |
| `Width` <= `MinWideModeWidth` e `Height` <= `MinTallModeHeight` | O modo `SinglePane` é usado |


> [!NOTE]
> Conforme mencionado anteriormente, quando o aplicativo é estendido entre telas duplas, essas propriedades são ignoradas e o modo de exibição é determinado com base na _postura_ do dispositivo.

#### <a name="wide-configuration-options"></a>Opções de configuração de Wide

A exibição de dois painéis entra no modo `Wide` quando há uma só exibição que é mais larga do que a propriedade `MinWideModeWidth`. `MinWideModeWidth` controla quando a exibição de dois painéis entra no modo horizontal. O valor padrão é 641 px, mas você pode alterá-lo para qualquer outro desejado. Em geral, você deve definir essa propriedade com o valor desejado para a largura mínima do painel.

Quando a exibição de dois painéis está no modo horizontal, a propriedade [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration) determina o que é mostrado:

| [Valor&nbsp;da enumeração](/uwp/api/microsoft.ui.xaml.controls.twopaneviewwidemodeconfiguration) | Descrição |
|---------|---------|
| `SinglePane` | Um único painel (conforme determinado pelo `PanePriority`). O painel ocupa o tamanho original do `TwoPaneView` (ou seja, é dimensionado em estrela em ambas as direções). |
| `LeftRight` | `Pane1` à esquerda/`Pane2` à direita. Ambos os painéis são dimensionados em estrela verticalmente: a largura de `Pane1` é dimensionada automaticamente e a largura de `Pane2` é dimensionada em estrela. |
| `RightLeft` | `Pane1` à direita/`Pane2` à esquerda. Ambos os painéis são dimensionados em estrela verticalmente: a largura de `Pane2` é dimensionada automaticamente e a largura de `Pane1` é dimensionada em estrela. |

A configuração padrão é `LeftRight`.

| LeftRight | RightLeft |
| - | - |
| ![Exibição de dois painéis configurada da esquerda para a direita](images/two-pane-view/tpv-left-right.png)  | ![Exibição de dois painéis configurada da direita para a esquerda](images/two-pane-view/tpv-right-left.png)  |

> **DICA:** Quando o dispositivo usa uma linguagem RTL (da direita para esquerda), a exibição de dois painéis troca automaticamente a ordem: `RightLeft` é renderizado como `LeftRight` e `LeftRight` é renderizado como `RightLeft`.

#### <a name="tall-configuration-options"></a>Opções de configuração de Tall

A exibição de dois painéis entra no modo `Tall` quando há uma só exibição que é mais estreita do que `MinWideModeWidth` e mais alta que `MinTallModeHeight`. O valor padrão é 641 px, mas você pode alterá-lo para qualquer outro desejado. Em geral, você deve definir essa propriedade com o valor desejado para a altura mínima do painel.

Quando a exibição de dois painéis está no modo vertical, a propriedade [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration) determina o que é mostrado:

| [Valor&nbsp;da enumeração](/uwp/api/microsoft.ui.xaml.controls.twopaneviewtallmodeconfiguration) | Descrição |
|---------|---------|
| `SinglePane` | Um único painel (conforme determinado pelo `PanePriority`). O painel ocupa o tamanho original do `TwoPaneView` (ou seja, é dimensionado em estrela em ambas as direções). |
| `TopBottom` | `Pane1` na parte superior/`Pane2` na parte inferior. Ambos os painéis são dimensionados em estrela horizontalmente: a altura de `Pane1` é dimensionada automaticamente e a altura de `Pane2` é dimensionada em estrela. |
| `BottomTop` | `Pane1` na parte inferior/`Pane2` na parte superior. Ambos os painéis são dimensionados em estrela horizontalmente: a altura de `Pane2` é dimensionada automaticamente e a altura de `Pane1` é dimensionada em estrela. |

O padrão é `TopBottom`.

| TopBottom | BottomTop |
| - | - |
| ![Exibição de dois painéis configurada da parte superior para a inferior](images/two-pane-view/tpv-top-bottom.png)  | ![Exibição de dois painéis configurada da parte inferior para a superior](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valores especiais para MinWideModeWidth e MinTallModeHeight

Use a propriedade `MinWideModeWidth` para impedir que a exibição de dois painéis entre no modo horizontal: basta definir `MinWideModeWidth` como [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0).

Se você definir `MinTallModeHeight` como [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0), isso impedirá que a exibição de dois painéis entre no modo vertical.

Se você definir `MinTallModeHeight` como 0, isso impedirá que a exibição de dois painéis entre no modo `SinglePane`.

#### <a name="responding-to-mode-changes"></a>Como responder às alterações de modo

Use a propriedade [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) somente leitura para obter o modo de exibição atual. Sempre que a exibição de dois painéis altera quais painéis são exibidos, o evento [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) ocorre antes da renderização do conteúdo atualizado. Você pode processar o evento para responder às alterações no modo de exibição.

> [!TIP]
> O evento `ModeChanged` não ocorre quando a página é carregada inicialmente, portanto, o XAML padrão deve representar a interface do usuário da maneira como ela deve aparecer quando carregada pela primeira vez.

Uma maneira de usar esse evento é atualizar a interface do usuário do aplicativo, de modo que os usuários possam exibir todo o conteúdo no modo `SinglePane`. Por exemplo, o aplicativo de exemplo tem um painel primário (a imagem) e um painel de informações.

![Imagem pequena do aplicativo de exemplo distribuída no modo vertical](images/two-pane-view/tpv-top-bottom.png)

> _Modo vertical_

Quando só houver espaço suficiente para exibição de um painel, será possível mover o conteúdo de `Pane2` para `Pane1`, de modo que o usuário possa rolar a página para ver todo o conteúdo. Ela terá a aparência a seguir.

![Imagem do aplicativo de exemplo em uma tela com toda a rolagem de conteúdo em um único painel](images/two-pane-view/tpv-single-pane.png)

> _Modo SinglePane_

Lembre-se de que as propriedades `MinWideModeWidth` e `MinTallModeHeight` determinam quando o modo de exibição é alterado, de modo que você pode alterar quando o conteúdo é movido entre os painéis ajustando os valores dessas propriedades.

Aqui está o código do manipulador de eventos `ModeChanged` que move o conteúdo entre `Pane1` e `Pane2`. Ele também define um [VisualState](/uwp/api/windows.ui.xaml.visualstate) para restringir a largura da imagem em modo horizontal.

```csharp
private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
{
    // Remove details content from it's parent panel.
    ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
    // Set Normal visual state.
    Windows.UI.Xaml.VisualStateManager.GoToState(this, "Normal", true);

    // Single pane
    if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
    {
        // Add the details content to Pane1.
        Pane1StackPanel.Children.Add(DetailsContent);
    }
    // Dual pane.
    else
    {
        // Put details content in Pane2.
        Pane2Root.Children.Add(DetailsContent);

        // If also in Wide mode, set Wide visual state
        // to constrain the width of the image to 2*.
        if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
        {
            Windows.UI.Xaml.VisualStateManager.GoToState(this, "Wide", true);
        }
    }
}
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Use a exibição de dois painéis sempre que possível para que o aplicativo possa aproveitar as telas duplas e as telas grandes.
- Não coloque uma exibição de dois painéis dentro de outra exibição de dois painéis.

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral do layout](../layout/index.md)
- [Desenvolvimento de tela dupla](/dual-screen)
- [Introdução aos dispositivos de tela dupla](/dual-screen/introduction)
