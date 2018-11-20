---
author: Karl-Bridge-Microsoft
Description: Respond to mouse input in your apps by handling the same basic pointer events that you use for touch and pen input.
title: Interações por mouse
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ec0f99f069950776ab934c82bd3197bdd6c9dd6
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7280407"
---
# <a name="mouse-interactions"></a>Interações por mouse


Otimize o design do seu aplicativo UWP (Plataforma Universal do Windows) para entrada touch e obtenha, por padrão, o suporte básico para mouse.

 

![mouse](images/input-patterns/input-mouse.jpg)



A entrada via mouse é a mais adequada às interações que exigem precisão do usuário para apontar e clicar. Essa precisão inerente é naturalmente aceita pela interface do usuário do Windows, que foi otimizado para a natureza imprecisa do toque.

Mouse e touch apresentam divergências quando o assunto é a capacidade do touch de emular mais precisamente a manipulação direta de elementos da interface do usuário na tela usando gestos físicos executados diretamente nesses objetos (por exemplo, passar o dedo, deslizar o dedo, arrastar, girar etc.). Manipulações com o mouse geralmente exigem alguma outra funcionalidade de interface do usuário, como o uso de identificadores para redimensionar ou girar um objeto.

Este tópico descreve as considerações de design para interações com mouse.

## <a name="the-uwp-app-mouse-language"></a>A linguagem de mouse de aplicativo UWP


Um conjunto conciso de interações de mouse é usado de forma consistente em todo o sistema.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Termo</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Focalizar para aprender</p></td>
<td align="left"><p>Focalize um elemento para mostrar informações mais detalhadas ou elementos visuais explicativos (como uma dica de ferramenta) antes de realizar uma ação.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clicar para uma ação principal</p></td>
<td align="left"><p>Clique em um elemento para invocar sua ação principal (como iniciar um aplicativo ou executar um comando).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Rolar para mudar o modo de exibição</p></td>
<td align="left"><p>Exiba barras de rolagem para mover para cima, para baixo, para a esquerda e para a direita em uma área de conteúdo. Os usuários podem rolar clicando nas barras de rolagem ou girando a roda do mouse. As barras de rolagem indicam o local do modo de exibição atual dentro da área de conteúdo (o movimento panorâmico com toque exibe uma IU parecida).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clicar com o botão direito do mouse para selecionar e executar um comando</p></td>
<td align="left"><p>Clique com o botão direito do mouse para exibir a barra de navegação (se disponível) e a barra de aplicativos com os comandos globais. Clique com o botão direito do mouse em um elemento para selecioná-lo e exibir a barra de apps com os comandos contextuais relacionados ao elemento selecionado.</p>
<div class="alert">
<strong>Observação</strong>com o botão direito para exibir um menu de contexto, se os comandos de barra de seleção ou o aplicativo não forem os comportamentos adequados da interface do usuário. Mas a nossa recomendação é que você use a barra de apps para os comportamentos de todos os comandos.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Comandos da interface do usuário para aplicar zoom</p></td>
<td align="left"><p>Exiba os comandos da interface do usuário na barra de aplicativos (como + e -) ou pressione Ctrl e gire a roda do mouse para emular gestos de pinçagem e ampliação para aplicar zoom.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Comandos da interface do usuário para girar</p></td>
<td align="left"><p>Exiba os comandos da interface do usuário na barra de aplicativos ou pressione Ctrl+Shift e gire a roda do mouse para emular o gesto de rotação para girar. Gire o próprio dispositivo para girar a tela inteira.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Clicar e arrastar para reorganizar</p></td>
<td align="left"><p>Clique e arraste um elemento para movê-lo.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clicar e arrastar para selecionar texto</p></td>
<td align="left"><p>Clique dentro de um texto selecionável e arraste para selecioná-lo. Clique duas vezes para selecionar uma palavra.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-events"></a>Eventos de mouse

Responda à entrada do mouse em seus aplicativos manipulando os mesmos eventos de ponteiro básicos que você usa para entrada touch e por caneta.

