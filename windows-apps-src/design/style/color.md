---
description: Saiba como usar temas e cores de destaque em seus aplicativos do Windows.
title: Cor em aplicativos do Windows
ms.date: 04/07/2019
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f5e103b7661c53fb70561dd1bd654188be2704ff
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970761"
---
# <a name="color"></a>Cor

![imagem Hero](images/header-color.svg)

A cor é uma maneira intuitiva de comunicar informações aos usuários em seu aplicativo: ela pode ser usada para indicar interatividade, enviar comentários sobre ações do usuário e oferecer uma noção de continuidade visual para a interface.

Nos aplicativos do Windows, as cores são determinadas principalmente por cor de destaque e tema. Neste artigo, discutiremos como você pode usar cores em seu aplicativo e como usar recursos de tema e cor de destaque para que o aplicativo do Windows possa ser usado em qualquer contexto de tema.

## <a name="color-principles"></a>Princípios de cores

:::row:::
    :::column:::
**Use as cores de modo relevante.**
Quando a cor é usada com moderação para realçar os elementos importantes, ela pode ajudar a criar uma interface do usuário flexível e intuitiva.
    :::column-end:::
    :::column:::
**Use a cor para indicar interatividade.**
É uma boa ideia escolher uma cor para indicar elementos interativos do seu aplicativo. Por exemplo, muitas páginas da Web usam texto em azul para indicar um hiperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
**A cor é pessoal.**
No Windows, os usuário pode escolher uma cor de destaque e um tema claro ou escuro, que são refletidos durante sua experiência. Você pode escolher como incorporar a cor de destaque e o tema do usuário no seu aplicativo, personalizando a experiência.
    :::column-end:::
    :::column:::
**A cor é cultural.**
Considere como as cores que você usa serão interpretadas por pessoas de culturas diferentes. Por exemplo, em algumas culturas, a cor azul está associada à virtude e à proteção, enquanto em outras representa tristeza.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Temas

Os aplicativos do Windows podem usar um tema de aplicativo claro ou escuro. O tema afeta as cores da tela de fundo, do texto, dos ícones e dos [controles comuns](../controls-and-patterns/index.md) do aplicativo.

### <a name="light-theme"></a>Tema claro

![tema claro](images/color/light-theme.svg)

### <a name="dark-theme"></a>Tema escuro

![tema escuro](images/color/dark-theme.svg)

Por padrão, o tema do aplicativo do Windows é a preferência de tema do usuário das Configurações do Windows ou o tema do padrão do dispositivo (ou seja, escuro no Xbox). No entanto, é possível definir o tema do aplicativo do Windows.

### <a name="changing-the-theme"></a>Alterar o tema

Você pode alterar temas facilmente alterando a propriedade **RequestedTheme** no arquivo `App.xaml`.

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

Os usuários também podem selecionar o tema de alto contraste, que usa uma pequena paleta de cores contrastantes que facilita a visualização da interface. Nesse caso, o sistema substituirá o RequestedTheme.

### <a name="testing-themes"></a>Testar temas

Se você não solicitar um tema para o aplicativo, certifique-se de testá-lo com temas claros e escuros para garantir que o aplicativo seja legível em todas as condições.

**Observação**: no Visual Studio, o RequestedTheme padrão é claro, portanto, é necessário alterar o RequestedTheme para testar os dois.

## <a name="theme-brushes"></a>Pincéis de temas

Os controles comuns automaticamente usam [pincéis de tema](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) para ajustar o contraste de temas claros e escuros.

Por exemplo, confira uma ilustração de como a [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) usa pincéis de tema:

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
Ao criar modelos para controles personalizados, use os pincéis de tema em vez dos valores de cores embutidos no código. Deste modo, seu aplicativo se adapta com facilidade a qualquer tema.

Por exemplo, esses [modelos de item para ListView](../controls-and-patterns/item-templates-listview.md) demonstram como usar pincéis de tema em um modelo personalizado.
    :::column-end:::
    :::column:::
 ![exemplo de item de lista de linha dupla com ícone](images/color/list-view.svg)
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

