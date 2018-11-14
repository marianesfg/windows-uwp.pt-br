---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: Direção e gravidade - animação em aplicativos UWP
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 09bccec40c25e5eba06ba7a7c428b7b771f84271
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6185492"
---
# <a name="directionality-and-gravity"></a>Direção e gravidade

Os sinais de direção ajudam a solidificar o modelo mental da jornada do usuário nas experiências. É importante que a direção de qualquer movimento ofereça suporte à continuidade do espaço, bem como à integridade dos objetos no espaço.

O movimento direcional está sujeito às forças como a gravidade. A aplicação de forças ao movimento reforça a aparência natural do movimento.

## <a name="direction-of-movement"></a>Direção do movimento

:::row:::
    :::column:::
        Direction of movement corresponds to physical motion. Just like in nature, objects can move in any world axis - X,Y,Z. This is how we think of the movement of objects on the screen.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
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
        **Forward-In**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Forward-Out**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-In**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-Out**

        Same as Forward-Out, but reversed.
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

- [Visão geral do movimento](index.md)
- [Tempo e suavização](timing-and-easing.md)