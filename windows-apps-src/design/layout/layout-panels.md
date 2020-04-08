---
Description: Use os painéis de layout para organizar e agrupar elementos de interface do usuário em seu aplicativo.
title: Painéis de layout para aplicativos UWP (Plataforma Universal do Windows)
ms.date: 04/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9322ba847aeb7eb64c2654e1105582478a0d3b47
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210202"
---
# <a name="layout-panels"></a>Painéis de layout

Os painéis de layout são contêineres que permitem organizar e agrupar elementos da interface do usuário no aplicativo. Os painéis de layout XAML internos incluem [**RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), [**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) e [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas). Descrevemos aqui cada painel e mostramos como usá-lo no layout dos elementos de interface do usuário XAML.

Há várias coisas a serem consideradas ao se escolher um painel de layout:
- Como o painel posiciona seus elementos filho.
- Como o painel dimensiona seus elementos filho.
- Como elementos filho sobrepostos são colocados uns sobre os outros (ordem z).
- O número e a complexidade dos elementos de painel aninhado necessários para criar o layout desejado.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, veja <a href="xamlcontrolsgallery:/item/RelativePanel">RelativePanel</a>, <a href="xamlcontrolsgallery:/item/StackPanel">StackPanel</a>, <a href="xamlcontrolsgallery:/item/Grid">Grid</a>, <a href="xamlcontrolsgallery:/item/VariableSizedWrapGrid">VariableSizedWrapGrid</a> e <a href="xamlcontrolsgallery:/item/Canvas">Canvas</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="panel-properties"></a>Propriedades do painel

Antes de falarmos sobre cada painel, abordaremos algumas propriedades comuns que todos eles têm. 

### <a name="panel-attached-properties"></a>Propriedades anexadas do painel

A maioria dos painéis de layout XAML usa propriedades anexadas para permitir que seus elementos filho informem ao painel pai como eles devem ser posicionados na interface do usuário. As propriedades anexadas usam a sintaxe *AttachedPropertyProvider.PropertyName*. Se você tiver painéis aninhados dentro de outros painéis, as propriedades anexadas em elementos de interface do usuário que especificam características de layout a um pai só serão interpretadas pelo painel pai mais imediato.

Aqui está um exemplo de como você pode definir a propriedade anexada [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) em um controle Button em XAML. Isso informa ao Canvas pai que o Button deve ser posicionado a 50 pixels efetivos em relação à borda esquerda do Canvas.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

Para obter mais informações sobre propriedades anexadas, consulte [Visão geral das propriedades anexadas](../../xaml-platform/attached-properties-overview.md).

### <a name="panel-borders"></a>Bordas do painel

Os painéis RelativePanel, StackPanel e Grid definem as propriedades da borda que permitem que você desenhe uma borda ao redor do painel sem encapsulá-los em um elemento Border adicional. As propriedades de borda são **BorderBrush**, **BorderThickness**, **CornerRadius** e **Padding**.

Aqui está um exemplo de como definir propriedades da borda em um Grid.

```xaml
<Grid BorderBrush="Blue" BorderThickness="12" CornerRadius="12" Padding="12">
    <TextBlock Text="Hello World!"/>
</Grid>
```

![Uma grade com bordas](images/layout-panel-grid-border.png)

O uso das propriedades de borda internas reduz a contagem de elementos XAML, o que pode melhorar o desempenho da interface do usuário do seu aplicativo. Para obter mais informações sobre painéis de layout e desempenho de interface do usuário, consulte [Otimizar seu layout XAML](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-your-xaml-layout).

## <a name="relativepanel"></a>RelativePanel

[ **RelativePanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) permite dispor os elementos da interface do usuário especificando onde eles devem ficar em relação a outros elementos e em relação ao painel. Por padrão, um elemento é posicionado no canto superior esquerdo do painel. Use RelativePanel com [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) e [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) para reorganizar a interface do usuário para diferentes tamanhos de janela.

Esta tabela mostra as propriedades anexadas que você pode usar para alinhar um elemento em relação ao painel e a outros elementos.

