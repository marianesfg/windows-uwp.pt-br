---
Description: Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita.
title: Menus e menus de contexto
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b008b12c5f92d56c127c5ec8026d305d3d57a869
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081678"
---
# <a name="menus-and-context-menus"></a>Menus e menus de contexto

Menus e menus de contexto exibem uma lista de comandos ou opções quando o usuário os solicita. Use o submenu de menu para exibir um menu único embutido. Use a barra de menus para exibir um conjunto de menus em uma linha horizontal, normalmente na parte superior de uma janela do aplicativo. Todo menu pode ter itens de menu e submenus.

![Exemplo de um menu de contexto típico](images/contextmenu_rs2_icons.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **MenuBar** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para saber obter mais informações, incluindo instruções de instalação, confira a [visão geral da biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da Biblioteca de interface do usuário do Windows:** [Classe MenuBar](/uwp/api/microsoft.ui.xaml.controls.menubar)
>
> **APIs de plataforma:** [classe MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [classe MenuBar](/uwp/api/windows.ui.xaml.controls.menubar), [propriedade ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [propriedade FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Menus e menus de contexto economizam espaço organizando os comandos e ocultando-os até que o usuário precise deles. Se um determinado comando for usado com frequência e você tiver o espaço disponível, considere a possibilidade de colocá-lo diretamente em seu próprio elemento, em vez de em um menu, para que os usuários não precisem passar por um menu para acessá-lo.

Os menus e os menus de contexto servem para organizar comandos; para exibir um conteúdo arbitrário, como uma notificação ou solicitação de confirmação, use uma [caixa de diálogo ou um submenu](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>MenuBar versus MenuFlyout

Para exibir um menu em um submenu anexado a um elemento da interface do usuário na tela, use o controle MenuFlyout para hospedar os itens do menu. É possível invocar um submenu de menu como um menu regular ou como um menu de contexto. Um submenu de menu hospeda um único menu de nível superior (e submenus opcionais).

Para exibir um conjunto de menus de nível superior em uma linha horizontal, use uma barra de menus. Normalmente, você posiciona a barra de menus na parte superior da janela do aplicativo.

### <a name="menubar-vs-commandbar"></a>MenuBar versus CommandBar

MenuBar e CommandBar representam as superfícies que você pode usar para expor comandos aos usuários. A MenuBar fornece uma maneira simples e rápida de exibir um conjunto de comandos de aplicativos que podem precisar de mais agrupamento ou organização do que o CommandBar permite.

Você também pode usar o MenuBar em conjunto com o CommandBar. A MenuBar pode ser usada para fornecer a maior parte dos comandos e o CommandBar para destacar os comandos mais usados.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/MenuFlyout">abri-lo e ver o MenuFlyout em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menus x menus de contexto

Os menus e os menus de contexto são semelhantes em termos de aparência e do que podem conter. Na verdade, é possível usar o mesmo controle, [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), para criá-los. A única diferença é como você permite que o usuário o acesse.

Quando você deve usar um menu ou um menu de contexto?

- Se o elemento host for um botão ou outro elemento de comando cuja função principal é apresentar comandos adicionais, use um menu.
- Se o elemento host for outro tipo de elemento que tenha outra finalidade principal (como apresentar texto ou imagem), use um menu de contexto.

Por exemplo, use um menu em um botão para fornecer opções de filtro e classificação para uma lista. Nesse cenário, a principal finalidade do controle de botão é fornecer acesso a um menu.

![Exemplo de menu no email](images/Mail_Menu.png)

Se você quiser adicionar comandos (como recortar, copiar e colar) a um elemento de texto, use um menu de contexto em vez de um menu. Nesse cenário, a função principal do elemento de texto é apresentar e editar texto; os comandos adicionais (como recortar, copiar e colar) são secundários e pertencem a um menu de contexto.

![Exemplo de um menu de contexto na galeria de fotos](images/ContextMenu_example.png)

### <a name="menus"></a>Menus

- Têm um único ponto de entrada (um menu Arquivo na parte superior da tela, por exemplo) que sempre é exibido.
- Geralmente são ligados a um botão ou um item de menu pai.
- São invocados por clique com o botão esquerdo (ou uma ação equivalente, como tocar com o dedo).
- São associados a um elemento por meio das propriedades [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout) ou [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) ou agrupados em uma barra de menus na parte superior da janela do aplicativo.

### <a name="context-menus"></a>Menus de contexto

- São anexados a um único elemento e exibem comandos secundários.
- São invocados clicando com o botão direito do mouse (ou por meio de uma ação equivalente, como pressionar e segurar com o dedo).
- São associados a elemento por meio de sua propriedade [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

## <a name="icons"></a>Ícones

Considere fornecer ícones de item de menu para:

- Os itens mais usados.
- Os itens de menu cujo ícone é bem conhecido ou padrão.
- Os itens de menu cujo ícone ilustra bem o que faz o comando.

Não se sinta obrigado a fornecer ícones para comandos que não têm uma visualização padrão. Os ícones criptografados não são úteis, criam poluição visual e impedem que os usuários se concentrem nos itens de menu importantes.

![Exemplo de um menu de contexto com ícones](images/contextmenu_rs2_icons.png)

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
> O tamanho dos ícones no MenuFlyoutItem é 16 x 16px. Se você usar SymbolIcon, FontIcon ou PathIcon, o ícone será dimensionado automaticamente para o tamanho correto sem perda de fidelidade. Se você usar BitmapIcon, certifique-se de que o ativo seja de 16 x 16px.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Criar um submenu de menu ou um menu de contexto

Para criar um submenu de menu ou um menu de contexto, use a [classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout). Para definir o conteúdo do menu, adicione os objetos [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) e [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) à MenuFlyout.

Estes objetos servem para:

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem) — Executar uma ação imediata.
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem)— com uma lista em cascata dos itens de menu.
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)—Ativar ou desativar uma opção.
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)— alternância entre itens de menu mutuamente exclusivos.
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) — Separar visualmente itens de menu.

Este exemplo cria uma [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) e usa a propriedade [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout), que é uma propriedade disponível para a maioria dos controles, para mostrar a MenuFlyout como um menu de contexto.

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

O próximo exemplo é praticamente idêntico, mas em vez de usar a propriedade [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) para mostrar a [classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) como menu de contexto, o exemplo usa a propriedade [Showattachedflyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para mostrá-la como menu.

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

### <a name="light-dismiss"></a>Light dismiss

Os controles light dismiss, como menus, menus de contexto e outros submenus, prendem o foco do teclado ou do gamepad dentro da interface do usuário transitória até que esse seja ignorado. Para fornecer uma indicação visual para esse comportamento, os controles light dismiss no Xbox desenharão uma sobreposição que esmaece a visibilidade da interface do usuário fora do escopo. Esse comportamento pode ser modificado com a propriedade [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode). Por padrão, as interfaces do usuário transitórias desenham a sobreposição light dismiss no Xbox (**Auto**), mas não em outras famílias de dispositivos. Você pode optar por forçar que a sobreposição esteja sempre **ativada** ou sempre **desativada**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Criar uma barra de menus

> [!IMPORTANT]
> A MenuBar exige o Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou posterior, ou a [biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Você pode usar os mesmos elementos para criar menus em uma barra de menus, como em um submenu de menu. No entanto, em vez de agrupar os objetos MenuFlyoutItem em um MenuFlyout, agrupe-os em um elemento MenuBarItem. Cada MenuBarItem é adicionado ao MenuBar como um menu de nível superior.

![Exemplo de uma barra de menus](images/menu-bar-submenu.png)

> [!NOTE]
> Este exemplo mostra apenas como criar a estrutura da interface do usuário, mas não mostra a implementação de nenhum comando.

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.
- [Exemplo do menu de contexto XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artigos relacionados

- [Classe MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [Classe MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
