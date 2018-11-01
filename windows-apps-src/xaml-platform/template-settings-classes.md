---
author: jwmsft
description: Classes TemplateSettings
title: Classes TemplateSettings
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d7b08138ab22d4cf2cbf4fb5273759f000a7c94
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5882792"
---
# <a name="template-settings-classes"></a>Classes TemplateSettings


## <a name="prerequisites"></a>Pré-requisitos

Presumimos que você saiba adicionar controles à sua interface do usuário, definir as suas propriedades e atribuir manipuladores de eventos. Para obter instruções sobre como adicionar controles ao seu aplicativo, consulte [Adicionar controles e manipular eventos](https://msdn.microsoft.com/library/windows/apps/mt228345). Também presumimos que você tem noções básicas sobre como definir um modelo personalizado para um controle, fazendo uma cópia do modelo padrão e editando-a. Para saber mais sobre isso, consulte [Início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

## <a name="the-scenario-for-templatesettings-classes"></a>O cenário para classes **TemplateSettings**

As classes **TemplateSettings** fornecem um conjunto de propriedades que são usadas quando você define um novo modelo de controle de um controle. As propriedades têm valores como medidas de pixel para o tamanho de determinadas partes do elemento de interface do usuário. Às vezes, os valores são valores calculados que são provenientes da lógica do controle que normalmente não é fácil de substituir ou até mesmo acessar. Algumas das propriedades servem como valores **From** e **To** que controlam as transições e as animações de partes e, portanto, as propriedades **TemplateSettings** relevantes vêm em pares.

Há várias classes **TemplateSettings**. Todas elas estão no namespace [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818). Aqui está uma lista das classes e um link para a propriedade **TemplateSettings** do controle relevante. Essa propriedade **TemplateSettings** é como você acessa os valores **TemplateSettings** para o controle e pode estabelecer associações de modelo às suas propriedades:

-   [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752): valor de [**ComboBox.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209364)
-   [**GridViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738499): valor de [**GridViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738503)
-   [**ListViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh701948): valor de [**ListViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br242923)
-   [**ProgressBarTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227856): valor de [**ProgressBar.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227537)
-   [**ProgressRingTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702248): valor de [**ProgressRing.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702581)
-   [**SettingsFlyoutTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn298721): valor de [**SettingsFlyout.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn252826)
-   [**ToggleSwitchTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209804): valor de [**ToggleSwitch.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209731)
-   [**ToolTipTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209813): valor de [**ToolTip.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227629)

As propriedades **TemplateSettings** sempre devem ser usadas em XAML, não o código. Elas são subpropriedades somente leitura de uma propriedade **TemplateSettings** somente leitura de um controle pai. Em um cenário de controle personalizado avançado, no qual você está criando uma nova classe baseada em [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) e portanto pode influenciar a lógica do controle, considere a definição de uma propriedade **TemplateSettings** personalizada no controle para comunicar informações que podem ser úteis para qualquer um que esteja remodelando o controle. Assim como esse valor somente leitura da propriedade, defina uma nova classe **TemplateSettings** para o seu controle, que tenha propriedades somente leitura para cada um dos itens de informação, seja relevante para as medidas do modelo, posicionamento da animação, e assim por diante, e forneça aos chamados a instância do tempo de execução dessa classe que é inicializada através da lógica do controle. As classes **TemplateSettings** são derivadas de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), de forma que as propriedades possam usar o sistema de propriedade de relevância dos retornos de chamada com propriedade alterada. Mas os identificadores de propriedade de dependência da propriedades não são expostos como API pública, pois as propriedades **TemplateSettings** não se destinam a ser somente leitura para os chamadores.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Como usar **TemplateSettings** a um modelo de controle.

Aqui está um exemplo que vêm dos modelos de controle XAML padrão iniciais. Este em especial é do modelo padrão de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538):

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

O XAML completo do modelo [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) tem centenas de linhas, portanto, este é apenas um trecho minúsculo. Este XAML define a parte de um controle que é um dos 6 elementos [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que representam a animação de rotação de progresso indeterminado. Como desenvolvedor, talvez você não goste de círculos e talvez use um primitivo de elemento gráfico diferente ou uma forma básica distinta para a progressão da animação. Por exemplo, você pode criar um **ProgressRing** que use um conjunto de elementos [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organizados em um quadrado. Se for o caso, cada componente **Rectangle** individual de seu novo modelo pode se parecer com o seguinte:

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

O motivo pelo qual as propriedades **TemplateSettings** são úteis aqui é porque elas são valores calculados vindos da lógica de controle básica de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538). O cálculo é feito dividindo [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) total e [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) de **ProgressRing** e distribuindo uma medida calculada de cada um dos elementos de movimento em seu modelo para que as partes do modelo possam acomodar o conteúdo.

Este é outro exemplo de uso dos modelos de controle do padrão XAML, desta vez mostrando um dos conjuntos de propriedades que são **From** e **To** de uma animação. Isso provém do modelo padrão [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348):

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

Novamente, existem muitos XAML no modelo, portanto, mostramos apenas um trecho. E este é apenas um dos vários estados e animações de tema que usam as mesmas propriedades [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752). Para [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348), o uso dos valores **ComboBoxTemplateSettings** por meio de associações reforça que as animações relacionadas no modelo pararão e começarão nas posições que são baseadas em valores compartilhados e, portanto, fazem a transição perfeitamente.

**Observação**  quando você usa **TemplateSettings** valores como parte de seu modelo de controle, verifique se você está definindo propriedades que correspondem ao tipo do valor. Caso contrário, talvez você precise criar um conversor de valor para a associação de forma que o tipo de destino da associação possa ser convertido de um tipo de origem diferente do valor **TemplateSettings**. Para saber mais, consulte [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903).

## <a name="related-topics"></a>Tópicos relacionados

* [Início rápido: modelos de controle](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)

