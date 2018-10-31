---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animações de composição
description: Muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38f9d0daf230007d1d32a7d2187d54baa90986e5
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830543"
---
# <a name="composition-animations"></a>Animações de composição

As APIs Windows.UI.Composition permitem criar, animar, transformar e manipular os objetos do compositor em uma camada de API unificada. As animações de composição fornecem uma maneira avançada e eficiente de executar animações na interface do usuário do aplicativo. Elas foram projetadas desde o início para garantir que suas animações sejam executadas em 60 FPS independente do thread da interface do usuário e para oferecer a você a flexibilidade para criar experiências incríveis usando não só propriedades de tempo, mas também de entrada e outras propriedades, para ativar as animações.

## <a name="motion-in-windows"></a>Movimento no Windows

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essa sensação nos nossos projetos, levando as pessoas de uma tarefa para a próxima com facilidade cinematográfica. O movimento é geralmente o fator de diferenciação entre uma Interface do usuário e uma experiência do usuário.

Como um componente fundamental da plataforma da interface do usuário do Windows, CompositionAnimations fornecem uma maneira avançada e eficiente para criar experiências de movimento na interface do usuário do seu aplicativo. O mecanismo de animação foi projetado desde o início para cima para garantir que sua animação seja executada em 60 FPS independente do thread da interface do usuário. Essas animações são projetadas para oferecer a flexibilidade para criar experiências de movimento inovadora com base no tempo, entrada e outras propriedades.

### <a name="examples-of-motion"></a>Exemplos de movimento

Aqui estão alguns exemplos de movimento em um aplicativo.

Aqui, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Um exemplo de animação conectada](images/animation/connected-animation-example.gif)

Aqui, um efeito de paralaxe visual move objetos diferentes em diferentes taxas quando a interface de usuário rola ou faz um movimento panorâmico para criar uma sensação de profundidade, perspectiva e movimento.

![Um exemplo de paralaxe com uma imagem de fundo e uma lista](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Usando o CompositionAnimations para criar um movimento

Para gerar movimento na interface do usuário, os desenvolvedores podem acessar animações em XAML (link para Storyboards aqui) ou a camada Visual. As animações na camada Visual fornecem os desenvolvedores com uma série de benefícios:

- Desempenho – em vez de animação associada ao Thread de interface do usuário tradicional, animações na plataforma Windows da interface do usuário operam em um thread independente a 60 FPS, habilitar experiências de movimento suave.
- Modelo de modelagem – animações na camada da interface do usuário do Windows são modelos, o significado pode usar uma única animação em vários objetos e ajuste as propriedades ou parâmetros sem se preocupar em obstruir anterior usa.
- Personalização – a camada de interface do usuário do Windows não apenas torna mais fácil fazer belo interface do usuário, mas com uma ampla variedade de tipos de animação, possível criar novos e incríveis experiências com um gradiente de personalizações

Como um desenvolvedor de criação de experiências na camada da interface do usuário do Windows, você tem acesso a uma variedade de conceitos de animação para dar vida a seus designs. Você pode usar qualquer um desses conceitos para animar uma propriedade ou subchannel componente (quando aplicável) de qualquer CompositionObject.

> [!NOTE]
> Nem todas as propriedades de um CompositionObject sejam animáveis. Consulte a documentação de CompositionObject individual para determinar se uma propriedade é que podem ser animada.

> [!NOTE]
> O termo _subcanal_ se refere a uma forma de componente de uma propriedade. Por exemplo, X ou XY subchannel de uma propriedade deslocamento Vector3.

| Conceito de animação | Descrição |
| ----------------- | ----------- |
| [Movimento baseadas em tempo com KeyFrameAnimations](time-animations.md)  | Os KeyFrameAnimations são usados para controlar diretamente na íntegra uma experiência de movimento durante um período de tempo. Desenvolvedores descrevendo um movimento inicial, final, interpolação entre e duração de maneira keyframed tradicional. |
| [Movimento relativo com os ExpressionAnimations](relation-animations.md)  | Os ExpressionAnimations são usados para descrever como um movimento de uma propriedade de objeto deve ser orientado em relação à propriedade do outro objeto. Os desenvolvedores definem uma equação matemática que define a relação de referência. |
| ImplicitAnimations | Essas animações são baseados em gatilhos e são definidas separadamente da lógica principal do aplicativo. ImplicitAnimations são usados para descrever como e quando as animações ocorrem como uma resposta às alterações de propriedade direta. |
| [Movimento controladas por entrada com animações de entrada](input-driven-animations.md)  | Animações de entrada abrange um conjunto de cenários que permitem que os desenvolvedores de descrever o movimento com base em manipulação por meio de toque ou outras modalidades de entrada. Essas animações são orientadas com base na entrada do usuário ativo ou gestos. |
| [Movimento baseados na física com os NaturalMotionAnimations](natural-animations.md)  | Os NaturalMotionAnimations são usados para descrever o movimento natural e familiar movimento controladas por forçar a experiências com base no mundo real. Em vez de definir o tempo, os desenvolvedores definir características do movimento (por exemplo, damping proporção para molas) |