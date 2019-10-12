---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animações de composição
description: Muitas propriedades de objeto e efeito de composição podem ser animadas usando animações de quadro chave e expressão permitindo que as propriedades de um elemento de interface do usuário mudem ao longo do tempo ou com base em um cálculo.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5991b5f9f3c63a25a7476cc35621488c6cb60b2c
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72281828"
---
# <a name="composition-animations"></a>Animações de composição

As APIs Windows.UI.Composition permitem criar, animar, transformar e manipular os objetos do compositor em uma camada de API unificada. As animações de composição fornecem uma maneira avançada e eficiente de executar animações na interface do usuário do aplicativo. Elas foram projetadas desde o início para garantir que suas animações sejam executadas em 60 FPS independente do thread da interface do usuário e para oferecer a você a flexibilidade para criar experiências incríveis usando não só propriedades de tempo, mas também de entrada e outras propriedades, para ativar as animações.

## <a name="motion-in-windows"></a>Movimento no Windows

Pense no design de movimento como um filme. As transições perfeitas mantêm você focado na história e trazem vida às suas experiências. Podemos incorporar essa sensação nos nossos projetos, levando as pessoas a fazer a transição de uma tarefa para a outra com facilidade cinematográfica. O Motion é geralmente o fator de diferenciação entre uma interface do usuário e uma experiência do usuário.

Como um bloco de construção fundamental da plataforma de interface do usuário do Windows, o CompositionAnimations fornece uma maneira eficiente e eficaz de criar experiências de movimento na interface do usuário do seu aplicativo. O mecanismo de animação foi projetado desde o início para garantir que seu movimento seja executado às 60 FPS, independentemente do thread da interface do usuário. Essas animações são projetadas para fornecer a flexibilidade para criar experiências de movimento inovadoras com base no tempo, na entrada e em outras propriedades.

### <a name="examples-of-motion"></a>Exemplos de movimento

Aqui estão alguns exemplos de movimento em um aplicativo.

Aqui, um aplicativo usa uma animação conectada para animar uma imagem conforme ela "continua" a fazer parte do cabeçalho da próxima página. O efeito ajuda a manter o contexto de usuário entre a transição.

![Um exemplo de animação conectada](images/animation/connected-animation-example.gif)

Aqui, um efeito de paralaxe visual move objetos diferentes em diferentes taxas quando a interface de usuário rola ou faz um movimento panorâmico para criar uma sensação de profundidade, perspectiva e movimento.

![Um exemplo de paralaxe com uma lista e uma imagem de fundo](images/animation/parallax-example.gif)

## <a name="using-compositionanimations-to-create-motion"></a>Usando o CompositionAnimations para criar o Motion

Para gerar o Motion na interface do usuário, os desenvolvedores podem acessar animações em XAML ou na camada Visual. As animações na camada Visual fornecem aos desenvolvedores uma série de benefícios:

- Desempenho – em vez da animação tradicional associada a threads da interface do usuário, as animações na plataforma de interface do usuário do Windows operam em um thread independente a 60 FPS, permitindo experiências de movimento suave.
- Modelo de modelagem – as animações na camada de interface do usuário do Windows são modelos, o que significa que pode usar uma única animação em vários objetos e ajustar propriedades ou parâmetros sem se preocupar em obstruir os usos anteriores.
- Personalização – a camada de interface do usuário do Windows não apenas facilita a criação da interface de usuário bonita, mas com uma gama completa de tipos de animação, possível para criar experiências novas e incríveis com um gradiente de personalizações

Como desenvolvedor que cria experiências na camada de interface do usuário do Windows, você tem acesso a uma variedade de conceitos de animação para dar vida aos seus designs. Você pode usar qualquer um desses conceitos para animar um componente de propriedade ou de subcanal (quando aplicável) de qualquer composicionaobject.

> [!NOTE]
> Nem todas as propriedades de um compositionobject são animáveis. Consulte a documentação do composicionaobject individual para determinar se uma propriedade é animável.

> [!NOTE]
> O termo _subchannel_ refere-se a um formulário de componente de uma propriedade. Por exemplo, o subcanal X ou XY de uma propriedade offset Vector3.

| Conceito de animação | Descrição |
| ----------------- | ----------- |
| [Movimento com base no tempo com KeyFrameAnimations](time-animations.md)  | KeyFrameAnimations são usados para controlar diretamente a totalidade de uma experiência de movimento em um período de tempo. Os desenvolvedores que descrevem o início, o fim, a interpolação entre e a duração de um movimento em um modo tradicional de quadro. |
| [Movimento relativo com ExpressionAnimations](relation-animations.md)  | ExpressionAnimations são usados para descrever como um movimento da propriedade de um objeto deve ser orientado em relação à propriedade de outro objeto. Os desenvolvedores definem uma equação matemática que define a relação baseada em referência. |
| ImplicitAnimations | Essas animações são baseadas em gatilho e são definidas separadamente da lógica do aplicativo principal. ImplicitAnimations são usados para descrever como e quando as animações ocorrem como uma resposta às alterações de propriedade direta. |
| [Movimento controlado por entrada com animações de entrada](input-driven-animations.md)  | As animações de entrada abrangem um conjunto de cenários que permitem aos desenvolvedores descrever o movimento com base na manipulação por toque ou outras modalidades de entrada. Essas animações são orientadas com base na entrada ou gestos ativos do usuário. |
| [Movimento baseado em física com NaturalMotionAnimations](natural-animations.md)  | Os NaturalMotionAnimations são usados para descrever experiências de movimento naturais e familiares com base em um movimento orientado ao mundo real. Em vez de definir a hora, os desenvolvedores definem características do movimento (por exemplo, taxa de amortecimento para molas) |