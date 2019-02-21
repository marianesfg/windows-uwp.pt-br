---
Description: You can customize a control's visual structure and visual behavior by creating a control template in the XAML framework.
MS-HAID: dev\_ctrl\_layout\_txt.control\_templates
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Modelos de controle
ms.assetid: 6E642626-A1D6-482F-9F7E-DBBA7A071DAD
label: Control templates
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 539f67079547db28a02ef34fc4b9af2e15d107d3
ms.sourcegitcommit: 4e80ee8d577c3475b6d247317a24411a48b02c29
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2019
ms.locfileid: "9083887"
---
# <a name="control-templates"></a>Modelos de controle

Você pode personalizar a estrutura e o comportamento visual de um controle criando um modelo de controle na estrutura XAML. Os controles têm muitas propriedades, como [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414), e [**FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209404), que você pode definir para especificar diferentes aspectos da aparência do controle. Mas as mudanças que você pode fazer ao definir essas propriedades são limitadas. Você pode especificar personalizações adicionais criando um modelo usando a classe [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). Aqui, mostramos a você como criar um **ControlTemplate** para personalizar a aparência de um controle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316).

> **APIs importantes**: [**classe ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), [**propriedade Control.Template**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.template.aspx)

## <a name="custom-control-template-example"></a>Amostra de modelo de controle personalizado

Por padrão, um controle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) coloca seu conteúdo (a cadeia de caracteres ou o objeto ao lado de **CheckBox**) à direita da caixa de seleção, e uma marca de seleção indica que um usuário marcou a **CheckBox**. Essas características representam a estrutura e o comportamento visual da **CheckBox**.

Consulte um [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) usando o [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) padrão mostrado nos estados `Unchecked`, `Checked` e `Indeterminate`.

![modelo de caixa de seleção padrão](images/templates-checkbox-states-default.png)

Você pode mudar estas características criando um [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) para [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316). Por exemplo, se você quiser que o conteúdo da caixa de seleção fique abaixo da caixa de seleção e quiser usar um **X** para indicar que um usuário marcou a caixa de seleção. Você especifica estas características no **ControlTemplate** de **CheckBox**.

Para usar um modelo personalizado com um controle, você atribui o [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) à propriedade [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) do controle. Aqui está uma [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) usando um **ControlTemplate** chamado `CheckBoxTemplate1`. Mostramos a linguagem XAML para o **ControlTemplate** na próxima seção.

```XAML
<CheckBox Content="CheckBox" Template="{StaticResource CheckBoxTemplate1}" IsThreeState="True" Margin="20"/>
```

Consulte como este [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) fica nos estados `Unchecked`, `Checked`, e `Indeterminate` após aplicarmos o nosso modelo.

![modelo personalizado de caixa de seleção](images/templates-checkbox-states.png)

## <a name="specify-the-visual-structure-of-a-control"></a>Especificar a estrutura visual de um controle

Ao criar um [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391), você combina os objetos [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) para construir um único controle. Um **ControlTemplate** deve ter apenas um **FrameworkElement** como seu elemento raiz. O elemento raiz normalmente contém outros objetos **FrameworkElement**. A combinação dos objetos compõe a estrutura visual do controle.

Este XAML cria um [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) para um [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) que especifica que o conteúdo do controle está abaixo da caixa de seleção. O elemento da raiz é um [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250). O exemplo especifica um [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) para criar um **X** que indica que um usuário selecionou o **CheckBox** e um [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que indica um estado indeterminado. Note que [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) está definido como 0 no **Path** e **Ellipse** de forma que, por padrão, nenhum aparece.

Um [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md) é um vinculador que vincula o valor de uma propriedade em um modelo de controle ao valor de outra propriedade exposta no controle de modelo. TemplateBinding só pode ser usado dentro de uma definição ControlTemplate em XAML. Consulte [TemplateBinding markup extension](../../xaml-platform/templatebinding-markup-extension.md) para mais informações.

> [!NOTE]
> A partir do Windows 10, versão 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)), você pode usar as extensões de marcação [**X:Bind**](https://msdn.microsoft.com/library/windows/apps/Mt204783) em locais você usar [TemplateBinding](../../xaml-platform/templatebinding-markup-extension.md). Consulte [TemplateBinding markup extension](../../xaml-platform/templatebinding-markup-extension.md) para mais informações.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

## <a name="specify-the-visual-behavior-of-a-control"></a>Especificar o comportamento visual do controle

Um comportamento visual especifica a aparência de um controle quando ele está em um certo estado. O controle [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) tem três estados de verificação: `Checked`, `Unchecked` e `Indeterminate`. O valor da propriedade [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798) determina o estado de **CheckBox** e o seu estado determina o que aparece na caixa.

Esta tabela lista os possíveis valores do [**IsChecked**](https://msdn.microsoft.com/library/windows/apps/br209798), os estados correspondentes do [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) e a aparência de **CheckBox**.

|                     |                    |                         |
|---------------------|--------------------|-------------------------|
| Valor **IsChecked** | Estado da **CheckBox** | Aparência da **CheckBox** |
| **true**            | `Checked`          | Contém um "X".        |
| **false**           | `Unchecked`        | Vazio.                  |
| **null**            | `Indeterminate`    | Contém um círculo.      |


Você especifica a aparência de um controle quando ele está em um determinado estado usando os objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007). Um **VisualState** contém um [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br243053) que muda a aparência dos elementos no [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). Quando o controle entra no estado que a propriedade [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031) especifica, as alterações de propriedade no **Setter** ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) são aplicadas. Quando o controle sai do estado, as alterações são removidas. Adicione objetos **VisualState** aos objetos [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/br209014). Adicione objetos **VisualStateGroup** à propriedade [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) anexada, a qual você definiu na raiz do [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) do **ControlTemplate**.

