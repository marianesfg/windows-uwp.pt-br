---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: Dimensionamento de texto
label: Text scaling
template: detail.hbs
keywords: Exibir UWP, texto, dimensionamento, acessibilidade, "ease of access,", "Tornar o texto maior", interação do usuário, entrada
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ce3ec15a45f812162c7aab0cb9683183d7196ae3
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5706973"
---
# <a name="text-scaling"></a>Dimensionamento de texto

![Exemplo de texto colocação em escala 100 a 225%](images/coretext/text-scaling-news-hero-small.png)  
*Exemplo de texto de dimensionamento no Windows 10 (100 a 225%)*

## <a name="overview"></a>Visão geral

Lendo texto em uma tela de computador (a partir do dispositivo móvel para laptop ao monitor da área de trabalho na tela giant de um Surface Hub) pode ser um desafio para muitas pessoas. Por outro lado, alguns usuários encontram os tamanhos de fonte usados em aplicativos e sites da web para ser maior que o necessário.

Para garantir que o texto é tão legível quanto possível para a mais ampla variedade de usuários, o Windows fornece a capacidade para os usuários alterem o tamanho da fonte relativa entre o sistema operacional e de aplicativos individuais. Em vez de usar um aplicativo de Lupa (que normalmente amplia tudo dentro de uma área da tela e apresenta seus próprios problemas de usabilidade), alterando a resolução de vídeo ou depender de escala DPI (que redimensiona tudo com base na exibição e exibição típica distância), um usuário pode acessar rapidamente uma configuração para redimensionar somente texto, que variam de 100% (o tamanho padrão) até 225%.

## <a name="support"></a>Suporte

Aplicativos universais do Windows (recurso padrão e PWA), suporte a texto de dimensionamento por padrão.

Se seu aplicativo UWP inclui controles personalizados, superfícies de texto personalizado, alturas de controle embutido, estruturas mais antigas ou 3ª estruturas de terceiros, você provavelmente precisará fazer algumas atualizações para garantir uma experiência consistente e úteis para seus usuários.  

DirectWrite, GDI e SwapChainPanels XAML não têm suporte dimensionamento de texto, enquanto o suporte do Win32 é limitado a menus, ícones e barras de ferramentas.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Experiência do usuário

Os usuários podem ajustar a escala do texto com o tornar o texto maior controle deslizante nas configurações -> facilidade de acesso -> tela de exibição/visão.

![Exemplo de texto colocação em escala 100 a 225%](images/coretext/text-scaling-settings-100-small.png)  
*Definição das configurações de escala de texto -> facilidade de acesso -> tela de exibição/visão*

## <a name="ux-guidance"></a>Diretrizes de experiência do usuário

Como o texto é redimensionado, controles e contêineres devem também redimensionar e refluir para acomodar o texto e seu novo layout. Como mencionado anteriormente, dependendo do aplicativo, a estrutura e a plataforma, grande parte desse trabalho é feito para você. As diretrizes de experiência do usuário a seguir abordam esses casos em que não é.

### <a name="use-the-platform-controls"></a>Use os controles de plataforma

Podemos disse isso já? Vale a pena repetir: quando possível, sempre use os controles internos fornecidos com as várias estruturas de aplicativo do Windows para obter a experiência do usuário mais abrangente possível para o mínimo de esforço.

Por exemplo, todos os controles de texto UWP suportam o dimensionamento experiência sem nenhuma personalização ou modelagem de texto completo.

Aqui está um trecho de código de um aplicativo UWP básico que inclui alguns dos controles de texto padrão:

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

![Texto animado colocação em escala 100 a 225%](images/coretext/text-scaling.gif)  
*Dimensionamento de texto animado*

### <a name="use-auto-sizing"></a>Usar o dimensionamento automático

Não especifique absolutos tamanhos para seus controles. Sempre que possível, deixe a plataforma redimensionar seus controles automaticamente com base nas configurações de usuários e dispositivos.  

Este trecho de código do exemplo anterior, usamos o `Auto` e `*` valores de largura para um conjunto de colunas de grade e permitir que a plataforma ajustam o layout do aplicativo com base no tamanho dos elementos contidos dentro da grade.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Use a disposição do texto

Para garantir que o layout do seu aplicativo é mais flexível e adaptável possível, habilite a disposição do texto em qualquer controle que contém texto (muitos controles não dão suporte a disposição do texto por padrão).

Se você não especificar a disposição do texto, a plataforma usa outros métodos para ajustar o layout, incluindo o recorte (veja o exemplo anterior).

Aqui, usamos o `AcceptsReturn` e `TextWrapping` propriedades TextBox para garantir que nosso layout é mais flexível possível.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Texto de escala 100 a 225% com disposição do texto animado](images/coretext/text-scaling-textwrap.gif)  
*Texto animado dimensionamento com disposição do texto*

### <a name="specify-text-trimming-behavior"></a>Especificar o comportamento de corte de texto

Se a disposição do texto não é o comportamento preferencial, a maioria dos controles de texto permitem que o seu texto de recorte ou especificar elipses para o comportamento de corte de texto. Recortar é preferencial para elipses como elipses ocupam espaço propriamente ditos.

> [!NOTE]
> Se você precisar recortar o texto, clip final da cadeia de caracteres, não o início.

Neste exemplo, mostramos como Recortar o texto em um TextBlock usando a propriedade [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) .

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Texto colocação em escala 100 a 225% com recortes de texto](images/coretext/text-scaling-clipping-small.png)  
*Texto dimensionamento com recortes de texto*

### <a name="use-a-tooltip"></a>Use uma dica de ferramenta

Se você recortar o texto, use uma dica de ferramenta para fornecer o texto completo para seus usuários.

Aqui, adicionamos uma dica de ferramenta a um TextBlock que não dá suporte a quebra automática de texto:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Corrigidos sem escala ícones baseados em fontes ou símbolos

Ao usar ícones baseados em fontes para ênfase ou decoração, desabilite o dimensionamento nesses caracteres.

Defina a propriedade [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) como `false` para XAML a maioria dos controles.

### <a name="support-text-scaling-natively"></a>Texto de suporte nativo dimensionamento

Manipule o evento de sistema de configurações de UI [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) em sua estrutura personalizada e controles. Este evento é gerado cada vez que o usuário define o fator de escala de texto no sistema.

## <a name="summary"></a>Resumo

Este tópico fornece uma visão geral de texto dimensionamento suporte no Windows e inclui diretrizes de experiência do usuário e desenvolvedor sobre como personalizar a experiência do usuário.

## <a name="related-articles"></a>Artigos relacionados

### <a name="api-reference"></a>Referência da API

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