Alinhamento do painel | Alinhamento do irmão | Posição do irmão
----------------|-------------------|-----------------
[**AlignTopWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithpanelproperty) | [**AlignTopWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.aligntopwithproperty) | [**Above**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)  
[**AlignBottomWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithpanelproperty) | [**AlignBottomWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignbottomwithproperty) | [**Below**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.belowproperty)  
[**AlignLeftWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel) | [**AlignLeftWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.getalignleftwith) | [**LeftOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.leftofproperty)  
[**AlignRightWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithpanelproperty) | [**AlignRightWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignrightwithproperty) | [**RightOf**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.setrightof)  
[**AlignHorizontalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanelproperty) | [**AlignHorizontalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithproperty) | &nbsp;   
[**AlignVerticalCenterWithPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithpanelproperty) | [**AlignVerticalCenterWith**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel.alignverticalcenterwithproperty) | &nbsp;   

 
Este XAML mostra como organizar os elementos em um RelativePanel.

```xaml
<RelativePanel BorderBrush="Gray" BorderThickness="1">
    <Rectangle x:Name="RedRect" Fill="Red" Height="44" Width="44"/>
    <Rectangle x:Name="BlueRect" Fill="Blue"
               Height="44" Width="88"
               RelativePanel.RightOf="RedRect" />

    <Rectangle x:Name="GreenRect" Fill="Green" 
               Height="44"
               RelativePanel.Below="RedRect" 
               RelativePanel.AlignLeftWith="RedRect" 
               RelativePanel.AlignRightWith="BlueRect"/>
    <Rectangle Fill="Orange"
               RelativePanel.Below="GreenRect" 
               RelativePanel.AlignLeftWith="BlueRect" 
               RelativePanel.AlignRightWithPanel="True"
               RelativePanel.AlignBottomWithPanel="True"/>
</RelativePanel>
```

O resultado tem a seguinte aparência. 

![Painel relativo](images/layout-panel-relative-panel.png)

Estes são alguns aspectos a serem observados sobre o dimensionamento dos retângulos:
- O retângulo vermelho recebe um tamanho explícito de 44 x 44. Ele é colocado no canto superior esquerdo do painel, que é a posição padrão.
- O retângulo verde recebe uma altura explícita de 44. Seu lado esquerdo é alinhado com o retângulo vermelho, e o lado direito é alinhado com o retângulo azul, o que determina a largura.
- O retângulo laranja não recebe um tamanho explícito. Seu lado esquerdo é alinhado com o retângulo azul. As bordas direita e inferior são alinhadas com a borda do painel. Seu tamanho é determinado por esses alinhamentos e será redimensionado à medida que o painel for redimensionado.

## <a name="stackpanel"></a>StackPanel

[**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) organiza os elementos filho em apenas uma linha que pode ser orientada horizontal ou verticalmente. Normalmente, O StackPanel é usado para organizar uma pequena subseção da interface do usuário em uma página.

Você pode usar a propriedade [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) para especificar a direção dos elementos filho. A orientação padrão é [**Vertical**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Orientation).

O XAML a seguir mostra como criar um StackPanel vertical dos itens.

```xaml
<StackPanel>
    <Rectangle Fill="Red" Height="44"/>
    <Rectangle Fill="Blue" Height="44"/>
    <Rectangle Fill="Green" Height="44"/>
    <Rectangle Fill="Orange" Height="44"/>
</StackPanel>
```


O resultado tem a seguinte aparência.

![Painel da pilha](images/layout-panel-stack-panel.png)

Em um StackPanel, se o tamanho de um elemento filho não for definido explicitamente, ele se ampliará para preencher a largura disponível (ou a altura, se Orientation for **Horizontal**). Neste exemplo, a largura dos retângulos não está definida. Os retângulos se expandem para preencher toda a largura do StackPanel.

## <a name="grid"></a>Grid

O painel [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) dá suporte a layouts fluidos e permite organizar controles em layouts com várias linhas e várias colunas. Especifique as linhas e colunas de um painel Grid usando as propriedades [**RowDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) e [**ColumnDefinitions**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.columndefinitions).

Para posicionar objetos em células específicas do Grid, use as propriedades anexadas [**Grid.Column**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.column) e [**Grid.Row**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.row).

