---
Description: Saiba como o Motion Fluent usa direcionalidade e gravidade.
title: Direcionalidade e gravidade – animação em aplicativos do Windows
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
ms.openlocfilehash: ddcfac5e36500a8fc6dc41c7c86037f5a1483203
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970641"
---
# <a name="directionality-and-gravity"></a>Direcionalidade e gravidade

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
A direção da movimentação corresponde ao movimento físico. Assim como na natureza, os objetos podem ser mover em qualquer eixo: X,Y e Z. Isso é como consideramos o movimento dos objetos na tela.
Ao mover objetos, evite colisões não naturais. Lembre-se de onde os objetos vêm e vão e sempre dão suporte a constructos de nível superior que podem ser usados na cena, como direção de rolagem ou hierarquia de layout.
    :::column-end:::
    :::column:::
        ![Direção para trás e para dentro](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Direção da navegação

A direção da navegação entre as cenas em seu aplicativo é conceitual. Os usuários navegam para frente e para trás. Cenas movem se movem para dentro e para fora com campo de visão. Esses conceitos combinam a movimentação física para orientar o usuário.

Quando a navegação provoca o deslocamento de um objeto da cena anterior para a nova, o objeto faz um simples movimento de A até B na tela. Para garantir que o movimento pareça mais físico, o padrão de suavização é adicionado, bem como a sensação de gravidade.

Para a navegação regressiva, o movimento é invertido (de B para A). Quando os usuários navegam de volta, eles esperam retornar para o estado anterior assim que possível. O tempo é mais rápido, mais direto e usa a suavização por desaceleração.

Aqui, esses princípios são aplicados à medida que o item selecionado permanece na tela durante a navegação para frente e para trás.

![Exemplo de interface do usuário de movimento contínuo](images/continuous3.gif)

Quando a navegação provoca a substituição de itens na tela, é importante mostrar para onde a cena de saída foi e de onde a nova cena vem.

Isso tem várias vantagens:

- Solidifica o modelo mental do usuário do espaço.
- A duração da cena existente fornece mais tempo para preparar o conteúdo que será animada da cena de entrada.
- Melhora o desempenho percebido do aplicativo.

Existem quatro direções discretas de navegação para considerar.

:::row:::
    :::column:::
**Encaminhar** Comemora o conteúdo entrando na cena de maneira que não colide com o conteúdo de saída. O conteúdo é desacelerado na cena.
    :::column-end:::
    :::column:::
        ![Avançar direção](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Encaminhamento** O conteúdo é fechado rapidamente. Objetos aceleram a tela.
    :::column-end:::
    :::column:::
        ![encaminhamento de direção](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Voltar** O mesmo que encaminhar, mas invertido.
    :::column-end:::
    :::column:::
        ![Direção para trás e para dentro](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Saída para trás** O mesmo que o encaminhamento, mas invertido.
    :::column-end:::
    :::column:::
        ![direção para fora](images/backwardOUT.gif)
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
