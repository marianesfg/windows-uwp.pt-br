---
title: Animações de propriedade XAML
description: Animar elementos XAML com animações de composição.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 183a5433553ff6fdfcb09f6960f6a642f2c8bc08
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444160"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animação de elementos XAML com animações de composição

Este artigo apresenta novas propriedades que permitem que você animar um UIElement XAML com o desempenho de animações de composição e a facilidade de propriedades de configuração de XAML.

Antes do Windows 10, versão 1809, você precisava 2 opções para criar animações em seus aplicativos UWP:

- usar construções XAML, como [Storyboarded animações](storyboarded-animations.md), ou o _* ThemeTransition_ e _* ThemeAnimation_ classes no [ Animation](/uwp/api/windows.ui.xaml.media.animation) namespace.
- usar animações de composição, conforme descrito em [usando a camada Visual com XAML](../../composition/using-the-visual-layer-with-xaml.md).

Usando a camada visual fornece desempenho melhor do que usar o XAML constrói. Mas o uso [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) obter o elemento subjacente do composição [Visual](/uwp/api/windows.ui.composition.visual) objeto e, em seguida, animando o Visual com animações de composição, é mais complexa de usar.

A partir do Windows 10, versão 1809, você pode animar as propriedades de um UIElement diretamente usando animações de composição sem a necessidade de obter a Visual de composição subjacente.

> [!NOTE]
> Para usar essas propriedades em UIElement, sua versão de destino do projeto UWP deve ser 1809 ou posterior. Para obter mais informações sobre como configurar sua versão do projeto, consulte [versão dos aplicativos adaptáveis](../../debug-test-perf/version-adaptive-apps.md).

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Se você tiver o <strong style="font-weight: semi-bold">da Galeria de controles XAML</strong> aplicativo instalado, clique aqui para <a href="xamlcontrolsgallery:/item/XamlCompInterop">abrir o aplicativo e ver a interoperabilidade de animação em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Novas propriedades de renderização substituir as propriedades de renderização antigo

Esta tabela mostra as propriedades que você pode usar para modificar a renderização de um UIElement, que também pode ser animado com um [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propriedade | Tipo | Descrição |
| -- | -- | -- |
| [Opacidade](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | O grau de opacidade do objeto |
| [Tradução](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Alterar a posição X/Y/Z do elemento |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | A matriz de transformação a ser aplicado ao elemento |
| [Escala](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Escala do elemento, centralizada no PontoCentral |
| [Rotação](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | Girar o elemento em torno de RotationAxis e PontoCentral |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | O eixo de rotação |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | O ponto central de escala e rotação |

O valor da propriedade TransformMatrix é combinado com as propriedades de escala, rotação e translação na seguinte ordem:  TransformMatrix, escala, rotação, translação.

Essas propriedades não afetam o layout do elemento, portanto, modificar essas propriedades não faz com que uma nova [medida](/uwp/api/windows.ui.xaml.uielement.measure)/[organizar](/uwp/api/windows.ui.xaml.uielement.arrange) passar.

Essas propriedades têm o mesmo objetivo e comportamento, como as propriedades de nome semelhante a composição [Visual](/uwp/api/windows.ui.composition.visual) classe (com exceção de conversão, não no Visual).

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

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusividade mútua entre propriedades novas e antigas

> [!NOTE]
> O **opacidade** propriedade não impõe a exclusividade mútua descrita nesta seção. Se você usar animações de composição ou de XAML, você usar a mesma propriedade de opacidade.

As propriedades que podem ser animadas com um CompositionAnimation são substituições para várias propriedades de UIElement existentes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Projection](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Quando você define (ou anima) qualquer uma das novas propriedades, é possível usar as propriedades antigas. Por outro lado, se você definir (ou anima) qualquer uma das propriedades antigas, você não pode usar as novas propriedades.

Você também não é possível usar as novas propriedades se você usar ElementCompositionPreview para obter e gerenciar o Visual por conta própria usando estes métodos:

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> A tentativa de combinar o uso de dois conjuntos de propriedades fará com que a chamada à API falhar e gerar uma mensagem de erro.

É possível alternar de um conjunto de propriedades desmarcando-las, embora para manter a simplicidade não é recomendável. Se a propriedade é apoiada por um DependencyProperty (por exemplo, UIElement.Projection é apoiada por UIElement.ProjectionProperty), em seguida, chame ClearValue para restaurá-lo ao estado "não usado". Caso contrário (por exemplo, a propriedade escala), defina a propriedade como seu valor padrão.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animação de propriedades de UIElement com CompositionAnimation

Você pode animar as propriedades de renderização listadas na tabela com um CompositionAnimation. Essas propriedades também podem ser referenciadas por um [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Use o [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) e [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) métodos em UIElement para animar as propriedades de UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Exemplo: Animar a propriedade de dimensionamento com um Vector3KeyFrameAnimation

Este exemplo mostra como animar a escala de um botão.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Exemplo: Animar a propriedade de dimensionamento com um ExpressionAnimation

Uma página tem dois botões. O segundo botão é animado para ser duas vezes tão grande (por meio de escala) como o primeiro botão.

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

- [Animações storyboarded](storyboarded-animations.md)
- [Usando a camada Visual com XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Visão geral de transformações](../layout/transforms.md)
