---
author: jwmsft
title: Animações de movimento natural
description: Saiba mais sobre animações de movimento natural e como usá-las na interface do usuário do app.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6205897"
---
# <a name="natural-motion-animations"></a>Animações de movimento natural

Este artigo fornece uma breve visão geral do espaço NaturalMotionAnimation e como pensar conceitualmente no uso desses tipos de animações na interface do usuário.

## <a name="making-motion-feel-familiar-and-natural"></a>Tornando o movimento familiar e natural

Apps excelentes são aqueles que proporcionam experiências que capturam e retêm a atenção do usuário e ajudam a conduzi-los pelas tarefas. O movimento é o fator chave que distingue uma interface do usuário de uma experiência do usuário, provocando uma conexão entre usuários e o aplicativo com o qual eles estão interagindo. Quanto melhor a conexão, maior o envolvimento e a satisfação dos usuários finais.

Uma maneira de o movimento ajudar a estabelecer essa conexão é por meio de experiências que pareçam familiar para os usuários. Os usuários têm uma expectativa inconsciente de como percebem o movimento baseado em experiências da vida real. Vemos como os objetos deslizam pelo chão, caem da mesa, saltam em direção a outro e oscilam com uma mola. O movimento que aproveita essa expectativa baseado na física do mundo real parece mais natural aos nossos olhos. O movimento torna-se mais natural e interativo, mas, o mais importante é que toda a experiência se torna mais fácil de memorizar e divertida.

![Movimento de escala sem animação](images/animation/scale-no-animation.gif)
![Movimento de escala com Bézier cúbico](images/animation/scale-cubic-bezier.gif)
![Movimento de escala com animação de mola](images/animation/scale-spring.gif)

O resultado é o maior envolvimento e retenção do usuário com o app.

## <a name="balancing-control-and-dynamism"></a>Balanceando controle e dinamismo

Na interface do usuário tradicional, os [KeyFrameAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation)s são a forma predominante de descrever o movimento. Os KeyFrames forneceram o máximo controle para os designers e desenvolvedores definirem o início, o fim e a interpolação. Embora isso seja muito útil em vários casos, as animações de quadro chave não são muito dinâmicas; o movimento não é adaptável e parece o mesmo em qualquer condição.

Na outra extremidade do espectro, há simulações frequentemente vistas em mecanismos de jogos e de física. Essas experiências são geralmente as mais realistas e dinâmicas com as quais os usuários interagem, conferindo a noção de ambiente e aleatoriedade que eles veem todos os dias. Embora isso torne o movimento mais dinâmico, os designers e os desenvolvedores têm menos controle, o que dificulta mais a integração à interface do usuário tradicional.

![Diagrama do espectro de controle](images/animation/natural-motion-diagram.png)

Os [NaturalMotionAnimation](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation)s existem para ajudar a diminuir esse distanciamento, permitindo o controle dos elementos importantes de uma animação, como início/término, mas mantendo o movimento que parece natural e dinâmico.

> [!NOTE]
> Os NaturalMotionAnimations não funcionam como um substituto das animações de quadro chave; ainda há locais na linguagem do Fluent Design em que os KeyFrames são recomendados. Os NaturalMotionAnimations devem ser usados em locais nos quais o movimento é necessário, mas as animações de quadro chave não são suficientemente dinâmicas.

## <a name="using-naturalmotionanimations"></a>Usando o NaturalMotionAnimations

A partir do Fall Creators Update, você tem acesso a uma nova experiência de movimento: as **animações de mola**. Consulte [Animações de mola](spring-animations.md) para obter um passo a passo mais aprofundado sobre as molas.

Esse tipo de movimento é obtido através do novo NaturalMotionAnimation, um novo tipo de animação que permite aos desenvolvedores criar um movimento mais familiar e natural na interface do usuário, com um equilíbrio entre controle e dinamismo. Eles expõem os seguintes recursos:

- Defina os valores de início e término.
- Defina InitialVelocity e associe-se à entrada com o InteractionTracker.
- Definas as propriedades específicas de movimento (como DampingRatio para molas).

Fórmula geral para começar:

1. Crie o NaturalMotionAnimation a partir do Compositor usando um dos métodos **Create**.
1. Defina as propriedades da animação.
1. Passe o NaturalMotionAnimation como um parâmetro para a chamada StartAnimation de um CompositionObject.
    - Ou defina a propriedade Motion de um InertiaModifier do InteractionTrackerr.

Um exemplo básico usando um NaturalMotionAnimation de mola para criar uma "mola" visual em um novo local de deslocamento X:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
