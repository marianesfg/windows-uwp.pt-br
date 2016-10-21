---
author: Jwmsft
Description: "O movimento panorâmico e a rolagem permitem que usuários acessem conteúdos que se estendem além das fronteiras da tela."
title: Diretrizes para barras de rolagem
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scroll bars
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 3dd5912bdd210751257bb9e495c5a95ce0be20a5

---
# Barras de rolagem

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

O movimento panorâmico e a rolagem permitem que usuários acessem conteúdos que se estendem além das fronteiras da tela.

Um controle do visualizador de rolagem é composto de tanto conteúdo quanto caberá no visor do aplicativo, além de uma ou então duas barras de rolagem. Gestos de toque podem ser utilizados para aplicar movimento panorâmico e zoom (as barras de rolagem desaparecem somente durante a manipulação) e o ponteiro pode ser utilizado para rolagem. O gesto de movimento rápido aplica movimento panorâmico com inércia.

**Observação**  O Windows tem duas visualizações do controle de rolagem, que se baseiam no modo de entrada do usuário: os indicadores de rolagem durante o uso de touch ou gamepad, além de barras de rolagem interativas para outros dispositivos de entrada, inclusive mouse, teclado e caneta.

![Uma amostra da aparência dos controles de barra de rolagem e indicador de movimento panorâmico](images/SCROLLBAR.png)


<div class="important-apis" >
<b>APIs Importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209527"><strong>Classe ScrollViewer</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.scrollbar.aspx"><strong>Classe ScrollBar</strong></a></li>
</ul>

</div>
</div>






## Exemplos

Um [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) permite que o conteúdo seja exibido em uma área menor que o tamanho real. Quando o conteúdo do visualizador de rolagem não é totalmente visível, esse visualizador mostra barras de rolagem que o usuário pode usar para mover a área de conteúdo que está visível. A área que inclui todo o conteúdo do visualizador de rolagem é a *extensão*. A área visível do conteúdo é o *visor*.

![Uma captura de tela que ilustra o controle de barra de rolagem padrão](images/ScrollBar_Standard.jpg)

## Criar um visualizador de rolagem
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

## ScrollViewer em um modelo de controle

É comum que um controle ScrollViewer exista como uma parte composta de outros controles. Uma parte de ScrollViewer, junto com a classe [**ScrollContentPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) para suporte, exibirá um visor com barras de rolagem somente quando o espaço de layout do controle host estiver sendo restrito com um tamanho menor que o do conteúdo expandido. Geralmente, esse é o caso de listas e, portanto, modelos [**ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) e [**GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) sempre incluem um ScrollViewer. [**TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) e [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) também incluem um ScrollViewer em seus modelos.

Quando uma parte de **ScrollViewer** existe em um controle, o controle host muitas vezes tem manipulação de eventos interna certos eventos de entrada e interações que permitem que o conteúdo seja rolado. Por exemplo, um GridView interpreta um gesto de deslizar o dedo, e isso faz com que o conteúdo rolar horizontalmente. Os eventos de entrada e interações brutas que o controle host recebe são considerados como manipulados pelo controle, enquanto eventos de nível inferior, como [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) não serão gerado e não se propagarão para nenhum contêiner pai. Você pode alterar uma parte da manipulação de controle interna substituindo uma classe de controle e os métodos virtuais **On*** para eventos ou remodelando o controle. Porém, seja qual for o caso, não é simples reproduzir o comportamento padrão original, que geralmente é implementado para que o controle reaja a eventos e a ações de entrada de um usuário de maneiras esperadas. Portanto, você deve considerar se realmente precisa que esse evento seja disparado. Convém investigar se há outros eventos de entrada ou gestos de entrada que não estejam sendo manipulados pelo controle e usá-los no seu aplicativo ou no seu design de interação de controles.

Para que seja possível que controles que incluem um ScrollViewer influenciem alguns dos comportamentos e das propriedades provenientes da parte de ScrollViewer, ScrollViewer define uma série de propriedades XAML anexadas que podem ser definidas em estilos e usadas em associações de modelos. Para obter mais informações sobre propriedades anexadas, consulte [Visão geral das propriedades anexadas](../xaml-platform/attached-properties-overview.md).

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


## Recomendações

-   Sempre que possível, projete a rolagem vertical em vez de horizontal.
-   Use o movimento panorâmico de eixo único para regiões de conteúdo que vão além do limite de um visor (vertical ou horizontal). Use o movimento panorâmico de dois eixos para regiões de conteúdo que vão além dos dois limites do visor (vertical e horizontal).
-   Use a funcionalidade de rolagem integrada na exibição em lista, exibição de grade, caixa de combinação, caixa de lista, caixa de entrada de texto e controles de hub. Com aqueles controles, se há muitos itens para exibir, todos de uma vez, o usuário é capaz de fazer a rolagem horizontalmente ou então verticalmente pela lista de itens.
-   Se você deseja que o usuário aplique movimento panorâmico em ambas as direções por uma área maior, além de possivelmente utilizar zoom também, por exemplo, se você deseja permitir que o usuário aplique movimento panorâmico e zoom em uma imagem de tamanho inteiro (em vez de uma imagem dimensionada para caber na tela), então posicione a imagem dentro de um visualizador de rolagem.
-   Se o usuário fará a rolagem por uma longa passagem de texto, configure o visualizador de rolagem para fazer somente a rolagem vertical.
-   Utilize um visualizador de rolagem para conter somente um objeto. Observe que esse objeto pode ser um painel de layout, que por sua vez pode conter em si qualquer determinado número de objetos.
-   Não coloque um controle [Pivot](tabs-pivot.md) dentro de um visualizador de rolagem para evitar conflitos de rolagem de pivô.

## Tópicos relacionados

**Para desenvolvedores (XAML)**
* [**Classe ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)



<!--HONumber=Aug16_HO3-->