Para distribuir o conteúdo em várias linhas e colunas, use as propriedades anexadas [**Grid.RowSpan**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms605035(v=vs.95)) e [**Grid.ColumnSpan**](https://docs.microsoft.com/dotnet/api/system.windows.controls.grid.columnspan).

Este exemplo de XAML mostra como criar um Grid com duas linhas e duas colunas.

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition Height="44"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <Rectangle Fill="Red" Width="44"/>
    <Rectangle Fill="Blue" Grid.Row="1"/>
    <Rectangle Fill="Green" Grid.Column="1"/>
    <Rectangle Fill="Orange" Grid.Row="1" Grid.Column="1"/>
</Grid>
```


O resultado tem a seguinte aparência.

![Grid](images/layout-panel-grid.png)

Neste exemplo, o dimensionamento funciona assim: 
- A segunda linha tem uma altura explícita de 44 pixels efetivos. Por padrão, a altura da primeira linha preenche o espaço deixado.
- A largura da primeira coluna é definida como **Auto**, de maneira que ele seja largo o suficiente para seus filhos. Nesse caso, são 44 pixels efetivos de largura para acomodar a largura do retângulo vermelho.
- Como não há outra restrição de tamanho em relação aos retângulos, cada um se amplia para preencher a célula da grade em que está.

Você pode distribuir o espaço em uma coluna ou linha usando o dimensionamento **Auto** ou em estrela. Você pode usar o dimensionamento automático para permitir que os elementos de interface do usuário sejam redimensionados para caber no contêiner de conteúdo ou pai. Você também pode usar o dimensionamento automático com as linhas e as colunas de uma grade. Para usar o dimensionamento automático, defina Height e/ou Width dos elementos de interface do usuário como **Auto**.

Você usa o dimensionamento proporcional, também chamado de *dimensionamento em estrela* para distribuir o espaço disponível entre as linhas e as colunas de uma grade segundo proporções ponderadas. Em XAML, os valores estrela são expressos como \* (ou *n*\* para dimensionamento em estrela ponderado). Por exemplo, para especificar se uma coluna é cinco vezes mais larga do que a segunda coluna em um layout de duas colunas, use "5\*" e "\*" para as propriedades [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.columndefinition.width) nos elementos [**ColumnDefinition**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ColumnDefinition).

Esse exemplo integra dimensionamentos fixo, automático e proporcional em um [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) com quatro colunas.

&nbsp;|&nbsp;|&nbsp;
------|------|------
Coluna_1 | **Automático** | A coluna será dimensionada para ajustar o conteúdo.
Coluna_2 | * | Depois que as colunas Auto forem calculadas, a coluna obterá parte da largura remanescente. Column_2 terá metade da largura de Column_4.
Coluna_3 | **44** | A coluna terá 44 pixels de largura.
Coluna_4 | **2**\* | Depois que as colunas Auto forem calculadas, a coluna obterá parte da largura remanescente. Column_4 terá o dobro da largura de Column_2.

Como a largura da coluna padrão é "*", você não precisa definir explicitamente esse valor para a segunda coluna.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

No designer XAML do Visual Studio, o resultado fica assim.

![Uma grade de quatro colunas no designer do Visual Studio](images/xaml-layout-grid-in-designer.png)

## <a name="variablesizedwrapgrid"></a>VariableSizedWrapGrid

[**VariableSizedWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VariableSizedWrapGrid) é um painel de layout em estilo de grade no qual as linhas ou as colunas são encapsuladas automaticamente em uma nova linha ou coluna quando o valor [**MaximumRowsOrColumns**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.maximumrowsorcolumns) é atingido. 

A propriedade [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.orientation) especifica se a grade adiciona seus itens em linhas ou colunas antes do encapsulamento. A orientação padrão é **Vertical**, o que significa que a grade adiciona itens de cima para baixo até que uma coluna esteja cheia e, em seguida, encapsula em uma nova coluna. Quando o valor é **Horizontal**, a grade adiciona itens da esquerda para a direita e, em seguida, encapsula em uma nova linha.

As dimensões da célula são especificadas por [**ItemHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemheight) e [**ItemWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.itemwidth). Cada célula tem o mesmo tamanho. Caso ItemHeight ou ItemWidth não seja especificado, o tamanho da primeira célula se ajusta a seu conteúdo, e todas as demais células mantêm o tamanho da primeira célula.

Você pode usar as propriedades anexadas [**VariableSizedWrapGrid.ColumnSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid) e [**VariableSizedWrapGrid.RowSpan**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.variablesizedwrapgrid.getrowspan) para especificar quantas células adjacentes um elemento filho deve preencher.

Aqui está como usar um VariableSizedWrapGrid em XAML.

```xaml
<VariableSizedWrapGrid MaximumRowsOrColumns="3" ItemHeight="44" ItemWidth="44">
    <Rectangle Fill="Red"/>
    <Rectangle Fill="Blue" 
               VariableSizedWrapGrid.RowSpan="2"/>
    <Rectangle Fill="Green" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
    <Rectangle Fill="Orange" 
               VariableSizedWrapGrid.RowSpan="2" 
               VariableSizedWrapGrid.ColumnSpan="2"/>
</VariableSizedWrapGrid>
```


O resultado tem a seguinte aparência.

![Grade de encapsulamento de tamanho variável](images/layout-panel-variable-size-wrap-grid.png)

Neste exemplo, o número máximo de linhas em cada coluna é três. A primeira coluna contém apenas dois itens (os retângulos vermelho e azul) porque o retângulo azul abrange duas linhas. O retângulo verde encapsula na parte superior da coluna seguinte.

## <a name="canvas"></a>Canvas

O painel [**Tela**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) posiciona os elementos filho usando pontos de coordenada fixos e não dá suporte a layouts fluidos. Especifique os pontos nos elementos filho individuais definindo as propriedades anexadas [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) e [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) em cada elemento. A tela pai lê os valores dessas propriedades anexadas de seus filhos durante o cálculo do layout [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange).

Objetos em um Canvas podem se sobrepor, quando um objeto é desenhado um sobre o outro. Por padrão, o Canvas renderiza os objetos filho na ordem em que estão declarados, logo, o último filho é renderizado na parte superior (cada elemento tem um padrão z-index igual a 0). Ele é o mesmo de outros painéis internos. No entanto, Canvas também dá suporte à propriedade anexada [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) que você pode definir em cada um dos elementos filho. Você pode definir essa propriedade no código para alterar a ordem de desenho dos elementos durante o tempo de execução. O elemento com o maior valor Canvas.ZIndex é desenhado por último e por isso é desenhado sobre quaisquer outros elementos que compartilhem o mesmo espaço ou se sobreponham de alguma maneira. Observe que o valor alfa (transparência) é respeitado, por isso mesmo se os elementos ficarem sobrepostos, o conteúdo mostrado em áreas sobrepostas poderá ser mesclado se o superior tiver um valor alfa que não seja máximo.

O Canvas não faz nenhum dimensionamento dos seus filhos. Cada elemento deve especificar seu tamanho.

Aqui está um exemplo de um Canvas em XAML.

```xaml
<Canvas Width="120" Height="120">
    <Rectangle Fill="Red" Height="44" Width="44"/>
    <Rectangle Fill="Blue" Height="44" Width="44" Canvas.Left="20" Canvas.Top="20"/>
    <Rectangle Fill="Green" Height="44" Width="44" Canvas.Left="40" Canvas.Top="40"/>
    <Rectangle Fill="Orange" Height="44" Width="44" Canvas.Left="60" Canvas.Top="60"/>
</Canvas>
```

O resultado tem a seguinte aparência.

![Canvas](images/layout-panel-canvas.png)

Use o painel Canvas com cautela. Apesar de ser conveniente poder controlar com precisão as posições dos elementos da interface do usuário para alguns cenários, um painel de layout com posição fixa faz com que a área da interface do usuário seja menos adaptável a mudanças gerais no tamanho das janelas do aplicativo. O redimensionamento de janelas do aplicativo pode ser proveniente de mudanças na orientação do dispositivo, janelas de aplicativo dividido, mudança de monitores e inúmeros outros cenários dos usuários.

## <a name="panels-for-itemscontrol"></a>Painéis para ItemsControl

Existem vários painéis de finalidade especial que podem ser usados apenas como um [**ItemsPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemspanel) para exibir itens em um [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl). Eles são [**ItemsStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsStackPanel), [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid), [**VirtualizingStackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.VirtualizingStackPanel) e [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid). Você não pode usar esses painéis no layout de interface do usuário geral.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.
