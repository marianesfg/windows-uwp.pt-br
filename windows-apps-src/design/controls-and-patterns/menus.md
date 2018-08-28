---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: Menus e menus de contexto
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e38e9d61e8546d412cc30bad26680243f3a188e4
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2891489"
---
# <a name="menus-and-context-menus"></a>Menus e menus de contexto



Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita.

> **APIs importantes**: [classe MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [propriedade ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), [propriedade FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![Exemplo de um menu de contexto típico](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>Este é o controle correto?
Menus e menus de contexto economizam espaço organizando os comandos e ocultando-os até que o usuário precise deles. Se um determinado comando for usado com frequência e você tiver o espaço disponível, considere a possibilidade de colocá-lo diretamente em seu próprio elemento, em vez de em um menu, para que os usuários não precisem passar por um menu para acessá-lo.

Menus e menus de contexto servem para organizar comandos; para exibir conteúdo arbitrário, como uma notificação, ou para solicitar uma confirmação, use uma [caixa de diálogo ou um submenu](dialogs.md).  

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/MenuFlyout">abrir o aplicativo e ver o MenuFlyout em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus x menus de contexto

Os menus e os menus de contexto são idênticos em termos de aparência e do que podem conter. Na verdade, você pode usar o mesmo controle, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), para criá-los. A única diferença é como você permite que o usuário o acessa.

Quando você deve usar um menu ou um menu de contexto?
* Se o elemento host for um botão ou outro elemento de comando cuja função principal é apresentar comandos adicionais, use um menu.
* Se o elemento host for outro tipo de elemento que tenha outra finalidade principal (como apresentar texto ou imagem), use um menu de contexto.

Por exemplo, use um menu em um botão em um painel de navegação para fornecer opções adicionais de navegação. Nesse cenário, a principal finalidade do controle de botão é fornecer acesso a um menu.

![Exemplo de menu no Mail](images/Mail_Menu.png)

Se você quiser adicionar comandos (como recortar, copiar e colar) a um elemento de texto, use um menu de contexto em vez de um menu. Nesse cenário, a função principal do elemento de texto é apresentar e editar texto; os comandos adicionais (como recortar, copiar e colar) são secundários e pertencem a um menu de contexto.

![Exemplo de menu de contexto na galeria de fotos](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Menus</b></p>
<ul>
<li>Têm um único ponto de entrada (um menu Arquivo na parte superior da tela, por exemplo) que sempre é exibido.</li>
<li>Geralmente são ligados a um botão ou um item de menu pai.</li>
<li>São invocados por clique com o botão esquerdo (ou uma ação equivalente, como tocar com o dedo).</li><li>São associados um elemento por meio de suas propriedades <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx">Flyout</a> ou <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout</a>.</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>Menus de contexto</b></p>

<ul>
<li>São anexados a um único elemento e exibem comandos secundários.</li>
<li>São invocados clicando com o botão direito do mouse (ou uma ação equivalente, como pressionar e segurar com o dedo).</li><li>São associados a elemento por meio de sua propriedade <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout</a>.</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>Ícones

Considere fornecer ícones de item de menu para:

<ul>
<li> Os itens mais usados </li>
<li> Itens de menu cujo ícone é bem conhecidas ou padrão </li>
<li> Itens de menu cujo ícone ilustra o que faz o comando </li>
</ul>

Não se sentir obrigada a fornecer ícones para comandos que não têm uma visualização padrão. Os ícones criptografados não são úteis, pois criam poluição visual e impedem que os usuários se concentrem nos itens de menu importantes.

![Exemplo de menu de contexto com ícones](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````
> O tamanho dos ícones em MenuFlyoutItems é 16 x 16px. Se você usar SymbolIcon, FontIcon ou PathIcon, o ícone será dimensionado automaticamente para o tamanho correto sem perda de fidelidade. Se você usar BitmapIcon, certifique-se de que o ativo tem 16 x 16 px.  

## <a name="create-a-menu-or-a-context-menu"></a>Criar um menu ou um menu de contexto

Para criar um menu ou um menu de contexto, use a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Para definir o conteúdo do menu, adicione os objetos [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) e [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a MenuFlyout. Estes objetos servem para:
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) — Executar uma ação imediata.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—Ativar ou desativar uma opção.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) — Separar visualmente itens de menu.


Este exemplo cria uma [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) e usa a propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), uma propriedade disponível para a maioria dos controles, para mostrar a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como menu de contexto.

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

O próximo exemplo é praticamente idêntico, mas em vez de usar a propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) para mostrar a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como menu de contexto, o exemplo usa a propriedade [Showattachedflyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para mostrá-la como menu.

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````


> Os controles light dismiss, como menus, menus de contexto e outros submenus, prendem o foco do teclado ou gamepad dentro da interface do usuário transitória até serem ignorados. Para fornecer uma indicação visual para esse comportamento, os controles light dismiss no Xbox desenharão uma sobreposição que esmaece a visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a propriedade [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Por padrão, as interfaces do usuário transitórias desenham a sobreposição de ignorar no Xbox (**Automático**), mas não em outras famílias de dispositivos, mas os aplicativos podem optar por forçar a sobreposição como sempre **Ativada** ou **Desativada**.

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.
- [Amostra de menu de contexto XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artigos relacionados

- [Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
