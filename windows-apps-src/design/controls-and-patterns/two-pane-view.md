---
Description: TwoPaneView é um controle de layout que ajuda você a gerenciar a exibição de aplicativos que têm duas áreas distintas de conteúdo.
title: Exibição de dois painéis
template: detail.hbs
ms.date: 01/22/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67b97aec970cc655700729743f10c63c666ab0a6
ms.sourcegitcommit: 09571e1c6a01fabed773330aa7ead459a47d94f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76929258"
---
# <a name="two-pane-view"></a>Exibição de dois painéis

[TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) é um controle de layout que ajuda você a gerenciar a exibição de aplicativos que têm duas áreas distintas de conteúdo, como exibição mestre/detalhe.

> [!IMPORTANT]
> Este artigo descreve a funcionalidade e as diretrizes que estão em versão prévia pública e podem ser substancialmente modificadas antes de passarem para a disponibilidade geral. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

Embora ele funcione em todos os dispositivos Windows, o controle TwoPaneView foi projetado para ajudar você a aproveitar ao máximo os dispositivos de tela dupla automaticamente, sem a necessidade de codificação especial. Em um dispositivo de tela dupla, a exibição de dois painéis garante que a interface do usuário seja dividida corretamente quando ela abranger o espaço entre as telas, de modo que o conteúdo seja apresentado em ambos os lados do espaço.

> **OBSERVAÇÃO:** Um _dispositivo de tela dupla_ é um tipo especial de dispositivo com funcionalidades exclusivas. Não é equivalente a um dispositivo de desktop com vários monitores. Para obter mais informações sobre os dispositivos de tela dupla, confira [Introdução aos dispositivos de tela dupla](/dual-screen/introduction).
>
>Neste artigo, usamos os termos _dispositivo de tela única_ ou _exibição de tela única_ para indicar qualquer dispositivo que não seja um dispositivo de tela dupla, seja um monitor individual ou parte de uma configuração de vários monitores. O controle TwoPaneView apresenta o mesmo comportamento de outros controles XAML em uma tela única. Confira [Mostrar várias exibições](/windows/uwp/design/layout/show-multiple-views) para obter mais informações sobre as maneiras de otimizar seu aplicativo para vários monitores.

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Este controle está incluído como parte da biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](/uwp/toolkits/winui/). |

| **APIs da plataforma** | **APIs da biblioteca de interface do usuário do Windows** |
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

Estas imagens mostram um aplicativo em execução em um dispositivo de tela única e em um dispositivo de tela dupla. A exibição de dois painéis adapta a interface do usuário do aplicativo a várias configurações de painel em cada dispositivo.

![tpv-single.png](images/two-pane-view/tpv-single.png)

_Aplicativo em um dispositivo de tela única._

![tpv-dual-wide.png](images/two-pane-view/tpv-dual-wide.png)

_Aplicativo que abrange um dispositivo de tela dupla no modo horizontal._

![tpv-dual-tall.png](images/two-pane-view/tpv-dual-tall.png)

_Aplicativo que abrange um dispositivo de tela dupla no modo vertical._

## <a name="how-it-works"></a>Como funciona

Na exibição de dois painéis, há dois painéis em que você coloca o conteúdo. Ela ajusta o tamanho e a organização dos painéis, dependendo do espaço disponível para a janela. Os layouts de painel possíveis são definidos pela enumeração [TwoPaneViewMode](/uwp/api/microsoft.ui.xaml.controls.twopaneviewmode):

- **SinglePane**: somente um painel é mostrado, conforme especificado pela propriedade [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).
- **Wide**: os painéis são mostrados lado a lado ou só um painel é mostrado, conforme especificado pela propriedade [WideModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.widemodeconfiguration).
- **Tall**: os painéis são mostrados de cima para baixo ou só um painel é mostrado, conforme especificado pela propriedade [TallModeConfiguration](/uwp/api/microsoft.ui.xaml.controls.twopaneview.tallmodeconfiguration).

