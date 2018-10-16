---
author: Jwmsft
description: Saiba como usar temas e cores de destaque em seus aplicativos UWP.
title: Cor em aplicativos UWP
ms.author: jimwalk
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: e5d26d0e2fc8c040af81645477e3b75b0d128738
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4681832"
---
# <a name="color"></a>Cor

![imagem hero](images/header-color.svg)

A cor é uma maneira intuitiva de comunicar informações aos usuários em seu aplicativo: ela pode ser usada para indicar interatividade, enviar comentários sobre ações do usuário e oferecer uma noção de continuidade visual para a interface. 

Nos aplicativos UWP, as cores são determinadas principalmente por cor de destaque e tema. Neste artigo, vamos discutir como você pode usar cores em seu aplicativo e como usar recursos de tema e cor de destaque para que o aplicativo UWP possa ser usado em qualquer contexto de tema. 

## <a name="color-principles"></a>Princípios de cores

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Temas

Os aplicativos UWP podem usar um tema de aplicativo claro ou escuro. O tema afeta as cores da tela de fundo, do texto, dos ícones e dos [controles comuns](../controls-and-patterns/index.md) do aplicativo.

### <a name="light-theme"></a>Tema claro

![tema claro](images/color/light-theme.svg)

### <a name="dark-theme"></a>Tema escuro

![tema escuro](images/color/dark-theme.svg)

Por padrão, o tema do seu aplicativo UWP é a preferência de tema do usuário nas Configurações do Windows ou o tema do padrão do dispositivo (isto é, escuro no XBox). No entanto, você pode definir o tema do aplicativo UWP. 

### <a name="changing-the-theme"></a>Alterar o tema

Você pode alterar temas facilmente alterando a propriedade **RequestedTheme** no arquivo `App.xaml`:

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

A remoção da propriedade **RequestedTheme** significa que seu aplicativo usará as configurações do sistema do usuário.

Os usuários também podem selecionar o tempo de alto contraste, que usam uma pequena paleta de cores contrastantes que facilitam a visualização da interface. Nesse caso, o sistema substituirá o RequestedTheme.

### <a name="testing-themes"></a>Teste de temas

Se você não solicitar um tema para o aplicativo, certifique-se de testá-lo com temas claros e escuros para garantir que o aplicativo será legível em todas as condições.

**Observação**: no Visual Studio, o RequestedTheme padrão é claro, portanto, é necessário alterar o RequestedTheme para testar os dois.

## <a name="theme-brushes"></a>Pincéis de temas

Os controles comuns automaticamente usam [pincéis de tema](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) para ajustar o contraste de temas claros e escuros.

Por exemplo, veja uma ilustração de como a [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) usa pincéis de tema:

![exemplo de controle de pincéis de tema](images/color/theme-brushes.svg)

Os pincéis de tema são usados para:

- **Base** é para texto.
- **Alt** é o inverso da Base.
- **Chrome** é para elementos de nível superior, como painéis de navegação ou barras de comandos.
- **List** é para controles de lista.

**Low**/**Medium**/**High** referem-se à intensidade da cor.

