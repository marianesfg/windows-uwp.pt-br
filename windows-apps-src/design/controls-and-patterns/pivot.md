---
Description: O controle Pivot permite a passagem de toque entre um pequeno conjunto de seções de conteúdo.
title: Pivô
template: detail.hbs
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6d55c24dbf84e16e0bb4dedc9eaf2b89982e2993
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081617"
---
# <a name="pivot"></a>Pivô

O controle [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) permite a passagem de toque entre um pequeno conjunto de seções de conteúdo.

![Foco padrão sublinha cabeçalho selecionado](images/pivot_focus_selectedHeader.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | A Biblioteca de interface do usuário do Windows 2.2 ou posterior inclui um novo modelo para esse controle que usa cantos arredondados. Para obter mais informações, confira [Raio de canto](/windows/uwp/design/style/rounded-corner). WinUI é um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos UWP. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da plataforma**: [classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot), [classe NavigationView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Pivot">abri-lo e ver o controle Pivot em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

O controle Pivot, assim como [NavigationView](navigationview.md), sublinha o item selecionado.

![Foco padrão sublinha cabeçalho selecionado](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Para alcançar os padrões comuns de navegação superior e guias, recomendamos o uso de [NavigationView](navigationview.md), que se adapta automaticamente a diferentes tamanhos de tela e permite uma maior personalização.

No entanto, se sua navegação requer entrada por toque, é recomendável usar o controle Pivot.

As outras diferenças importantes entre os controles NavigationView e Pivot são o comportamento de estouro padrão e a API de navegação:

- Os carrosséis dinâmicos estouram itens, enquanto o NavigationView usa um menu suspenso para que os usuários possam ver todos os itens.
- O controle Pivot manipula a navegação entre seções de conteúdo, enquanto o NavigationView permite mais controle sobre o comportamento da navegação.

## <a name="use-navigationview-instead-of-pivot"></a>Usar o NavigationView em vez do Pivot

Se a interface de usuário do seu aplicativo usa o controle Pivot, em seguida, você pode converter Pivot em NavigationView com o código a seguir.

Esse XAML cria um NavigationView com 3 seções de conteúdo, como o exemplo de Pivot em [Criar um controle dinâmico](#create-a-pivot-control).

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

O NavigationView fornece mais controle sobre a personalização de navegação e requer o code-behind correspondente. Para acompanhar o XAML acima, use o seguinte code-behind:

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

Esse código simula a experiência de navegação interna do controle Pivot, menos a experiência de entrada por toque entre seções de conteúdo. No entanto, como você pode ver, você também pode personalizar vários pontos, incluindo a transição animada, os parâmetros de navegação e os recursos de pilha.

## <a name="create-a-pivot-control"></a>Criar um controle de pivô

Esse código cria um controle Pivot básico com três seções de conteúdo.

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

O Pivot é um [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) e, por isso, pode conter uma coleção de itens de qualquer tipo. Qualquer item que você adicionar ao Pivot que não seja explicitamente um [PivotItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PivotItem) será implicitamente encapsulado em um PivotItem. Como um Pivot é frequentemente usado para navegar entre as páginas de conteúdo, é comum preencher a coleção [Items](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.items) diretamente com elementos de XAML UI. Você também pode definir a propriedade [ItemsSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) para uma fonte de dados. Os itens vinculados a ItemsSource podem ser de qualquer tipo, mas se eles não forem explicitamente PivotItems, você deverá definir um [ItemTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) e [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.headertemplate) para especificar como os itens são exibidos.

Você pode usar a propriedade [SelectedItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selecteditem) para obter ou definir o item ativo do Pivot. Use a propriedade [SelectedIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.selectedindex) para obter ou definir o índice do item ativo.

### <a name="pivot-headers"></a>Cabeçalhos de pivô

Você pode usar as propriedades [LeftHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.leftheader) e [RightHeader](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.rightheader) para adicionar outros controles ao cabeçalho do Pivot.

Por exemplo, você pode adicionar um [CommandBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/app-bars) no RightHeader do Pivot.

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

> **Observação** Os cabeçalhos dinâmicos não devem girar em um [ambiente de 3 metros](../devices/designing-for-tv.md). Defina a nova propriedade [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) como **false** se o aplicativo for executado no Xbox.

## <a name="recommendations"></a>Recomendações

- Evite usar mais do que cinco cabeçalhos ao usar o modo de carrossel (viagem de ida e volta), pois ter um número maior pode gerar confusão.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) – confira todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados

- [Classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Noções básicas de design de navegação](../basics/navigation-basics.md)