Configure a exibição de dois painéis definindo a [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority) para especificar qual painel será mostrado quando houver espaço para apenas um painel. Em seguida, especifique se Pane1 é mostrado na parte superior ou inferior nas janelas verticais ou à esquerda ou à direita nas janelas horizontais.

A exibição de dois painéis lida com o tamanho e a organização dos painéis, mas você ainda precisa fazer com que o conteúdo dentro do painel se adapte às alterações no tamanho e na orientação. Confira [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml) e [Painéis de layout](/windows/uwp/design/layout/layout-panels) para obter mais informações sobre como criar uma interface do usuário adaptável.

A exibição de dois painéis gerencia a exibição dos painéis com base no tipo de dispositivo em que o aplicativo está sendo executado:

- Em um dispositivo de tela dupla

    A exibição de dois painéis foi projetada para facilitar a otimização da interface do usuário em dispositivos de tela dupla. A janela será dimensionada para usar todo o espaço disponível nas telas. Quando o aplicativo estiver em apenas uma das telas do dispositivo, só um painel será exibido, conforme especificado pela propriedade [PanePriority](/uwp/api/microsoft.ui.xaml.controls.twopaneview.panepriority).

    Quando o aplicativo abranger ambas as telas de um dispositivo de tela dupla, cada tela exibirá o conteúdo de um dos painéis e abrangerá corretamente o conteúdo ao longo do espaço. O reconhecimento de abrangência é interno quando você usa a exibição de dois painéis. Você só precisa definir a configuração vertical/horizontal para especificar qual painel é mostrado em qual tela. A exibição de dois painéis cuida do resto.


- Em dispositivos de tela única

    Durante a execução em um dispositivo de tela única, como um laptop ou um computador desktop, a exibição de dois painéis fornece um comportamento que você esperaria de qualquer controle XAML. Quando a janela é redimensionada, a exibição de dois painéis ajusta o tamanho e a posição dos painéis com base no tamanho da janela. Ela tem propriedades adicionais configuradas para definir o comportamento do controle quando ele é mostrado em uma só tela em uma janela redimensionável. Explicaremos essas propriedades mais detalhadamente na próxima seção.

## <a name="how-to-use-the-two-pane-view-control"></a>Como usar o controle de exibição de dois painéis

O [TwoPaneView](/uwp/api/microsoft.ui.xaml.controls.twopaneview) não precisa ser o elemento raiz do layout da página. Na verdade, você geralmente o usará dentro de um controle [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview) que fornece a navegação geral do aplicativo. O TwoPaneView se adapta de acordo, independentemente da localização dele na árvore XAML; no entanto, recomendamos que você não aninhe um TwoPaneView dentro de outro TwoPaneView.

### <a name="add-content-to-the-panes"></a>Adicionar conteúdo aos painéis

Cada painel de uma exibição de dois painéis pode conter um só UIElement XAML. Para adicionar conteúdo, você normalmente coloca um painel de layout XAML em cada painel e, em seguida, adiciona outros controles e o conteúdo ao painel. Os painéis podem ter o tamanho alterado e alternar entre os modos horizontal e vertical e, portanto, você precisará garantir que o conteúdo de cada painel possa se adaptar a essas alterações. Confira [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml) e [Painéis de layout](/windows/uwp/design/layout/layout-panels) para obter mais informações sobre como criar uma interface do usuário adaptável.

Este exemplo cria a interface do usuário do aplicativo de informações/imagens simples mostrada aqui. Quando há espaço para dois painéis, a imagem e as informações são mostradas em painéis separados. (Quando só houver espaço para um painel, mova o conteúdo de Pane2 para Pane1 e permita que o usuário role a página para ver o conteúdo oculto. Você verá o código disso mais adiante na seção _Como responder às alterações de modo_.)

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

