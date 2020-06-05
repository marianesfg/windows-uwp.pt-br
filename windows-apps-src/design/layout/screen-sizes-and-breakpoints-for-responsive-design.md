---
title: Tamanhos de tela e pontos de interrupção para um design responsivo
description: Em vez de otimizar a interface do usuário dos vários dispositivos no ecossistema do Windows 10, é recomendável projetar algumas categorias de largura chave chamadas pontos de interrupção.
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2e385f6b9977eead6aed52215080588e4f9d8c27
ms.sourcegitcommit: cc645386b996f6e59f1ee27583dcd4310f8fb2a6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84262767"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tamanhos de tela e pontos de interrupção

Os aplicativos do Windows podem ser executados em qualquer dispositivo que executa o Windows, o que inclui telefones, tablets, desktops, TVs e muito mais. Com um grande número de destinos de dispositivo e tamanhos de tela no ecossistema do Windows 10, em vez de otimizar a interface do usuário de cada dispositivo, é recomendável projetar algumas categorias de largura chave (também denominadas "pontos de interrupção"): 
- Pequena (menor que 640 px)
- Média (641 px a 1.007 px)
- Grande (1.008 px e maior)

> [!TIP]
> Ao projetar pontos de interrupção específicos, projete a quantidade de espaço de tela disponível para seu aplicativo (a janela do aplicativo), e não o tamanho da tela. Quando o aplicativo é executado em tela inteira, a janela do aplicativo tem o mesmo tamanho da tela, mas quando o aplicativo não está em tela inteira, a janela é menor do que a tela.

## <a name="breakpoints"></a>Pontos de interrupção
Esta tabela descreve as diferentes classes de tamanho e pontos de interrupção.

![Pontos de interrupção do design dinâmico](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Classe Size</th>
<th align="left">Pontos de interrupção</th>
<th align="left">Tamanho da tela típico (diagonal)</th>
<th align="left">Dispositivos</th>
<th align="left">Tamanhos de janela</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Pequeno</td>
<td style="vertical-align:top;">640px ou menos</td>
<td style="vertical-align:top;">4&quot; a 6&quot;; 20&quot; a 65&quot;</td>
<td style="vertical-align:top;">Telefones e TVs</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Médio</td>
<td style="vertical-align:top;">641px a 1007px</td>
<td style="vertical-align:top;">7&quot; a 12&quot;</td>
<td style="vertical-align:top;">Phablets, tablets</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1008px ou mais</td>
<td style="vertical-align:top;">13&quot; e maior</td>
<td style="vertical-align:top;">Computadores, laptops, Surface Hubs</td>
<td style="vertical-align:top;">1\.024 x 640, 1.366 x 768, 1.920 x 1.080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>Por que as TVs são consideradas "pequenas"? 

Embora a maioria das TVs sejam fisicamente muito grandes (40 a 65 polegadas é comum) e tenham resoluções altas (HD ou 4K), projetar para TVs de 1080p colocadas a 10 pés de distância é diferente de projetar para um monitor de 1080p a um pé distância em sua mesa. Quando você considera a distância, as TVs de 1080 pixels são mais como um monitor de 540 pixels muito mais próximo.

O sistema de pixel efetivo da UWP automaticamente considera a distância de exibição para você. Ao especificar um tamanho para um controle ou um intervalo de ponto de interrupção, você está realmente usando pixels "efetivos". Por exemplo, se você criar código responsivo para 1080 pixels e superior, um monitor 1080 usará esse código, mas uma TV de 1080p não porque embora uma TV de 1080 tenha 1080 pixels físicos, ela só tem 540 pixels efetivos. Projetar para TV é semelhante a projetar para um telefone.

## <a name="effective-pixels-and-scale-factor"></a>Pixels efetivos e fator de escala

Os aplicativos UWP ajustam automaticamente a escala da interface do usuário para garantir que o aplicativo seja legível em todos os dispositivos Windows 10. O Windows ajusta automaticamente a escala de cada monitor com base em seu DPI (pontos por polegada) e na distância de exibição do dispositivo. Os usuários podem substituir o valor padrão e ao acessar a página de definições de **Configurações** > **Exibição** > **Escala e layout**. 


## <a name="general-recommendations"></a>Recomendações gerais

### <a name="small"></a>Pequeno
- Defina as margens esquerda e direita de janela para 12px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.
- Encaixe [barras de aplicativo](../controls-and-patterns/app-bars.md) na parte inferior da janela para aprimorar a acessibilidade.
- Use uma coluna/região de cada vez.
- Use um ícone para representar a pesquisa (não mostre uma caixa de pesquisa).
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo de sobreposição para conservar espaço na tela.
- Se você estiver usando o [padrão de detalhes mestre](../controls-and-patterns/master-details.md), use o modo de apresentação empilhada para economizar espaço na tela.

### <a name="medium"></a>Médio
- Defina as margens esquerda e direita de janela para 24px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.
- Coloque elementos de comando, como [barras de aplicativo](../controls-and-patterns/app-bars.md), na parte superior da janela do aplicativo.
- Use até duas colunas/regiões.
- Mostre a caixa de pesquisa.
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo de fragmento de forma que uma faixa estreita de ícones sempre seja exibida.
- Considere fazer mais adaptações para [experiências de TV](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN).

### <a name="large"></a>Grande
- Defina as margens esquerda e direita de janela para 24px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.
- Coloque elementos de comando, como [barras de aplicativo](../controls-and-patterns/app-bars.md), na parte superior da janela do aplicativo.
- Use até três colunas/regiões.
- Mostre a caixa de pesquisa.
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo encaixado para que ele sempre apareça.
