---
author: mijacobs
title: Tamanhos de tela e pontos de interrupção para um design responsivo
description: Em vez de otimizar a interface do usuário dos vários dispositivos no ecossistema do Windows 10, é recomendável projetar algumas categorias de largura chave chamadas pontos de interrupção.
ms.author: mijacobs
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0d84530c1a7c3795c566495c1eae121691b0766a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1688932"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tamanhos de tela e pontos de interrupção

Os aplicativos UWP podem ser executados em qualquer dispositivo que executa o Windows 10, que inclui telefones, tablets, desktops, TVs e muito mais. Com um grande número de destinos de dispositivo e tamanhos de tela no ecossistema do Windows 10, em vez de otimizar a interface do usuário de cada dispositivo, é recomendável projetar algumas categorias de largura chave (também denominadas "pontos de interrupção"): 
- Pequena (menor que 640 px)
- Média (641 px a 1007 px)
- Grande (1008 px e maior)

> [!TIP]
> Ao projetar pontos de interrupção específicos, projete a quantidade de espaço de tela disponível para seu app (a janela do app), e não o tamanho da tela. Quando o app é executado em tela inteira, a janela do app tem o mesmo tamanho da tela, mas quando o app não está em tela inteira, a janela é menor do que a tela.

## <a name="breakpoints"></a>Pontos de interrupção
Esta tabela descreve as diferentes classes de tamanho e pontos de interrupção.

![pontos de interrupção de design dinâmico](images/rsp-design/rspd-breakpoints.png)

<table>
<thead>
<tr class="header">
<th align="left">Classe de tamanho</th>
<th align="left">Pontos de interrupção</th>
<th align="left">Tamanho da tela (diagonal)</th>
<th align="left">Dispositivos</th>
<th align="left">Tamanhos de janela</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Pequeno</td>
<td style="vertical-align:top;">640 px ou menos</td>
<td style="vertical-align:top;">4&quot; a 6&quot;</td>
<td style="vertical-align:top;">Telefones</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Médio</td>
<td style="vertical-align:top;">641 px a 1007 px</td>
<td style="vertical-align:top;">7&quot; a 12&quot;</td>
<td style="vertical-align:top;">Phablets, tablets, TVs</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1008 px ou maior</td>
<td style="vertical-align:top;">13&quot; e maior</td>
<td style="vertical-align:top;">Computadores, laptops, Surface Hubs</td>
<td style="vertical-align:top;">1024 x 640, 1366 x 768, 1920 x 1080</td>
</tr>
</tbody>
</table>

## <a name="general-recommendations"></a>Recomendações gerais

### <a name="small"></a>Pequeno
- Defina as margens esquerda e direita de janela para 12 px para criar uma separação visual entre as margens esquerda e direita da janela do app.
- Encaixe as [barras de aplicativos](../controls-and-patterns/app-bars.md) na parte inferior da janela para melhorar a acessibilidade.
- Use uma coluna/região de cada vez.
- Use um ícone para representar a pesquisa (não mostre uma caixa de pesquisa).
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo de sobreposição para conservar espaço na tela.
- Se você estiver usando o [padrão de detalhes mestre](../controls-and-patterns/master-details.md), use o modo de apresentação empilhada para economizar espaço na tela.

### <a name="medium"></a>Médio
- Defina as margens esquerda e direita de janela para 24 px para criar uma separação visual entre as margens esquerda e direita da janela do app.
- Coloque elementos de comando, como [barras de aplicativos](../controls-and-patterns/app-bars.md), na parte superior da janela do app.
- Use até duas colunas/regiões.
- Mostre a caixa de pesquisa.
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo de fragmento de forma que uma faixa estreita de ícones sempre seja exibida.
- É recomendável fazer mais adaptações para [experiências de TV](http://go.microsoft.com/fwlink/?LinkId=760736).

### <a name="large"></a>Grande
- Defina as margens esquerda e direita de janela para 24 px para criar uma separação visual entre as margens esquerda e direita da janela do app.
- Coloque elementos de comando, como [barras de aplicativos](../controls-and-patterns/app-bars.md), na parte superior da janela do app.
- Use até três colunas/regiões.
- Mostre a caixa de pesquisa.
- Coloque o [painel de navegação](../controls-and-patterns/navigationview.md) no modo encaixado para que ele sempre apareça.

>[!TIP] 
> Com o [**Continuum para Telefones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), os usuários podem conectar dispositivos móveis compatíveis com o Windows 10 a um monitor, um mouse e um teclado para fazê-los funcionar como laptops. Tenha essa nova funcionalidade em mente ao projetar pontos de interrupção específicos; um celular não permanecerá sempre na classe de tamanho.

## <a name="effective-pixels-and-scale-factor"></a>Pixels efetivos e fator de escala

Os aplicativos UWP ajustam automaticamente a escala da interface do usuário para garantir que o app seja legível em todos os dispositivos Windows 10. O Windows ajusta automaticamente a escala de cada monitor com base em seu DPI (pontos por polegada) e na distância de exibição do dispositivo. Os usuários podem substituir o valor padrão e ao acessar a página de definições de **Configurações** > **Exibição** > **Escala e layout**. 
