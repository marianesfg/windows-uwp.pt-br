---
author: Jwmsft
Description: Siga estas diretrizes ao selecionar fontes e especificar seus tamanhos e cores.
title: Fontes
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 7db364240dd98f59a4a4d1d0c23cee1195682de2
ms.openlocfilehash: 52de4d9517c7f3064ad9e589a95e6f96400524cc

---

# Diretrizes de fontes

**APIs importantes**

-   [**Propriedade FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

O uso apropriado de tamanhos, espessuras, cores, rastreamento e espaçamento de fontes pode ajudar a dar a seu aplicativo UWP (Plataforma Universal do Windows (UWP) um visual limpo e organizado que o torna mais fácil de usar. Siga estas diretrizes ao selecionar fontes e especificar seus tamanhos e cores.

Se você estiver procurando uma lista de ícones Segoe UI Symbol, veja [**Diretrizes de ícones Segoe UI Symbol**](segoe-ui-symbol-font.md).

## A rampa de tipos do Windows 10


A rampa de tipos estabelece uma relação de design fundamental dos títulos ao texto do corpo e garante uma hierarquia clara e compreensível entre os diferentes níveis. Os usuários compreendem imediatamente onde encontrar as informações e como analisar a página.

Veja a rampa de tipos que é recomendável para aplicativos UWP:

| Estilo de texto | Face de tipos | Espessura    | Tamanho (epx) | Espaçamento entre linhas (epx) | Espaçamento entre palavras | Rastreamento (1/1000 em) | Chave de estilo XAML          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| Cabeçalho     | Segoe UI | Light     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| Subcabeçalho  | Segoe UI | Light     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| Título      | Segoe UI | Semileve | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| Subtítulo   | Segoe UI | Regular   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| Base       | Segoe UI | Semileve  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| Corpo       | Segoe UI | Regular   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| Legenda    | Segoe UI | Regular   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## Fontes recomendadas


Você não precisa usar a fonte Segoe UI para tudo. Você pode usar outras fontes para determinados cenários, como leitura, ou quando exibir texto em idiomas diferentes do inglês.

Esta é a lista de fontes que certamente estão disponíveis em todas as edições do Windows 10 que dão suporte a aplicativos UWP.

**Observação**  Se você usar uma fonte que não esteja nessa lista, seu aplicativo poderá disparar um download automático dos dados de fontes de um serviço Microsoft. Isso pode causar problemas de desempenho e outros impactos preocupantes, especialmente para dispositivos móveis. Em particular, observe que isso pode consumir parte do plano de dados móveis do usuário ou resultar em custos de uso de dados móveis. Os aplicativos UWP que serão disponibilizados em dispositivos móveis nunca devem usar fontes para conteúdo de interface do usuário diferentes das fontes nessa lista.

 

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
<th align="left">Comentário</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">Regular, itálico, negrito, negrito itálico, preto</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">Regular, itálico, negrito, negrito itálico, leve, itálico leve</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">Regular, negrito</td>
<td align="left">Fonte de interface do usuário para scripts da África (etíope, n’Ko, osmanli, tifinagh, Vai)</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para scripts na América do Norte (silábico canadense, Cherokee)</td>
</tr>
<tr class="odd">
<td align="left">Geórgia</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Fonte de Fallback regular para texto em javanês para script javanês</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script javanês</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">Regular, semileve, negrito</td>
<td align="left">Fonte de interface do usuário para scripts do Sudeste Asiático (bugi, laosiano, khmer, tailandês)</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para coreano</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script tibetano</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para chinês tradicional</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script New Tai Lue</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Phags-pa</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Tai Le</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para chinês simplificado</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Yi</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script mongol</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script Thaana</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para script de Myanmar</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">Regular, semileve, negrito</td>
<td align="left">Fonte de interface do usuário para scripts do Sul Asiático (bengali, devanágari, gujarati, gurmukhi, kannada, malaiala, odia, ol chiki, cingalês, sora sompeng, tâmil, télugo)</td>
</tr>
<tr class="odd">
<td align="left">Ativos de Segoe MDL2</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para ícones de aplicativos</td>
</tr>
<tr class="even">
<td align="left">Segoe Print</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">Regular, itálico, negrito, negrito itálico, leve, semileve, seminegrito, preto</td>
<td align="left">Fonte de interface do usuário para scripts da Europa e do Oriente Médio (árabe, armênio, cirílico, georgiano, grego, hebraico, latino) e também script Lisu</td>
</tr>
<tr class="even">
<td align="left">Emoji Segoe da interface do usuário</td>
<td align="left">Regular</td>
<td align="left">A versão fornecida no Windows Phone inclui um contorno branco ao redor de cada emoji para garantir que ele será exibido em qualquer tela de fundo colorida. Ela é metricamente compatível com a versão fornecida no Windows.</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para scripts históricos</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Regular</td>
<td align="left">Fonte de fallback para símbolos</td>
</tr>
<tr class="odd">
<td align="left">SimSun</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">Regular, itálico, negrito, negrito itálico</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">Médio</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Interface do usuário Yu Gothic</td>
<td align="left">Regular</td>
<td align="left">Fonte de interface do usuário para japonês</td>
</tr>
</tbody>
</table>

 

## Tópicos relacionados

**Para designers**
* [Rótulo (ou bloco de texto)](../controls-and-patterns/labels.md)
* [Ícones Segoe UI Symbol](segoe-ui-symbol-font.md)
**para desenvolvedores (XAML)**
* [Recursos de temas XAML](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [Definindo o layout de uma página de aplicativo](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Ícones Segoe UI Symbol](segoe-ui-symbol-font.md)
* [**Propriedade TextBlock.FontFamily**](https://msdn.microsoft.com/library/windows/apps/br209655)

**Exemplos**
* [Amostra de exibição de texto XAML](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [Estilo CSS: atribuindo uma marca à sua amostra de aplicativo](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [Amostra de mapeamento de fonte de idioma](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 







<!--HONumber=Jul16_HO2-->


