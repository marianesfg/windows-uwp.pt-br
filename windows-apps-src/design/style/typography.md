---
description: Aprenda a usar a tipografia em seu aplicativo para ajudar os usuários a entenderem o conteúdo facilmente.
title: Tipografia em aplicativos UWP
ms.date: 04/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb2aef514c8787b5afe11ea5a2818012bfdf2f41
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72282408"
---
# <a name="typography"></a>Tipografia

![imagem Hero](images/header-typography.svg)

Como a representação visual da linguagem, a principal tarefa da tipografia é informar. Seu estilo nunca deve atrapalhar essa meta. Neste artigo, discutiremos como definir o estilo de tipografia em seu aplicativo UWP para ajudar os usuários a entenderem o conteúdo de modo fácil e eficiente.

## <a name="font"></a>Fonte

Você precisa usar uma fonte em toda a interface do usuário do aplicativo, e recomendamos manter a fonte padrão para aplicativos UWP, a **Segoe UI**. Ela foi projetado para manter a legibilidade ideal em todos os tamanhos e densidades de pixel, e oferece uma estética limpa, leve e aberta que complementa o conteúdo do sistema.

![Texto de exemplo da fonte Segoe UI](images/type/segoe-sample.svg)

Para exibir idiomas diferentes do inglês ou selecionar uma fonte diferente para o seu aplicativo, confira [Idiomas](#languages) e [Fontes](#fonts) e veja as fontes recomendadas para aplicativos UWP.

:::row:::
    :::column:::
![fazer](images/do.svg) Escolher uma fonte para a sua interface do usuário.
    :::column-end:::
    :::column:::
![não fazer](images/dont.svg) Não misturar várias fontes.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Tamanho e dimensionamento

Os tamanhos de fonte em aplicativos UWP são ajustados automaticamente em todos os dispositivos. O algoritmo de dimensionamento garante que uma fonte de 24 px no Surface Hub a 3 m de distância seja tão legível quanto uma fonte de 24 px no telefone de 5 polegadas a alguns centímetros de distância.

![distâncias de visualização em diferentes dispositivos](images/type/scaling-chart.svg)

Devido ao funcionamento do sistema de dimensionamento, você está projetando em pixels efetivos e não em pixels físicos reais, e não precisa alterar os tamanhos da fonte de acordo com diferentes telas ou resoluções.

:::row:::
    :::column:::
![fazer](images/do.svg) Seguir o dimensionamento da [rampa de tipos da UWP](#type-ramp).
    :::column-end:::
    :::column:::
![não fazer](images/dont.svg) Usar um tamanho de fonte inferior a 12 px.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Hierarquia

:::row:::
    :::column:::
Os usuário dependem de uma hierarquia visual ao examinar uma página: os cabeçalhos resumem o conteúdo e o texto do corpo fornece mais detalhes. Para criar uma hierarquia visual clara em seu aplicativo, siga a rampa de tipos da UWP.
    :::column-end:::
    :::column:::
![estilos de bloco de texto](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Rampa de tipos

A rampa de tipos da UWP estabelece relações específicas entre os estilos de tipo em uma página, ajudando os usuários a ler o conteúdo facilmente. Todos os tamanhos estão em pixels efetivos e são otimizados para aplicativos UWP executados em todos os dispositivos.

![Rampa de tipos](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>Uso da rampa de tipos

:::row:::
    :::column:::
Você pode acessar os níveis da rampa de tipos como [recursos estáticos](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp) XAML. Os estilos seguem a convenção de nomenclatura `*TextBlockStyle`.
    :::column-end:::
    :::column:::
![estilos de bloco de texto](images/type/text-block-type-ramp.svg)
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
![fazer](images/do.svg) Usar "Corpo" para a maioria dos textos.

Usar "Base" para títulos quando o espaço é limitado.
    :::column-end:::
    :::column:::
![não fazer](images/dont.svg) Usar "Legenda" para ação principal ou cadeias de caracteres longas.

Usar "Cabeçalho" ou "Subcabeçalho" se for preciso quebra automática de linha.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Alinhamento

O [TextAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.textalignment) padrão é à esquerda e, na maioria dos casos, essa abordagem com recuo à esquerda e irregular à direita fornece ancoragem consistente de conteúdo e um layout uniforme. Para os idiomas da direita para a esquerda, confira [Ajustando layout e fontes para dar suporte à globalização](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Mostra texto à esquerda.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Contagem de caracteres

:::row:::
    :::column:::
![fazer](images/do.svg) Manter 50 a 60 letras por linha para facilitar a leitura.
    :::column-end:::
    :::column:::
![não fazer](images/dont.svg) Menos de 20 caracteres ou mais de 60 caracteres por linha dificulta a leitura.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Recorte e elipses

Quando a quantidade de texto se estende para além do espaço disponível, recomendamos dividir o texto, que é o comportamento padrão da maioria dos [controles de texto da UWP](../controls-and-patterns/text-controls.md).

![Mostra um quadro de dispositivo com alguns recortes de texto](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![fazer](images/do.svg) Recortar o texto e usar a quebra automática se várias linhas estiverem habilitadas.
    :::column-end:::
    :::column:::
![não fazer](images/dont.svg) Usar reticências para evitar a poluição visual.
    :::column-end:::
:::row-end:::

**Observação**: se os contêineres não estão bem definidos (por exemplo, sem cor de diferenciação da tela de fundo) ou quando há um link para ver mais texto, use as reticências.

## <a name="languages"></a>Idiomas 

A Segoe UI é nossa fonte para inglês, idiomas europeus, grego, hebraico, armênio, georgiano e árabe. Para outros idiomas, confira as recomendações a seguir.

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
<td align="left">Fonte de interface do usuário para scripts do Sul Asiático (bengali, devanágari, gujarati, gurmukhi, kannada, malaiala, oriá, ol chiki, cingalês, sora sompeng, tâmil, télugo)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Regular</td>
<td align="left">Uma fonte de interface do usuário chinesa herdada. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Interface do usuário Yu Gothic</td>
<td align="left">Leve, semileve, regular, seminegrito e negrito</td>
<td align="left">Fonte de interface do usuário para japonês.</td>
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
<td align="left">Uma fonte de código-fonte que é metricamente compatível com Segoe UI, destinada a aplicativos em outras plataformas que não desejam usar Segoe UI. <a href="https://github.com/Microsoft/Selawik">Obtenha a Selawik no GitHub.</a></td>
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
<td style="font-family: Georgia;">Geórgia</td>
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
