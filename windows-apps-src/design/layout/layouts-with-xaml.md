---
author: muhsinking
Description: XAML gives you a flexible layout system to create a responsive UI.
title: Layouts dinâmicos com o XAML
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b45196a83edf45a69f6b79ab82542cef6817703
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4260469"
---
# <a name="responsive-layouts-with-xaml"></a>Layouts dinâmicos com o XAML

O sistema de layout XAML oferece dimensionamento automático, painéis de layout, estados visuais e até mesmo definições de interface do usuário separadas para criar uma interface do usuário dinâmica. Com um layout dinâmico, você pode melhorar a aparência do app em telas com diferentes tamanhos, resoluções, densidades de pixel e orientações de janela. Você também pode usar o XAML para reposicionar, redimensionar, refluir, mostrar/ocultar, substituir e reformular a interface do usuário do app, conforme discutido em [Técnicas de design dinâmicas](responsive-design.md). Discutiremos aqui como implementar layouts dinâmicos com o XAML.

## <a name="fluid-layouts-with-properties-and-panels"></a>Layouts fluidos com propriedades e painéis

A base de um layout dinâmico é o uso apropriado de propriedades e painéis de layout XAML para reposicionar, redimensionar e refluir o conteúdo de forma fluido. 

O sistema de layout XAML dá suporte a layouts estáticos e fluidos. Em um layout estático, você pode dar posições e tamanhos de pixel explícitos aos controles. Quando o usuário altera a resolução ou a orientação de seu dispositivo, a interface do usuário não é alterada. Layouts estáticos podem ser recortados em diferentes fatores forma e tamanhos de tela. Por outro lado, os layouts fluidos reduzem, crescem e refluem para responderem ao espaço visual disponível em um dispositivo. 

Na prática, você usa uma combinação de elementos estáticos e fluidos para criar sua interface do usuário. Você ainda usa elementos e valores estáticos em alguns lugares, mas verifique se a interface do usuário geral responde a diferentes resoluções, tamanhos de tela e modos de exibição.

Discutiremos aqui como usar painéis de layout e propriedades XAML para criar um layout fluido.

### <a name="layout-properties"></a>Propriedades do layout
As propriedade de layout controlam o tamanho e a posição de um elemento. Para criar um layout fluido, use o dimensionamento automático ou proporcional de elementos e permite que os painéis de layout posicionem seus filhos conforme necessário. 

Aqui estão algumas propriedades de layout comuns e como usá-las para criar layouts fluidos.

**Height e Width**

As propriedades [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) e [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) especificam o tamanho de um elemento. Você pode usar valores fixos medidos em pixels efetivos ou usar o dimensionamento automático ou proporcional. 

O dimensionamento automático redimensiona os elementos de interface do usuário para caber no contêiner de conteúdo ou pai. Você também pode usar o dimensionamento automático com as linhas e as colunas de uma grade. Para usar o dimensionamento automático, defina Height e/ou Width dos elementos de interface do usuário como **Auto**.

