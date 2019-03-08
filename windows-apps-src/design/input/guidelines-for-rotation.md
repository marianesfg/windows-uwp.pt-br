---
Description: Este tópico descreve a interface do usuário do Windows novo para a rotação e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação no seu aplicativo UWP.
title: Rotação
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f631f3178b4af4fe1c1d2d8b27e8ae6ac25c6ad1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617201"
---
# <a name="rotation"></a>Rotação


Este artigo descreve a nova IU do Windows para rotação. Também fornece diretrizes para a experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação no seu aplicativo UWP.

> **APIs importantes**: [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente.

## <a name="additional-usage-guidance"></a>Diretriz de uso adicional


**Visão geral de rotação**

A rotação é uma técnica otimizada para toque usada por aplicativos UWP para permitir que os usuários girem um objeto em uma direção circular (sentido horário ou anti-horário).

Dependendo do dispositivo de entrada, a interação de rotação é realizada com:

-   Um mouse ou uma caneta ativa para mover a garra de rotação de um objeto selecionado.
-   Uma caneta de toque ou passiva para girar o objeto na direção desejada usando o gesto de girar.

**Quando usar a rotação**

Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente. Os diagramas a seguir mostram algumas das posições de dedos com suporte para a interação de rotação.

![diagrama demonstrando várias posturas de dedos suportadas pela rotação.](images/ux-rotate-positions.png)

**Observação**    intuitivamente e na maioria dos casos, o ponto de rotação é um dos pontos de duas toque, a menos que o usuário pode especificar um ponto de rotação não relacionado com os pontos de contato (por exemplo, em um aplicativo de desenho ou layout). As imagens a seguir demonstram como a experiência do usuário pode ser reduzida se o ponto de rotação não for restringida dessa forma.

A primeira foto mostra os pontos inicial (polegar) e secundário (dedo indicador): o dedo indicador está tocando uma árvore e o polegar está tocando uma tora.

![Imagem mostrando os dois pontos de toque inicial para o gesto de rotação.](images/ux-rotate-points1.png)
Nesta segunda foto, a rotação é realizada em torno do ponto de toque inicial (dedão). Após a rotação, o dedo indicador permanece tocando o tronco da árvore e o polegar a tora (o ponto de rotação).

![Imagem mostrando uma imagem girada com o ponto de rotação restritos a um dos dois pontos de toque inicial.](images/ux-rotate-points2.png)
Nesta terceira foto, o centro da rotação foi definido pelo aplicativo (ou definido pelo usuário) para ser o ponto central da foto. Após a rotação, como a foto não foi girada por um dos dedos, a ilusão de uma manipulação direta é quebrada (a menos que o usuário tenha escolhido essa configuração).

![Imagem mostrando uma imagem girada com o ponto de rotação é restrito para o centro da imagem em vez de qualquer um dos dois pontos de toque inicial.](images/ux-rotate-points3.png)
Nesta última foto, o centro da rotação foi definido pelo aplicativo (ou definido pelo usuário) para ser um ponto no meio da borda esquerda da foto. Novamente, a menos que o usuário tenha escolhido essa configuração, nesse caso, a ilusão de uma manipulação direta é quebrada.

![imagem mostrando uma foto girada com o ponto de rotação restringido ao centro mais à esquerda da foto em vez de a um dos dois pontos de toque iniciais.](images/ux-rotate-points4.png)

 

Windows 8 oferece suporte a três tipos de rotação: gratuito, restrita e combinados.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Rotação livre</td>
<td align="left"><p>A rotação livre permite ao usuário rotacionar conteúdos livremente onde deseja em um arco de 360 graus. Quando o usuário solta o objeto, o mesmo permanece na posição escolhida. A rotação livre é útil para aplicativos de desenho e layout como: Microsoft PowerPoint, Word, Visio, e Paint, e Adobe Photoshop, Illustrator, e Flash.</p></td>
</tr>
<tr class="even">
<td align="left">Rotação restringida</td>
<td align="left"><p>A rotação restringida suporta a rotação livre durante a manipulação, mas aplica pontos de alinhamento em bases de 90 graus (0, 90, 180 e 270) ao soltar. Quando o usuário solta o objeto, o mesmo rotaciona automaticamente para o ponto de alinhamento mais próximo.</p>
<p>A rotação restringida é o método de rotação mais comum e funciona de forma semelhante à rolagem de conteúdo. Pontos de alinhamento permitem que um usuário seja impreciso e ainda assim alcance seu objetivo. A rotação restringida é útil para aplicativos como navegadores da web ou álbuns de fotos.</p></td>
</tr>
<tr class="odd">
<td align="left">Rotação combinada</td>
<td align="left"><p>A rotação combinada suporta a rotação livre com zonas (parecidas com trilhos em <a href="guidelines-for-panning.md">Diretrizes para movimento panorâmico</a>) em cada ponto de alinhamento de 90 graus imposta pela rotação restringida. Se o usuário soltar o objeto fora de uma das zonas de 90 graus, o objeto permanecerá naquela posição. Caso contrário, o objeto girará automaticamente para um ponto de alinhamento.</p>
<div class="alert">
<strong>Observação</strong>  um rail de interface do usuário é um recurso no qual uma área ao redor de um destino restringe o movimento na direção de algum valor específico ou um local para influenciar sua seleção.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Tópicos relacionados


**Exemplos**
* [Exemplo de entrada básico](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Exemplo de entrada de baixa latência](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Amostra do modo de interação do usuário](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Amostra de elementos visuais de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Amostras de arquivo-morto**
* [Entrada: Exemplo de eventos de entrada do usuário XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: Exemplo de recursos do dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: Exemplo de teste de hit de toque](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML de rolagem, movimento panorâmico e zoom de exemplo](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: Exemplo simplificado de tinta](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: Gestos e manipulações com GestureRecognizer](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: Manipulações e exemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Exemplo de entrada de toque do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




