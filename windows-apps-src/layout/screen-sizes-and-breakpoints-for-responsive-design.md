---
author: mijacobs
title: "Tamanhos de tela e pontos de interrupção para um design responsivo"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 153652c9fcc9745bdee087033d65eec2bc860e53

---

#  Tamanhos de tela e pontos de interrupção para um design responsivo

O número de destinos de dispositivo e tamanhos de tela do ecossistema do Windows 10 é muito grande para você se preocupar com a otimização de sua interface do usuário em cada um deles. Em vez disso, recomendamos projetar para algumas larguras principais (também chamadas de "pontos de interrupção"): 360, 640, 1024 e 1366 epx.

**Dica** Ao projetar para pontos de interrupção específicos, projete pela quantidade de espaço de tela disponível para seu aplicativo (a janela do aplicativo). Quando o aplicativo é executado em tela inteira, a janela do aplicativo é do mesmo tamanho da tela, mas em outros casos, ela é menor.
 

Esta tabela descreve as classes de tamanho diferentes e fornece recomendações gerais para se adequar a essas classes de tamanho.

![pontos de interrupção do design responsivo](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Classe Size</th>
<th align="left">pequeno</th>
<th align="left">médio</th>
<th align="left">grande</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Tamanho da tela típico (diagonal)</td>
<td align="left">4&quot; a 6&quot;</td>
<td align="left">7&quot; a 12&quot; ou TVs</td>
<td align="left">13&quot; e maior</td>
</tr>
<tr class="even">
<td align="left">Dispositivos comuns</td>
<td align="left">Telefones</td>
<td align="left">Phablets, tablets, TVs</td>
<td align="left">Computadores, laptops, Surface Hubs</td>
</tr>
<tr class="odd">
<td align="left">Tamanhos de janela comuns em pixels efetivos</td>
<td align="left">320x569, 360x640, 480x854</td>
<td align="left">960x540, 1024x640</td>
<td align="left">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td align="left">Pontos de interrupção de largura de janela em pixels efetivos</td>
<td align="left">640px ou menos</td>
<td align="left">641px a 1007px</td>
<td align="left">1008px ou mais</td>
</tr>
<tr class="odd">
<td align="left" valign="top">Recomendações gerais</td>
<td align="left" valign="top"><ul>
<li>Centralize elementos de guia.</li>
<li>Defina as margens esquerda e direita de janela para 12px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.</li>
<li>Encaixe [barras de aplicativo](../controls-and-patterns/app-bars.md) na parte inferior da janela para melhorar a acessibilidade</li>
<li>Usar uma coluna/região de cada vez</li>
<li>Use um ícone para representar a pesquisa (não mostre uma caixa de pesquisa).</li>
<li>Coloque o [painel de navegação](../controls-and-patterns/nav-pane.md) no modo de sobreposição para conservar espaço na tela.</li>
<li>Se você estiver usando o [padrão de detalhes mestre](../controls-and-patterns/master-details.md), use o modo de apresentação empilhada para economizar espaço na tela.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Crie elementos de guia alinhados à esquerda.</li>
<li>Defina as margens esquerda e direita de janela para 24px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.</li>
<li>Coloque elementos de comando, como [barras de aplicativo](../controls-and-patterns/app-bars.md), na parte superior da janela do aplicativo.</li>
<li>Até duas colunas/regiões</li>
<li>Mostre a caixa de pesquisa.</li>
<li>Coloque o [painel de navegação](../controls-and-patterns/nav-pane.md) no modo de fragmento de forma que uma faixa estreita de ícones sempre seja exibida.</li>
<li>Considere fazer mais adaptações para [experiências de TV](http://go.microsoft.com/fwlink/?LinkId=760736).</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Crie elementos de guia alinhados à esquerda.</li>
<li>Defina as margens esquerda e direita de janela para 24px para criar uma separação visual entre as margens esquerda e direita da janela do aplicativo.</li>
<li>Coloque elementos de comando, como [barras de aplicativo](../controls-and-patterns/app-bars.md), na parte superior da janela do aplicativo.</li>
<li>Até três colunas/regiões</li>
<li>Mostre a caixa de pesquisa.</li>
<li>Coloque o [painel de navegação](../controls-and-patterns/nav-pane.md) no modo encaixado para que ele sempre apareça.</li>
</ul></td>
</tr>
</tbody>
</table>

Com o [**Continuum para Telefones**](http://go.microsoft.com/fwlink/p/?LinkID=699431), uma nova experiência para dispositivos móveis compatíveis com o Windows 10, os usuários podem conectar seus telefones a um mouse e um teclado para fazê-los funcionar como laptops. Mantenha essa nova funcionalidade em mente ao criar designs para pontos de interrupção específicos - um telefone móvel não permanecerá sempre na classe de tamanho pequeno.
 



<!--HONumber=Jun16_HO4-->