Este XAML mostra os objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) para os estados `Checked`, `Unchecked` e `Indeterminate`. O exemplo define a propriedade [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) anexada no [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), que é o elemento raiz do [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391). O `Checked` **VisualState** especifica que o [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) do [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) chamado `CheckGlyph` (que mostramos no exemplo anterior) é 1. O `Indeterminate` **VisualState** especifica que o **Opacity** do [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) nomeado `IndeterminateGlyph` é 1. O `Unchecked` **VisualState** não tem nenhum [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) ou [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490), assim, a [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) volta a sua aparência padrão.

```XAML
<ControlTemplate x:Key="CheckBoxTemplate1" TargetType="CheckBox">
    <Border BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            Background="{TemplateBinding Background}">

        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CheckStates">
                <VisualState x:Name="Checked">
                    <VisualState.Setters>
                        <Setter Target="CheckGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="CheckGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
                <VisualState x:Name="Unchecked"/>
                <VisualState x:Name="Indeterminate">
                    <VisualState.Setters>
                        <Setter Target="IndeterminateGlyph.Opacity" Value="1"/>
                    </VisualState.Setters>
                    <!-- This Storyboard is equivalent to the Setter. -->
                    <!--<Storyboard>
                        <DoubleAnimation Duration="0" To="1"
                         Storyboard.TargetName="IndeterminateGlyph" Storyboard.TargetProperty="Opacity"/>
                    </Storyboard>-->
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="25"/>
            </Grid.RowDefinitions>
            <Rectangle x:Name="NormalRectangle" Fill="Transparent" Height="20" Width="20"
                       Stroke="{ThemeResource SystemControlForegroundBaseMediumHighBrush}"
                       StrokeThickness="{ThemeResource CheckBoxBorderThemeThickness}"
                       UseLayoutRounding="False"/>
            <!-- Create an X to indicate that the CheckBox is selected. -->
            <Path x:Name="CheckGlyph"
                  Data="M103,240 L111,240 119,248 127,240 135,240 123,252 135,264 127,264 119,257 111,264 103,264 114,252 z"
                  Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                  FlowDirection="LeftToRight"
                  Height="14" Width="16" Opacity="0" Stretch="Fill"/>
            <Ellipse x:Name="IndeterminateGlyph"
                     Fill="{ThemeResource CheckBoxForegroundThemeBrush}"
                     Height="8" Width="8" Opacity="0" UseLayoutRounding="False" />
            <ContentPresenter x:Name="ContentPresenter"
                              ContentTemplate="{TemplateBinding ContentTemplate}"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}" Grid.Row="1"
                              HorizontalAlignment="Center"
                              VerticalAlignment="{TemplateBinding VerticalContentAlignment}"/>
        </Grid>
    </Border>
</ControlTemplate>
```

Para entender melhor como os objetos [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) funcionam, considere o que ocorre quando [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) vai do estado `Unchecked` ao estado `Checked` e, em seguida, ao estado `Indeterminate` e, em seguida, de volta ao estado `Unchecked`. Estes são as transições:

