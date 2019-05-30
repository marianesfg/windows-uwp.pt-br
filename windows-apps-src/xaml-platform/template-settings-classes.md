---
description: Classes TemplateSettings
title: Classes TemplateSettings
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0d68b31a1574d23dd66de37e2f708d8d77fc052
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371121"
---
# <a name="template-settings-classes"></a>Classes TemplateSettings


## <a name="prerequisites"></a>Pré-requisitos

Presumimos que você saiba adicionar controles à sua interface do usuário, definir as suas propriedades e atribuir manipuladores de eventos. Para obter instruções sobre como adicionar controles ao seu aplicativo, consulte [Adicionar controles e manipular eventos](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro). Também presumimos que você tem noções básicas sobre como definir um modelo personalizado para um controle, fazendo uma cópia do modelo padrão e editando-a. Para obter mais informações sobre isso, consulte [guia de início rápido: Modelos de controle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

## <a name="the-scenario-for-templatesettings-classes"></a>O cenário para classes **TemplateSettings**

As classes **TemplateSettings** fornecem um conjunto de propriedades que são usadas quando você define um novo modelo de controle de um controle. As propriedades têm valores como medidas de pixel para o tamanho de determinadas partes do elemento de interface do usuário. Às vezes, os valores são valores calculados que são provenientes da lógica do controle que normalmente não é fácil de substituir ou até mesmo acessar. Algumas das propriedades servem como valores **From** e **To** que controlam as transições e as animações de partes e, portanto, as propriedades **TemplateSettings** relevantes vêm em pares.

Há várias classes **TemplateSettings**. Todas elas estão no namespace [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives). Aqui está uma lista das classes e um link para a propriedade **TemplateSettings** do controle relevante. Essa propriedade **TemplateSettings** é como você acessa os valores **TemplateSettings** para o controle e pode estabelecer associações de modelo às suas propriedades:

-   [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): valor de [ **ComboBox.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): valor de [ **GridViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): valor de [ **ListViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): valor de [ **ProgressBar.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): valor de [ **ProgressRing.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): valor de [ **SettingsFlyout.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): valor de [ **ToggleSwitch.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): valor de [ **ToolTip.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

As propriedades **TemplateSettings** sempre devem ser usadas em XAML, não o código. Elas são subpropriedades somente leitura de uma propriedade **TemplateSettings** somente leitura de um controle pai. Em um cenário de controle personalizado avançado, no qual você está criando uma nova classe baseada em [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) e portanto pode influenciar a lógica do controle, considere a definição de uma propriedade **TemplateSettings** personalizada no controle para comunicar informações que podem ser úteis para qualquer um que esteja remodelando o controle. Assim como esse valor somente leitura da propriedade, defina uma nova classe **TemplateSettings** para o seu controle, que tenha propriedades somente leitura para cada um dos itens de informação, seja relevante para as medidas do modelo, posicionamento da animação, e assim por diante, e forneça aos chamados a instância do tempo de execução dessa classe que é inicializada através da lógica do controle. As classes **TemplateSettings** são derivadas de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject), de forma que as propriedades possam usar o sistema de propriedade de relevância dos retornos de chamada com propriedade alterada. Mas os identificadores de propriedade de dependência da propriedades não são expostos como API pública, pois as propriedades **TemplateSettings** não se destinam a ser somente leitura para os chamadores.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Como usar **TemplateSettings** a um modelo de controle.

Aqui está um exemplo que vêm dos modelos de controle XAML padrão iniciais. Este em especial é do modelo padrão de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing):

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

O XAML completo do modelo [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) tem centenas de linhas, portanto, este é apenas um trecho minúsculo. Este XAML define a parte de um controle que é um dos 6 elementos [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que representam a animação de rotação de progresso indeterminado. Como desenvolvedor, talvez você não goste de círculos e talvez use um primitivo de elemento gráfico diferente ou uma forma básica distinta para a progressão da animação. Por exemplo, você pode criar um **ProgressRing** que use um conjunto de elementos [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organizados em um quadrado. Se for o caso, cada componente **Rectangle** individual de seu novo modelo pode se parecer com o seguinte:

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

O motivo pelo qual as propriedades **TemplateSettings** são úteis aqui é porque elas são valores calculados vindos da lógica de controle básica de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing). O cálculo é feito dividindo [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) total e [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) de **ProgressRing** e distribuindo uma medida calculada de cada um dos elementos de movimento em seu modelo para que as partes do modelo possam acomodar o conteúdo.

Este é outro exemplo de uso dos modelos de controle do padrão XAML, desta vez mostrando um dos conjuntos de propriedades que são **From** e **To** de uma animação. Isso provém do modelo padrão [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox):

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

Novamente, existem muitos XAML no modelo, portanto, mostramos apenas um trecho. E este é apenas um dos vários estados e animações de tema que usam as mesmas propriedades [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings). Para [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), o uso dos valores **ComboBoxTemplateSettings** por meio de associações reforça que as animações relacionadas no modelo pararão e começarão nas posições que são baseadas em valores compartilhados e, portanto, fazem a transição perfeitamente.

**Observação**    quando você usa **TemplateSettings** valores como parte de seu modelo de controle, verifique se você estiver definindo as propriedades que correspondem ao tipo do valor. Caso contrário, talvez você precise criar um conversor de valor para a associação de forma que o tipo de destino da associação possa ser convertido de um tipo de origem diferente do valor **TemplateSettings**. Para saber mais, consulte [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

## <a name="related-topics"></a>Tópicos relacionados

* [Guia de início rápido: Modelos de controle](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))

