---
Description: Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita.
title: Menus e menus de contexto
label: Menus and context menus
template: detail.hbs
ms.date: 01/08/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 194e8578244fee72d0f122ee96caaf22bf2b8807
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610481"
---
# <a name="menus-and-context-menus"></a>Menus e menus de contexto

Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita. Use um submenu do menu para mostrar um único, menu embutido. Use uma barra de menus para mostrar um conjunto de menus em uma linha horizontal, normalmente na parte superior de uma janela do aplicativo. Cada menu pode ter itens de menu e submenus.

![Exemplo de um menu de contexto típico](images/contextmenu_rs2_icons.png)

| **Obter a biblioteca de interface do usuário do Windows** |
| - |
| Esse controle é incluído como parte da biblioteca de interface do usuário do Windows, um pacote do NuGet que contém os novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, consulte o [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **APIs de plataforma** | **APIs da biblioteca de interface do usuário do Windows** |
| - | - |
| [Classe MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [classe da barra de menus](/uwp/api/windows.ui.xaml.controls.menubar), [propriedade ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [FlyoutBase.AttachedFlyout propriedade](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) | [Classe de barra de menus](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Menus e menus de contexto economizam espaço organizando os comandos e ocultando-os até que o usuário precise deles. Se um determinado comando for usado com frequência e você tiver o espaço disponível, considere a possibilidade de colocá-lo diretamente em seu próprio elemento, em vez de em um menu, para que os usuários não precisem passar por um menu para acessá-lo.

Menus e menus de contexto são para organizar os comandos; Para exibir conteúdo arbitrário, como uma solicitação de notificação ou confirmação, use uma [caixa de diálogo ou um submenu](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Barra de menus vs. MenuFlyout

Para mostrar um menu em um submenu anexado a um elemento de interface do usuário na tela, use o controle MenuFlyout para hospedar seus itens de menu. Você pode invocar um submenu do menu como um menu regular ou como um menu de contexto. Um submenu do menu hospeda um único menu de nível superior (e submenus opcionais).

Para mostrar um conjunto de vários menus de nível superior em uma linha horizontal, use uma barra de menus. Normalmente, você posicionar a barra de menus na parte superior da janela do aplicativo.

### <a name="menubar-vs-commandbar"></a>Barra de menus vs. CommandBar

Barra de menus e barra de comandos ambos representam as superfícies que você pode usar para expor comandos para seus usuários. Barra de menus fornece uma maneira rápida e simple para expor um conjunto de comandos para aplicativos que podem precisar de mais de organização ou agrupamento que permite um CommandBar.

Você também pode usar uma barra de menus em conjunto com uma barra de comandos. Use a barra de menus para fornecer a maior parte dos comandos e a barra de comandos para realçar os comandos mais usados.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/MenuFlyout">abrir o aplicativo e ver o MenuFlyout em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo da Galeria de controles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus x menus de contexto

Menus e menus de contexto são semelhantes em sua aparência e o que eles contêm. Na verdade, você pode usar o mesmo controle [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), criá-los. A diferença é como você permitir que o usuário acessá-lo.

Quando você deve usar um menu ou um menu de contexto?

- Se o elemento host for um botão ou outro elemento de comando cuja função principal é apresentar comandos adicionais, use um menu.
- Se o elemento host for outro tipo de elemento que tenha outra finalidade principal (como apresentar texto ou imagem), use um menu de contexto.

Por exemplo, use um menu em um botão para fornecer filtragem e opções para obter uma lista de classificação. Nesse cenário, a principal finalidade do controle de botão é fornecer acesso a um menu.

![Exemplo de menu no Mail](images/Mail_Menu.png)

Se você quiser adicionar comandos (como recortar, copiar e colar) a um elemento de texto, use um menu de contexto em vez de um menu. Nesse cenário, a função principal do elemento de texto é apresentar e editar texto; os comandos adicionais (como recortar, copiar e colar) são secundários e pertencem a um menu de contexto.

![Exemplo de menu de contexto na galeria de fotos](images/ContextMenu_example.png)

### <a name="menus"></a>Menus:

- Têm um único ponto de entrada (um menu Arquivo na parte superior da tela, por exemplo) que sempre é exibido.
- Geralmente são ligados a um botão ou um item de menu pai.
- São invocados por clique com o botão esquerdo (ou uma ação equivalente, como tocar com o dedo).
- Estão associados um elemento por meio de seu [submenu](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) ou [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) propriedades, ou agrupados de uma barra de menus na parte superior da janela do aplicativo.

### <a name="context-menus"></a>Menus de contexto

- São anexados a um único elemento e exibem comandos secundários.
- São invocados clicando com o botão direito do mouse (ou uma ação equivalente, como pressionar e segurar com o dedo).
- São associados a elemento por meio de sua propriedade [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).

## <a name="icons"></a>Ícones

Considere fornecer ícones de item de menu para:

- O mais usado itens.
- Itens de menu cuja ícone é standard ou bem conhecidas.
- Itens de menu cuja ícone também ilustra o que faz o comando.

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

> [!TIP]
> O tamanho do ícone em um MenuFlyoutItem é 16x16px. Se você usar SymbolIcon, FontIcon ou PathIcon, o ícone é dimensionado automaticamente para o tamanho correto sem perda de fidelidade. Se você usar BitmapIcon, certifique-se de que o ativo tem 16 x 16 px.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Criar um submenu do menu ou um menu de contexto

Para criar um submenu do menu ou um menu de contexto, você deve usar o [MenuFlyout classe](https://msdn.microsoft.com/library/windows/apps/dn299030). Para definir o conteúdo do menu, adicione os objetos [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) e [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a MenuFlyout.

Estes objetos servem para:

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx) — Executar uma ação imediata.
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx)—Ativar ou desativar uma opção.
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) — Separar visualmente itens de menu.

Este exemplo cria um [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) e usa o [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) propriedade, uma propriedade disponível para a maioria dos controles, para mostrar o MenuFlyout como um menu de contexto.

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

### <a name="light-dismiss"></a>Ignorar a luz

Os controles light dismiss, como menus, menus de contexto e outros submenus, prendem o foco do teclado ou gamepad dentro da interface do usuário transitória até serem ignorados. Para fornecer uma indicação visual para esse comportamento, os controles light dismiss no Xbox desenharão uma sobreposição que esmaece a visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a propriedade [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). Por padrão, as interfaces do usuário transitórias desenham a sobreposição de ignorar no Xbox (**Automático**), mas não em outras famílias de dispositivos, mas os aplicativos podem optar por forçar a sobreposição como sempre **Ativada** ou **Desativada**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Criar uma barra de menus

> [!IMPORTANT]
> Barra de menus requer o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou o [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Você pode usar os mesmos elementos para criar menus em uma barra de menus, como em um submenu do menu. No entanto, em vez de agrupar objetos MenuFlyoutItem em um MenuFlyout, você agrupá-los em um elemento MenuBarItem. Cada MenuBarItem é adicionado à barra de menus, como um menu de nível superior.

![Exemplo de uma barra de menus](images/menu-bar-submenu.png)

> [!NOTE]
> Este exemplo mostra apenas como criar a estrutura de interface do usuário, mas não mostra a implementação de qualquer um dos comandos.

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator/>
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.
- [Exemplo de menu de contexto de XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artigos relacionados

- [Classe MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [Classe de barra de menus](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
