---
author: Jwmsft
Description: Siga estas diretrizes ao selecionar fontes e especificar seus tamanhos e cores para aplicativos UWP.
title: Fontes para aplicativos UWP
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7d72d5251000e16349efa8d4f7716e12d22764d6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="fonts-for-uwp-apps"></a>Fontes para aplicativos UWP

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Este artigo lista as fontes recomendadas para aplicativos UWP. Essas fontes certamente estão disponíveis em todas as edições do Windows 10 que dão suporte a aplicativos UWP.

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Propriedade FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

O [guia de tipografia UWP](typography.md) recomenda que os aplicativos usem a fonte Segoe UI e, embora Segoe UI seja uma excelente escolha para a maioria dos aplicativos, você não precisa usá-la para tudo. Você pode usar outras fontes para determinados cenários, como leitura, ou ao exibir texto em alguns idiomas diferentes do inglês. 
 
## <a name="sans-serif-fonts"></a>Fontes sans-serif

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

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para scripts históricos</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Regular, semileve, claro, negrito, seminegrito</td>
<td align="left">Uma fonte de código-fonte que é metricamente compatível com Segoe UI, destinada a aplicativos em outras plataformas que não desejam usar Segoe UI. [Obtenha Selawik no GitHub.](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left">Dá suporte a scripts europeus (latino, grego, cirílico e armênio).</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Fontes serif

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

## <a name="symbols-and-icons"></a>Ícones e símbolos


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
<td align="left">Fonte de interface do usuário para ícones de aplicativos. Para obter mais informações, consulte o [artigo de ativos de Segoe MDL2](segoe-ui-symbol-font.md).</td>
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



## <a name="fonts-for-non-latin-languages"></a>Fontes para idiomas não latinos

Embora muitas dessas fontes forneçam caracteres latinos.

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
<tr class="even">
<td align="left">Fonte de Fallback regular para texto em javanês para script javanês</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script javanês</td>
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
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script tibetano.</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Regular, negrito, claro</td>
<td align="left">Fonte de interface do usuário para chinês tradicional.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script New Tai Lue.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Phags-pa.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Tai Le.</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Regular, negrito, claro</td>
<td align="left">Fonte de interface do usuário para chinês simplificado.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Yi.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script mongol.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Thaana.</td>
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
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">Médio</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Interface do usuário Yu Gothic</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para japonês.</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>Fontes de globalização/localização
Use as [APIs de mapeamento de fonte LanguageFont](https://msdn.microsoft.com/library/windows/apps/br206864) para acesso via programação à família de fontes, tamanho, peso e estilo recomendados para um idioma específico. O objeto LanguageFont oferece acesso às informações corretas de fonte para várias categorias de conteúdo, incluindo cabeçalhos de interface do usuário, notificações, texto do corpo e fontes de corpo de documento editáveis pelo usuário. Para obter mais informações, consulte [Ajustando layout e fontes para dar suporte à globalização](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl).


## <a name="get-the-samples"></a>Obter os exemplos

* [Amostra de fontes baixáveis](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [Amostra de noções básicas de interface do usuário](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [Amostra de espaçamento entre linhas com DirectWrite](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>Artigos relacionados

* [Ajustando layout e fontes para dar suporte à globalização](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [Controles de texto)](../controls-and-patterns/text-controls.md)
* [Recursos de temas XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 




