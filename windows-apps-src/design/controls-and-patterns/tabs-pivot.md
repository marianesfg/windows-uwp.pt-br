---
author: serenaz
Description: Tabs and pivots enable users to navigate between frequently accessed content.
title: Guias e pivôs
ms.assetid: 556BC70D-CF5D-4295-A655-D58163CC1824
label: Tabs and pivots
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 09e88fd070f7e7517e55d6f57563dd7608696770
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675473"
---
# <a name="pivot-and-tabs"></a>Guias e pivôs



O controle Pivô e o padrão de guias relacionadas são usados para navegar em categorias de conteúdo distintas, acessadas com frequência. Os pivôs possibilitam a navegação entre dois ou mais painéis de conteúdo e dependem de cabeçalhos de texto para identificar as diferentes seções de conteúdo.

> **APIs importantes**: [classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)

![Um exemplo de controle pivô](images/pivot_Hero_main.png)

As guias são uma variante visual do Pivô que usam uma combinação de ícones e texto ou apenas ícones para articular o conteúdo da seção. As guias são criadas ao usar o controle [Pivot](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.aspx). A [amostra de Pivot](http://go.microsoft.com/fwlink/p/?LinkId=619903) mostra como personalizar o controle Pivot segundo o padrão das guias.

![Um exemplo do padrão de guias](images/tabs.png) 

## <a name="the-pivot-control"></a>O controle de pivô

Durante a compilação de um app usando-se um pivô, há algumas variáveis de design importantes a serem levadas em consideração.

- **Rótulos do cabeçalho.**  Cabeçalhos podem ter um ícone com texto, somente ícone ou somente texto.
- **Alinhamento do cabeçalho.**  Cabeçalhos podem ser justificados à esquerda ou centralizados.
- **Navegação de nível superior ou inferior.**  Pivôs podem ser usados em qualquer nível de navegação. Como alternativa, a [visualização de navegação](navigationview.md) também pode servir como o nível principal com pivôs funcionando como o nível secundário.
- **Suporte a gestos de toque.**  Para dispositivos que deem suporte a gestos de toque, é possível usar um dos dois conjuntos de interação para navegar entre as categorias de conteúdo:
    1. Toque em um cabeçalho de guia/pivô para navegar para a categoria.
    2. Passe o dedo para a esquerda ou direita na área de conteúdo para navegar para a categoria adjacente.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Pivot">abrir o aplicativo e ver o Pivot em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Controle de pivô no telefone.

![Exemplo de pivô](images/pivot_example.png)

Padrão de guias no aplicativo Alarmes e Relógio.

![Um exemplo do padrão de guias em Alarmes e Relógio](images/tabs_alarms-and-clock.png)

## <a name="create-a-pivot-control"></a>Criar um controle de pivô

O controle [Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot) é fornecido com a funcionalidade básica descrita nesta seção.

Este XAML cria um controle de pivô básico com três seções de conteúdo.

```xaml
<Pivot x:Name="rootPivot" Title="Pivot Title">
    <PivotItem Header="Pivot Item 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 1."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 2."/>
    </PivotItem>
    <PivotItem Header="Pivot Item 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of pivot item 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>Item de pivô

Pivot é um [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx) e, por isso, pode conter uma coleção de itens de qualquer tipo. Qualquer item que você adicionar ao Pivô que não seja explicitamente um [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) será implicitamente encapsulado em um PivotItem. Como uma tabela dinâmica é frequentemente usada para navegar entre as páginas de conteúdo, é comum preencher a coleção [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) diretamente com elementos de XAML UI. Você também pode definir a propriedade [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) para uma fonte de dados. Itens vinculados a ItemsSource podem ser de qualquer tipo, mas se eles não forem explicitamente PivotItems, você deverá definir um [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) e [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) para especificar como os itens são exibidos.

Você pode usar a propriedade [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) para obter ou definir o item ativo do pivô. Use a propriedade [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) para obter ou definir o índice do item ativo.

### <a name="pivot-headers"></a>Cabeçalhos de pivô

Você pode usar as propriedades [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) e [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) para adicionar outros controles ao cabeçalho do pivô.

Por exemplo, você pode adicionar um [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars) em RightHeader do pivô.

![Um exemplo de uma barra de comandos no cabeçalho direito do controle de pivô](images/PivotHeader.png)

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar OverflowButtonVisibility="Collapsed" Background="Transparent">
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

-   Tocar em um item de pivô navega para o conteúdo da seção do cabeçalho.
-   Passar o dedo para a direita ou para a esquerda em um item de pivô navega para a seção adjacente.
-   Passar o dedo para a direita ou para a esquerda no conteúdo da seção navega para a seção adjacente.

![Exemplo de passar o dedo para esquerda no conteúdo da seção](images/pivot_w_hand.png)

O controle é fornecido em dois modos:

**Estático**

-   Pivôs são estáticos quando todos os cabeçalhos cabem dentro do espaço permitido.
-   Tocar em um rótulo do pivô navega para a página correspondente, embora o próprio pivô não se mova. O pivô ativo é realçado.

**Carrossel**

-   Pivôs giram quando todos os cabeçalhos não cabem no espaço permitido.
-   Tocar em um rótulo do pivô navega para a página correspondente, e o rótulo do pivô ativo gira para a primeira posição.
-   Os itens de pivô em um loop de carrossel da última à primeira seção de pivô.

> **Observação** Cabeçalhos de pivô não devem girar em um [ambiente de 3 metros](../devices/designing-for-tv.md). Defina a nova propriedade [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) como **false** se o aplicativo for executado no Xbox.

### <a name="pivot-focus"></a>Foco do pivô

Por padrão, o foco do teclado em um cabeçalho de pivô é representado com um sublinhado.

![Foco padrão sublinha cabeçalho selecionado](images/pivot_focus_selectedHeader.png)

Aplicativos que personalizaram Pivot e incorporam o sublinhado em elementos visuais de seleção do cabeçalho podem usar a nova propriedade [HeaderFocusVisualPlacement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pivot.HeaderFocusVisualPlacement) para alterar o padrão. Quando `HeaderFocusVisualPlacement="ItemHeaders"`, o foco será desenhado ao redor de todo o painel do cabeçalho.

![A opção ItemsHeader desenha o retângulo de foco em torno de todos os cabeçalhos de pivô](images/pivot_focus_headers.png)

## <a name="recommendations"></a>Recomendações

- Baseie o alinhamento de cabeçalhos de guia/pivô no tamanho da tela. Para larguras de tela abaixo de 720 epx, o alinhamento central geralmente funciona melhor, enquanto o alinhamento à esquerda para larguras de tela acima de 720 epx é recomendado na maioria dos casos.
- Evite usar mais do que cinco cabeçalhos ao usar o modo de carrossel (viagem de ida e volta), pois ter um número maior pode gerar confusão.
- Use o padrão de guias somente se os itens de pivô tiverem ícones diferentes.
- Inclua texto em cabeçalhos de item de pivô para ajudar os usuários a entender o significado de cada seção de pivô. Ícones não são necessariamente autoexplicativos para todos os usuários.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- A [amostra de Pivot](http://go.microsoft.com/fwlink/p/?LinkId=619903) Demonstra como personalizar o controle Pivot segundo o padrão das guias.
- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.

## <a name="related-topics"></a>Tópicos relacionados

- [Classe Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [Noções básicas de design de navegação](../basics/navigation-basics.md)
