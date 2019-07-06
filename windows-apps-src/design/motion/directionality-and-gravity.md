---
Description: Saiba como Fluent direcionalidade de usos de movimento e a gravidade.
title: Direção e gravidade - animação em aplicativos UWP
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f1e36f0febeeaac5a12d408d7be8a717f0ab398
ms.sourcegitcommit: 7c3b88198178d6f6a535f35e1bf8665410d41d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569119"
---
# <a name="directionality-and-gravity"></a>Direção e gravidade

Os sinais de direção ajudam a solidificar o modelo mental da jornada do usuário nas experiências. É importante que a direção de qualquer movimento ofereça suporte à continuidade do espaço, bem como à integridade dos objetos no espaço.

O movimento direcional está sujeito às forças como a gravidade. A aplicação de forças ao movimento reforça a aparência natural do movimento.

## <a name="examples"></a>Exemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/category/Motion">abrir o aplicativo e ver o Movimento em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>Direção do movimento

:::row:::
    :::column:::
Direção do movimento corresponde ao movimento físico. Assim como na natureza, os objetos podem ser mover em qualquer eixo: X,Y e Z. Isso é como consideramos o movimento dos objetos na tela.
Quando você move objetos, evite colisões não naturais. Tenha em mente onde objetos provenientes e vá para e sempre dar suporte a construções de nível superior que podem ser usadas na cena, como a hierarquia de direção ou do layout de rolagem.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Direção da navegação

A direção da navegação entre as cenas em seu aplicativo é conceitual. Os usuários navegam para frente e para trás. Cenas movem se movem para dentro e para fora com campo de visão. Esses conceitos combinam a movimentação física para orientar o usuário.

Quando a navegação provoca o deslocamento de um objeto da cena anterior para a nova, o objeto faz um simples movimento de A até B na tela. Para garantir que o movimento pareça mais físico, o padrão de suavização é adicionado, bem como a sensação de gravidade.

Para a navegação regressiva, o movimento é invertido (de B para A). Quando os usuários navegam de volta, eles esperam retornar para o estado anterior assim que possível. O tempo é mais rápido, mais direto e usa a suavização por desaceleração.

Aqui, esses princípios são aplicados à medida que o item selecionado permanece na tela durante a navegação para frente e para trás.

![Exemplo de interface do usuário de movimento contínuo](images/continuous3.gif)

Quando a navegação provoca a substituição de itens na tela, é importante mostrar para onde a cena de saída foi e de onde a nova cena vem.

Isso tem vários benefícios:

- Solidifica o modelo mental do usuário do espaço.
- A duração da cena existente fornece mais tempo para preparar o conteúdo que será animada da cena de entrada.
- Melhora o desempenho percebido do aplicativo.

Existem quatro direções discretas de navegação para considerar.

:::row:::
    :::column:::
**Encaminhamento em** celebrar o conteúdo, inserindo a cena de maneira que não colide com conteúdo de saída. Conteúdo desacelerada na cena.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Avanço horizontal** conteúdo surgir rapidamente. Aceleram a objetos fora da tela.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Com versões anteriores no** igual Forward-In, mas revertidas.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Horizontal com versões anteriores** mesmo que o encaminhamento de saída, mas invertidos.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Gravidade

A gravidade faz com que suas experiências sejam mais naturais. Os objetos que se movem no eixo Z e não são ancorados à cena por uma funcionalidade na tela têm o potencial de serem afetados pela gravidade. Quando um objeto sai da cena e antes de atingir a velocidade de escape, a gravidade o atrai para baixo, criando uma curva mais natural de trajetória à medida em que o objeto move.

Normalmente, a gravidade se manifesta quando um objeto deve saltar de uma cena para outra. Por isso a animação conectada usa o conceito de gravidade.

Aqui, um elemento na linha superior da grade é afetado pela gravidade, provocando sua queda à medida que deixa seu lugar e se move para a frente.

![Direção para trás e para dentro](images/continuity-photos.gif)

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral de animação](index.md)
- [Atingir o tempo e atenuação](timing-and-easing.md)
