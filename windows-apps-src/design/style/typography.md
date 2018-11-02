---
author: mijacobs
description: Saiba como usar a tipografia em seu aplicativo para ajudar os usuários a entenderem o conteúdo facilmente.
title: Tipografia nos aplicativos UWP
ms.author: mijacobs
ms.date: 04/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62f5aa7d1089b38131c742c588ae741e88197533
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5944011"
---
# <a name="typography"></a>Tipografia

![imagem hero](images/header-typography.svg)

Como a representação visual da linguagem, a tarefa principal da tipografia é informar. Seu estilo nunca deve atrapalhar essa meta. Neste artigo, vamos discutir como definir o estilo de tipografia em seu aplicativo UWP para ajudar os usuários a entender o conteúdo de modo fácil e eficiente.

## <a name="font"></a>Fonte

Você deve usar uma fonte em toda a interface do usuário do aplicativo e recomendamos manter a fonte padrão para aplicativos UWP, **Segoe UI**. Ela foi projetada para manter a legibilidade ideal em todos os tamanhos e densidades de pixel, e oferece uma estética limpa, leve e aberta que complementa o conteúdo do sistema.

![Texto de exemplo da fonte Segoe UI](images/type/segoe-sample.svg)

Para exibir idiomas diferentes do inglês ou selecionar uma fonte diferente para o aplicativo, consulte [Idiomas](#Languages) e [Fontes](#Fonts) e veja as fontes recomendadas para aplicativos UWP.

:::row:::
    :::column:::
        ![do](images/do.svg)
        Pick one font for your UI.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Don't mix multiple fonts.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Tamanho e dimensionamento

Os tamanhos de fonte em aplicativos UWP são ajustados automaticamente em todos os dispositivos. O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a 3 m de distância seja tão legível quanto uma fonte de 24 px no telefone de 5 polegadas a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/type/scaling-chart.svg)

Devido ao funcionamento do sistema de dimensionamento, você está projetando em pixels efetivos e não pixels físicos reais, e você não precisa alterar os tamanhos da fonte de acordo com diferentes telas ou resoluções.

