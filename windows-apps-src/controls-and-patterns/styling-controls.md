---
author: Jwmsft
Description: "Os estilos permitem definir propriedades de controle e reutilizar essas configurações para criar uma aparência consistente em vários controles."
MS-HAID: dev\_ctrl\_layout\_txt.styling\_controls
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Aplicando estilos a controles
ms.assetid: AB469A46-FAF5-42D0-9340-948D0EDF4150
label: Styling controls
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b9bc1e68e26830a283ed49b753f63a8f7ae63637
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="styling-controls"></a>Aplicando estilos a controles



É possível personalizar a aparência de seus aplicativos de muitas formas usando a estrutura XAML. Os estilos permitem definir propriedades de controle e reutilizar essas configurações para criar uma aparência consistente em vários controles.

## <a name="style-basics"></a>Noções básicas de estilos


Use estilos para extrair configurações de propriedades visuais para recursos reutilizáveis. Aqui está um exemplo que mostra 3 botões com um estilo que define as propriedades [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397), [**BorderThickness**](https://msdn.microsoft.com/library/windows/apps/br209399) e [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414). Ao aplicar um estilo, você torna igual a aparência dos controles sem precisar definir essas propriedades em cada controle separadamente.

![botões com estilo definido](images/styles-rainbow-buttons.png)

Você pode definir um estilo embutido na linguagem XAML para um controle, ou como um recurso reutilizável. Defina recursos no arquivo XAML de uma página individual, no arquivo App.xaml ou em um arquivo XAML de dicionário de recursos separado. Um arquivo XAML do dicionário de recursos pode ser compartilhado entre aplicativos e mais de um dicionário de recursos pode ser mesclado em um único aplicativo. O local onde o recurso é definido determina o escopo no qual ele pode ser usado. Recursos de nível de página estão disponíveis somente na página em que são definidos. Se recursos com a mesma chave forem definidos tanto no App.xaml quanto em uma página, o recurso na página substituirá o recurso no aplicativo App.xaml. Se um recurso estiver definido em um arquivo de dicionário de recurso separado, seu escopo será determinado pela referência de onde partiu o dicionário do recurso.

Na definição de [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849), você precisa de um atributo [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) e de uma coleção de um ou mais elementos [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817). O atributo **TargetType** é uma cadeia de caracteres que especifica um tipo [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) ao qual aplicar o estilo. O valor de **TargetType** deve especificar um tipo derivado de **FrameworkElement** definido pelo Windows Runtime ou um tipo personalizado disponível em um assembly referenciado. Se você tentar aplicar um estilo a um controle, e o tipo do controle não corresponder ao atributo **TargetType** do estilo que você está tentando aplicar, uma exceção ocorrerá.

Cada elemento [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) requer uma [**Property**](https://msdn.microsoft.com/library/windows/apps/br208836) e um [**Value**](https://msdn.microsoft.com/library/windows/apps/br208838). Essas configurações de propriedade indicam a qual propriedade de controle a configuração se aplica e qual é o valor a ser definido para essa propriedade. Você pode definir **Setter.Value** com a sintaxe de elemento de propriedade ou atributo. O XAML aqui mostra o estilo aplicado aos botões mostrados anteriormente. Nesse XAML, os dois primeiros elementos **Setter** usam a sintaxe de atributos, mas o último **Setter** para a propriedade [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397) usa a sintaxe de elementos de propriedade. O exemplo não usa o [atributo x:Key](../xaml-platform/x-key-attribute.md); portanto, o estilo é aplicado implicitamente aos botões. A aplicação implícita ou explícita de estilos é explicada na próxima seção.

```XAML
<Page.Resources>
    <Style TargetType="Button">
        <Setter Property="BorderThickness" Value="5" />
        <Setter Property="Foreground" Value="Blue" />
        <Setter Property="BorderBrush" >
            <Setter.Value>
                <LinearGradientBrush StartPoint="0.5,0" EndPoint="0.5,1">
                    <GradientStop Color="Yellow" Offset="0.0" />
                    <GradientStop Color="Red" Offset="0.25" />
                    <GradientStop Color="Blue" Offset="0.75" />
                    <GradientStop Color="LimeGreen" Offset="1.0" />
                </LinearGradientBrush>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<StackPanel Orientation="Horizontal">
    <Button Content="Button"/>
    <Button Content="Button"/>
    <Button Content="Button"/>
</StackPanel>
```

## <a name="apply-an-implicit-or-explicit-style"></a>Aplicar um estilo implícito ou explícito

Se você definir um estilo como um recurso, haverá duas maneiras de aplicá-lo aos seus controles:

-   Implicitamente, especificando apenas um [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) para o [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849).
-   Explicitamente, especificando um atributo [**TargetType**](https://msdn.microsoft.com/library/windows/apps/br208857) e um [atributo x:Key](../xaml-platform/x-key-attribute.md) para [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) e, em seguida, definindo a propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) do controle de destino com uma referência da [extensão de marcação {StaticResource}](https://msdn.microsoft.com/library/windows/apps/mt185588) que use a chave explícita.

Se um estilo contiver o [atributo x:Key](../xaml-platform/x-key-attribute.md), você só poderá aplicá-lo a um controle definindo a propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) do controle como o estilo com chave. Por outro lado, um estilo sem um atributo x:Key é automaticamente aplicado a todos os controles do seu tipo de destino que, de outra forma, não teriam uma definição de estilo explícita.

Aqui há dois botões que demonstram estilos implícitos e explícitos.

![botões com o estilo definido de forma implícita e explícita.](images/styles-buttons-implicit-explicit.png)

Neste exemplo, o primeiro estilo tem um [atributo x:Key](../xaml-platform/x-key-attribute.md), e seu tipo de destino é [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). A propriedade [**Style**](https://msdn.microsoft.com/library/windows/apps/br208743) do primeiro botão é definida para essa chave, de maneira que esse estilo é aplicado explicitamente. O segundo estilo é aplicado explicitamente ao segundo botão porque seu tipo de destino é **Button** e o estilo não tem um atributo x:Key.

```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="RenderTransform">
            <Setter.Value>
                <RotateTransform Angle="25"/>
            </Setter.Value>
        </Setter>
        <Setter Property="BorderBrush" Value="Orange"/>
        <Setter Property="BorderThickness" Value="2"/>
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

## <a name="use-based-on-styles"></a>Usar estilos baseados em outros

Para facilitar a manutenção de estilos e otimizar sua reutilização, você pode criar estilos que herdam de outros estilos. Você usa a propriedade [**BasedOn**](https://msdn.microsoft.com/library/windows/apps/br208852) para criar estilos herdados. Estilos que herdam de outros estilos devem ter como destino o mesmo tipo de controle ou um controle derivado do tipo de destino do estilo base. Por exemplo, se um estilo base tiver como destino [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365), estilos baseados nesse estilo podem ter como alvo **ContentControl** ou tipos derivados de **ContentControl**, como [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) e [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527). Se um valor não é definido no estilo baseado, ele é herdado do estilo base. Para alterar um valor do estilo base, o estilo baseado substitui esse valor. O próximo exemplo mostra um **Button** e uma [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) com estilos herdados do mesmo estilo base.

![botões estilizados com estilos baseados.](images/styles-buttons-based-on.png)

O estio base tem como destino [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/br209365) e define as propriedades [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) e [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751). Os estilos baseados neste estilo têm como destino [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) e [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265), que derivam de **ContentControl**. Os estilos baseados definem cores diferentes para as propriedades [**BorderBrush**](https://msdn.microsoft.com/library/windows/apps/br209397) e [**Foreground**](https://msdn.microsoft.com/library/windows/apps/br209414). (Normalmente você não coloca uma borda ao redor de uma **CheckBox**. Fazemos isso aqui para mostrar os efeitos do estilo.)

```XAML
<Page.Resources>
    <Style x:Key="BasicStyle" TargetType="ContentControl">
        <Setter Property="Width" Value="130" />
        <Setter Property="Height" Value="30" />
    </Style>

    <Style x:Key="ButtonStyle" TargetType="Button" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Orange" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Red" />
    </Style>

    <Style x:Key="CheckBoxStyle" TargetType="CheckBox" 
           BasedOn="{StaticResource BasicStyle}">
        <Setter Property="BorderBrush" Value="Blue" />
        <Setter Property="BorderThickness" Value="2" />
        <Setter Property="Foreground" Value="Green" />
    </Style>
</Page.Resources>

<StackPanel>
    <Button Content="Button" Style="{StaticResource ButtonStyle}" Margin="0,10"/>
    <CheckBox Content="CheckBox" Style="{StaticResource CheckBoxStyle}"/>
</StackPanel>
```

## <a name="use-tools-to-work-with-styles-easily"></a>Use ferramentas para trabalhar facilmente com estilos

Uma forma rápida de aplicar estilos aos seus controles é clicar com o botão direito do mouse em um controle na superfície de design XAML do Microsoft Visual Studio e selecionar **Editar Estilo** ou **Editar Modelo** (dependendo do controle no qual você está clicando com o botão direito). Você pode, então, aplicar um estilo existente selecionando **Aplicar Recurso** ou pode definir um novo estilo selecionando **Criar Vazio**. Se você criar um estilo vazio, terá a opção de defini-lo na página, no arquivo App.xaml ou em um dicionário de recursos separado.

## <a name="modify-the-default-system-styles"></a>Modificar os estilos de sistema padrão

Você deve usar os estilos provenientes dos recursos XAML padrão do Windows Runtime sempre que possível. Quando for necessário definir seus próprios estilos, tente baseá-los nos estilos padrão sempre que possível (usando estilos baseados conforme explicado anteriormente, ou editando uma cópia do estilo padrão original).

## <a name="the-template-property"></a>A propriedade Template

Um setter de estilo pode ser usado para a propriedade [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) de um [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) e, na verdade, isso compõe a maior parte de um estilo XAML típico e dos recursos XAML de um aplicativo. Isso é discutido com mais detalhes no tópico [Modelos de controle](control-templates.md).

