---
author: QuinnRadich
description: Saiba como usar temas e cores de destaque em seus aplicativos UWP.
title: Cor em aplicativos UWP
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3929788"
---
# <a name="color"></a>Cor

![imagem hero](images/header-color.svg)

A cor é uma maneira intuitiva de comunicar informações aos usuários em seu aplicativo: ela pode ser usada para indicar interatividade, enviar comentários sobre ações do usuário e oferecer uma noção de continuidade visual para a interface. 

Nos aplicativos UWP, as cores são determinadas principalmente por cor de destaque e tema. Neste artigo, vamos discutir como você pode usar cores em seu aplicativo e como usar recursos de tema e cor de destaque para que o aplicativo UWP possa ser usado em qualquer contexto de tema. 

## <a name="color-principles"></a>Princípios de cores

:::row:::
    :::column:::
        **Use a cor de forma significativa.**
Quando a cor é usada com moderação para realçar os elementos importantes, ela pode ajudar a criar uma interface do usuário flexível e intuitiva.
    :::column-end:::
    :::column:::
        **Use a cor para indicar interatividade.**
É uma boa ideia escolher uma cor para indicar elementos interativos do aplicativo. Por exemplo, muitas páginas da Web usam texto em azul para indicar um hiperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Cor é pessoal.**
No Windows, os usuário pode escolher uma cor de destaque e um tema claro ou escuro, que são refletidos durante sua experiência. Você pode escolher como incorporar a cor de destaque e o tema do usuário no aplicativo, personalizando a experiência.
    :::column-end:::
    :::column:::
        **A cor é cultural.**
Considere como as cores que você usa serão interpretadas por pessoas de culturas diferentes. Por exemplo, em algumas culturas, a cor azul está associada à virtude e à proteção, enquanto em outras representa tristeza.
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
        Ao criar modelos para controles personalizados, use os pincéis de tema em vez de valores de cores embutidos no código. Assim, seu aplicativo pode adaptar-se a qualquer tema.

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
        ![cabeçalho de destaque selecionado pelo usuário](images/color/user-accent.svg) ![cor de destaque selecionado pelo usuário](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![cabeçalho de destaque personalizada](images/color/custom-accent.svg) ![cor de destaque da marca personalizada](images/color/brand-color.svg)
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

## <a name="usability"></a>Usabilidade

:::row:::
    :::column:::
        ![Ilustração de contraste](images/color/illo-contrast.svg)
    :::column-end:::
    ::: extensão da coluna = "2"::: **contraste**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Ilustração de contraste](images/color/illo-lighting.svg)
    :::column-end:::
    ::: extensão da coluna = "2"::: **iluminação**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Ilustração de contraste](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: extensão da coluna = "2"::: **daltonismo**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artigos relacionados

- [Estilos de XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md)