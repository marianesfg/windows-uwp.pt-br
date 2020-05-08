---
Description: Este tópico descreve a nova interface de usuário do Windows para rotação e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação em seu aplicativo do Windows.
title: Rotação
ms.assetid: f098bc05-35b3-46b2-9e9b-9ff292d067ca
label: Rotation
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 08d0eb18d59c9a5c19826eb7b6e8d4b65179b6fd
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970101"
---
# <a name="rotation"></a>Rotação


Este artigo descreve a nova interface de usuário do Windows para rotação e fornece diretrizes de experiência do usuário que devem ser consideradas ao usar esse novo mecanismo de interação em seu aplicativo do Windows.

> **APIs importantes**: [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="dos-and-donts"></a>Recomendações

-   Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente.

## <a name="additional-usage-guidance"></a>Diretrizes de uso adicional


**Visão geral da rotação**

A rotação é a técnica otimizada para toque usada por aplicativos do Windows para permitir que os usuários transformem um objeto em direção circular (no sentido horário ou no sentido anti-horário).

Dependendo do dispositivo de entrada, a interação de rotação é realizada com:

-   Um mouse ou uma caneta ativa para mover a garra de rotação de um objeto selecionado.
-   Uma caneta de toque ou passiva para girar o objeto na direção desejada usando o gesto de girar.

**Quando usar a rotação**

Use a rotação para ajudar os usuários a girarem os elementos da interface diretamente. Os diagramas a seguir mostram algumas das posições de dedos com suporte para a interação de rotação.

![diagrama demonstrando várias posturas de dedos suportadas pela rotação.](images/ux-rotate-positions.png)

**Observe**    intuitivamente e, na maioria dos casos, o ponto de rotação é um dos dois pontos de toque, a menos que o usuário possa especificar um ponto de rotação não relacionado aos pontos de contato (por exemplo, em um aplicativo de desenho ou layout). As imagens a seguir demonstram como a experiência do usuário pode ser reduzida se o ponto de rotação não for restringida dessa forma.

A primeira foto mostra os pontos inicial (polegar) e secundário (dedo indicador): o dedo indicador está tocando uma árvore e o polegar está tocando uma tora.

![imagem mostrando os dois pontos iniciais de toque para o gesto de girar.](images/ux-rotate-points1.png)
Nesta segunda foto, a rotação é realizada em torno do ponto de toque inicial (dedão). Após a rotação, o dedo indicador permanece tocando o tronco da árvore e o polegar a tora (o ponto de rotação).

![imagem mostrando uma foto girada com o ponto de rotação restringido a um dos dois pontos de toque iniciais.](images/ux-rotate-points2.png)
Nesta terceira foto, o centro da rotação foi definido pelo aplicativo (ou definido pelo usuário) para ser o ponto central da foto. Após a rotação, como a foto não foi girada por um dos dedos, a ilusão de uma manipulação direta é quebrada (a menos que o usuário tenha escolhido essa configuração).

![imagem mostrando uma foto girada com o ponto de rotação restringido ao centro da foto em vez de algum dos dois pontos de toque iniciais.](images/ux-rotate-points3.png)
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
<th align="left">Type</th>
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
<strong>Observe</strong>  que um trilho de interface de usuário é um recurso no qual uma área em volta de um destino restringe a movimentação em direção a um valor ou local específico para influenciar sua seleção.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

## <a name="related-topics"></a>Tópicos relacionados

### <a name="samples"></a>Exemplos

- [Amostra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Amostra de entrada de baixa latência](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Amostra do modo de interação do usuário](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Amostra de visuais de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Exemplos de arquivo-morto

- [Entrada: amostra de eventos de entrada do usuário XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: amostra de funcionalidades do dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: amostra de teste de hit de toque](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Exemplo de rolagem, panorâmica e zoom do XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: amostra de tinta simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: gestos e manipulações com o GestureRecognizer](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: exemplo de manipulações e gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Amostra de entrada por toque do DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