:::row:::
    :::column:::
        ![do](images/do.svg)
        Follow the UWP [type ramp](#type-ramp) sizing.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use a font size smaller than 12 px.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Hierarquia

:::row:::
    :::column:::
        Users rely on visual hierarchy when scanning a page: headers summarize content, and body text provides more detail. To create a clear visual hierarchy in your app, follow the UWP type ramp.
    :::column-end:::
    :::column:::
        ![text block styles](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Rampa de tipos

A rampa de tipos da UWP estabelece relações específicas entre os estilos de tipo em uma página, ajudando os usuários a ler o conteúdo facilmente. Todos os tamanhos estão em pixels efetivos e são otimizados para aplicativos UWP executados em todos os dispositivos.

![Rampa de tipos](images/type/type-ramp.svg)

### <a name="using-the-type-ramp"></a>Uso da rampa de tipos

:::row:::
    :::column:::
        You can access levels of the type ramp as XAML [static resources](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp). The styles follow the `*TextBlockStyle` naming convention.
    :::column-end:::
    :::column:::
        ![text block styles](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

:::row:::
    :::column:::
        ![do](images/do.svg)
        Use "Body" for most text.

        Use "Base" for titles when space is constrained.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use "Caption" for primary action or any long strings.

        Use "Header" or "Subheader" if text needs to wrap.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Alinhamento

O [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) padrão é à esquerda e, na maioria dos casos, essa abordagem com recuo à esquerda e irregular à direita fornece ancoragem consistente de conteúdo e um layout uniforme. Para os idiomas da direita para a esquerda, consulte [Ajuste de layout e fontes para oferecer suporte à globalização](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Mostra texto à esquerda.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Contagem de caracteres

:::row:::
    :::column:::
        ![do](images/do.svg)
        Keep to 50–60 letters per line for ease of reading.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Less than 20 characters or more than 60 characters per line is difficult to read.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Recorte e elipses

Quando a quantidade de texto se estende além do espaço disponível, recomendamos recortar o texto, que é o comportamento padrão da maioria dos [controles de texto UWP](../controls-and-patterns/text-controls.md).

![Mostra um quadro de dispositivo com alguns recortes de texto](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
        ![do](images/do.svg)
        Clip text, and wrap if multiple lines are enabled.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        Use ellipses to avoid visual clutter.
    :::column-end:::
:::row-end:::

**Observação**: se os contêineres não estiverem bem definidos (por exemplo, nenhuma cor da tela de fundo diferente), ou quando há um link para ver mais texto, use as elipses.

## <a name="languages"></a>Idiomas 

A Segoe UI é nossa fonte para inglês, idiomas europeus, grego, hebraico, armênio, georgiano e árabe. Para outros idiomas, consulte as recomendações a seguir.

### <a name="globalizinglocalizing-fonts"></a>Fontes de globalização/localização

Use as [APIs de mapeamento de fonte LanguageFont](https://docs.microsoft.com/uwp/api/Windows.Globalization.Fonts.LanguageFont) para acesso via programação à família de fontes, tamanho, peso e estilo recomendados para um idioma específico. O objeto LanguageFont oferece acesso às informações corretas de fonte para várias categorias de conteúdo, incluindo cabeçalhos de interface do usuário, notificações, texto do corpo e fontes de corpo de documento editáveis pelo usuário. Para obter mais informações, consulte [Ajustando layout e fontes para dar suporte à globalização](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

### <a name="fonts-for-non-latin-languages"></a>Fontes para idiomas não latinos

<table>
<thead>
<tr class="header">
<th align="left">Família de fontes</th>
<th align="left">Estilos</th>
<th align="left">Observações</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Regular, negrito</td>
<td align="left">Fonte de interface do usuário para scripts da África (etíope, n’Ko, osmanli, tifinagh, Vai).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Regular, negrito</td>
<td align="left">Fonte de interface do usuário para scripts na América do Norte (silábico canadense, Cherokee).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Regular, semileve, negrito</td>
<td align="left">Fonte de interface do usuário para scripts do Sudeste Asiático (bugi, laosiano, khmer, tailandês).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para coreano.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Regular, negrito, claro</td>
<td align="left">Fonte de interface do usuário para chinês tradicional.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Regular, negrito, claro</td>
<td align="left">Fonte de interface do usuário para chinês simplificado.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script de Myanmar.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Regular, semileve, negrito</td>
<td align="left">Fonte de interface do usuário para scripts do Sul Asiático (bengali, devanágari, gujarati, gurmukhi, kannada, malaiala, odia, ol chiki, cingalês, sora sompeng, tâmil, télugo)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">Uma fonte de interface do usuário chinesa herdada. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Interface de usuário Yu Gothic</td>
<td align="left">Leve, Semi-leve, Regular, Semi-negrito, Negrito</td>
<td align="left">Fonte de interface de usuário para japonês.</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Fontes

### <a name="sans-serif-fonts"></a>Fontes sans-serif

As fontes sans-serif são uma excelente escolha para títulos e elementos de interface do usuário. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Família de fontes</th>
<th align="left">Estilos</th>
<th align="left">Observações</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Regular, itálico, negrito, negrito itálico, preto</td>
<td align="left">Dá suporte a scripts da Europa e do Oriente Médio (latino, grego, cirílico, árabe, armênio e hebraico). O padrão preto oferece suporte somente a scripts europeus.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Regular, itálico, negrito, negrito itálico, leve, itálico leve</td>
<td align="left">Dá suporte a scripts da Europa e do Oriente Médio (latino, grego, cirílico, árabe e hebraico). Árabe disponível apenas em linha reta.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Regular, itálico, negrito, negrito itálico</td>
<td>Fonte de largura fixa que oferece suporte a scripts europeus (latino, grego e cirílico).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Regular, itálico, itálico claro, itálico preto, negrito, negrito itálico, claro, semileve, seminegrito, preto</td>
<td>Fonte de interface do usuário para scripts da Europa e do Oriente Médio (árabe, armênio, cirílico, georgiano, grego, hebraico, latino) e também script Lisu.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Regular, semileve, claro, negrito, seminegrito</td>
<td align="left">Uma fonte de código-fonte que é metricamente compatível com Segoe UI, destinada a aplicativos em outras plataformas que não desejam usar Segoe UI. <a href="https://github.com/Microsoft/Selawik">Obtenha Selawik no GitHub.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Fontes serif

As fontes serif são adequadas para apresentar grandes quantidades de texto. 

<table>
<thead>
<tr class="header">
<th align="left">Família de fontes</th>
<th align="left">Estilos</th>
<th align="left">Observações</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Regular</td>
<td align="left">Fonte serif que oferece suporte a scripts europeus (latino, grego, cirílico).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left">A fonte de largura fixa serif dá suporte a scripts da Europa e do Oriente Médio (latino, grego, cirílico, árabe, armênio e hebraico).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left">Dá suporte a scripts europeus (latino, grego e cirílico).</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left">Fonte herdada que oferece suporte a scripts europeus (latino, grego, cirílico, árabe, armênio, hebraico).</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>Ícones e símbolos

<table>
<thead>
<tr class="header">
<th align="left">Família de fontes</th>
<th align="left">Estilos</th>
<th align="left">Observações</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Ativos de Segoe MDL2</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para ícones de aplicativos. Para obter mais informações, consulte o <a href="segoe-ui-symbol-font.md">artigo de ativos de Segoe MDL2</a>.</td>
</tr>
<tr class="even">
<td align="left">Emoji Segoe da interface do usuário</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para símbolos</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>Artigos relacionados

* [Controles de texto](../controls-and-patterns/text-controls.md)
* [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [Estilos de XAML](../controls-and-patterns/xaml-styles.md)
* [Tipografia da Microsoft](https://docs.microsoft.com/typography/)