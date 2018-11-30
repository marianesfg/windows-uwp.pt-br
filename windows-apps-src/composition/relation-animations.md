---
title: Animações baseadas em relações
description: Crie um movimento com base em uma propriedade em outro objeto.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: b6fdc59e8a7203a3bb8c6ad79adabd446b884639
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8352420"
---
# <a name="relation-based-animations"></a>Animações baseadas em relações

Este artigo fornece uma breve visão geral de como fazer animações baseadas em relações usando ExpressionAnimations de composição.

## <a name="dynamic-relation-based-experiences"></a>Experiências dinâmicas baseadas em relações

Durante a criação de experiências de movimento em um app, há momentos em que o movimento não é baseado em tempo, mas depende de uma propriedade em outro objeto. As KeyFrameAnimations não são capazes de expressar facilmente esses tipos de experiências de movimento. Nesses casos específicos, o movimento não precisa mais ser discreto e predefinido. Em vez disso, o movimento pode se adaptar dinamicamente com base no seu relacionamento com outras propriedades de objeto. Por exemplo, você pode animar a opacidade de um objeto com base na sua posição horizontal. Outros exemplos incluem experiências de movimento como cabeçalhos fixos e paralaxe.

Esses tipos de experiências de movimento permitem que você crie uma interface do usuário mais conectada, e não singular e independente. O usuário tem a impressão de uma experiência de interface do usuário dinâmica.

![Círculo em órbita](images/animation/orbit.gif)

