---
author: jwmsft
title: Aprimorar as experiências existentes do ScrollViewer
description: Saiba como usar um ScrollViewer XAML e o ExpressionAnimations para criar experiências dinâmicas de movimento controladas por entrada.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animação
ms.localizationpriority: medium
ms.openlocfilehash: a078d096a9cffe26e9b342250726dd75cdf48817
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6662959"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Aprimorar as experiências existentes do ScrollViewer

Este artigo explica como usar um ScrollViewer XAML e o ExpressionAnimations para criar experiências dinâmicas de movimento controladas por entrada.

## <a name="prerequisites"></a>Pré-requisitos

Aqui, presumimos que você esteja familiarizado com os conceitos abordados neste artigo:

- [Animações controladas por entrada](input-driven-animations.md)
- [Animações baseadas em relações](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>Por que criar sobre o ScrollViewer?

Normalmente, você usa o ScrollViewer XAML existente para criar uma superfície de rolagem e zoom para o conteúdo do app. Com a introdução da linguagem do Fluent Design, agora você deve também abordar o uso da rolagem ou do zoom em uma superfície para criar outras experiências de movimento. Por exemplo, usando a rolagem para criar uma animação de desfoque de uma tela de fundo ou criar a posição de um "cabeçalho fixo".

Nesses cenários, você está aproveitando as experiências de comportamento ou manipulação, como rolagem e zoom, para tornar outras partes do app mais dinâmicas. Eles, por sua vez, permitem que o app pareça mais coeso, tornando as experiências mais fáceis de memorizar aos olhos dos usuários finais. Com a interface do usuário mais fácil de memorizar, os usuários finais se envolverão com o app com mais frequência e por períodos mais longos.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>O que você pode criar sobre o ScrollViewer?

Você pode aproveitar a posição de um ScrollViewer para criar diversas experiências dinâmicas:

- Paralaxe – use a posição de um ScrollViewer para mover o conteúdo de primeiro plano ou de segundo plano em um ritmo relativo à posição de rolagem.
- StickyHeaders – use a posição de um ScrollViewer para animar e "fixar" um cabeçalho em uma posição
- Efeitos controlados por entrada – use a posição de um Scrollviewer para animar um efeito de composição, como o desfoque.

Em geral, fazendo referência à posição de um ScrollViewer com um ExpressionAnimation, você pode criar uma animação que muda dinamicamente em relação à quantidade de rolagem.

![Exibição de lista com paralaxe](images/animation/parallax.gif)

![Um cabeçalho tímido](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>Usando o ScrollManipulationPropertySet

Para criar essas experiências dinâmicas por meio de um ScrollViewer XAML, você precisará fazer referência à posição de rolagem em uma animação. Isso é feito através do acesso de um CompositionPropertySet a partir do ScrollViewer XAML chamado ScrollManipulationPropertySet.
O ScrollManipulationPropertySet contém uma única propriedade Vector3 chamada Translation que fornece acesso à posição de rolagem do ScrollViewer. Em seguida, você poderá referenciar isso como qualquer outro CompositionPropertySet no ExpressionAnimation.

Etapas gerais para começar:

1. Acesse o ScrollManipulationPropertySet via ElementCompositionPreview.
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. Crie uma ExpressionAnimation que faça referência à propriedade Translation no PropertySet.
    - Não se esqueça de definir o parâmetro de referência!
1. Indique uma propriedade de CompositionObject com o ExpressionAnimation.

> [!NOTE]
> É recomendável que você atribua o PropertySet retornado pelo método GetScrollManipulationPropertySet a uma variável de classe. Isso garante que o conjunto de propriedades não será eliminado pela coleta de lixo e, portanto, não tem nenhum efeito sobre o ExpressionAnimation no qual ele é referenciado. O ExpressionAnimations não mantém uma referência forte a nenhum dos objetos usados na equação.

## <a name="example"></a>Exemplo

Vamos analisar como o paralaxe de exemplo mostrada acima é combinado. Para referência, todo o código-fonte do app é encontrado no [repositório de laboratórios de desenvolvimento de interface do usuário do Windows no GitHub](https://github.com/Microsoft/WindowsUIDevLabs).

A primeira providência a ser tomada é obter uma referência ao ScrollManipulationPropertySet.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

A próxima etapa é criar o ExpressionAnimation que define uma equação que utiliza a posição de rolagem do ScrollViewer.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> Você também pode utilizar classes auxiliares do ExpressionBuilder para criar essa mesma expressão sem usar cadeias de caracteres:

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Por fim, use esse ExpressionAnimation e indique o elemento visual ao qual você deseja atribuir o efeito paralaxe. Nesse caso, é a imagem de cada um dos itens da lista.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```