### <a name="using-theme-brushes"></a>Uso de pincéis de tema

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Para obter mais informações sobre como usar pincéis para temas em seu aplicativo, consulte [Recursos de tema](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Cor de destaque

Os controles comuns usam uma cor de destaque para transmitir informações de estado. Por padrão, a cor de destaque é a `SystemAccentColor` que os usuários selecionam em suas Configurações. No entanto, você também pode personalizar a cor de destaque do aplicativo para refletir sua marca.

![controles do Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>Substituição da cor de destaque

Para alterar a cor de destaque do aplicativo, coloque o seguinte código em `app.xaml`.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>Escolher uma cor de destaque

Se você selecionar uma cor de destaque personalizada para seu aplicativo, certifique-se de que o texto e a tela de fundo que usam a cor de destaque têm contraste suficiente para permitir a leitura ideal. Para testar o contraste, você pode usar a ferramenta do seletor de cor nas configurações do Windows ou você pode usar as [ferramentas de contraste online](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Seletor de cores de destaque personalizadas nas configurações do Windows](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>Paleta de cor de destaque

Um algoritmo de cor de destaque no shell do Windows gera tonalidades claras e escuras da cor de destaque.

![paleta de cor de destaque](images/color/accent-color-palette.svg)

Esses tons podem ser acessados como [recursos de tema](../controls-and-patterns/xaml-theme-resources.md):

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
Você também pode acessar a paleta de cores de destaque programaticamente com o método [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) method e a enumeração [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

Você pode usar a paleta de cores de destaque para temas de cores em seu aplicativo. Veja abaixo um exemplo de como você pode usar a paleta de cores de destaque em um botão.

![Paleta de cores de destaque aplicada a um botão](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

Ao usar o texto colorido em um plano de fundo colorido, verifique se que há contraste suficiente entre o texto e a tela de fundo. Por padrão, os hiperlinks ou o hipertexto usarão a cor de destaque. Se você aplicar variações de cor de destaque à tela de fundo, você deve usar uma variação da cor de destaque original para otimizar o contraste de texto colorido em uma tela de fundo colorida.

O gráfico a seguir ilustra um exemplo dos diversos tons de claro/escuro da cor de destaque e como tipos de cores podem ser aplicados em uma superfície colorida.

![cor com base em outra cor](images/color/color-on-color.svg)

Para obter mais informações sobre os controles de estilo, consulte [Estilos XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de cor

Existem várias APIs que podem ser usadas para adicionar cores ao aplicativo. Primeiro, a classe [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors), que implementa uma lista abrangente de cores predefinidas. Elas podem ser acessadas automaticamente com as propriedades XAML. No exemplo a seguir, é possível criar um botão e definir as propriedades de cor de primeiro e segundo plano de acordo com os membros da classe **Colors**.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Você pode criar suas próprias cores de RGB ou valores hexadecimais usando a estrutura [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) em XAML.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

Você também pode criar a mesma cor no código usando o método **FromArgb**.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

As letras "Argb" significam alfa (opacidade), vermelho, verde e azul, que são os quatro componentes de uma cor. Cada argumento pode variar de 0 a 255. Você pode optar por omitir o primeiro valor, o que oferece uma opacidade padrão de 255 ou 100% opaco.

> [!Note]
> Se você estiver usando C++, é necessário criar cores usando a classe [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

O uso mais comum para uma **Cor** é como um argumento para [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), que pode ser usado para pintar elementos da interface do usuário como uma única cor sólida. Esses pincéis geralmente são definidos em um [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary), portanto, eles podem ser reutilizados em vários elementos.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Para obter mais informações sobre como usar pincéis, consulte [Pincéis XAML](brushes.md).

## <a name="scoping-system-colors"></a>Escopo de cores do sistema

Além de definir suas próprias cores em seu aplicativo, você também pode abranger nossas cores systematized para regiões desejados em todo o aplicativo usando a marca **ColorSchemeResources** . Essa API permite que você não só colorização e grupos grandes de tema de controles ao mesmo tempo ao definir algumas propriedades, mas também permite que você muitos outro sistema benefícios que você normalmente não obtém com definindo suas próprias cores personalizadas manualmente:

- Qualquer cor definida usando **ColorSchemeResources** não terão efeito alto contraste
  * Ou seja, seu aplicativo estarão acessível a mais pessoas sem adicionais de design ou desenvolvimento custo
- Pode facilmente definir cores generalizada, escuro ou claro para ambos os temas, definindo uma propriedade sobre a API
- Cores definida em **ColorSchemeResources** serão transmitida para todos os controles semelhantes que também usam essa cor do sistema
  * Isso garante que você tem uma história de cores consistente em seu aplicativo, mantendo a aparência de sua marca
- Todos os estados visuais, animações e variações de opacidade de efeitos sem a necessidade de novo modelo

### <a name="how-to-use-colorschemeresources"></a>Como usar ColorSchemeResources

ColorSchemeResources é uma API que informa o sistema quais recursos estão sendo no escopo onde. ColorSchemeResources deve realizar uma [X:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), que pode ser uma das três opções:
- Padrão
  * Mostrará as alterações de cor no tema [claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) e [escuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)
- Luz
  * Mostrará as alterações de cor apenas no [tema claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- Dark
  * Mostrará as alterações de cor apenas no [tema escuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

Definir esse X:Key garantirá que suas cores alterar corretamente para o tema do sistema ou aplicativo, caso você queira uma aparência personalizada diferente quando no tema.

### <a name="how-to-apply-scoped-colors"></a>Como aplicar cores analisadas

Escopo de recursos por meio do **ColorSchemeResources** API em XAML permite que você se qualquer cor do sistema ou pincel que está em nossa biblioteca de [recursos de tema](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) e redefini-los no escopo de uma página ou um contêiner.

Por exemplo, se você definido duas cores do sistema - **SystemBaseLowColor** e **SystemBaseMediumLowColor** dentro de uma grade e, em seguida, colocado dois botões na sua página: um dentro dessa grade e fora de um:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Você obteria **Button_A** com as novas cores aplicadas e **Button_B** permaneceriam aparência como nosso botão padrão do sistema:

![cores do sistema analisados no botão](images/color/scopedcolors_cyan_button.png)

No entanto, desde que todos os nossos cores do sistema também cascata para outros controles, definir **SystemBaseLowColor** e **SystemBaseMediumLowColor** afetará mais do que apenas os botões. Nesse caso, os controles como **ToggleButton**, **RadioButton** e **Slider** também serão afetados por essas alterações de cor do sistema, esses controles seja colocados acima escopo da grade de exemplo.
Se você deseja analisar um sistema cor alterar *a um único controles só* é possível fazer isso definindo **ColorSchemeResources** dentro de recursos do controle:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Você essencialmente têm exatamente a mesma coisa como antes, mas agora quaisquer outros controles adicionados à grade não obterão as alterações de cor. Isso ocorre porque essas cores de sistema passam para **Button_A** somente.

### <a name="nesting-scoped-resources"></a>Recursos de aninhamento escopo

Aninhamento de cores do sistema também é possível e isso é feito colocando **ColorSchemeResources** nos recursos dos elementos aninhados dentro da marcação do layout do aplicativo:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

Neste exemplo, **Button_A** é Herdar cores definem em recursos de **Grid_A**do e **Botão aninhado** é Herdar cores de recursos de **Grid_B**do. Por extensão, isso significa que todos os outros controles colocados dentro **Grid_B** verificar ou aplicar recursos **Grid_B**do primeiro, antes de verificar ou aplicar **Grid_A**do recursos, e finalmente aplicação nossas cores padrão se nada é definido no nível de página ou do aplicativo.

Isso funciona para qualquer número de elementos aninhados cujos recursos têm definições de cores.

### <a name="scoping-with-a-resourcedictionary"></a>Escopo com um ResourceDictionary

Você não está limitado a um contêiner ou recursos da página e também pode definir essas cores do sistema em um ResourceDictionary que pode então ser mesclado em qualquer escopo a maneira como você normalmente faria mesclar um dicionário.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Primeiro, você criaria um ResourceDictionary. Em seguida, coloque o **ColorSchemeResources** dentro do ThemeDictionaries e substituir as cores do sistema desejado:

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>

        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

Na página que contém o layout, simplesmente mescle esse dicionário no escopo desejado:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>
             
    <Button Content="Button_A"/>
</Grid>
```

Agora, todos os recursos de temas e cores personalizadas podem ser colocadas em um dicionário de recursos **MyCustomTheme** único e escopo onde for necessário sem precisar preocupar extra desorganização em sua marcação de layout.

### <a name="other-ways-to-define-color-resources"></a>Outras maneiras de definir recursos de cor

ColorSchemeResources também permite cores do sistema deve ser colocado e definindo diretamente dentro dele, como um wrapper em vez de linha:

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>Usabilidade

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artigos relacionados

- [Estilos de XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md)