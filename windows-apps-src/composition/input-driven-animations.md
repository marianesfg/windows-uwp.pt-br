---
title: Animações controladas por entrada
description: Saiba como usar animações de entrada na interface do usuário do app.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: 94d15fc7f2443475020aa7e134c076b833db46a8
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8743829"
---
# <a name="input-driven-animations"></a>Animações controladas por entrada

Este artigo fornece uma introdução à API de InputAnimation e recomenda como usar esses tipos de animações em sua interface do usuário.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações baseadas em relações](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Movimento suave controlado pelas interações do usuário

Na linguagem do Fluent Design, a interação entre usuários finais e apps é de extrema importância. Os apps não só precisam visualizar a parte, mas também responder de forma natural e dinâmica aos usuários que interagem com eles. Isso significa que, quando um dedo toca a tela, a interface do usuário deve reagir normalmente aos novos graus de entrada; a rolagem deve ser suave enquanto o dedo faz a panorâmica na tela.

Criar uma interface do usuário que responda de forma dinâmica e fluida à entrada do usuário resultará no maior envolvimento do usuário; o movimento agora não só parece bom, mas é bom e natural quando os usuários interagem com suas diferentes experiências de interface do usuário. Isso faz com que os usuários finais se conectem ao seu aplicativo com mais facilidade, tornando a experiência mais divertida e fácil de memorizar.

## <a name="expanding-past-just-touch"></a>Transcendendo o simples toque

Embora o toque seja uma das formas mais comuns de entrada utilizada pelos usuários finais para manipular o conteúdo da interface do usuário, eles também usarão várias outras modalidades, como mouse e caneta. Nesses casos, é importante que os usuários finais percebam que a interface do usuário responde dinamicamente à sua entrada, independentemente da modalidade de entrada que escolham. Você deve estar ciente das diferentes modalidades de entrada ao projetar experiências de movimento controladas por entrada.

## <a name="different-input-driven-motion-experiences"></a>Diferentes experiências de movimento controladas por entrada

O espaço InputAnimation fornece várias experiências diferentes para você criar um movimento de resposta dinâmica. Assim como o restante do sistema de animação de interface do usuário do Windows, essas animações controladas por entrada operam em um thread independente, favorecendo assim a experiência de movimento dinâmico. No entanto, em alguns casos nos quais a experiência aproveita os controles e componentes XAML existentes, partes dessas experiências ainda estão ligadas ao thread da interface do usuário.

Existem três experiências principais que você cria ao criar animações dinâmicas de movimento orientadas por entrada:

1. Aprimorando as experiências de ScrollView existentes - habilite a posição de um ScrollViewer XMLL para gerar experiências dinâmicas de animação.
1. Experiências do ponteiro controladas por posição - utilize a posição de um cursor em um UIElement que passou pelo teste de acertos para obter experiências dinâmicas de animação.
1. Experiências de manipulação personalizadas com o InteractionTracker - crie experiências de manipulação totalmente personalizadas e fora do thread com o InteractionTracker (como uma tela de rolagem/zoom).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Aprimorando as experiências de ScrollViewer existentes

Uma das formas comuns de gerar experiências mais dinâmicas é compilar sobre um controle ScrollViewer XAML existente. Nessas situações, você aproveita a posição de rolagem de um ScrollViewer para criar componentes adicionais de interface do usuário que tornam uma experiência de rolagem simples mais dinâmicas. Alguns exemplos incluem cabeçalhos fixos/tímidos e paralaxe.

![Exibição de lista com paralaxe](images/animation/parallax.gif)

![Um cabeçalho tímido](images/animation/shy-header.gif)

Ao criar esses tipos de experiências, há uma fórmula geral a ser seguida:

1. Acesse o ScrollManipulationPropertySet no ScrollViewer XAML, você deseja dirigir uma animação.
    - Feito por meio da API ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element)
    - Retorna um CompositionPropertySet contendo uma propriedade chamada **Translation**
1. Crie um Composition ExpressionAnimation com uma equação que faça referência à propriedade Translation.
1. Inicie a animação em uma propriedade de CompositionObject.

Para obter mais informações sobre como criar essas experiências, consulte [Aprimorar as experiências de ScrollViewer existentes](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Experiências de ponteiro controladas por posição

Outra experiência dinâmica comum que envolve entrada é criar uma animação com base na posição de um ponteiro, como um cursor de mouse. Nessas situações, os desenvolvedores aproveitam o local de um cursor quando ele passa pelo teste de acertos em um UIElement XAML, que possibilita a criação de experiências como o Spotlight Reveal.

![Exemplo de destaque do ponteiro](images/animation/spotlight-reveal.gif)

![Exemplo de rotação do ponteiro](images/animation/pointer-rotate.gif)

Ao criar esses tipos de experiências, há uma fórmula geral a ser seguida:

1. Acesse o PointerPositionPropertySet em um UIElement XAML cuja posição de cursor você deseja saber quando ela passa pelo teste de acertos.
    - Feito através da API ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element)
    - Retorna um CompositionPropertySet contendo uma propriedade chamada **Position**
1. Crie uma CompositionExpressionAnimation com uma equação que faça referência à propriedade Position.
1. Inicie a animação em uma propriedade de CompositionObject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiências personalizadas de manipulação com o InteractionTracker

Um dos desafios do uso de um ScrollViewer XAML é que ele está vinculado ao thread da interface do usuário. Consequentemente, a experiência de rolagem e de zoom geralmente pode atrasar e tremular se o thread da interface do usuário ficar ocupado e resultar em uma experiência pouco atrativa. Além disso, não é possível personalizar vários aspectos da experiência do ScrollViewer. O InteractionTracker foi criado para resolver ambos os problemas, fornecendo um conjunto de blocos de construção capaz de criar experiências de manipulação personalizadas que são executadas em um thread independente.

![Exemplo de interações 3D](images/animation/interactions-3d.gif)

![Exemplo de puxar para animar](images/animation/pull-to-animate.gif)

Ao criar experiências com o InteractionTracker, há uma fórmula geral a ser seguida:

1. Crie o objeto InteractionTracker e defina suas propriedades.
1. Crie VisualInteractionSources para qualquer CompositionVisual que deva capturar entrada para o InteractionTracker consumir.
1. Crie um Composition ExpressionAnimation com uma equação que faça referência à propriedade Position do InteractionTracker.
1. Inicie a animação em uma propriedade de CompositionVisual que deva ser acionada pelo InteractionTracker.