![Exibição de lista com paralaxe](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Usando os ExpressionAnimations

Para criar experiências de movimento baseadas em relações, use o tipo ExpressionAnimation. Os ExpressionAnimations (ou simplesmente Expressions) são um novo tipo de animação que permite expressar um relacionamento matemático, relacionamento que o sistema usa para calcular o valor de uma propriedade de animação a cada quadro. Em outras palavras, os Expressions são simplesmente uma equação matemática que define o valor desejado de uma propriedade de animação por quadro. Os Expressions são um componente muito versátil que pode ser usado em uma ampla variedade de cenários, incluindo:

- Tamanho relativo, animações de deslocamento.
- Cabeçalhos fixos, paralaxe com ScrollViewer. (Consulte [Aprimorar as experiências existentes do ScrollViewer](scroll-input-animations.md).)
- Pontos de ajuste com InertiaModifiers e InteractionTracker. (Consulte [Criar pontos de ajuste com modificadores de inércia](inertia-modifiers.md).)

Ao trabalhar com os ExpressionAnimations, vale a pena mencionar o seguinte antecipadamente:

- Nunca termina – diferente do seu irmão KeyFrameAnimation, os Expressions não têm duração finita. Como os Expressions são relacionamentos matemáticos, essas animações estão constantemente "em execução". Você poderá interrompê-las se preferir.
- Em execução, mas nem sempre em avaliação – o desempenho sempre é uma preocupação nas animações em execução constante. No entanto, não é preciso se preocupar, o sistema é inteligente o suficiente para que a expressão só seja avaliada novamente se uma de suas entradas ou parâmetros for alterada.
- Retornando o tipo de objeto certo – Como os Expressions são relacionamentos matemáticos, é importante garantir que a equação que define a expressão retorne o mesmo tipo da propriedade pretendida pela animação. Por exemplo, se o deslocamento for animado, a expressão retornará o tipo Vector3.

### <a name="components-of-an-expression"></a>Componentes de uma expressão

Ao criar o relacionamento matemático de uma expressão, há vários componentes principais:

- Parâmetros – representam valores de constante ou referências a outros objetos de composição.
- Operadores matemáticos – os operadores matemáticos típico plus(+), minus(-), multiply(*), divide(/) que unem os parâmetros para formar uma equação. Também estão incluídos operadores condicionais, como maior que(>), igual a(==), operador ternário (condition ? ifTrue: ifFalse) etc.
- Funções matemáticas – funções matemáticos/atalhos com base em System.Numerics. Para obter uma lista completa de funções com suporte, consulte [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Os Expressions também oferecem suporte a um conjunto de palavras-chave – frases especiais inteligíveis apenas no sistema ExpressionAnimation. Esses recursos são listados (juntamente com a lista completa de funções matemáticas) na documentação do [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

### <a name="creating-expressions-with-expressionbuilder"></a>Criando expressões com o ExpressionBuilder

Há duas opções para a criação de expressões no aplicativo UWP:

1. Criando a equação como uma cadeia de caracteres por meio da API pública oficial.
1. Criando a equação em um modelo de objeto de tipo seguro por meio da ferramenta ExpressionBuilder de código-fonte aberto. Consulte [Código-fonte e documentação do Github](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder).

Para os fins deste documento, definiremos nossas expressões usando o ExpressionBuilder.

### <a name="parameters"></a>Parâmetros

Os parâmetros formam o núcleo de uma expressão. Existem dois tipos de parâmetros:

- Constantes: são parâmetros que representam a variáveis tipadas do System.Numeric. Esses parâmetros obtêm os valores atribuídos uma vez quando a animação é iniciada.
- Referências: são parâmetros que representam as referências aos CompositionObjects; atualizam continuamente os valores depois que uma animação é iniciada.

Em geral, as referências são o principal aspecto de como a saída de uma expressão pode ser alterada dinamicamente. Como essas referências mudam, a saída da expressão também muda. Se você criar a expressão com cadeias de caracteres ou usá-las em um cenário de modelagem (usando a expressão para direcionar vários CompositionObjects), precisará nomear e definir os valores dos parâmetros. Consulte a seção Exemplo para obter mais informações.

### <a name="working-with-keyframeanimations"></a>Trabalhando com KeyFrameAnimations

As expressões também podem ser usadas com KeyFrameAnimations. Nesses casos, você quer usar uma expressão para definir o valor de um KeyFrame em um momento específico; esses tipos de KeyFrames são chamados ExpressionKeyFrames.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

No entanto, diferente dos ExpressionAnimations, os ExpressionKeyFrames são avaliados apenas uma vez, quando o KeyFrameAnimation é iniciado. Tenha em mente, você não transmite um ExpressionAnimation como o valor do KeyFrame, mas como uma cadeia de caracteres (ou um ExpressionNode, se você estiver usando o ExpressionBuilder).

## <a name="example"></a>Exemplo

Vamos percorrer agora um exemplo de uso de expressões, especificamente o exemplo de PropertySet da galeria de exemplos de interface do usuário do Windows. Vamos analisar a expressão gerenciando o comportamento de movimento de órbita da bola azul.

![Círculo em órbita](images/animation/orbit.gif)

Três componentes se mantêm em execução para a experiência total:

1. Um KeyFrameAnimation, animando o deslocamento Y da bola vermelha.
1. Um PropertySet com a propriedade **Rotation** que ajuda a impulsionar a órbita, animada por outro KeyFrameAnimation.
1. Um ExpressionAnimation que impulsiona o deslocamento da bola azul referenciando o deslocamento da bola vermelha e a propriedade Rotation para manter uma órbita perfeita.

Abordaremos o ExpressionAnimation definido no N° 3. Usaremos também as classes do ExpressionBuilder para construir essa expressão. Uma cópia do código usado para criar essa experiência por meio de cadeias de caracteres é listada no final.

Nesta equação, há duas propriedades do PropertySet que você precisa referenciar; uma é um deslocamento de ponto central e a outra é a rotação.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

Em seguida, você precisará definir o componente Vector3 que considera a rotação em órbita real.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` é um atalho que "usa" a notação para definir ExpressionBuilder.ExpressionFunctions.

Por fim, combine esses componentes e referencie a posição da bola vermelha para definir o relacionamento matemático.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

Em uma situação hipotética, e se você quisesse usar essa mesma expressão, mas com dois outros elementos visuais, o que significa dois conjuntos de círculos em órbita. Com o CompositionAnimations, você pode reutilizar a animação e direcionar vários CompositionObjects. A única coisa que você precisa alterar quando usa essa expressão no caso da órbita adicional é a referência ao elemento Visual. Chamamos isso de modelagem.

Nesse caso, você pode modificar a expressão que criou anteriormente. Em vez de "obter" uma referência ao CompositionObject, crie uma referência com um nome e, depois, atribua valores diferentes:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Este será o código se você tiver definido sua expressão com cadeias de caracteres por meio da API pública.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
