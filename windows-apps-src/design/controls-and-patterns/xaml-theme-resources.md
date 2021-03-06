---
Description: Recursos de temas no XAML são um conjunto de recursos que aplicam valores diferentes de acordo com o tema do sistema que estiver ativo.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Recursos de temas XAML
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b16ad72541f34e40d1b0cf534082eb68b0843141
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234728"
---
# <a name="xaml-theme-resources"></a>Recursos de temas XAML

Recursos de temas no XAML são um conjunto de recursos que aplicam valores diferentes de acordo com o tema do sistema que estiver ativo. Há três temas que são aceitos pela estrutura XAML: "Light", "Dark" E "HighContrast".

**Pré-requisitos**: Este tópico pressupõe que você tenha lido as [Referências a recursos ResourceDictionary e XAML](resourcedictionary-and-xaml-resource-references.md).

## <a name="theme-resources-v-static-resources"></a>Recursos de tema vs. recursos estáticos

Existem duas extensões de marcação XAML que podem referenciar um recurso XAML de um dicionário de recursos XAML existente: a[extensão de marcação {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) e a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md).

A avaliação de uma [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) ocorre quando o aplicativo é carregado e, posteriormente, toda vez que o tema é alterado em runtime. Normalmente é o resultado da alteração que o usuário faz nas configurações do dispositivo ou de uma alteração programática dentro do aplicativo que altera o tema atual.

Por outro lado, uma [extensão de marcação {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) é avaliada somente quando o XAML é carregado pela primeira vez pelo aplicativo. Ele não é atualizado. Isso é semelhante a localizar e substituir no XAML pelo valor do runtime real na inicialização do aplicativo.

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>Recursos de tema na estrutura de dicionário de recursos

Cada recurso de tema é parte do arquivo XAML themeresources.xaml. Para fins de design, themeresources.xaml está disponível na pasta \\(Arquivos de Programas)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;versão do SDK&gt;\\Generic em uma instalação do SDK (Software Development Kit (SDK) do Windows. Os dicionários de recursos em themeresources.xaml também são reproduzidos em generic.xaml no mesmo diretório.

O Windows Runtime não usa esses arquivos físicos para pesquisa de tempo de execução. É por isso que eles estão especificamente em uma pasta DesignTime, e eles não são copiados para aplicativos por padrão. Em vez disso, esses dicionários de recursos existem na memória como parte do Windows Runtime propriamente dito, e as referências de recurso XAML do aplicativo a recursos de tema (ou recursos do sistema) são resolvidas no tempo de execução.

## <a name="guidelines-for-custom-theme-resources"></a>Diretrizes para recursos de temas personalizados

Siga estas diretrizes ao definir e consumir os seus próprios recursos de tema personalizados:

- Especifique dicionários de temas para "Light" e "Dark", além do dicionário "HighContrast". Embora você possa criar um [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) com "Default" como a chave, é preferível ser explícito e, em vez disso, usar "Light", "Dark" e "HighContrast".

- Use a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) em: Estilos, Setters, Modelos de controle, Setters de propriedade e Animações.

- Não use a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) em suas definições de recurso dentro de seu [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries). Use a [extensão de marcação {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) no lugar.

    EXCEÇÃO: você pode usar a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) para fazer referência a recursos independentes do tema do aplicativo em [ThemeDictionaries](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries). Exemplos desses recursos são recursos de cor de destaque como `SystemAccentColor`, ou recursos de cor do sistema, que normalmente têm o prefixo "SystemColor" como `SystemColorButtonFaceColor`.

