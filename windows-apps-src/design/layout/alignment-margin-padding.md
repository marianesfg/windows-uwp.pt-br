---
Description: Use as propriedades de alinhamento, margem e preenchimento para organizar o layout dos elementos em uma página.
title: Alinhamento, margem e preenchimento para layout
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d7f702d145740703b9fbc4ca2e7fd8eba8957cc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684460"
---
# <a name="alignment-margin-padding"></a>Alinhamento, margem, preenchimento

Em aplicativos UWP, a maioria dos elementos de interface do usuário é herdada da classe [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement). Cada FrameworkElement tem dimensões e propriedades de alinhamento, margem e preenchimento que influenciam o comportamento de layout. A orientação a seguir fornece uma visão geral de como usar essas propriedades de layout para garantir que a interface do usuário do seu aplicativo seja legível e fácil de usar em qualquer contexto.

## <a name="dimensions-height-width"></a>Dimensões (altura, largura)
O dimensionamento adequado garante que todo o conteúdo seja claro e legível. Os usuários não precisam rolar nem aplicar zoom para decifrar o conteúdo principal.

![diagrama mostrando as dimensões](images/dimensions.svg)

- [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) e [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) especificam o tamanho de um elemento. Os valores padrão são matematicamente NaN (não numéricos). É possível definir valores fixos medidos em [pixels efetivos](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) ou usar o dimensionamento **Automático** ou [proporcional](layout-panels.md#grid) para o comportamento do fluido.

- [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) e [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) são propriedades somente leitura que fornecem o tamanho de um elemento em runtime. Se os layouts de fluido aumentam ou diminuem, os valores mudam em um evento [**SizeChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Observe que um [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) não alterará os valores ActualHeight e ActualWidth.

- [**MinWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) e [**MinHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) especificam valores que limitam o tamanho de um elemento e, ao mesmo tempo, permitem o redimensionamento fluido.

- [**FontSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.fontsize) e outras propriedades de texto controlam o tamanho do layout para elementos de texto. Embora os elementos de texto não tenham dimensões declaradas explicitamente, eles calculam ActualWidth e ActualHeight. 

## <a name="alignment"></a>Alinhamento
O alinhamento torna sua interface do usuário mais organizada e equilibrada e também pode ser usado para estabelecer hierarquia visual e relacionamentos.

![diagrama mostrando alinhamento](images/alignment.svg)

- [**HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) e [**VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) especificam como um elemento deve ser posicionado no contêiner pai.
    - Os valores para **HorizontalAlignment** são **Left**, **Center**, **Right** e **Stretch**.
    - Os valores para **VerticalAlignment** são **Top**, **Center**, **Bottom** e **Stretch**.

- **Stretch** é o padrão para ambas as propriedades e os elementos preenchem todo o espaço oferecido no contêiner pai. Números reais Height e Width cancelam um valor Stretch, que atuarão, em vez disso, como valor Center. Alguns controles, como Button, substituem o valor padrão Stretch no seu estilo padrão.

- [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) e [**VerticalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) especificam como elementos filhos são posicionados em um contêiner.

- O alinhamento pode afetar recortes em um painel de layout. Por exemplo, com `HorizontalAlignment="Left"`, o lado direito do elemento será cortado se o conteúdo for maior do que o ActualWidth.

- Elementos de texto usam a propriedade [**TextAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment). Em geral, recomendamos usar o alinhamento à esquerda, valor padrão. Para obter mais informações sobre como estilizar o texto, confira [Tipografia](../style/typography.md).

## <a name="margin-and-padding"></a>Margem e preenchimento
As propriedades de margem e preenchimento fazem com que a interface do usuário não pareça poluída ou esparsa demais e elas também podem facilitar o uso de determinadas entradas, como caneta e toque. Esta é uma ilustração que exibe margens e preenchimento para um contêiner e seu conteúdo.

![diagrama de preenchimento e margens de xaml](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Margem
[**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin) controla a quantidade de espaço vazio ao redor de um elemento. Margin não adiciona pixels a ActualHeight e ActualWidth, além de não ser considerada parte do elemento para teste de acertos e eventos de entrada de origem.

- Os valores de margem podem ser uniformes ou distintos. Com `Margin="20"`, uma margem uniforme de 20 pixels seria aplicada ao elemento, nos lados esquerdo, superior, direito e inferior. Com `Margin="0,10,5,25"`, os valores são aplicados nos lados esquerdo, superior, direito e inferior (nessa ordem). 

- As margens são aditivas. Se dois elementos especificam uma margem uniforme de 10 pixels e são adjacentes em qualquer orientação, a distância entre eles é de 20 pixels.

- Margens negativas são permitidas. Contudo, o uso de uma margem negativa costuma causar recortes, ou excesso de pares, por isso, o uso de margens negativas não é uma técnica comum.

- Os valores de margem são limitados por último, por isso, tenha cuidado com as margens, pois os contêineres podem recortar ou restringir elementos. Um valor de Margem pode ser a causa de um elemento que parece não renderizar; com a margem aplicada, a dimensão do elemento pode ser limitada a 0.

### <a name="padding"></a>Preenchimento
[**Preenchimento**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.padding) controla a quantidade de espaço entre a borda interna de um elemento e seus elementos ou conteúdos filhos. Um valor Padding positivo diminui a área de conteúdo do elemento. 

Ao contrário da Margem, o Preenchimento não é uma propriedade de FrameworkElement. Há várias classes que definem, cada qual, a própria propriedade Preenchimento:

-   [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding): herda todas as classes derivadas [**Control**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls). Nem todos os controles têm conteúdo, portanto, para esses controles, a configuração da propriedade não faz nada. Se o controle tiver uma borda, o preenchimento será aplicado dentro dessa borda.
-   [**Border.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.padding): define o espaço entre a linha do retângulo criada por [**BorderThickness**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.borderbrush) e o elemento [**Child**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border.child).
-   [**ItemsPresenter.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): contribui para a aparência de itens em controles de item, colocando o preenchimento especificado ao redor de cada item.
-   [**TextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.padding) e [**RichTextBlock.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.padding): expandem a caixa delimitadora ao redor do texto do elemento de texto. Esses elementos de texto não têm **tela de fundo**, por isso, podem ser difíceis de visualizar. Por esse motivo, em vez disso, use as configurações [**Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block.margin) nos contêineres [**Block**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.block).

Em cada um desses casos, os elementos também têm uma propriedade Margin. Se forem aplicados Margin and Padding, eles serão aditivos: a distância aparente entre um contêiner externo e qualquer conteúdo interno será a margem mais o preenchimento.

### <a name="example"></a>Exemplo
Consultemos os efeitos de Margin e Padding sobre controles reais. Eis um TextBox dentro de um Grid com os valores padrão Margin e Padding iguais a 0.

![TextBox com margem e preenchimento iguais a 0](images/xaml-layout-textbox-no-margins-padding.svg)

Eis o mesmo TextBox e Grid com valores Margin e Padding no TextBox conforme mostrado neste XAML.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![TextBox com valores de margem e preenchimento positivos](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>Recursos de estilo
Você não precisa definir cada valor de propriedade individualmente em um controle. Costuma ser mais eficiente agrupar valores de propriedade em um recurso [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) e aplicar o Style a um controle. Isso é especialmente verdadeiro quando você precisa aplicar os mesmos valores de propriedade a muitos controles. Para obter mais informações sobre como usar estilos, confira [Estilos XAML](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Recomendações gerais
- Aplique valores de medição apenas em determinados elementos-chave e use o comportamento de layout fluido nos outros elementos. Isso prevê uma [interface do usuário responsiva](responsive-design.md) quando a largura da janela é alterada.

- Se você usar valores de medição, **todas as dimensões, margens e preenchimentos serão em incrementos de 4 epx**. Quando o UWP usa [dimensionamento e pixels efetivos](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para tornar seu aplicativo legível em todos os dispositivos e tamanhos de tela, ele dimensiona elementos de interface do usuário por múltiplos de 4. O uso de valores em incrementos de 4 resulta em melhor renderização em alinhamento com pixels inteiros.

- Com relação a pequenas larguras de janela (menos de 640 pixels), recomendamos 12 epx medianizes e, para larguras de janela maiores, recomendamos 24 epx medianizes.

![medianizes recomendadas](images/12-gutter.svg)

## <a name="related-topics"></a>Tópicos relacionados
* [**FrameworkElement.Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.padding)
