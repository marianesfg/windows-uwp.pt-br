---
Description: Crie aplicativos do Windows e controles personalizados/de modelo que dão suporte ao dimensionamento de texto da plataforma.
title: Colocar texto em escala
label: Text scaling
template: detail.hbs
keywords: UWP, texto, dimensionamento, acessibilidade, "facilidade de acesso", exibição, "tornar texto maior", interação do usuário, entrada
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4db3af0d2ec0ce1dbd0866f569ad9bf9b0392aa8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970561"
---
# <a name="text-scaling"></a>Colocar texto em escala

![Exemplo de dimensionamento de texto de 100% para 225%](images/coretext/text-scaling-news-hero-small.png)  
*Exemplo de dimensionamento de texto no Windows 10 (100% a 225%)*

## <a name="overview"></a>Visão geral

A leitura de texto em uma tela de computador (do dispositivo móvel ao laptop para o monitor de área de trabalho na tela gigante de um Surface Hub) pode ser desafiadora para muitas pessoas. Por outro lado, alguns usuários encontram os tamanhos de fonte usados em aplicativos e sites para serem maiores do que o necessário.

Para garantir que o texto seja o mais legível possível para a maior variedade de usuários, o Windows fornece a capacidade de os usuários alterarem o tamanho da fonte relativa no sistema operacional e nos aplicativos individuais. Em vez de usar um aplicativo de lupa (que normalmente amplia tudo dentro de uma área da tela e apresenta seus próprios problemas de usabilidade), alterar a resolução de vídeo ou depender do ajuste de DPI (que redimensiona tudo com base na exibição e na distância de exibição comum), um usuário pode acessar rapidamente uma configuração para redimensionar somente o texto, de 100% (o tamanho padrão) até 225%.

## <a name="support"></a>Suporte

Os aplicativos universais do Windows (Standard e PWA) oferecem suporte ao dimensionamento de texto por padrão.

Se seu aplicativo do Windows inclui controles personalizados, superfícies de texto personalizadas, alturas de controle embutidas em código, estruturas mais antigas ou estruturas de terceiros, é provável que você precise fazer algumas atualizações para garantir uma experiência consistente e útil para os usuários.  

DirectWrite, GDI e XAML SwapChainPanels não dão suporte nativo ao dimensionamento de texto, enquanto o suporte a Win32 é limitado a menus, ícones e barras de ferramentas.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiência do usuário

Os usuários podem ajustar a escala de texto com o controle deslizante tornar o texto maior na tela Configurações – > facilidade de acesso – > visão/exibição.

![Exemplo de dimensionamento de texto de 100% para 225%](images/coretext/text-scaling-settings-100-small.png)  
*Configuração de escala de texto de configurações-> facilidade de acesso-> visão/tela de exibição*

## <a name="ux-guidance"></a>Diretrizes de experiência do usuário

À medida que o texto é redimensionado, os controles e os contêineres também devem ser redimensionados e refluídos para acomodar o texto e seu novo layout. Conforme mencionado anteriormente, dependendo do aplicativo, da estrutura e da plataforma, grande parte desse trabalho é feito para você. As diretrizes de UX a seguir abordam os casos em que não é.

### <a name="use-the-platform-controls"></a>Usar os controles de plataforma

Já dissemos isso? Vale a pena repetir: quando possível, sempre use os controles internos fornecidos com as várias estruturas de aplicativo do Windows para obter a experiência de usuário mais abrangente possível para a menor quantidade de esforço.

Por exemplo, todos os controles de texto UWP dão suporte à experiência de dimensionamento de texto completo sem qualquer personalização ou modelagem.

Aqui está um trecho de um aplicativo UWP básico que inclui alguns controles de texto padrão:

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Dimensionamento de texto animado 100% para 225%](images/coretext/text-scaling.gif)  
*Dimensionamento de texto animado*

### <a name="use-auto-sizing"></a>Usar o dimensionamento automático

Não especifique tamanhos absolutos para seus controles. Sempre que possível, permita que a plataforma redimensione os controles automaticamente com base nas configurações do usuário e do dispositivo.  

Neste trecho de código do exemplo anterior, usamos os `Auto` valores de `*` largura e para um conjunto de colunas de grade e permitimos que a plataforma ajuste o layout do aplicativo com base no tamanho dos elementos contidos na grade.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Usar quebra automática de texto

Para garantir que o layout do seu aplicativo seja tão flexível e adaptável quanto possível, habilite a quebra de texto em qualquer controle que contenha texto (muitos controles não dão suporte ao encapsulamento de texto por padrão).

Se você não especificar a quebra automática de texto, a plataforma usará outros métodos para ajustar o layout, incluindo recorte (Veja o exemplo anterior).

Aqui, usamos as propriedades `AcceptsReturn` de `TextWrapping` TextBox e para garantir que o layout seja o mais flexível possível.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Dimensionamento de texto animado 100% para 225% com disposição de texto](images/coretext/text-scaling-textwrap.gif)  
*Dimensionamento de texto animado com disposição de texto*

### <a name="specify-text-trimming-behavior"></a>Especificar o comportamento de corte de texto

Se a quebra automática de texto não for o comportamento preferencial, a maioria dos controles de texto permitirá que você recorte o texto ou especifique reticências para o comportamento de corte de texto. O recorte é preferencial para reticências, pois as reticências ocupam o próprio espaço.

> [!NOTE]
> Se você precisar recortar o texto, recorte o final da cadeia de caracteres, não o início.

Neste exemplo, mostramos como recortar o texto em um TextBlock usando a propriedade [Textaparation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Dimensionamento de texto de 100% para 225% com recorte de texto](images/coretext/text-scaling-clipping-small.png)  
*Dimensionamento de texto com recorte de texto*

### <a name="use-a-tooltip"></a>Usar uma dica de ferramenta

Se você cortar o texto, use uma dica de ferramenta para fornecer o texto completo aos seus usuários.

Aqui, adicionamos uma dica de ferramenta a um TextBlock que não dá suporte ao encapsulamento de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Não dimensionar ícones ou símbolos baseados em fonte

Ao usar ícones baseados em fonte para ênfase ou decoração, desabilite o dimensionamento nesses caracteres.

Defina a propriedade [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) como `false` para a maioria dos controles XAML.

### <a name="support-text-scaling-natively"></a>Suporte ao dimensionamento de texto nativamente

Manipule o evento do sistema [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings na estrutura e nos controles personalizados. Esse evento é gerado cada vez que o usuário define o fator de dimensionamento de texto em seu sistema.

## <a name="summary"></a>Resumo

Este tópico fornece uma visão geral do suporte ao dimensionamento de texto no Windows e inclui orientações de UX e desenvolvedor sobre como personalizar a experiência do usuário.

## <a name="related-articles"></a>Artigos relacionados

### <a name="api-reference"></a>Referência de API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
