---
author: Jwmsft
title: Animações de propriedade XAML
description: Animando elementos XAML com animações de composição.
ms.author: jimwalk
ms.date: 09/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.openlocfilehash: a03ffc8d5ea78ee6cbdf78feaae7ba1cd1448f37
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5434533"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animando elementos XAML com animações de composição

Este artigo apresenta novas propriedades que permitem que você animar um UIElement XAML com o desempenho de animações de composição e a facilidade de definindo propriedades do XAML.

Antes do Windows 10, versão 1809, você tinha 2 opções para criar animações nos aplicativos UWP:

- usar XAML construções como [animações com storyboard](storyboarded-animations.md), ou o _* ThemeTransition_ e _* ThemeAnimation_ classes no namespace [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) .
- Use animações de composição, conforme descrito em [usando a camada Visual com XAML](../../composition/using-the-visual-layer-with-xaml.md).

Usando a camada visual fornece melhor desempenho que constrói usar XAML. Mas usando [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) para obter o objeto subjacente de [Visual](/uwp/api/windows.ui.composition.visual) de composição do elemento e, em seguida, animando o elemento Visual com animações de composição, são mais complexo para usar.

A partir do Windows 10, versão 1809, você pode animar propriedades em um UIElement diretamente usando animações de composição sem a exigência de obter a Visual de composição subjacente.

> [!NOTE]
> Para usar essas propriedades em UIElement, sua versão de destino do projeto UWP deve ser 1809 ou posterior. Para obter mais informações sobre como configurar sua versão do projeto, consulte [aplicativos adaptáveis de versão](../../debug-test-perf/version-adaptive-apps.md).

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Novas propriedades de renderização substituir antigo propriedades de renderização

Esta tabela mostra as propriedades que você pode usar para modificar a renderização de um UIElement, que também pode ser animado com um [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| [Opacidade](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | O grau de opacidade do objeto |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Deslocar a posição X/Y/Z do elemento |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | A matriz de transformação para aplicar ao elemento |
| [Scale](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Dimensionar o elemento, centralizado sobre o ponto central |
| [Rotação](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | Girar o elemento em torno da RotationAxis e o ponto central |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | O eixo de rotação |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | O ponto central de dimensionamento e rotação |

O valor da propriedade TransformMatrix é combinado com as propriedades de escala, rotação e conversão na seguinte ordem: TransformMatrix, escala, rotação e conversão.

Essas propriedades não afetam o layout do elemento, portanto, modificar essas propriedades não faz com que uma nova [medição](/uwp/api/windows.ui.xaml.uielement.measure)/[cálculo](/uwp/api/windows.ui.xaml.uielement.arrange) .

Essas propriedades têm a mesma finalidade e o comportamento que as propriedades de nome semelhante na composição [Visual](/uwp/api/windows.ui.composition.visual) classe (exceto a conversão não está em Visual).

### <a name="example-setting-the-scale-property"></a>Exemplo: Definindo a propriedade de escala

Este exemplo mostra como definir a propriedade de escala em um botão.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusividade mútua entre as propriedades de novas e antigas

> [!NOTE]
> A propriedade de **opacidade** não impõe a exclusividade mútua descrita nesta seção. Use a mesma propriedade de opacidade se você usa animações de composição ou de XAML.

As propriedades que podem ser animadas com um CompositionAnimation são substituições para várias propriedades de UIElement existentes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projeção](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Quando você define (ou animar) qualquer uma das novas propriedades, você não pode usar as propriedades antigas. Por outro lado, se você definir (ou animar) qualquer uma das propriedades antigas, você não pode usar as novas propriedades.

Você também não pode usar as novas propriedades se você usar ElementCompositionPreview para obter e gerenciar o Visual por conta própria usando estes métodos:

- [Elementcompositionpreview. Getelementvisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> A tentativa de combinar o uso de dois conjuntos de propriedades fará com que a chamada de API falhar e produzir uma mensagem de erro.

É possível alternar de um conjunto de propriedades, desmarcando-los, mas para simplificar não é recomendado. Se a propriedade é feita por uma DependencyProperty (por exemplo, Projection é sustentado por UIElement.ProjectionProperty), em seguida, chame ClearValue para restaurá-lo para seu estado "não utilizado". Caso contrário, (por exemplo, a propriedade Scale), defina a propriedade para o valor padrão.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animar propriedades UIElement com CompositionAnimation

Você pode animar as propriedades de renderização listadas na tabela com um CompositionAnimation. Essas propriedades também podem ser referenciadas por um [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Use os métodos [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) e [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) em UIElement para animar as propriedades de UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Exemplo: Como animar a propriedade de escala com uma Vector3KeyFrameAnimation

Este exemplo mostra como animar a escala de um botão.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Exemplo: Como animar a propriedade de escala com um ExpressionAnimation

Uma página tem dois botões. O segundo botão anima para duas vezes ser tão grande (via scale) como o primeiro botão.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>Tópicos relacionados

- [Animações de storyboard](storyboarded-animations.md)
- [Usando a Camada Visual com XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Visão geral das transformações](../layout/transforms.md)