> [!CAUTION]
> Caso não siga essas diretrizes, você pode notar um comportamento inesperado relacionado aos temas no aplicativo. Para obter mais informações, consulte a seção [Solução de problemas de recursos de tema](#troubleshooting-theme-resources).

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>A rampa de cores XAML e os pincéis dependentes de temas

O conjunto combinado de cores para temas "Light", "Dark" e "HighContrast" formam a *rampa de cores do Windows* em XAML. Independentemente de desejar modificar os temas do sistema ou aplicar um tema do sistema aos elementos XAML próprios, é importante compreender como os recursos de cores são estruturados.

Para obter informações adicionais sobre como aplicar cor em seu aplicativo do Windows, confira [Cor em aplicativos do Windows](../style/color.md).

### <a name="light-and-dark-theme-colors"></a>Cores dos temas Light e Dark

A estrutura XAML fornece um conjunto de recursos nomeado [Color](/uwp/api/Windows.UI.Color) com valores personalizados para os temas "Light" e "Dark". As chaves que você usa para fazer referência a eles seguem o formato de nomenclatura: `System[Simple Light/Dark Name]Color`.

Esta tabela lista a chave, o nome simples e a representação da cadeia de caracteres da cor (usando o formato \#aarrggbb) dos recursos "Light" e "Dark" fornecidos pela estrutura XAML. A chave é usada para fazer referência ao recurso em um aplicativo. O "Simple light/dark name" é usado como parte da convenção de nomenclatura de pincel que explicaremos mais adiante.

| Chave                             | Simple light/dark name | Leve      | Escuro       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### <a name="light-theme"></a>Tema claro
    :::column-end:::
    :::column:::
        #### <a name="dark-theme"></a>Tema escuro
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Base

:::row:::
    :::column:::
        ![O tema claro base](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![O tema escuro base](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>Alt

:::row:::
    :::column:::
        ![O tema claro alternativo](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![O tema escuro alternativo](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>List

:::row:::
    :::column:::
        ![O tema claro de lista](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![O tema escuro de lista](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>Cromado

:::row:::
    :::column:::
        ![O tema claro cromado](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![O tema escuro cromado](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Cores de alto contraste do sistema Windows

Além do conjunto de recursos fornecido pela estrutura XAML, há um conjunto de valores de cor derivado da paleta do sistema Windows. Essas cores não são específicas dos aplicativos do Windows ou Windows Runtime. Porém, muitos dos recursos XAML [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) consomem essas cores quando o sistema está em operação (e o aplicativo está em execução) usando o tema "HighContrast". A estrutura XAML fornece essas cores de todo o sistema como recursos inseridos. As chaves seguem o formato de nomenclatura: `SystemColor[name]Color`.

Esta tabela lista as cores de todo o sistema que XAML fornece como objetos de recursos derivados da paleta do sistema Windows. A coluna "Ease of Access name" mostra como a cor é identificada na interface do usuário das configurações do Windows. A coluna "Simple HighContrast name" é uma descrição de uma palavra de como a cor é aplicada entre os controles comuns XAML. Ela é usada como parte da convenção de nomenclatura de pincel que explicaremos mais adiante. A coluna "Initial default" mostra os valores que você obteria se o sistema não estivesse em execução em alto contraste.

| Chave                           | Ease of Access name            | Simple HighContrast name | Initial default |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **Texto do Botão** (em segundo plano)   | Tela de fundo               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **Texto do Botão** (em primeiro plano)   | Primeiro plano               | \#FF000000      |
| SystemColorGrayTextColor      | **Texto Desabilitado**              | Desabilitado                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **Texto Selecionado** (em segundo plano) | Realce                | \#FF3399FF      |
| SystemColorHighlightTextColor | **Texto Selecionado** (em primeiro plano) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **Hiperlinks**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **Tela de fundo**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **Texto**                       | PageText                 | \#FF000000      |

O Windows oferece diferentes temas de alto contraste e permite que o usuário defina as cores específicas para as configurações de alto contraste na Central de Facilidade de Acesso, conforme mostrado aqui. Por isso, não é possível fornecer uma lista definitiva de valores de cores de alto contraste.

![A interface do usuário das configurações de alto contraste do Windows](images/high-contrast-settings.png)

Para obter mais informações sobre como dar suporte a temas de alto contraste, consulte [Temas de alto contraste](https://docs.microsoft.com/windows/uwp/accessibility/high-contrast-themes).

### <a name="system-accent-color"></a>Cor de destaque do sistema

Além das cores do tema de alto contraste do sistema, a cor de destaque do sistema é fornecida como um recurso de cores especial usando-se a chave `SystemAccentColor`. Em runtime, esse recurso obtém a cor que o usuário especificou como a cor de destaque nas configurações de personalização do Windows.

> [!NOTE]
> Embora seja possível substituir os recursos de cor do sistema, é uma prática recomendada respeitar as opções de cor do usuário, especialmente para configurações de alto contraste.

### <a name="theme-dependent-brushes"></a>Pincéis dependentes de temas

Os recursos de cor mostrados nas seções anteriores são usados para definir a propriedade [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) dos recursos [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) nos dicionários de recursos de temas do sistema. Use os recursos de pincel para aplicar a cor a elementos XAML. As chaves dos recursos de pincel seguem o formato de nomenclatura: `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`. Por exemplo, `SystemControlBackgroundAltHighBrush`.

Consultemos como o valor de cores desse pincel é determinado no tempo de execução. Nos dicionários de recursos "Light" e "Dark", esse pincel é definido desta forma:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

No dicionário de recursos "HighContrast", esse pincel é definido desta forma:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

Quando esse pincel é aplicado a um elemento XAML, a cor é determinada em tempo de execução pelo tema atual, conforme mostrado nesta tabela.

| Tema        | Nome simples da cor | Recurso de cores             | Valor do runtime                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| Leve        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| Escuro         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| HighContrast | Tela de fundo        | SystemColorButtonFaceColor | A cor especificada nas configurações do segundo plano do botão. |

Você pode usar o esquema de nomenclatura `SystemControl[Simple HighContrast name][Simple light/dark name]Brush` para determinar qual pincel aplicar aos próprios elementos XAML.

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> Nem toda combinação de \[*nome simples HighContrast*\]\[*nome claro/escuro simples*\] é fornecida como um recurso de pincel.

## <a name="the-xaml-type-ramp"></a>A rampa de tipos XAML

O arquivo themeresources.xaml define vários recursos que determinam um [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) que você pode aplicar a contêineres de texto na interface do usuário, especificamente para [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) ou [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Eles não são os estilos implícitos padrão. Eles são fornecidos para facilitar a criação de definições da interface do usuário XAML que correspondam à *rampa de tipos do Windows* documentada em [Diretrizes para fontes](../style/typography.md).

Esses estilos se destinam a atributos de texto que você deseja aplicar ao contêiner de texto inteiro. Se desejar que o estilos sejam aplicados somente a seções do texto, defina atributos nos elementos de texto dentro do contêiner, como em um [Run](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Run) em [TextBlock.Inlines](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.inlines) ou em um [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph) em [RichTextBlock.Blocks](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richtextblock.blocks).

Os estilos têm esta aparência quando aplicados a um [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock):

![estilos de bloco de texto](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

Para obter diretrizes sobre como usar a rampa de tipos do Windows em seu aplicativo, confira [Tipografia em aplicativos do Windows](../style/typography.md).

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**: [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

Fornece as propriedades comuns para todos os outros estilos de contêiner [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="14"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**: [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)

Fornece as propriedades comuns para todos os outros estilos de contêiner [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock).

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**Observação**:  Os estilos [RichTextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) não têm todos os estilos de rampa de texto de [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), principalmente porque o modelo de objeto de documento baseado em bloco de **RichTextBlock** facilita a definição de atributos nos elementos de texto individuais. Além disso, definir [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) usando a propriedade de conteúdo XAML provoca uma situação em que não há elementos de texto para os quais definir um estilo e, portanto, você teria que definir um estilo para o contêiner. Isso não é um problema para **RichTextBlock**, pois o conteúdo de texto sempre precisa estar em elementos de texto específicos, como [Paragraph](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Paragraph), onde você pode aplicar estilos XAML do cabeçalho de página, do subcabeçalho de página e das definições de rampa de texto semelhantes.

## <a name="miscellaneous-named-styles"></a>Estilos nomeados diversos

Há um conjunto adicional de definições [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) inseridas que você pode aplicar para criar um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) diferente do estilo implícito padrão.

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**: [ButtonBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)

Aplique esse estilo a um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) quando você precisar mostrar o texto em que um usuário pode clicar para realizar uma ação. O texto é criado usando-se a cor de destaque atual para diferenciá-lo como interativo e tem retângulos de foco que funcionam bem para textos. Diferentemente do estilo implícito de um [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton), o **TextBlockButtonStyle** não sublinha o texto.

O modelo também cria o estilo do texto apresentado para usar **SystemControlHyperlinkBaseMediumBrush** (para o estado "PointerOver"), **SystemControlHighlightBaseMediumLowBrush** (para o estado "Pressionado") e **SystemControlDisabledBaseLowBrush** (para o estado "Desabilitado").

Veja a seguir um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) com o recurso **TextBlockButtonStyle** aplicado.

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

Ele é semelhante ao seguinte:

![Um botão criado para ter a aparência de texto](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**: [Botão](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)

Esse [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) oferece um modelo completo para um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) que pode ser o botão voltar de navegação de um aplicativo de navegação. As dimensões padrão são 40 x 40 pixels. Para personalizar o estilo, você pode definir explicitamente [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [FontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontsize) e outras propriedades no **Button** ou criar um estilo derivado usando [BasedOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.style.basedon).

Veja a seguir um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) com o recurso **NavigationBackButtonNormalStyle** aplicado.

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

Ele é semelhante ao seguinte:

![Um botão com o estilo de um botão voltar](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**: [Botão](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)

Esse [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) oferece um modelo completo para um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) que pode ser o botão voltar de navegação de um aplicativo de navegação. Ele é semelhante a **NavigationBackButtonNormalStyle**, mas as dimensões são 30 x 30 pixels.

Veja a seguir um [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) com o recurso **NavigationBackButtonSmallStyle** aplicado.

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>Solução de problemas de recursos de temas

Caso não siga as [diretrizes para usar recursos de tema](#guidelines-for-custom-theme-resources), você pode notar um comportamento inesperado relacionado aos temas no aplicativo.

Por exemplo, quando você abre um submenu com tema claro, partes do aplicativo com tema escuro também mudam como se estivessem no tema claro. Ou, caso você navegue até uma página com tema claro e, em seguida, volte à navegação original, a página com tema escuro original (ou partes dele) agora parece ser do tema claro.

Normalmente, esses tipos de problemas ocorrem quando você fornece um tema "Default" e um tema "HighContrast" para dar suporte a cenários de alto contraste e, em seguida, usa ambos os temas "Light" e "Dark" em partes diferentes do aplicativo.

Por exemplo, considere esta definição do dicionário de temas:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Intuitivamente, ela parece correta. Você deseja alterar a cor apontada por `myBrush` quando está em alto contraste, mas, quando não está em alto contraste, você depende da [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) para verificar se `myBrush` aponta para a cor certa do tema. Se o aplicativo jamais tiver [FrameworkElement.RequestedTheme](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.requestedtheme) definido em elementos dentro da árvore visual, isso normalmente funcionará conforme esperado. No entanto, você tem problemas no aplicativo assim que começa a recriar partes diferentes do tema da árvore visual.

O problema ocorre porque pincéis são recursos compartilhados, diferentemente da maioria dos outros tipos XAML. Caso você tenha dois elementos em subárvores XAML com temas diferentes que referenciam o mesmo recurso pincel, à medida que a estrutura percorre cada subárvore para atualizar as expressões da [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), as alterações feitas no recurso pincel compartilhado se refletem na outra subárvore, que não é o resultado desejado.

Para corrigir isso, substitua o dicionário "Default" por dicionários de temas separados para os dois temas "Light" e "Dark", além de "HighContrast":

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

No entanto, ainda haverá problemas se qualquer um desses recursos forem referenciados em propriedades herdadas, como [Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground). O modelo de controle personalizado pode especificar a cor de primeiro plano de um elemento usando a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), mas, quando a estrutura propaga o valor herdado para elementos filho, ela fornece uma referência direta para o recurso que foi resolvido pela expressão da extensão de marcação {ThemeResource}. Isso causa problemas quando a estrutura processa alterações de tema à medida que percorre a árvore visual do controle. Ele reavalia a expressão da extensão de marcação {ThemeResource} para obter um novo recurso pincel, mas ainda não propaga essa referência para os filhos do controle; isso acontecerá mais tarde como, por exemplo, durante o próximo cálculo de medição.

Dessa forma, depois de percorrer a árvore visual do controle em resposta a uma alteração feita no tema, a estrutura orienta os filhos e atualiza todas as expressões da [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) definidas neles ou em objetos definidos nas propriedades. É onde o problema ocorre; a estrutura percorre o recurso pincel e, como ela especifica a cor usando uma extensão de marcação {ThemeResource}, ela é reavaliada.

A esta altura, a estrutura aparentemente poluiu o dicionário de temas porque agora tem um recurso de um dicionário com a cor definida de outro dicionário.

Para corrigir esse problema, use a [extensão de marcação {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) no lugar da [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Com as diretrizes aplicadas, os dicionários de temas têm esta aparência:

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Observe que a [extensão de marcação {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) ainda é usada no dicionário "HighContrast" no lugar da [extensão de marcação {StaticResource}](../../xaml-platform/staticresource-markup-extension.md). Essa situação se enquadra na exceção apresentada anteriormente nas diretrizes. A maioria dos valores de pincéis usados para o tema "HighContrast" está usando opções de cores controladas globalmente pelo sistema, mas expostas a XAML como um recurso especialmente nomeado (os prefixados com "SystemColor" no nome). O sistema permite que o usuário defina as cores específicas que devem ser usadas nas configurações de alto contraste por meio da Central de Facilidade de Acesso. Essas opções de cores são aplicadas aos recursos especialmente nomeados. A estrutura XAML usa o mesmo evento de tema alterado para também atualizar esses pincéis ao detectar que eles foram alterados no nível do sistema. É por isso que a extensão de marcação {ThemeResource} é usada aqui.