|                                      |                                                                                                                                                                                                                                                                                                                                                |                                                   |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| Transição de estado                     | O que ocorre                                                                                                                                                                                                                                                                                                                                   | Aparência da caixa de seleção quando a transição completa |
| De `Unchecked` a `Checked`.       | O valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) do `Checked` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) é aplicado, então o [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `CheckGlyph` é 1.                                                                                                                                                         | Um X é exibido.                                |
| De `Checked` a `Indeterminate`.   | O valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) do `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) é aplicado, então o [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `IndeterminateGlyph` é 1. O valor **Setter** do `Checked` **VisualState** é removido, então o [**Opacity**](https://msdn.microsoft.com/library/windows/apps/br228078) de `CheckGlyph` é 0. | Um círculo é exibido.                            |
| De `Indeterminate` a `Unchecked`. | O valor [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) do `Indeterminate` [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) é removido, então o [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de `IndeterminateGlyph` é 0.                                                                                                                                           | Nada é exibido.                             |

 
Para saber mais sobre como criar estados visuais para controles e, em especial, como usar a classe [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490) e os tipos de animação, consulte [Animações de storyboard para estados visuais](https://msdn.microsoft.com/library/windows/apps/xaml/jj819808).

## <a name="use-tools-to-work-with-themes-easily"></a>Use as ferramentas para trabalhar com os temas facilmente

Uma forma rápida de aplicar temas aos seus controles é clicar com o botão direito em um controle no **Estrutura de Tópicos do Documento** do Microsoft Visual Studio e selecionar **Editar Tema** ou **Editar Estilo** (dependendo do controle no qual você está clicando com o botão direito). Em seguida, você pode aplicar um tema existente selecionando **Aplicar Recurso** ou excluir um novo selecionando **Criar Vazio**.

## <a name="controls-and-accessibility"></a>Controles e acessibilidade

Ao criar um novo modelo de controle, além da possibilidade de mudar o comportamento e a aparência visual do controle, você também pode alterar como o controle se representa nas estruturas de acessibilidade. A Plataforma Universal do Windows (UWP) é compatível com a estrutura de Automação da IU Microsoft para acessibilidade. Todos os controles padrão e seus modelos permitem tipos e padrões comuns de controle de Automação da Interface do Usuário adequados para a finalidade e a função do controle. Esses tipos e padrões de controle são interpretados por clientes de Automação da Interface do Usuário, como tecnologias adaptativas, permitindo acessar um controle como parte de uma interface do usuário de aplicativo acessível maior.

Para separar a lógica de controle básica e também atender a alguns requisitos de arquitetura da Automação da Interface do Usuário, as classes de controle incluem o suporte para acessibilidade em uma classe separada, um par de automação. Os pares de automação às vezes têm interações com os modelos de controle, pois os pares esperam que existam determinadas partes nomeadas nos modelos para que funcionalidades, como a habilitação de tecnologias adaptativas, possam invocar as ações dos botões.

Ao criar um controle personalizado completamente novo, convém também criar um novo par de automação para trabalhar com ele. Para obter mais informações, consulte [Pares de automação personalizados](../accessibility/custom-automation-peers.md).

## <a name="learn-more-about-a-controls-default-template"></a>Saiba mais sobre o modelo padrão de um controle

Os tópicos que documentam os estilos e modelos dos controles de XAML mostram trechos do mesmo XAML inicial que você veria se usasse as técnicas **Editar Tema** ou **Editar Estilo** explicadas anteriormente. Cada tópico lista os nomes dos estados visuais, os recursos de temas usados e o XAML completo para o estilo que contém o modelo. Os tópicos podem ser diretrizes úteis se você já começou a modificar um modelo e quer ver qual era a aparência do modelo original ou verificar se o novo modelo tem todos os estados visuais nomeados obrigatórios.

## <a name="theme-resources-in-control-templates"></a>Recursos de tema em modelos de controle

Para alguns dos atributos nos exemplos de XAML, você pode ter percebido referências de recursos que usam a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Essa é uma técnica que permite que um único modelo de controle use recursos que podem ser valores diferentes dependendo de qual tema está ativo no momento. Isso é particularmente importante para pincéis e cores, porque a finalidade principal dos temas é permitir que os usuários escolham se querem um tema de contraste escuro, claro ou alto aplicado ao sistema como um todo. Os aplicativos que usam o sistema de recursos de XAML podem usar um conjunto de recursos apropriado para esse tema, de maneira que as escolhas de tema na interface do usuário de um aplicativo reflitam a escolha de tema em todo o sistema feita pelo usuário.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

* [Exemplo de galeria de controles XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Exemplo de controle de edição de texto personalizado](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomEditControl)
