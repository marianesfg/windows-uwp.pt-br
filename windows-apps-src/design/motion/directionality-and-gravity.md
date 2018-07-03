---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: Direção e gravidade - animação em aplicativos UWP
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: a5216e81bc556a2e761e88b071e988bf6e4f457e
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843710"
---
# <a name="directionality-and-gravity"></a>Direção e gravidade

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Os sinais de direção ajudam a solidificar o modelo mental da jornada do usuário nas experiências. É importante que a direção de qualquer movimento ofereça suporte à continuidade do espaço, bem como à integridade dos objetos no espaço.

O movimento direcional está sujeito às forças como a gravidade. A aplicação de forças ao movimento reforça a aparência natural do movimento.

## <a name="direction-of-movement"></a>Direção do movimento

:::linha::: :::coluna::: A direção do movimento corresponde ao movimento físico. Assim como na natureza, os objetos podem ser mover em qualquer eixo: X,Y e Z. Isso é como consideramos o movimento dos objetos na tela.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::fim da linha:::

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

:::linha::: :::coluna::: **Para frente-para dentro**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::fim da linha::: :::linha::: :::coluna::: **Para frente-para fora**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::fim da linha::: :::linha::: :::coluna::: **Para trás-para dentro**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::fim da linha::: :::linha::: :::coluna::: **Para trás-para fora**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::fim da linha:::

## <a name="gravity"></a>Gravidade

A gravidade faz com que suas experiências sejam mais naturais. Os objetos que se movem no eixo Z e não são ancorados à cena por uma funcionalidade na tela têm o potencial de serem afetados pela gravidade. Quando um objeto sai da cena e antes de atingir a velocidade de escape, a gravidade o atrai para baixo, criando uma curva mais natural de trajetória à medida em que o objeto move.

Normalmente, a gravidade se manifesta quando um objeto deve saltar de uma cena para outra. Por isso a animação conectada usa o conceito de gravidade.

Aqui, um elemento na linha superior da grade é afetado pela gravidade, provocando sua queda à medida que deixa seu lugar e se move para a frente.

![Direção para trás e para dentro](images/continuity-photos.gif)

## <a name="related-articles"></a>Artigos relacionados

- [Visão geral do movimento](index.md)
- [Tempo e suavização](timing-and-easing.md)