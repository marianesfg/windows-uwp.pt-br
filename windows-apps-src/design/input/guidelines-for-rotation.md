---
Description: Este tópico descreve a nova interface de usuário do Windows para rotação e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação em seu aplicativo UWP.
title: Rotação
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 71a09d304b0d68bf01012166173360ec6304fb2c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257933"
---
# <a name="rotation"></a>Rotação


Este artigo descreve a nova IU do Windows para rotação. Também fornece diretrizes para a experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação no seu aplicativo UWP.

> **APIs importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

-   Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional


**Visão geral da rotação**

A rotação é uma técnica otimizada para toque usada por aplicativos UWP para permitir que os usuários girem um objeto em uma direção circular (sentido horário ou anti-horário).

Dependendo do dispositivo de entrada, a interação de rotação é realizada com:

-   Um mouse ou uma caneta ativa para mover a garra de rotação de um objeto selecionado.
-   Uma caneta de toque ou passiva para girar o objeto na direção desejada usando o gesto de girar.

**Quando usar a rotação**

Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente. Os diagramas a seguir mostram algumas das posições de dedos com suporte para a interação de rotação.

![diagrama demonstrando várias posturas de dedos suportadas pela rotação.](images/ux-rotate-positions.png)

**Observe**   intuitivamente e, na maioria dos casos, o ponto de rotação é um dos dois pontos de toque, a menos que o usuário possa especificar um ponto de rotação não relacionado aos pontos de contato (por exemplo, em um aplicativo de desenho ou layout). As imagens a seguir demonstram como a experiência do usuário pode ser reduzida se o ponto de rotação não for restringida dessa forma.

A primeira foto mostra os pontos inicial (polegar) e secundário (dedo indicador): o dedo indicador está tocando uma árvore e o polegar está tocando uma tora.

![imagem mostrando os dois pontos de toque iniciais para o gesto de rotação.](images/ux-rotate-points1.png)
Nesta segunda foto, a rotação é realizada em torno do ponto de toque inicial (dedão). Após a rotação, o dedo indicador permanece tocando o tronco da árvore e o polegar a tora (o ponto de rotação).

![imagem mostrando uma imagem girada com o ponto de rotação restrito a um dos dois pontos de toque iniciais.](images/ux-rotate-points2.png)
Nesta terceira foto, o centro da rotação foi definido pelo aplicativo (ou definido pelo usuário) para ser o ponto central da foto. Após a rotação, como a foto não foi girada por um dos dedos, a ilusão de uma manipulação direta é quebrada (a menos que o usuário tenha escolhido essa configuração).

![imagem mostrando uma imagem girada com o ponto de rotação restrito ao centro da imagem, em vez de qualquer um dos dois pontos de toque iniciais.](images/ux-rotate-points3.png)
Nesta última foto, o centro da rotação foi definido pelo aplicativo (ou definido pelo usuário) para ser um ponto no meio da borda esquerda da foto. Novamente, a menos que o usuário tenha escolhido essa configuração, nesse caso, a ilusão de uma manipulação direta é quebrada.

![imagem mostrando uma foto girada com o ponto de rotação restringido ao centro mais à esquerda da foto em vez de a um dos dois pontos de toque iniciais.](images/ux-rotate-points4.png)

 

O Windows 10 dá suporte a três tipos de rotação: gratuita, restrita e combinada.

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
<td align="left"><p>A rotação livre permite ao usuário girar conteúdos livremente onde desejar em um arco de 360 graus. Quando o usuário solta o objeto, este permanece na posição escolhida. A rotação livre é útil para aplicativos de desenho e layout como: Microsoft PowerPoint, Word, Visio, e Paint, e Adobe Photoshop, Illustrator, e Flash.</p></td>
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
<strong>Observe</strong>  um trilho de interface de usuário é um recurso no qual uma área em volta de um destino restringe a movimentação em direção a um valor ou local específico para influenciar sua seleção.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>Tópicos relacionados


**Exemplos**
* [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
* [Exemplo de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
* [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
* [Amostra de elementos visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

**Amostras de arquivo-morto**
* [Entrada: exemplo de eventos de entrada do usuário XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
* [Entrada: exemplo de recursos do dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
* [Entrada: exemplo de teste de colisão de toque](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
* [Exemplo de rolagem, panorâmica e zoom do XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
* [Entrada: exemplo de tinta simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
* [Entrada: gestos e manipulações com GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: exemplo de manipulações e gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
* [Exemplo de entrada do DirectX Touch](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
 

 




