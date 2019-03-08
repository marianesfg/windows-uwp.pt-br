---
Description: Crie aplicativos UWP e controles personalizados/modelos que dão suporte ao dimensionamento de texto com a plataforma.
title: Dimensionamento de texto
label: Text scaling
template: detail.hbs
keywords: Exibir UWP, texto, dimensionamento, acessibilidade, "facilidade de acesso", "Tornar o texto maior", a interação do usuário, a entrada
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600811"
---
# <a name="text-scaling"></a>Dimensionamento de texto

![Exemplo de texto de dimensionamento de 100 a 225%](images/coretext/text-scaling-news-hero-small.png)  
*Exemplo de dimensionamento no Windows 10 (100% para % 225) de texto*

## <a name="overview"></a>Visão geral

Lendo o texto na tela do computador (a partir do dispositivo móvel para o laptop ao monitor da área de trabalho para a tela gigante de um Surface Hub) pode ser um desafio para muitas pessoas. Por outro lado, alguns usuários consideram os tamanhos de fonte usados em aplicativos e sites da web para ser maior que o necessário.

Para garantir que o texto é tão legível quanto possível para o intervalo mais amplo de usuários, o Windows fornece a capacidade dos usuários alterar o tamanho da fonte relativa entre o sistema operacional e de aplicativos individuais. Em vez de usando um aplicativo de Lupa (que normalmente amplia tudo dentro de uma área da tela e apresenta seus próprios problemas de usabilidade), alterar a resolução de vídeo ou no ajuste de DPI (que redimensiona tudo com base na exibição e exibição típica de terceira parte confiável distância), um usuário pode acessar rapidamente uma configuração para redimensionar somente texto, que variam de 100% (o tamanho padrão) até 225%.

## <a name="support"></a>Suporte

Aplicativos universais do Windows (padrão e PWA), dão suporte a texto dimensionamento por padrão.

Se seu aplicativo UWP inclui controles personalizados, superfícies de texto personalizado, alturas de controle codificadas, estruturas mais antigas ou estruturas de terceiros 3ª, provavelmente terá que fazer algumas atualizações para garantir uma experiência consistente e útil para seus usuários.  

O DirectWrite, GDI e SwapChainPanels XAML não suportados nativamente escala de texto, enquanto Win32 suporte é limitado a menus, ícones e barras de ferramentas.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiência de usuário

Os usuários podem ajustar a escala do texto com o texto de marca -> maior controle deslizante nas configurações de facilidade de acesso -> tela de visão/exibição.

![Exemplo de texto de dimensionamento de 100 a 225%](images/coretext/text-scaling-settings-100-small.png)  
*Configuração das configurações de dimensionamento de texto -> a facilidade de acesso -> tela de visão/exibição*

## <a name="ux-guidance"></a>Diretrizes de experiência do usuário

Como o texto é redimensionado, controles e os contêineres também devem redimensionar e refluir para acomodar o texto e seu novo layout. Conforme mencionado anteriormente, dependendo do aplicativo, a estrutura e a plataforma, boa parte desse trabalho é feita para você. As diretrizes de experiência do usuário a seguir aborda os casos em que não é.

### <a name="use-the-platform-controls"></a>Use os controles de plataforma

Dissemos isso já? Vale a pena repetir: Quando possível, sempre use os controles internos fornecidos com as diversas estruturas de aplicativo do Windows para obter a experiência do usuário mais completa possível para o mínimo de esforço.

Por exemplo, todos os controles de texto UWP dão suporte o dimensionamento experiência sem nenhuma personalização ou modelagem de texto completo.

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

![Texto animado de 100 a 225% de dimensionamento](images/coretext/text-scaling.gif)  
*Dimensionamento do texto animado*

### <a name="use-auto-sizing"></a>Usar o dimensionamento automático

Não especifique tamanhos absolutos para seus controles. Sempre que possível, deixe a plataforma para redimensionar os controles automaticamente com base nas configurações de usuário e dispositivo.  

Neste trecho de código do exemplo anterior, podemos usar o `Auto` e `*` valores de largura de um conjunto de colunas da grade e permitem a plataforma de ajustar o layout do aplicativo com base no tamanho dos elementos contidos dentro da grade.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Use a disposição do texto

Para garantir que o layout do seu aplicativo é tão flexível e adaptável quanto possível, habilite a disposição do texto em qualquer controle que contém o texto (muitos controles não dão suporte a quebra automática de texto por padrão).

Se você não especificar a disposição do texto, a plataforma usa outros métodos para ajustar o layout, incluindo o recorte (consulte o exemplo anterior).

Aqui, podemos usar o `AcceptsReturn` e `TextWrapping` propriedades de caixa de texto para garantir que o layout é tão flexível quanto possível.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Texto dimensionamento 100% a % de 225 com quebra de texto animado](images/coretext/text-scaling-textwrap.gif)  
*Texto animado, dimensionando com quebra automática de texto*

### <a name="specify-text-trimming-behavior"></a>Especificar o comportamento de filtragem de texto

Se a quebra automática de texto não é o comportamento preferencial, a maioria dos controles de texto permitem que você recortar o texto ou especificar reticências para o comportamento de filtragem de texto. Recorte é preferencial para elipses pois elipses ocupam espaço em si.

> [!NOTE]
> Se você precisar recortar o texto, recorte o final da cadeia de caracteres, não o início.

Neste exemplo, vamos mostrar como Recortar o texto em um TextBlock usando o [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) propriedade.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Dimensionamento 100% a % de 225 com distorção de textos de texto](images/coretext/text-scaling-clipping-small.png)  
*Dimensionamento com distorção de textos de texto*

### <a name="use-a-tooltip"></a>Use uma dica de ferramenta

Se você recortar o texto, use uma dica de ferramenta para fornecer o texto completo para seus usuários.

Aqui, vamos adicionar uma dica de ferramenta a um TextBlock que não oferece suporte a quebra de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Não escalam ícones baseados em fonte ou símbolos

Ao usar ícones baseados em fonte para a ênfase ou decoração, desabilite o dimensionamento sob esses caracteres.

Defina as [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) propriedade `false` para XAML a maioria dos controles.

### <a name="support-text-scaling-natively"></a>Texto de suporte ao dimensionamento nativamente

Lidar com o [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) configurações de UI evento de sistema em sua estrutura personalizada e controles. Esse evento é gerado sempre que o usuário define o fator de escala do texto em seu sistema.

## <a name="summary"></a>Resumo

Este tópico fornece uma visão geral de suporte no Windows de dimensionamento de texto e inclui diretrizes de experiência do usuário e o desenvolvedor sobre como personalizar a experiência do usuário.

## <a name="related-articles"></a>Artigos relacionados

### <a name="api-reference"></a>Referência de API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