> [!NOTE]
> Um elemento ser redimensionado para seu conteúdo ou seu contêiner depende de como o contêiner pai manipula o dimensionamento dos filhos. Para obter mais informações, consulte [Painéis de layout](#layout-panels) posteriormente neste artigo.

O dimensionamento proporcional, também chamado de *dimensionamento em estrela*, distribui o espaço disponível entre as linhas e as colunas de uma grade segundo proporções ponderadas. Em XAML, os valores estrela são expressos como \* (ou *n*\* para dimensionamento em estrela ponderado). Por exemplo, para especificar se uma coluna é cinco vezes mais larga do que a segunda coluna em um layout de 2 colunas, use "5\*" e "\*" para as propriedades [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) nos elementos [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx).

Esse exemplo integra dimensionamentos fixo, automático e proporcional em um [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) com quatro colunas.

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

Para obter o tamanho de um elemento em tempo de execução, use as propriedades somente leitura [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) e [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx), em vez de Height e Width.

**Restrições de tamanho**

Ao usar o dimensionamento automático na interface do usuário, você talvez ainda precise fazer restrições ao tamanho de um elemento. Você pode definir as propriedades [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minwidth.aspx)/[**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxwidth.aspx) e [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minheight.aspx)/[**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) para especificar valores que restrinjam o tamanho de um elemento ao mesmo tempo em que permitam o redimensionamento fluido.

Em um Grid, MinWidth/MaxWidth também pode ser usado com definições de coluna, e MinHeight/MaxHeight pode ser usado com definições de linha.

**Alinhamento**

Use as propriedades [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) e [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) para especificar como um elemento deve ser posicionado no contêiner pai.
- Os valores para **HorizontalAlignment** são **Left**, **Center**, **Right** e **Stretch**.
- Os valores para **VerticalAlignment** são **Top**, **Center**, **Bottom** e **Stretch**.

Com o alinhamento **Stretch**, os elementos preenchem todos o espaço oferecido no contêiner pai. Stretch é o valor padrão para ambas as propriedades de alinhamento. No entanto, alguns controles, como [**Button**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx), substituem esse valor no seu estilo padrão.
Qualquer elemento que possa ter elementos filho pode tratar o valor Stretch das propriedades HorizontalAlignment e VerticalAlignment de maneira exclusiva. Por exemplo, um elemento que use os valores Stretch padrão colocados em um Grid se amplia para preencher a célula que o contém. O mesmo elemento colocado em um Canvas é dimensionado segundo seu conteúdo. Para obter mais informações sobre como cada painel manipula o valor Stretch, consulte o artigo [Painéis de layout](layout-panels.md).

Para obter mais informações, consulte o artigo [Alinhamento, margem e preenchimento](alignment-margin-padding.md) e as páginas de referência [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) e [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

**Visibilidade**

Você pode revelar ou ocultar um elemento definindo sua propriedade [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx) como um dos valores de enumeração [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx): **Visible** ou **Collapsed**. Quando um elemento é Collapsed, ele não ocupa nenhum espaço no layout da interface do usuário.

Você pode alterar a propriedade Visibility de um elemento no código ou em um estado visual. Quando o Visibility de um elemento é alterado, todos os seus elementos filho também são alterados. Você pode substituir seções de sua interface do usuário, revelando um painel e recolhendo outro.

> [!Tip]
> Quando você tiver elementos da interface do usuário que são **Collapsed** por padrão, os objetos ainda são criados na inicialização, mesmo que eles não estiverem visíveis. Você pode adiar o carregamento desses elementos até que eles sejam mostrados definindo o **atributo x:DeferLoadStrategy** como "Lazy". Isso pode melhorar o desempenho da inicialização. Para obter mais informações, consulte [Atributo x:DeferLoadStrategy](../../xaml-platform/x-deferloadstrategy-attribute.md).

### <a name="style-resources"></a>Recursos de estilo

Você não precisa definir cada valor de propriedade individualmente em um controle. Costuma ser mais eficiente agrupar valores de propriedade em um recurso [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) e aplicar o Style a um controle. Isso é especialmente verdadeiro quando você precisa aplicar os mesmos valores de propriedade a muitos controles. Para obter mais informações sobre como usar estilos, consulte [Definindo o estilo de controles](../controls-and-patterns/xaml-styles.md).

### <a name="layout-panels"></a>Painéis de layout

Para posicionar objetos visuais, você deve colocá-los em um painel ou em outro objeto de contêiner. A estrutura XAML fornece diversas classes de painel, como [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) e [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), que servem como contêineres e que permitem posicionar e organizar os elementos da interface do usuário neles.

O principal ponto a ser considerado ao escolher um painel de layout é como o painel posiciona e dimensiona seus elementos filho. Você também pode precisar considerar como os elementos filho sobrepostos são colocados uns sobre os outros.

Eis uma comparação dos principais recursos dos controles de painel fornecidos na estrutura XAML.

Controle do painel | Descrição
--------------|------------
[**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) | **Canvas** não dá suporte à interface do usuário fluida; você controla todos os aspectos do posicionamento e do dimensionamento dos elementos filho. Você normalmente o usa em casos especiais, como a criação de elementos gráficos, ou para definir pequenas áreas estáticas de uma interface do usuário adaptável maior. Você pode usar o código ou os estados visuais para reposicionar elementos em tempo de execução.<li>Os elementos são posicionados de maneira absoluta usando-se as propriedades anexadas Canvas.Top e Canvas.Left.</li><li>A disposição em camadas pode ser especificada explicitamente usando-se a propriedade anexada Canvas.ZIndex.</li><li>Os valores Stretch de HorizontalAlignment/VerticalAlignment são ignorados. Caso o tamanho do elemento não seja definido explicitamente, ele é dimensionado segundo seu conteúdo.</li><li>O conteúdo filho não será recortado visualmente se for maior do que o painel. </li><li>O conteúdo filho não é restringido pelos limites do painel.</li>
[**Grade**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) | **Grid** dá suporte ao redimensionamento fluido de elementos filho. Você pode usar o código ou os estados visuais para reposicionar e refluir elementos.<li>Os elementos são organizados em linhas e colunas usando-se as propriedades anexadas Grid.Row e Grid.Column.</li><li>Os elementos podem abranger várias linhas e colunas usando-se as propriedades anexadas Grid.RowSpan e Grid.ColumnSpan.</li><li>Os valores Stretch de HorizontalAlignment/VerticalAlignment são respeitados. Se o tamanho de um elemento não for definido explicitamente, ele se ampliará para preencher o espaço disponível na célula da grade.</li><li>O conteúdo filho será recortado visualmente se for maior do que o painel.</li><li>Como o tamanho do conteúdo é restringido pelos limites do painel, o conteúdo rolável mostra barras de rolagem, caso necessário.</li>
[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) | <li>Os elementos são organizados em relação à borda ou ao centro do painel e em relação uns aos outros.</li><li>Os elementos são posicionados usando-se uma variedade de propriedades anexadas que controlam o alinhamento do painel, o alinhamento do irmão e a posição do irmão. </li><li>Os valores Stretch de HorizontalAlignment/VerticalAlignment são ignorados, a menos que as propriedades anexadas RelativePanel do alinhamento causem o alongamento (por exemplo, um elemento está alinhado com as bordas direita e esquerda do painel). Caso o tamanho de um elemento não seja definido explicitamente e não seja ampliado, ele é dimensionado segundo seu conteúdo.</li><li>O conteúdo filho será recortado visualmente se for maior do que o painel.</li><li>Como o tamanho do conteúdo é restringido pelos limites do painel, o conteúdo rolável mostra barras de rolagem, caso necessário.</li>
[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) |<li>Os elementos são empilhados em uma única linha vertical ou horizontalmente.</li><li>Os valores Stretch de HorizontalAlignment/VerticalAlignment são respeitados na direção oposta à da propriedade Orientation. Caso o tamanho de um elemento não seja definido explicitamente, ele se amplia para preencher a largura disponível (ou a altura caso o Orientation seja Horizontal). Na direção especificada pela propriedade Orientation, um elemento é dimensionado segundo seu conteúdo.</li><li>O conteúdo filho será recortado visualmente se for maior do que o painel.</li><li>Como o tamanho do conteúdo não é restringido pelos limites do painel na direção especificada pela propriedade Orientation, o conteúdo rolável se amplia além dos limites do painel e não mostra barras de rolagem. Você deve restringir explicitamente a altura (ou a largura) do conteúdo filho para fazer suas barras de rolagem serem mostradas.</li>
[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) |<li>Os elementos são organizados em linhas ou colunas que se ajustam automaticamente em uma nova linha ou coluna quando o valor MaximumRowsOrColumns é atingido.</li><li>A propriedade Orientation especifica se os elementos devem ser organizados em linhas ou colunas.</li><li>Os elementos podem abranger várias linhas e colunas usando-se as propriedades anexadas VariableSizedWrapGrid.RowSpan e VariableSizedWrapGrid.ColumnSpan.</li><li>Os valores Stretch de HorizontalAlignment/VerticalAlignment são ignorados. Os elementos são dimensionados conforme especificados pelas propriedades ItemHeight e ItemWidth. Caso essas propriedades não sejam definidas, o item na primeira célula é dimensionado segundo seu conteúdo, e todas as outras células herdam esse tamanho.</li><li>O conteúdo filho será recortado visualmente se for maior do que o painel.</li><li>Como o tamanho do conteúdo é restringido pelos limites do painel, o conteúdo rolável mostra barras de rolagem, caso necessário.</li>

Para obter informações detalhadas e exemplos desses painéis, consulte [Painéis de layout](layout-panels.md). Consulte também a [Amostra de técnicas dinâmicas](http://go.microsoft.com/fwlink/p/?LinkId=620024).

Os painéis de layout permitem organizar sua interface do usuário em grupos lógicos de controles. Ao usá-los com as configurações de propriedade apropriadas, você tem certo suporte para redimensionamento automático, reposicionamento e refluxo de elementos da interface do usuário. No entanto, a maioria dos layouts de interface do usuário ainda precisa de mais modificações quando ocorrem alterações significativas no tamanho da janela. Para isso, você pode usar estados visuais.

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>Layouts adaptáveis com estados visuais e gatilhos de estado
Use estados visuais para fazer alterações significativas na interface do usuário com base no tamanho da janela ou em outras alterações.

Quando a janela do aplicativo é ampliada ou reduzida além de um determinado valor, convém alterar as propriedades do layout para reposicionar, redimensionar, refluir, revelar ou substituir seções de sua interface do usuário. Você pode definir estados visuais diferentes para sua interface do usuário e aplicá-los quando a largura ou altura da janela ultrapassar um limite especificado. 

Um [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) oferece uma maneira fácil para definir o limite (também chamado de "ponto de interrupção") em que um estado é aplicado. Um [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) define valores de propriedade aplicados a um elemento quando ele está em um estado específico. Você agrupa estados visuais em um [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) que aplica o VisualState apropriado quando as condições especificadas são atendidas.

### <a name="set-visual-states-in-code"></a>Definir estados visuais no código

Para aplicar um estado visual no código, você chama o método [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx). Por exemplo, para aplicar um estado quando a janela do aplicativo tem um determinado tamanho, manipule o evento [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx) e chame **GoToState** para aplicar o estado apropriado.

Aqui, um [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) contém duas definições VisualState. A primeira, `DefaultState`, está vazia. Quando ela é aplicada, os valores definidos na página XAML são aplicados. A segunda, `WideState`, altera a propriedade [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx) do [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) para **Inline** e abre o painel. Esse estado é aplicado no manipulador de eventos caso a largura da janela seja superior a 640 pixels efetivos.

> [!NOTE]
> O Windows não permite que o app detecte o dispositivo em que o app está sendo executado. Ele pode informar a família de dispositivos (móvel, área de trabalho etc) em que o aplicativo está sendo executado, a resolução efetiva e a quantidade de espaço na tela disponível para o aplicativo (o tamanho da janela do aplicativo). É recomendável definir estados visuais para [tamanhos de tela e pontos de interrupção](screen-sizes-and-breakpoints-for-responsive-design.md).


```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

### <a name="set-visual-states-in-xaml-markup"></a>Definir estados visuais na marcação XAML

Antes do Windows 10, as definições VisualState exigiam objetos [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.storyboard.aspx) para alterações de propriedade, e você precisava chamar **GoToState** no código para aplicar o estado. Isso é mostrado no exemplo anterior. Você ainda verá muitos exemplos que usam essa sintaxe, ou pode ter um código existente que o use.

A partir do Windows 10, você pode usar a sintaxe [**Setter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.setter.aspx) simplificada mostrada aqui e usar um [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) na marcação XAML para aplicar o estado. Você usa gatilhos de estado para criar regras simples que disparam alterações de estado visuais automaticamente em resposta a um evento do aplicativo.

Este exemplo faz a mesma coisa que o exemplo anterior, mas usa a sintaxe simplificada **Setter**, em vez de um Storyboard para definir as alterações de propriedade. E, em vez de chamar GoToState, ele usa o gatilho de estado [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) interno para aplicar o estado. Ao usar gatilhos de estado, você não precisa definir um `DefaultState` vazio. As configurações padrão são reaplicadas automaticamente quando as condições do gatilho de estado não são mais atendidas.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=720 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> No exemplo anterior, a propriedade anexada VisualStateManager. Visualstategroups é definida no elemento **Grid** . Ao usar StateTriggers, certifique-se sempre de que VisualStateGroups esteja anexado ao primeiro filho da raiz para que os gatilhos entrem em vigor automaticamente. (Aqui, **Grid** é o primeiro filho do elemento **Page** raiz.)

### <a name="attached-property-syntax"></a>Sintaxe da propriedade anexada

Em um VisualState, você normalmente define um valor para uma propriedade de controle ou para uma das propriedades anexadas do painel que contém o controle. Ao definir uma propriedade anexada, use parênteses no nome da propriedade anexada.

Este exemplo mostra como definir a propriedade anexada [**RelativePanel.AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) em um TextBox chamado `myTextBox`. O primeiro XAML usa uma sintaxe [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.objectanimationusingkeyframes.aspx) e o segundo usa uma sintaxe **Setter**.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>Gatilhos de estado personalizados

Você pode estender a classe [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) para criar gatilhos personalizados para uma ampla variedade de cenários. Por exemplo, você pode criar um StateTrigger para disparar estados diferentes com base no tipo de entrada e, em seguida, aumentar as margens em torno de um controle quando o tipo de entrada é toque. Ou crie um StateTrigger para aplicar estados diferentes com base na família de dispositivos em que o aplicativo está em execução. Para obter exemplos de como compilar gatilhos personalizados e usá-los para criar experiências de interface do usuário otimizadas dentro de um único modo de exibição XAML, consulte a [Amostra de gatilhos de estado](http://go.microsoft.com/fwlink/p/?LinkId=620025).

### <a name="visual-states-and-styles"></a>Estilos e estados visuais

Você pode usar recursos Style em estados visuais para aplicar um conjunto de alterações de propriedade a vários controles. Para obter mais informações sobre como usar estilos, consulte [Definindo o estilo de controles](../controls-and-patterns/xaml-styles.md).

Neste XAML simplificado da Amostra de gatilhos de estado, um recurso Style é aplicado a um Button para ajustar o tamanho e as margens da entrada por mouse ou toque. Para obter o código completo e a definição do gatilho de estado personalizado, consulte [Amostra de gatilhos de estado](http://go.microsoft.com/fwlink/p/?LinkId=620025).

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="tailored-layouts"></a>Layouts personalizados

Ao fazer alterações significativas no layout da interface do usuário em dispositivos diferentes, talvez você considere mais prático definir um arquivo de interface do usuário separado com um layout personalizado de acordo com o dispositivo, em vez de adaptar uma única interface do usuário. Caso a funcionalidade seja a mesma em todos os dispositivos, você pode definir modos de exibição XAML separados que compartilhem o mesmo arquivo de código. Caso o modo de exibição e a funcionalidade sejam significativamente diferentes entre os dispositivos, você pode definir Pages separados e escolher para qual Page navegar quando o aplicativo for carregado.

### <a name="separate-xaml-views-per-device-family"></a>Modos de exibição XAML separados por família de dispositivos

Use modos de exibição XAML para criar definições de interface do usuário diferentes que compartilhem o mesmo code-behind. Você pode fornecer uma definição de interface do usuário exclusiva para cada família de dispositivos. Siga estas etapas para adicionar um modo de exibição XAML a seu aplicativo.

**Para adicionar um modo de exibição XAML a um aplicativo**
1. Selecione Projeto > Adicionar Novo Item. A caixa de diálogo Adicionar Novo Item é aberta.
    > **Dica**&nbsp;&nbsp;Verifique se uma pasta ou o projeto, e não a solução, está selecionado no Gerenciador de Soluções.
2. Em Visual C# ou Visual Basic no painel esquerdo, selecione o tipo de modelo XAML.
3. No painel central, selecione Exibição XAML.
4. Insira o nome do modo de exibição. O modo de exibição deve ser nomeado corretamente. Para obter mais informações sobre a nomenclatura, consulte o restante desta seção.
5. Clique em Adicionar. O arquivo é adicionado ao projeto.

As etapas anteriores criam apenas um arquivo XAML, mas não um arquivo code-behind associado. Em vez disso, o modo de exibição XAML é associado a um arquivo código-behind existente usando-se um qualificador "DeviceName" que faz parte do nome do arquivo ou da pasta. Esse nome de qualificador pode ser mapeado para um valor de cadeia de caracteres que representa a família do dispositivo em que o app está em execução no momento, como "Desktop", "Tablet" e os nomes das outras famílias de dispositivos (consulte [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx)).

Você pode adicionar o qualificador ao nome do arquivo ou adicionar o arquivo a uma pasta que tenha o nome do qualificador.

**Usar nome do arquivo**

Para usar o nome do qualificador com o arquivo, use este formato: *[pageName]*.DeviceFamily-*[qualifierString]*.xaml.

Consultemos um exemplo de um arquivo chamado MainPage.xaml. Para criar um modo de exibição para dispositivos de tablet, atribua um nome ao modo de exibição XAML MainPage.DeviceFamily-Tablet.xaml. Para criar um modo de exibição para os dispositivos de computador, atribua um nome ao modo de exibição MainPage.DeviceFamily-Desktop.xaml. Aqui está como a solução fica no Microsoft Visual Studio.

![Modos de exibição XAML com nomes de arquivo qualificados](images/xaml-layout-view-ex-1.png)

**Usar nome da pasta**

Para organizar os modos de exibição no projeto do Visual Studio usando pastas, você pode usar o nome de qualificador com a pasta. Para isso, nomeie a pasta assim: DeviceFamily-*[qualifierString]*. Neste caso, cada arquivo do modo de exibição XAML tem o mesmo nome. Não inclua o qualificador no nome do arquivo.

Aqui está um exemplo, novamente para um arquivo chamado MainPage.xaml. Para criar um modo de exibição para dispositivos de tablet, crie uma pasta chamada "DeviceFamily-Tablet" e coloque um modo de exibição XAML chamado MainPage.xaml nela. Para criar um modo de exibição para dispositivos de computador, crie uma pasta chamada "DeviceFamily-Desktop" e coloque outro modo de exibição XAML chamado MainPage.xaml nela. Aqui está como a solução fica no Visual Studio.

![Modos de exibição XAML em pastas](images/xaml-layout-view-ex-2.png)

Em ambos os casos, um modo de exibição exclusivo é usado para dispositivos de tablet e de computador. O arquivo padrão MainPage.xaml é usado caso o dispositivo em execução não corresponda a nenhum dos modos de exibição específicos da família de dispositivos.

### <a name="separate-xaml-pages-per-device-family"></a>Páginas XAML separadas por família de dispositivos

Para oferecer modos de exibição e funcionalidade exclusivos, você pode criar arquivos Page separados (XAML e código) e depois navegar até a página apropriada quando a página for necessária.

**Para adicionar uma página XAML a um aplicativo**
1. Selecione Projeto > Adicionar Novo Item. A caixa de diálogo Adicionar Novo Item é aberta.
    > **Dica**&nbsp;&nbsp;Verifique se o projeto, e não a solução, está selecionado no Gerenciador de Soluções.
2. Em Visual C# ou Visual Basic no painel esquerdo, selecione o tipo de modelo XAML.
3. No painel central, selecione Página em branco.
4. Insira o nome da página. Por exemplo, "MainPage_Tablet". Um MainPage_Tablet.xaml e o arquivo de código MainPage_Tablet.xaml.cs/vb/cpp são criados.
5. Clique em Adicionar. O arquivo é adicionado ao projeto.

Em tempo de execução, verifique a família de dispositivos em que o aplicativo está em execução e navegue até a página correta.

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Tablet")
{
    rootFrame.Navigate(typeof(MainPage_Tablet), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

Você também pode usar critérios diferentes para determinar para qual página navegar. Para obter mais exemplos, consulte o [Exemplo de vários modos de exibição personalizados](http://go.microsoft.com/fwlink/p/?LinkId=620636), que usa a função [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx) para verificar o tamanho físico de uma tela integrada.

## <a name="related-topics"></a>Tópicos relacionados
- [Tutorial: Criar layouts adaptáveis](../basics/xaml-basics-adaptive-layout.md)
- [Exemplo de técnicas dinâmicas (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [Exemplo de gatilhos de estado (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [Exemplo de vários modos de exibição personalizados (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