Use eventos [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) para implementar funcionalidade de entrada básica sem a necessidade de gravar código para cada dispositivo de entrada de ponteiro. No entanto, você ainda pode aproveitar as funcionalidades especiais de cada dispositivo (como eventos do botão de rolagem do mouse) usando os eventos de ponteiro, gesto e manipulação desse objeto.

**Exemplos:** Consulte essa funcionalidade em ação em nossos [exemplos de aplicativo](https://go.microsoft.com/fwlink/p/?LinkID=264996).


- [Entrada: exemplo de funcionalidades do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)

- [Exemplo de entrada](https://go.microsoft.com/fwlink/p/?linkid=226855)

- [Entrada: gestos e manipulações com o GestureRecognizer](https://go.microsoft.com/fwlink/p/?LinkID=231605)

## <a name="guidelines-for-visual-feedback"></a>Diretrizes de feedback visual


-   Quando o mouse é detectado (por eventos de movimentação ou focalização), mostre a interface do usuário específica do mouse para indicar a funcionalidade exposta pelo elemento. Se o mouse não for movimentado por algum tempo ou se o usuário começar uma interação por toque, faça com que a interface do usuário do mouse desapareça gradualmente. Dessa forma, a interface do usuário fica mais organizada.
-   Não use o cursor para comentário de foco; o comentário apresentado pelo elemento é suficiente (veja Cursores, a seguir).
-   Não exiba respostas visuais quando o elemento não permite manipulação (como texto estático).
-   Não use retângulos de foco nas manipulações por mouse. Reserve-os para as interações por teclado.
-   Exiba respostas visuais simultaneamente para todos os elementos que representam o mesmo destino de entrada.
-   Inclua botões (como + e -) para emular manipulações baseadas em toque, como movimento panorâmico, giro, zoom, etc.

Para obter diretrizes mais gerais sobre comentários visuais, consulte [Diretrizes de comentários visuais](guidelines-for-visualfeedback.md).


## <a name="cursors"></a>Cursores


Um conjunto de cursores padrão está disponível para ponteiros de mouse. São usados para indicar a ação primária de um elemento.

Cada cursor padrão tem uma imagem padrão correspondente associada. O usuário ou um aplicativo substitui a imagem padrão associada a qualquer cursor padrão quando desejado. Especifique uma imagem de cursor usando a função [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273).

Se você precisa personalizar o cursor do mouse:

-   Sempre use o cursor de seta (![cursor de seta](images/cursor-arrow.png)) para elementos clicáveis. não use o de cursor de mão apontando (![cursor de mão apontando](images/cursor-pointinghand.png)) para links ou outros elementos interativos. No lugar dele, use os efeitos de focalização (descritos anteriormente).
-   Use o cursor de texto (![cursor de texto](images/cursor-text.png)) para texto selecionável.
-   Use o de cursor de movimentação (![cursor de movimentação](images/cursor-move.png)) quando o movimento é a ação principal (como arrastar ou cortar). Não use o cursor de movimentação para elementos em que a ação principal é navegar (como os blocos na tela inicial).
-   Use os cursores de redimensionamento horizontal, vertical e diagonal (![cursor de redimensionamento vertical](images/cursor-vertical.png), ![cursor de redimensionamento horizontal](images/cursor-horizontal.png), ![cursor de redimensionamento diagonal (inferior esquerdo, superior direito)](images/cursor-diagonal2.png), ![cursor de redimensionamento diagonal (superior esquerdo, inferior direito)](images/cursor-diagonal1.png)), quando um objeto for redimensionável.
-   Use os cursores de mão agarrando (![cursor de mão agarrando (aberto)](images/cursor-pan1.png), ![cursor de mão agarrando (fechado)](images/cursor-pan2.png)) durante o movimento panorâmico do conteúdo dentro de uma tela fixa (como um mapa).

## <a name="related-articles"></a>Artigos relacionados

* [Identificar entrada do ponteiro](handle-pointer-input.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)

**Exemplos**
* [Amostra de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Amostra de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais do foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Exemplos de arquivo morto**
* [Entrada: amostra de funcionalidades do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: amostra de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Amostra de rolagem, movimento panorâmico e aplicação de zoom em XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: gestos e interações com o GestureRecognizer](https://go.microsoft.com/fwlink/p/?LinkID=231605)
 
 

 




