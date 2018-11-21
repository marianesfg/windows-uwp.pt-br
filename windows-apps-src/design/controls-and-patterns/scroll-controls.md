---
author: muhsinking
Description: Panning and scrolling allows users to reach content that extends beyond the bounds of the screen.
title: Controles do visualizador de rolagem
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 983f80c705aba44fe096f9dfa05b71b0cf28f294
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7436456"
---
# <a name="scroll-viewer-controls"></a>Controles do visualizador de rolagem



Quando há mais conteúdo de interface do usuário para mostrar do que pode se encaixar em uma área, use o controle de visualizador de rolagem.

> **APIs importantes**: [classe ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527), [classe ScrollBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.scrollbar.aspx)

Os visualizadores de rolagem habilitam conteúdo além dos limites do visor (área visível). Os usuários chegam a esse conteúdo manipulando a superfície do visualizador de rolagem por toque, pela roda do mouse, teclado ou um gamepad, ou usando o cursor do mouse ou caneta para interagir com a barra de rolagem do visualizador de rolagem. Esta imagem mostra vários exemplos de controles do visualizador de rolagem.

![Uma captura de tela que ilustra o controle de barra de rolagem padrão](images/ScrollBar_Standard.jpg)

Dependendo da situação, a barra de rolagem do visualizador de rolagem usa duas visualizações diferentes, mostradas na ilustração a seguir: o indicador de movimento panorâmico (à esquerda) e a barra de rolagem tradicional (à direita).

![Uma amostra da aparência dos controles de barra de rolagem e indicador de movimento panorâmico](images/SCROLLBAR.png)

O visualizador de rolagem reconhece o método de entrada do usuário e o usa para determinar qual visualização exibir.

* Quando a região é rolada sem manipulação direta da barra de rolagem, por exemplo, por toque, o indicador de movimento panorâmico é exibido, exibindo a posição de rolagem atual.
* Quando o cursor do mouse ou caneta se move sobre o indicador de movimento panorâmico, ele se transforma na barra de rolagem tradicional.  A área de rolagem é manipulada ao arrastar a barra de rolagem.

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![Barras de rolagem em ação](images/conscious-scroll.gif)

> [!NOTE]
> Quando a barra de rolagem fica visível, ela é sobreposta como 16 px sobre o conteúdo no ScrollViewer. Para garantir o bom design de experiência do usuário, você procura garantir que nenhum conteúdo interativo seja obscurecido pela sobreposição. Além disso, se você preferir não ter uma sobreposição de experiência do usuário, deixe 16 px de preenchimento na borda do visor para permitir a barra de rolagem.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/ScrollViewer">abrir o aplicativo e ver o ScrollViewer em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>Criar um visualizador de rolagem

Para adicionar a rolagem vertical à página, encapsule o conteúdo da página em um visualizador de rolagem.

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```

Este XAML mostra como colocar uma imagem em um visualizador de rolagem e habilitar o zoom.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>ScrollViewer em um modelo de controle

É comum que um controle ScrollViewer exista como uma parte composta de outros controles. Uma parte de ScrollViewer, junto com a classe [ScrollContentPresenter](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) para suporte, exibirá um visor com barras de rolagem somente quando o espaço de layout do controle host estiver sendo restrito com um tamanho menor que o do conteúdo expandido. Em geral, esse é o caso de listas, portanto, os modelos [ListView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) e [GridView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) sempre incluem um ScrollViewer. [TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) e [RichEditBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) também incluem um ScrollViewer em seus modelos.

Quando uma parte de **ScrollViewer** existe em um controle, o controle host muitas vezes tem manipulação de eventos interna certos eventos de entrada e interações que permitem que o conteúdo seja rolado. Por exemplo, um GridView interpreta um gesto de deslizar o dedo, e isso faz com que o conteúdo rolar horizontalmente. Os eventos de entrada e interações brutas que o controle host recebe são considerados como manipulados pelo controle, enquanto eventos de nível inferior, como [PointerPressed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) não serão gerado e não se propagarão para nenhum contêiner pai. Você pode alterar uma parte da manipulação de controle interna substituindo uma classe de controle e os métodos virtuais **On*** para eventos ou remodelando o controle. Porém, seja qual for o caso, não é simples reproduzir o comportamento padrão original, que geralmente é implementado para que o controle reaja a eventos e a ações de entrada de um usuário de maneiras esperadas. Portanto, você deve considerar se realmente precisa que esse evento seja disparado. Convém investigar se há outros eventos de entrada ou gestos de entrada que não estejam sendo manipulados pelo controle e usá-los no seu aplicativo ou no seu design de interação de controles.

Para que seja possível que controles que incluem um ScrollViewer influenciem alguns dos comportamentos e das propriedades provenientes da parte de ScrollViewer, ScrollViewer define uma série de propriedades XAML anexadas que podem ser definidas em estilos e usadas em associações de modelos. Para obter mais informações sobre propriedades anexadas, consulte [Visão geral das propriedades anexadas](../../xaml-platform/attached-properties-overview.md).

**Propriedades XAML anexadas de ScrollViewer**

ScrollViewer define as seguintes propriedades XAML anexadas:

- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx)
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx)
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx)
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx)
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx)
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx)
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

Essas propriedades XAML anexadas foram projetadas para casos em que o ScrollViewer está implícito, como quando o ScrollViewer existe no modelo padrão para um ListView ou GridView, e você deseja ser capaz de influenciar o comportamento de rolagem do controle sem acessar partes do modelo.

Por exemplo, veja como tornar as barras de rolagem vertical sempre visíveis para o visualizador de rolagem interno de um ListView.

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

Para os casos em que um ScrollViewer é explícito na sua XAML, conforme mostrado no exemplo de código, você não precisa usar a sintaxe de propriedade anexada. Basta usar a sintaxe de atributo, por exemplo `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`.


## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Sempre que possível, projete a rolagem vertical em vez de horizontal.
- Use o movimento panorâmico de eixo único para regiões de conteúdo que vão além do limite de um visor (vertical ou horizontal). Use o movimento panorâmico de dois eixos para regiões de conteúdo que vão além dos dois limites do visor (vertical e horizontal).
- Use a funcionalidade de rolagem integrada na exibição em lista, exibição de grade, caixa de combinação, caixa de lista, caixa de entrada de texto e controles de hub. Com aqueles controles, se há muitos itens para exibir, todos de uma vez, o usuário é capaz de fazer a rolagem horizontalmente ou então verticalmente pela lista de itens.
- Se você deseja que o usuário aplique movimento panorâmico em ambas as direções por uma área maior, além de possivelmente utilizar zoom também, por exemplo, se você deseja permitir que o usuário aplique movimento panorâmico e zoom em uma imagem de tamanho inteiro (em vez de uma imagem dimensionada para caber na tela), então posicione a imagem dentro de um visualizador de rolagem.
- Se o usuário fará a rolagem por uma longa passagem de texto, configure o visualizador de rolagem para fazer somente a rolagem vertical.
- Utilize um visualizador de rolagem para conter somente um objeto. Observe que esse objeto pode ser um painel de layout, que por sua vez pode conter em si qualquer determinado número de objetos.
- Não coloque um controle [Pivot](tabs-pivot.md) dentro de um visualizador de rolagem para evitar conflitos de rolagem de pivô.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados

**Para desenvolvedores (XAML)**

* [Classe ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527)
