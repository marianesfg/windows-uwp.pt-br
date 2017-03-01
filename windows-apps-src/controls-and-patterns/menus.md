---
author: mijacobs
Description: "Um submenu é um pop-up leve que é usado para mostrar temporariamente a interface do usuário relacionada ao que o usuário está fazendo no momento."
title: Menus e menus de contexto
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 515c63d5612358cf90684427f8f747e19384c6ff
ms.lasthandoff: 02/08/2017

---
# <a name="menus-and-context-menus"></a>Menus e menus de contexto

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita.

![Exemplo de um menu de contexto típico](images/controls_contextmenu_singlepane.png)

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)</li>
<li>[Propriedade ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx)</li>
<li>[Propriedade FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)</li>
</ul>
</div>


## <a name="is-this-the-right-control"></a>Esse é o controle correto?
Menus e menus de contexto economizam espaço organizando os comandos e ocultando-os até que o usuário precise deles. Se um determinado comando for usado com frequência e você tiver o espaço disponível, considere a possibilidade de colocá-lo diretamente em seu próprio elemento, em vez de em um menu, para que os usuários não precisem passar por um menu para acessá-lo. 

Menus e menus de contexto servem para organizar comandos; para exibir conteúdo arbitrário, como uma notificação, ou para solicitar uma confirmação, use uma [caixa de diálogo ou um submenu](dialogs.md).  


## <a name="menus-vs-context-menus"></a>Menus x menus de contexto

Os menus e os menus de contexto são idênticos em termos de aparência e do que podem conter. Na verdade, você pode usar o mesmo controle, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), para criá-los. A única diferença é como você permite que o usuário o acessa. 

Quando você deve usar um menu ou um menu de contexto?
* Se o elemento host for um botão ou outro elemento de comando cuja função principal é apresentar comandos adicionais, use um menu.
* Se o elemento host for outro tipo de elemento que tenha outra finalidade principal (como apresentar texto ou imagem), use um menu de contexto. 

Por exemplo, use um menu em um botão em um painel de navegação para fornecer opções adicionais de navegação. Nesse cenário, a principal finalidade do controle de botão é fornecer acesso a um menu. 

Se você quiser adicionar comandos (como recortar, copiar e colar) a um elemento de texto, use um menu de contexto em vez de um menu. Nesse cenário, a função principal do elemento de texto é apresentar e editar texto; os comandos adicionais (como recortar, copiar e colar) são secundários e pertencem a um menu de contexto. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Menus:</b></p>
<p>
<ul>
<li>Têm um único ponto de entrada (um menu Arquivo na parte superior da tela, por exemplo) que sempre é exibido.</li>
<li>Geralmente são ligados a um botão ou um item de menu pai.</li>
<li>São invocados por clique com o botão esquerdo (ou uma ação equivalente, como tocar com o dedo).</li>  
<li>São associados um elemento por meio de suas propriedades [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) ou [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx).</li> 
</ul>
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Menus de contexto</b></p>
   
<ul>
<li>São ligados a um único elemento, mas só são acessíveis quando o contexto faz sentido.</li>
<li>São invocados clicando com o botão direito (ou uma ação de equivalente, como pressionar e segurar com o dedo).</li>
<li>São associados a elemento por meio de sua propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).  
</ul><br/>

  </div>
</div>
</div>

## <a name="create-a-menu-or-a-context-menu"></a>Criar um menu ou um menu de contexto

Para criar um menu ou um menu de contexto, use a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Para definir o conteúdo do menu, adicione os objetos [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) e [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a MenuFlyout. Estes objetos servem para:
* [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) — Executar uma ação imediata.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—Ativar ou desativar uma opção.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) — Separar visualmente itens de menu.


Este exemplo cria uma [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) e usa a propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), uma propriedade disponível para a maioria dos controles, para mostrar a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como menu de contexto.

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
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

O próximo exemplo é praticamente idêntico, mas em vez de usar a propriedade [ContextFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) para mostrar a [classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como menu de contexto, o exemplo usa a propriedade [Showattachedflyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para mostrá-la como menu. 

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


> Os controles light dismiss, como menus, menus de contexto e outros submenus, prendem o foco do teclado ou gamepad dentro da interface do usuário transitória até serem ignorados. Para fornecer uma indicação visual para esse comportamento, os controles light dismiss no Xbox desenharão uma sobreposição que esmaece a visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a nova propriedade [LightDismissOverlayMode](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Por padrão, interfaces do usuário transitórias desenham a sobreposição light dismiss no Xbox, mas não em outras famílias de dispositivos, mas os apps podem optar por forçar a sobreposição para estar sempre **Ativada** ou **Desativada**.

> ```xaml
> <MenuFlyout LightDismissOverlayMode=\"Off\">
> ```

## <a name="get-the-sample-code"></a>Obter o código de exemplo
*   [Amostra de noções básicas de interface do usuário XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

- [**Classe MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)

