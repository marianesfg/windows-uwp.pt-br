---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animações de composição
description: Muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b94f14b32c5dd74e0aefb9b9a99f64bbd905a05d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616701"
---
# <a name="composition-animations"></a>Animações de composição

As APIs Windows.UI.Composition permitem criar, animar, transformar e manipular os objetos do compositor em uma camada de API unificada. As animações de composição fornecem uma maneira avançada e eficiente de executar animações na interface do usuário do aplicativo. Elas foram projetadas desde o início para garantir que suas animações sejam executadas em 60 FPS independente do thread da interface do usuário e para oferecer a você a flexibilidade para criar experiências incríveis usando não só propriedades de tempo, mas também de entrada e outras propriedades, para ativar as animações.

## <a name="motion-in-windows"></a>Movimento no Windows

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essa sensação nos nossos projetos, levando as pessoas a fazer a transição de uma tarefa para a outra com facilidade cinematográfica. Movimento geralmente é o fator de diferenciação entre uma Interface do usuário e uma experiência de usuário.

Como um bloco de construção fundamental da plataforma de interface do usuário do Windows, CompositionAnimations fornecem uma maneira poderosa e eficiente para criar experiências de movimento na interface de usuário do seu aplicativo. O mecanismo de animação foi projetado desde o backup para garantir que sua animação seja executada em 60 FPS, independente do thread da interface do usuário. Essas animações são projetadas para fornecer a flexibilidade para criar experiências inovadoras de movimento com base no tempo, entrada e outras propriedades.

### <a name="examples-of-motion"></a>Exemplos de movimento

Aqui estão alguns exemplos de movimento em um aplicativo.

Aqui, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Um exemplo de animação conectada](images/animation/connected-animation-example.gif)

Aqui, um efeito de paralaxe visual move objetos diferentes em diferentes taxas quando a interface de usuário rola ou faz um movimento panorâmico para criar uma sensação de profundidade, perspectiva e movimento.

![Um exemplo de paralaxe com uma lista e uma imagem de fundo](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Usando CompositionAnimations para criar um movimento

Para gerar o movimento na interface do usuário, os desenvolvedores podem acessar as animações em XAML (link para Storyboards aqui), ou camada Visual. Animações na camada Visual oferecem aos desenvolvedores uma série de benefícios:

- Desempenho – em vez da animação de limite de Thread de interface do usuário tradicional, animações na plataforma da interface do usuário do Windows operam em um thread independente em 60 FPS, possibilitar experiências de movimento suave.
- Modelo de modelagem – animações na camada de interface do usuário do Windows são modelos, o significado pode usar uma única animação em vários objetos e ajustar as propriedades ou parâmetros sem se preocupar de obstruindo anterior usa.
- Personalização – a camada de interface do usuário do Windows não apenas torna mais fácil fazer a interface do usuário Linda, mas com uma gama completa de tipos de animação, o possível para criar novas e incríveis experiências com um gradiente de personalizações

Como um desenvolvedor de criação de experiências na camada de interface do usuário do Windows, você tem acesso a uma variedade de conceitos de animação para dar vida a seus designs. Você pode usar qualquer um desses conceitos para animar uma propriedade ou subchannel componente (quando aplicável) de qualquer CompositionObject.

> [!NOTE]
> Nem todas as propriedades de um CompositionObject sejam animáveis. Consulte a documentação do CompositionObject individual para determinar se uma propriedade é animável.

> [!NOTE]
> O termo _subchannel_ refere-se a um formulário de componente de uma propriedade. Por exemplo, X ou XY subchannel de uma propriedade de deslocamento Vector3.

| Conceito de animação | Descrição |
| ----------------- | ----------- |
| [Com base no tempo de animação com KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations são usados para controlar diretamente a totalidade de uma experiência de movimento em um período de tempo. Desenvolvedores que descrevem o início, final, a interpolação entre e duração de uma forma tradicional keyframed um movimento. |
| [Movimento relativo com ExpressionAnimations](relation-animations.md)  | ExpressionAnimations são usados para descrever como um movimento de uma propriedade de objeto deve ser orientado em relação à propriedade de outro objeto. Os desenvolvedores definem uma equação matemática que define a relação de referência. |
| ImplicitAnimations | Essas animações são baseados em gatilho e são definidas separadamente da lógica do aplicativo principal. ImplicitAnimations são usados para descrever como e quando animações ocorrem como uma resposta às alterações de propriedade direta. |
| [Animação controlada por entrada com animações de entrada](input-driven-animations.md)  | Animações de entrada aborda um conjunto de cenários que permitem aos desenvolvedores descrever o movimento com base em manipulação por meio de toque ou outras modalidades de entrada. Essas animações são conduzidas com base na entrada do usuário do Active Directory ou gestos. |
| [Movimento com base em física com NaturalMotionAnimations](natural-animations.md)  | NaturalMotionAnimations são usados para descrever o movimento natural e familiar, forçar o movimento orientado por experiências com base no mundo real. Em vez de definir o tempo, os desenvolvedores definem as características do movimento (por exemplo, amortecimento taxa para Springs) |