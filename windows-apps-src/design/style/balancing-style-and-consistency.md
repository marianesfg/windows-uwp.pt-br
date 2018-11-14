---
author: mijacobs
description: Dicas para criação de aplicativos consistentes e utilizáveis, que também demonstram originalidade e criatividade.
title: Equilibrando estilo e consistência (UWP para design de aplicativo)
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c6344f6737e9628961393eb1e3080daf31740537
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6655132"
---
# <a name="balancing-style-and-consistency"></a>Equilibrando estilo e consistência

 

> Observação: Este artigo é um rascunho inicial para Windows 10 RS2. Os nomes, a terminologia e a funcionalidade dos recursos não são finais.

Ao projetar um produto, você é o porta-voz do consumidor. Todos nos esforçamos para criar o melhor design, apropriado e fiel à nossa intenção. Este artigo explora o equilíbrio entre seguir convenções para a criação de uma experiência de usuário consistente, versus a criação de recursos e experiências únicas que diferenciam seu aplicativo. 

 
## <a name="the-importance-of-consistency"></a>A importância da consistência
Por que a consistência é importante? A consistência pode tornar um aplicativo mais fácil de usar. Uma parte fundamental do bom design é a capacidade de aprendizado; possuir um design consistente entre os aplicativos reduz o número de vezes em que o usuário deve "re-aprender" a utilizá-lo. Veja os exemplos do mundo real: um pedal de acelerador está sempre do lado direito, a maçaneta de uma porta sempre gira no sentido anti-horário para abrir, o sinal de pare é sempre vermelho. 

Fazer com que um aplicativo seja consistente não significa, no entanto, fazer com que todos os aplicativos se pareçam e se comportem de maneira idêntica. Voltando aos exemplos do mundo real, existe quase um número infinito de designs de cadeiras e, ainda assim, quase nenhum exige um reaprendizado sobre como se sentar. Isso acontece pois os elementos importantes--a superfície para sentar--são suficientemente consistentes para que o usuário os entenda. 

Um dos desafios na criação de um bom design de aplicativo é entender onde é importante ser consistente, e onde é importante ser original. 

## <a name="the-consistency-spectrum"></a>O espectro da consistência
 É de grande ajuda pensar a consistência como um espectro com duas pontas diferentes:


![O espectro da consistência](images/consistency/consistency-spectrum.png)

Elementos de experiências familiares incluem:
-   Paradigmas da interface de usuário estabelecidos (comportamento em um clique do mouse, pressionar e segurar um elemento, ícones que parecem semelhantes)
-   Elementos da sua marca que deseja aplicar aos produtos (tipografia, cores)

Os diferenciadores incluem:
-   Elementos que formam a "alma" original dos produtos
-   Elementos que ajudam a adaptar a experiência ao fator de forma pretendido

Vamos criar um modelo de design tomando esse espectro e aplicando-o aos elementos principais de um aplicativo. 

![O modelo de consistência de design](images/consistency/design-consistency-model.png)

Neste modelo, as camadas base fornecem um pilar de consistência testado e aprovado enquanto as camadas superiores se concentram na flexibilidade e na personalização.  

1. As bases do aplicativo compõem a primeira camada do nosso modelo: a grade de layout, [paleta de cores](color.md), [convenções de tipografia](typography.md), e [estilos de ícone](icons.md). Esses recursos fundamentais devem ser consistentemente aplicados. 

2. Para a segunda camada, a UWP oferece um conjunto básico de [controles comuns](../controls-and-patterns/index.md) que equilibra eficiência com flexibilidade. Também criamos diretrizes para ajudar a estabelecer uma voz e um tom consistentes, usando as mesmas palavras para descrever e orientar pessoas através de experiências de aplicativos. Criamos um conjunto de padrões que podem ser usados para designs de aplicativos, visando garantir que nossos designs possam ser dimensionados em dispositivos e entradas de diferentes tamanhos. 
3. A camada três é onde você adapta a navegação a diferentes dispositivos e contextos. Por exemplo, como você navega com toque em um telefone provavelmente será diferente de um monitor de 32" com teclado e mouse ou um hololens com gestos e 100 pontos de toque em uma superfície de 84".
4. A camada quatro é onde você define a personalidade da sua marca. Que elementos de design exclusivos reforçam sua marca e diferenciam-na da concorrência? Também é aqui que você adapta seu aplicativo para diferentes usuários finais. O seu aplicativo é para jogadores, para profissionais da informação ou estudantes e educadores de nível primário? Quais são as necessidades únicas desses diferentes consumidores, podemos fazer o design funcionar melhor para eles? Não basta fazer o mesmo, continue buscando criar mais valor para seus diferentes consumidores.  


## <a name="design-principles"></a>Princípios do design
Para utilizar esse modelo efetivamente, precisamos de um conjunto de princípios de design que nos ajude a realizar as trocas corretas. Aqui estão nossos princípios de trabalho de consistência de design:

**Se parecer o mesmo, faça com que ele seja o mesmo**
-   Quando o usuário visualiza uma caixa de texto, ou o controle de hambúrguer, ele provavelmente espera que o comportamento seja o mesmo em dispositivos diferentes. Se você tiver uma boa razão para se desviar de um comportamento estabelecido, defina as expectativas dos usuários ao fazer com que ele se pareça diferente.

**Se um elemento se parece muito com outro elemento ou convenção existente, considere fazê-lo igual**
-   Você precisa de um ícone de "novo documento". Por que conceber um novo que é apenas um pouco diferente, quando você pode usar um que já foi reconhecido pelo usuário.

**Usabilidade supera consistência**
-   É melhor ser utilizável do que consistente. Em alguns casos, você pode precisar desenvolver novos controles ou comportamentos para auxiliar a usabilidade. Utilizar o telefone com apenas uma mão traz desafios únicos. Assim como trabalhar com uma tela de 80". Um bom design faz com que o usuário se sinta um especialista. 

**O envolvimento é importante**
-   Não deixe ele monótono. Se tudo é plano, com a mesma cor, cheio de quadrados, será que nossos consumidores irão querer pegar e usar? Crie satisfação. Introduza novos elementos que surpreendam sem comprometer a capacidade de aprendizado. 

**Comportamentos evoluem**
-   Essa é a parte complicada: à medida que a indústria evolui, novas convenções são estabelecidas. É possível que comportamentos atuais desapareçam, e nossos comportamentos consistentes precisarem se adaptar a novos padrões. Veja a ação apertar e fazer zoom. Era de costume prever a aplicação de zoom através dos +/- da interface de usuário, no entanto, na interface de usuário moderna espera-se o zoom através da pinça. Observe os paradigmas de novas experiências, e evolua. 
