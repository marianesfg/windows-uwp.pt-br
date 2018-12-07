---
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: Movimento e animação em aplicativos UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2701844ccefdc5a535fa8fc20086c550cb7bc29e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8781316"
---
# <a name="motion-for-uwp-apps"></a>Movimento para aplicativos UWP

![imagem hero](images/header-motion2.svg)

O movimento fluente tem um propósito em seu aplicativo. Ele fornece um comentários inteligentes com base no comportamento do usuário, mantém a interface do usuário ativa e guia a navegação do usuário no aplicativo. O movimento fluente gera uma conexão emocional entre um usuário e sua experiência digital. Aproveitamos uma base de movimento natural que o usuário já entende do mundo físico e ampliamos nosso sistema a partir daí.

## <a name="fluent-motion-principles"></a>Princípios de movimento fluente

### <a name="physical"></a>Físico

Objetos em movimento apresentam comportamentos de objetos no mundo real. Um movimento fluente e dinâmico faz com que a experiência seja natural, criando conexões emocionais e adicionando personalidade.

![Exemplo de interface do usuário de movimento físico](images/Physical.gif)
> Quando você interage com a interface do usuário por meio de toque, o movimento da interface está diretamente relacionado à velocidade da interação. Como o toque é a manipulação direta, o objeto com o qual você interage afeta os objetos ao redor.

### <a name="functional"></a>Funcional

O movimento tem um propósito e uma convicção. Ele orienta o usuário pela complexidade e ajuda a estabelece a hierarquia. O movimento oferece a impressão de desempenho aprimorado e otimiza a experiência do usuário, ocultando a latência percebida.

![Exemplo de interface do usuário de movimento funcional](images/functional.gif)
> As transições de página têm finalidade específica. Elas fornecem dicas sobre como as páginas são relacionadas entre si. Elas se movem de forma considerada rápida até mesmo quando o desempenho não é ideal.

### <a name="continuous"></a>Contínuo

O movimento fluente de um ponto a outro naturalmente atrai a atenção e orienta o usuário. Com elegância, ele une uma tarefa do usuário, fazendo parecer mais amigável e consumível.

![Exemplo de interface do usuário de movimento contínuo](images/continuous3.gif)
> Objetos podem deslocar de cena em cena ou se transformar dentro de uma cena para fornecer continuidade e ajudar o usuário a manter o contexto.

### <a name="contextual"></a>Contextual

Movimento inteligente fornece comentários ao usuário de forma alinhada com a manipulação da interface do usuário. A interação está centralizada no usuário. O movimento parece apropriado ao fator forma e é projetado tendo em vista o cenário. Deve ser confortável para cada usuário.

![Exemplo de interface do usuário de movimento contextual](images/Contextual.gif)
> A animação deve se relacionar à interação do usuário. Um menu de contexto é implantado a partir do ponto em que o usuário o ativa. 

## <a name="motion-articles"></a>Artigos sobre movimento

:::row:::
    :::column:::
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
