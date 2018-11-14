---
author: QuinnRadich
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: Pivô
template: detail.hbs
ms.author: quradic
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 386fba3cec00de6c443daa60409fe3bb74621fa1
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "6471401"
---
# <a name="pivot"></a>Pivô

Permite que o controle [Pivot segundo](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) toque dedo entre um pequeno conjunto de seções de conteúdo.

> **APIs importantes**: [classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [classe NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Pivot">Abrir o aplicativo e ver o controle Pivot em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

O controle de pivô, assim como [NavigationView](navigationview.md), sublinha o item selecionado.

![Foco padrão sublinha cabeçalho selecionado](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Para alcançar comuns de navegação superior e padrões de guias, recomendamos usar o [NavigationView](navigationview.md), que se adapte a diferentes tamanhos de tela automaticamente e permite a personalização maior.

No entanto, se sua navegação requer o dedo por toque, recomendamos usar o pivô.

Principais diferenças entre os controles NavigationView e pivô são o comportamento de excedentes padrão e a API de navegação:

- Estouro carrosséis itens, enquanto o NavigationView usa um menu suspenso de estouro para que os usuários possam ver todos os itens de pivô.
- Pivot manipula a navegação entre as seções de conteúdo, enquanto NavigationView permite mais controle sobre o comportamento de navegação.

## <a name="use-navigationview-instead-of-pivot"></a>Usar o NavigationView em vez de pivô

Se a interface do usuário do seu aplicativo usa o controle de pivô, em seguida, você pode converter pivô para NavigationView com o código a seguir.

Este XAML cria um NavigationView com 3 seções de conteúdo, como o exemplo de pivô em [criar um controle de pivô](#create-a-pivot-control).

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView fornece mais controle sobre a personalização de navegação e requer código-behind correspondente. Para acompanhar o XAML acima, use o seguinte code-behind:

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

Esse código imita a experiência de navegação interna do controle de pivô, menos a experiência de toque-passar o dedo entre seções de conteúdo. No entanto, como você pode ver, você também pode personalizar vários pontos, incluindo a transição animada, parâmetros de navegação e recursos de pilha.

## <a name="create-a-pivot-control"></a>Criar um controle de pivô

Esse código cria um controle de pivô básico com 3 seções de conteúdo.

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Item de pivô

Pivot é um [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) e, por isso, pode conter uma coleção de itens de qualquer tipo. Qualquer item que você adicionar ao Pivô que não seja explicitamente um [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) será implicitamente encapsulado em um PivotItem. Como uma tabela dinâmica é frequentemente usada para navegar entre as páginas de conteúdo, é comum preencher a coleção [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) diretamente com elementos de XAML UI. Você também pode definir a propriedade [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) para uma fonte de dados. Itens vinculados a ItemsSource podem ser de qualquer tipo, mas se eles não forem explicitamente PivotItems, você deverá definir um [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) e [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar como os itens são exibidos.

Você pode usar a propriedade [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obter ou definir o item ativo do pivô. Use a propriedade [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obter ou definir o índice do item ativo.

### <a name="pivot-headers"></a>Cabeçalhos de pivô

Você pode usar as propriedades [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) e [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para adicionar outros controles ao cabeçalho do pivô.

Por exemplo, você pode adicionar um [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) em RightHeader do pivô.

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>Interação de pivô

O controle apresenta estas interações de gesto de toque:

- Tocar em um item de pivô navega para o conteúdo da seção do cabeçalho.
- Passar o dedo para a direita ou para a esquerda em um item de pivô navega para a seção adjacente.
- Passar o dedo para a direita ou para a esquerda no conteúdo da seção navega para a seção adjacente.

O controle é fornecido em dois modos:

**Estático**

- Pivôs são estáticos quando todos os cabeçalhos cabem dentro do espaço permitido.
- Tocar em um rótulo do pivô navega para a página correspondente, embora o próprio pivô não se mova. O pivô ativo é realçado.

**Carrossel**

- Pivôs giram quando todos os cabeçalhos não cabem no espaço permitido.
- Tocar em um rótulo do pivô navega para a página correspondente, e o rótulo do pivô ativo gira para a primeira posição.
- Os itens de pivô em um loop de carrossel da última à primeira seção de pivô.

> **Observação** Cabeçalhos de pivô não devem girar em um [ambiente de 3 metros](../devices/designing-for-tv.md). Defina a nova propriedade [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) como **false** se o aplicativo for executado no Xbox.

## <a name="recommendations"></a>Recomendações

- Evite usar mais do que cinco cabeçalhos ao usar o modo de carrossel (viagem de ida e volta), pois ter um número maior pode gerar confusão.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados

- [Classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Noções básicas de design de navegação](../basics/navigation-basics.md)