Para saber mais sobre como usar pincéis para temas em seu aplicativo, confira [Recursos de tema](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Cor de destaque

Os controles comuns usam uma cor de destaque para transmitir informações de estado. Por padrão, a cor de destaque é a `SystemAccentColor` que os usuários selecionam em suas Configurações. No entanto, você também pode personalizar a cor de destaque do aplicativo para refletir sua marca.

![controles do Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
![cabeçalho de destaque selecionado pelo usuário](images/color/user-accent.svg)
![cor de destaque selecionada pelo usuário](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
![cabeçalho de destaque personalizado](images/color/custom-accent.svg)
![cor de destaque de marca personalizada](images/color/brand-color.svg)
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

Se você selecionar uma cor de destaque personalizada para seu aplicativo, certifique-se de que o texto e a tela de fundo que usam a cor de destaque têm contraste suficiente para permitir uma ótima legibilidade. Para testar o contraste, você pode usar a ferramenta de seleção de cores nas configurações do Windows ou usar as [ferramentas de contraste online](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Seletor de cor de destaque personalizada nas configurações do Windows](images/color/color-picker.svg)

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
Você também pode acessar a paleta de cores de destaque programaticamente com o método [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) e a enumeração [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

Você pode usar a paleta de cores de destaque para temas de cores em seu aplicativo. Confira abaixo um exemplo de como você pode usar a paleta de cores de destaque em um botão.

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

Ao usar texto colorido em uma tela de fundo colorida, verifique se há contraste suficiente entre o texto e a tela de fundo. Por padrão, hiperlinks ou o hipertextos usarão a cor de destaque. Se você aplicar variações da cor de destaque à tela de fundo, deverá usar uma variação da cor de destaque original para otimizar o contraste do texto colorido em uma tela de fundo colorida.

O gráfico a seguir ilustra um exemplo dos diversos tons de claro/escuro da cor de destaque e como tipos de cores podem ser aplicados em uma superfície colorida.

![cor com base em outra cor](images/color/color-on-color.png)

Para saber mais sobre os controles de estilo, confira [Estilos de XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de cor

Existem várias APIs que podem ser usadas para adicionar cor ao seu aplicativo. Primeiro, a classe [**Colors**](https://docs.microsoft.com/uwp/api/windows.ui.colors), que implementa uma lista abrangente de cores predefinidas. Elas podem ser acessadas automaticamente com as propriedades XAML. No exemplo a seguir, é possível criar um botão e definir as propriedades de cor de primeiro e segundo plano de acordo com os membros da classe **Colors**.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Você pode criar suas próprias cores de RGB ou valores hexadecimais usando a estrutura [**Color**](https://docs.microsoft.com/uwp/api/windows.ui.color) em XAML.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

Você também pode criar a mesma cor no código usando o método **FromArgb**.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

As letras "Argb" significam alfa (opacidade), vermelho, verde e azul, que são os quatro componentes de uma cor. Cada argumento pode variar de 0 a 255. Você pode optar por omitir o primeiro valor, o que oferece uma opacidade padrão de 255 ou 100% opaco.

> [!Note]
> Se você estiver usando C++, é necessário criar cores usando a classe [**ColorHelper**](https://docs.microsoft.com/uwp/api/windows.ui.colorhelper).

O uso mais comum para uma **Cor** é como um argumento para [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.solidcolorbrush), que pode ser usado para pintar elementos da interface do usuário como uma única cor sólida. Esses pincéis geralmente são definidos em um [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary), portanto, eles podem ser reutilizados em vários elementos.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Para saber mais sobre como usar pincéis, confira [Pincéis XAML](brushes.md).

## <a name="scoping-system-colors"></a>Cores do sistema de escopo

Além de definir suas próprias cores no aplicativo, você também pode definir o escopo das cores sistematizadas nas regiões desejadas em todo o aplicativo usando a marcação **ColorPaletteResources**. Essa API permite não apenas colorir e criar grandes grupos de controles ao mesmo tempo definindo algumas propriedades, mas também oferece muitos outros benefícios do sistema que você normalmente não obteria ao definir suas próprias cores personalizadas de forma manual:

- Qualquer cor definida usando **ColorPaletteResources** não afetará o Alto Contraste
  * Isso significa que seu aplicativo estará acessível a mais pessoas sem qualquer design adicional ou custo de desenvolvimento
- Você pode definir cores facilmente como claras, escuras ou difusas em ambos os temas, definindo uma propriedade na API
- As cores definidas em **ColorPaletteResources** serão colocadas em cascata em todos os controles semelhantes que também usam essa cor do sistema
  * Isso garante que você tenha uma história de cores consistente em todo o seu aplicativo, mantendo a aparência da sua marca
- Afeta todos os estados visuais, animações e variações de opacidade sem precisar reformular o modelo

### <a name="how-to-use-colorpaletteresources"></a>Como usar a ColorPaletteResources

A ColorPaletteResources é uma API que informa ao sistema quais recursos estão sendo definidos onde. ColorPaletteResources deve ter um [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), que pode ser uma das três opções:
- Padrão
  * Irá mostrar as alterações de cor nos temas [Claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) e [Escuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)
- Leve
  * Irá mostrar as alterações de cor apenas no tema [Claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme)
- Escuro
  * Irá mostrar as alterações de cor apenas no tema [Escuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

Definir esse x:Key garantirá que as cores sejam alteradas adequadamente para o sistema ou o tema do aplicativo, caso você deseje uma aparência personalizada diferente quando estiver em um dos temas.

### <a name="how-to-apply-scoped-colors"></a>Como aplicar cores com escopo

O escopo de recursos através da API **ColorPaletteResources** em XAML permite que você selecione qualquer cor ou pincel do sistema que esteja em nossa biblioteca de [recursos de tema](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) e redefina-a dentro do escopo de uma página ou contêiner.

Por exemplo, se você definiu duas cores de sistema – **BaseLow** e **BaseMediumLow** dentro de uma grade e, em seguida, posicionou dois botões em sua página: um dentro dessa grade e um fora:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Você obteria **Button_A** com as novas cores aplicadas e **Button_B** permaneceria semelhante ao botão padrão do nosso sistema:

![cores do sistema com escopo no botão](images/color/scopedcolors_cyan_button.png)

No entanto, como todas as cores do nosso sistema se conectam a outros controles também, definir **BaseLow** e **BaseMediumLow** afetará mais do que apenas os botões. Neste caso, controles como **ToggleButton**, **RadioButton** e **Slider** também serão afetados por essas mudanças de cor do sistema, caso esses controles sejam colocados no escopo da grade do exemplo acima.
Se você deseja definir o escopo de uma mudança de cor do sistema *para um único controle apenas*, você pode fazer isso definindo **ColorPaletteResources** dentro dos recursos do controle:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Falando de forma básica, você tem exatamente a mesma coisa de antes, mas agora quaisquer outros controles adicionados à grade não irão captar as mudanças de cor. Isso ocorre porque essas cores do sistema têm escopo apenas para **Button_A**.

### <a name="nesting-scoped-resources"></a>Recursos do escopo de aninhamento

O aninhamento das cores do sistema também é possível, e é feito posicionando **ColorPaletteResources** nos recursos dos elementos aninhados na marcação do layout do seu aplicativo:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

Neste exemplo, **Button_A** está herdando as cores definidas nos recursos de **Grid_A**, e o **Botão Aninhado** está herdando as cores dos recursos de **Grid_B** . Por extensão, isso significa que qualquer outro controle posicionado dentro de **Grid_B** verificará ou aplicará os recursos de **Grid_B** primeiro, antes de verificar ou aplicar os recursos de **Grid_A**, e finalmente aplicando nossas cores padrão se nada for definido na página ou no nível do aplicativo.

Isso funciona para qualquer número de elementos aninhados cujos recursos tenham definições de cores.

### <a name="scoping-with-a-resourcedictionary"></a>Escopo com um ResourceDictionary

Você não está limitado a um contêiner ou a recursos da página, e também pode definir essas cores do sistema em um ResourceDictionary que pode ser mesclado em qualquer escopo da mesma forma que você normalmente mesclaria um dicionário.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

Primeiro, você criaria um ResourceDictionary. Em seguida, coloque **ColorPaletteResources** nos ThemeDictionaries e substitua as cores do sistema desejadas:

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

Na página que contém seu layout, basta mesclar esse dicionário no escopo desejado:

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

Agora, todos os recursos, temas e cores personalizados podem ser colocados em um único recurso **MyCustomTheme** e delimitados quando necessário, sem ter que se preocupar com a confusão extra na marcação de layout.

### <a name="other-ways-to-define-color-resources"></a>Outras maneiras de definir recursos de cores

ColorPaletteResources também permite que as cores do sistema sejam posicionadas e definidas diretamente dentro dele como um wrapper, em vez de na linha:

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>Usabilidade

:::row:::
    :::column:::
![ilustração de contraste](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
**Contraste**

Verifique se os elementos e as imagens têm contraste suficiente para diferenciarem-se entre si, independentemente da cor ou do tema de destaque.

Ao considerar quais cores usar em seu aplicativo, a acessibilidade deve ser uma preocupação principal. Use as diretrizes abaixo para garantir que seu aplicativo esteja acessível para o máximo de usuários possível.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![ilustração de contraste](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
**Iluminação**

Lembre-se de que a variação na iluminação ambiente pode afetar a usabilidade do seu aplicativo. Por exemplo, uma página com tela de fundo preto pode se tornar ilegível em um ambiente externo devido ao brilho da tela, ao passo que uma página com tela de fundo branco pode ser difícil de olhar em uma sala escura.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
![ilustração de contraste](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
**Daltonismo**

Lembre-se de que o daltonismo pode afetar a usabilidade do seu aplicativo. Por exemplo, um usuário com daltonismo para vermelho-verde terá dificuldade em distinguir elementos vermelhos e verdes entre si. Cerca de **8% dos homens** e **0,5% das mulheres** são daltônicos para vermelho-verde, portanto, evite usar essas combinações de cores como fator de diferenciação único entre os elementos do seu aplicativo.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artigos relacionados

- [Estilos de XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md)