```xaml
<muxc:TwoPaneView
    Pane1Length="*"
    ModeChanged="TwoPaneView_ModeChanged">

    <muxc:TwoPaneView.Pane1>
        <Grid x:Name="Pane1Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <CommandBar x:Name="MyCommandBar" DefaultLabelPosition="Right">
                <AppBarButton x:Name="Share" Icon="Share" Label="Share"/>
                <AppBarButton x:Name="Print" Icon="Print" Label="Print"/>
            </CommandBar>

            <ScrollViewer Grid.Row="1">
                <StackPanel x:Name="Pane1StackPanel">
                    <Image x:Name="TheImage" Source="Assets\LandscapeImage8.jpg"
                           VerticalAlignment="Top" HorizontalAlignment="Center" 
                           Margin="16,0"/>
                </StackPanel>
            </ScrollViewer>

        </Grid>
    </muxc:TwoPaneView.Pane1>

    <muxc:TwoPaneView.Pane2>
        <Grid x:Name="Pane2Root">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" MinHeight="40"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <StackPanel x:Name="DetailsContent" Grid.Row="1"
                Orientation="Vertical" Padding="16">

                <TextBlock Text="Mountain.jpg" Margin="0,0,0,12"
                   FontWeight="SemiBold" FontSize="18"/>

                <TextBlock Text="Date Taken:" FontWeight="SemiBold"/>
                <TextBlock Text="8/29/2019 9:55am" Margin="0,0,0,12"/>

                <TextBlock Text="Dimensions:" FontWeight="SemiBold"/>
                <TextBlock Text="1000x750" Margin="0,0,0,12"/>

                <TextBlock Text="Resolution:" FontWeight="SemiBold"/>
                <TextBlock Text="96 dpi" Margin="0,0,0,12"/>

                <TextBlock Text="Description:" FontWeight="SemiBold"/>
                <TextBlock TextWrapping="Wrap" 
                           Text="Lorem ipsum dolor sit amet."/>
            </StackPanel>
        </Grid>
    </muxc:TwoPaneView.Pane2>
</muxc:TwoPaneView>
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

O tamanho dos painéis em um dispositivo de tela única é determinado pelas propriedades [Pane1Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane1length) e [Pane2Length](/uwp/api/microsoft.ui.xaml.controls.twopaneview.pane2length). Elas usam valores de [GridLength](/uwp/api/windows.ui.xaml.gridlength) que dão suporte ao dimensionamento _automático_ e _em estrela_(\*). Confira a seção _Propriedades de layout_ de [Layouts dinâmicos com XAML](/windows/uwp/design/layout/layouts-with-xaml#layout-properties) para obter uma explicação do dimensionamento automático e em estrela.

Por padrão, Pane1Length é definido como **Auto** e é dimensionado para se ajustar ao conteúdo. Pane2Length é definido como * e usa todo o espaço restante.

![tpv-size-default.png](images/two-pane-view/tpv-size-default.png)

_Painéis com dimensionamento padrão_

Os valores padrão são úteis para um layout típico de mestre/detalhe, no qual você tem uma lista de itens em Pane1 e muitos detalhes em Pane2. No entanto, dependendo do conteúdo, talvez você prefira dividir o espaço de outra maneira. Aqui, Pane1Length é definido como 2* e, portanto, ele obtém duas vezes mais espaço que Pane2.

```xaml
<muxc:TwoPaneView x:Name="MyTwoPaneView" Pane1Length="2*">
```

![tpv-size-2.png](images/two-pane-view/tpv-size-2.png)

_Painéis dimensionados 2* e *_

> **OBSERVAÇÃO:** conforme mencionado anteriormente, em um dispositivo de tela dupla, essas propriedades são ignoradas, e os painéis são dimensionados e organizados automaticamente com base na _posição_ do dispositivo.

Se você definir um painel para usar o dimensionamento automático, poderá controlar o tamanho definindo a altura e a largura do painel que apresenta o conteúdo do painel. Nesse caso, talvez seja necessário processar o evento ModeChanged e definir as restrições de altura e largura do conteúdo conforme apropriado do modo atual.

### <a name="display-in-wide-or-tall-mode"></a>Exibição no modo horizontal ou vertical

Em uma exibição da área de trabalho, o [Modo](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) de exibição da exibição de dois painéis é determinado pelas propriedades [MinWideModeWidth](/uwp/api/microsoft.ui.xaml.controls.twopaneview.minwidemodewidth) e [MinTallModeHeight](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mintallmodeheight). Ambas as propriedades têm um valor padrão igual a 641 px, o mesmo que [NavigationView.CompactThresholdWidth](/uwp/api/windows.ui.xaml.controls.navigationview.compactmodethresholdwidth).

Se a janela for:

- Mais larga do que MinWideModeWidth, o modo **Wide** será usado.
- Mais estreita do que MinWideModeWidth e mais alta do que MinTallModeHeight, o modo **Tall** será usado.
- Mais estreita do que MinWideModeWidth e mais curta do que MinTallModeHeight, o modo **SinglePane** será usado.

> **OBSERVAÇÃO:** conforme mencionado anteriormente, em um dispositivo de tela dupla, essas propriedades são ignoradas, e os painéis são dimensionados e organizados automaticamente com base na _posição_ do dispositivo.

#### <a name="wide-configuration-options"></a>Opções de configuração de Wide

A exibição de dois painéis entra no modo Wide quando há uma só exibição que é mais larga do que a propriedade MinWideModeWidth. MinWideModeWidth controla quando a exibição de dois painéis entra no modo horizontal. O valor padrão é 641 px, mas você pode alterá-lo para qualquer outro desejado. Em geral, você deve definir essa propriedade com o valor desejado para a largura mínima do painel.

Quando a exibição de dois painéis está no modo horizontal, a propriedade WideModeConfiguration determina o que é mostrado:

- **SinglePane**: um só painel (conforme determinado por PanePriority). O painel ocupa o tamanho original do TwoPaneView (ou seja, é dimensionado em estrela em ambas as direções).
- **LeftRight**: Pane1 à esquerda/Pane2 à direita. Ambos os painéis são dimensionados em estrela verticalmente: a largura de Pane1 é dimensionada automaticamente, e a largura de Pane2 é dimensionada em estrela.
- **RightLeft**: Pane1 à direita/Pane2 à esquerda. Ambos os painéis são dimensionados em estrela verticalmente: a largura de Pane2 é dimensionada automaticamente, e a largura de Pane1 é dimensionada em estrela.

A configuração padrão é **LeftRight**.

| LeftRight | RightLeft |
| - | - |
| ![tpv-left-right.png](images/two-pane-view/tpv-left-right.png)  | ![tpv-right-left.png](images/two-pane-view/tpv-right-left.png)  |

> **DICA:** quando o dispositivo usa uma linguagem da direita para esquerda, a exibição de dois painéis troca automaticamente a ordem: RightLeft é renderizado como LeftRight, e LeftRight é renderizado como RightLeft.

#### <a name="tall-configuration-options"></a>Opções de configuração de Tall

A exibição de dois painéis entra no modo vertical quando há uma exibição mais estreita do que MinWideModeWidth e mais alta do que MinTallModeHeight. O valor padrão é 641 px, mas você pode alterá-lo para qualquer outro desejado. Em geral, você deve definir essa propriedade com o valor desejado para a altura mínima do painel.

Quando a exibição de dois painéis está no modo horizontal, a propriedade TallLayout determina o que é mostrado:

- **SinglePane**: um só painel (conforme determinado por PanePriority). O painel ocupa o tamanho original do TwoPaneView (ou seja, é dimensionado em estrela em ambas as direções).
- **TopBottom**: Pane1 na parte superior/Pane2 na parte inferior. Ambos os painéis são dimensionados em estrela horizontalmente: a altura de Pane1 é dimensionada automaticamente, e a altura de Pane2 é dimensionada em estrela.
- **BottomTop**: Pane1 na parte inferior/Pane2 na parte superior. Ambos os painéis são dimensionados em estrela horizontalmente: a altura de Pane2 é dimensionada automaticamente, e a altura de Pane1 é dimensionada em estrela.

O padrão é **TopBottom**.

| TopBottom | BottomTop |
| - | - |
| ![tpv-top-bottom.png](images/two-pane-view/tpv-top-bottom.png)  | ![tpv-bottom-top.png](images/two-pane-view/tpv-bottom-top.png)  |

#### <a name="special-values-for-minwidemodewidth-and-mintallmodeheight"></a>Valores especiais para MinWideModeWidth e MinTallModeHeight

Use a propriedade MinWideModeWidth para impedir que a exibição de dois painéis entre no modo Wide: basta definir MinWideModeWidth como [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0).

Se você definir MinTallModeHeight como [Double.PositiveInfinity](/dotnet/api/system.double.positiveinfinity?view=dotnet-uwp-10.0), isso impedirá que a exibição de dois painéis entre no modo Tall.

Se você definir MinTallModeHeight como 0, isso impedirá que a exibição de dois painéis entre no modo SinglePane.

#### <a name="responding-to-mode-changes"></a>Como responder às alterações de modo

Use a propriedade [Mode](/uwp/api/microsoft.ui.xaml.controls.twopaneview.mode) somente leitura para obter o modo de exibição atual. Sempre que a exibição de dois painéis altera quais painéis são exibidos, o evento [ModeChanged](/uwp/api/microsoft.ui.xaml.controls.twopaneview.modechanged) ocorre antes da renderização do conteúdo atualizado. Você pode processar o evento para responder às alterações no modo de exibição.

Uma maneira de usar esse evento é atualizar a interface do usuário do aplicativo, de modo que os usuários possam exibir todo o conteúdo no modo SinglePane. Por exemplo, o aplicativo de exemplo tem um painel primário (a imagem) e um painel de informações.

![tpv-add-content.png](images/two-pane-view/tpv-add-content.png)

_Modo Wide_

Quando só houver espaço suficiente para exibição de um painel, mova o conteúdo de Pane2 para Pane1, de modo que o usuário possa rolar a página para ver todo o conteúdo. Ela terá a aparência a seguir.

![tpv-mode-change.png](images/two-pane-view/tpv-mode-change.png)

_Modo SinglePane_

```csharp
 private void TwoPaneView_ModeChanged(Microsoft.UI.Xaml.Controls.TwoPaneView sender, object args)
 {
     ((Panel)DetailsContent.Parent).Children.Remove(DetailsContent);
     ((Panel)MyCommandBar.Parent).Children.Remove(MyCommandBar);

     // Single pane
     if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.SinglePane)
     {
         // Add the command bar and details content to Pane1.
         Pane1StackPanel.Children.Add(DetailsContent);
         Pane1Root.Children.Add(MyCommandBar);
     }
     // Dual pane.
     else
     {
         // Wide mode.
         if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Wide)
         {
             // Put the command bar in Pane2.
             Pane2Root.Children.Add(MyCommandBar);
         }
         // Tall mode.
         else if (sender.Mode == Microsoft.UI.Xaml.Controls.TwoPaneViewMode.Tall)
         {
             // Put the command bar in Pane1
             Pane1Root.Children.Add(MyCommandBar);
         }

         // Put details content in Pane2.
         Pane2Root.Children.Add(DetailsContent);
     }
 }
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Use a exibição de dois painéis sempre que possível, para que o aplicativo possa aproveitar as telas duplas e as telas grandes.
- Não coloque uma exibição de dois painéis dentro de outra exibição de dois painéis.

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral do layout](../layout/index.md)
- [Desenvolvimento de tela dupla](/dual-screen)
- [Introdução aos dispositivos de tela dupla](/dual-screen/introduction)
